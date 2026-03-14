+++
title = "交换 Ctrl 和 Alt 键"
date = 2026-02-25T20:59:00+08:00
tags = ["Emacs", "keybinding"]
categories = ["Trifle"]
draft = false
creator = "Emacs 30.2 (Org mode 9.7.39 + ox-hugo)"
+++

{{< figure src="/ox-hugo/lisp-machine-keyboard-2.jpg" >}}

> 这篇修改过**翻译腔**的问题。原文放在后面，留作纪念。


## 修订版 {#修订版}

这是我所有计算机上的必备配置，全是因为 Emacs。

Emacs 最早是 TECO 编辑器的宏程序([Editing MACroS](https://blog.djmnet.org/2008/08/05/origin-of-emacs/))。当年开发者用的键盘，Ctrl 键在
空格键两侧[^fn:1]，大拇指就能按，所以 Emacs 最常用的命令都是用 Ctrl 修饰。不知为
何，后来的个人电脑标准键盘把 Ctrl 移到了最边上，在这些键盘上使用 Emacs 就显得不
太顺手。

我的解决办法是把 Ctrl 和 Alt 互换，让 Ctrl 回到大拇指能按到的位置。这样就可以用
大拇指来操作 Emacs 最常用的命令，既轻松又高效。

具体方法：

-   Windows 用[ SharpKeys ](https://github.com/randyrants/sharpkeys) 改键位；
-   Linux (GNOME 桌面)用 GNOME Tweak Tool直接设置[^fn:2]。

这是一个“一旦用上就再也回不去”的设置。它还有一个“隐藏功能”——让试图操作我键
盘的人抓狂。


## 原版 {#原版}

这是我所有所用计算机上的必备配置，全是因为 Emacs。

Emacs 最初是为 TECO 编辑器开发的宏程序([Editing MACroS](https://blog.djmnet.org/2008/08/05/origin-of-emacs/))。当时的开发者所用键盘布局
将 Ctrl 键放在空格键两侧[^fn:1]，便于大拇指操作。不知什么原因，微型机普及后的标准
键盘却将 Ctrl 键移至最外侧，只能靠小拇指操作。这种设计容易导致小拇指疲劳。

因此，我选择交换 Ctrl 和 Alt 键，让 Ctrl 键重回大拇指位置，既缓解小拇指负担，又
显著提升效率。

我的具体方法是：

-   Windows：通过[ SharpKeys ](https://github.com/randyrants/sharpkeys)实现键位重映射。
-   Linux (GNOME 桌面)：GNOME Tweak Tool可轻松完成设置[^fn:2]。

这是一个“一旦用上就回不去”的配置。它还有一个“隐藏功能”——让其他尝试操作我键
盘的人抓狂。


## 脚注： {#脚注}

[^fn:1]: Knight Keyboard <http://xahlee.info/kbd/knight_keyboard.html>
[^fn:2]: GNOME Treak Tool <https://askubuntu.com/questions/885045/how-to-swap-ctrl-and-alt-keys-in-ubuntu-16-04/885047>
