+++
title = "X Macros"
author = ["Victor Ren"]
description = "C/C++ 项目中解决一致性问题的一种简单技巧"
date = 2026-03-12T00:00:00+08:00
keywords = ["X", "Macros", [",", "C语言"], [",", "元编程"]]
tags = ["C/C++", "Programming"]
categories = ["Trifle"]
draft = false
+++

> 我们无法用制造问题的同一思维层次来解决问题。[^fn:1]
> --- “阿尔伯特·爱因斯坦”


## 一致性挑战 {#一致性挑战}

我第一次接触 X Macros，是在排查一个越界访问问题的时候。

下面是一段简化后的示例代码：

```C
// a.h
typedef enum {
    SYS_OK,
    SYS_ERR_TIMEOUT,
    SYS_ERR_BUSY,
    SYS_ERR_INVALID_ARG,
    SYS_ERR_NOT_FOUND
} SysState;

// a.c
const char *state_desc[] = {
    [SYS_OK] = "System OK",
    [SYS_ERR_TIMEOUT] = "Timeout",
    [SYS_ERR_BUSY] = "System Busy",
    [SYS_ERR_INVALID_ARG] = "Invalid Argument"
    // 这里没有 SYS_ERR_NOT_FOUND
};

int main()
{
    printf("State = %s\n", state_desc[SYS_ERR_NOT_FOUND]);
}
```

问题显而易见：枚举中新增了`SYS_ERR_NOT_FOUND`，但字符串数组没有同步更新。

程序执行到 `state_desc[SYS_ERR_NOT_FOUND]`时会越过数组边界，最终导致崩溃。

只需要在字符串数组里加一行就可以防止访问越界。但是这样的修复并不能防止同类的问题
再次发生。原因在于，这里实际上维护了**两份必须保持一致的数据**：

-   一份是枚举定义
-   一份是字符串表

只要这两份数据分散在不同位置，并且依赖人工同步维护，就很容易再次出现类似问题。

这里需要一种能够防止同类问题再次发生的方案。


## X Macros {#x-macros}

在查找解决方案的过程中，我看到了 Randy Meyers 发表在 _Dr. Dobb's Journal_ 上的一
篇文章 **The New C: X Macros**&nbsp;[^fn:2]。文章介绍了一种利用 C 预处理器实现简单代码生
成的技巧。

这项技巧通常被称为 **X Macros** 。

它的基本思路是维护一份统一的数据列表，然后通过不同的宏定义方式生成所需要的代码结构。

我们可以对前面的例子做一个简单改造, 来展示 X Macros 的用法。

首先定义**单一数据源**：

```C
#define SYS_STATE_LIST                         \
    X(SYS_OK,              "System OK")        \
    X(SYS_ERR_TIMEOUT,     "Timeout")          \
    X(SYS_ERR_BUSY,        "System Busy")      \
    X(SYS_ERR_INVALID_ARG, "Invalid Argument") \
    X(SYS_ERR_NOT_FOUND,   "Not Found")
```

这里创建了一个包含所有信息的“主列表”，每个条目都包裹在一个占位宏 `X()` 中 。
`X()`就是这项技巧名称的由来，并没有什么特殊含义，当然也可以用其他名字。

接下来，我们定义宏 `X` ，利用这份数据生成枚举类型：

```C
#define X(name, desc) name,
typedef enum {
    SYS_STATE_LIST
} SysState;
#undef X
```

然后，重新定义宏 `X` ，让同一份数据展开成字符串描述数组：

```C
#define X(name, desc) [name] = desc,
const char *state_desc[] = {
    SYS_STATE_LIST
};
#undef X
```

对于熟悉 Python 或 Java 等动态语言的开发者，上述例子可能让您联想到反射机制。可
以看到，X Macros 把分散的信息收拢到单一的数据表中，通过对宏 `X` 的多次定义和撤销
(`#undef`)，我们可以将上述数据“注入”到不同的模板中 。不论你添加还是删除数据，
甚至前后移动都不会带来不一致的问题。

这项技巧适用于许多需要维护数据一致性的场合：

-   错误码，状态，事件类型及描述的管理
-   命令，协议定义，解析代码的生成
-   序列化与反序列化
-   配置数据初始化与读写代码的生成


## 进阶提示 {#进阶提示}

X Macros 带来的好处并不是没有代价的。宏的写法会略微降低代码的直观可读性，可调试性。

-   **提高可读性**：可以参考 Andrew Lucas 的建议，把宏 `X` 作为参数传递给数据列表，以提
    高代码的可读性[^fn:3]。

-   **大数据量**：如果数据量足够大，可能会碰到编译器行长度限制。 Randy Meyers在文章
    中已经给出了方案 —— 把数据列表定义在一个独立的 .def 或者 .h 文件中, 在需要展开
    的地方用 `#include` 包含进来。

-   **预编译检查**：可以通过 gcc -E 查看预编译结果。如果你用 IDE 结合语言服务器的话，
    就更容易了。

-   **团队沟通**：这项技巧并非人人皆知，最好在代码中加上必要注释，并向团队成员说明
    其用途。

-   **适度使用**：与其他的宏技巧一样，应当谨慎使用，避免滥用。


## 总结 {#总结}

X Macros 本质上是一种基于 C/C++ 预处理器的简单元编程技巧。在缺乏反射或代码生成机
制的情况下，这种方法为 C/C++ 提供了一种轻量而实用的工程技巧。

它通过维护一份**单一数据源（Single Source of Truth）**，再利用宏展开生成不同的代
码结构，从而避免多处重复定义带来的同步问题。

它的历史可以溯源到上世纪60年代的汇编语言编程，传承至今， 在需要维护大量一致性数
据的场景中，它仍然有不可替代的价值, 是每一个C/C++工程师的必备工具。

[^fn:1]: 溯源的时候，发现这个：
    <https://www.quora.com/Einstein-said-that-you-cannot-solve-a-problem-from-the-same-level-of-consciousness-that-created-it-What-did-he-mean-with-it-Can-you-use-a-concrete-example>
[^fn:2]: Randy Meyers. "The New C: X macros", Dr.Dobb's 2001, 可访问链接：
    <https://jacobfilipp.com/DrDobbs/articles/CUJ/2001/0105/meyers/meyers.htm>
[^fn:3]: Andrew Lucas. ["Reduce C-language coding errors with X macros"](https://www.embedded.com/reduce-c-language-coding-errors-with-x-macros-part-1/), Embedded.com, 2013.
