+++
title = "X Macros"
author = ["Victor Ren"]
description = "A simple technique for solving consistency problems in C/C++ projects"
date = 2026-03-12T00:00:00+08:00
keywords = ["X", "Macros", [",", "C"], "language", [",", "metaprogramming"]]
tags = ["C/C++"]
categories = ["Trifle"]
draft = false
+++

> No problem can be solved at the same level of thinking that created it.[^fn:1]
> --- "Albert Einstein"


## Consistency Challenge {#consistency-challenge}

The first time I ran into X Macros was while debugging an out-of-bounds access issue.

Here is a simplified example:

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
    // SYS_ERR_NOT_FOUND is missing here
};

int main()
{
    printf("State = %s\n", state_desc[SYS_ERR_NOT_FOUND]);
}
```

The issue is easy to spot: `SYS_ERR_NOT_FOUND` was added to the enum,
but the string table was not updated.

When the program evaluates `state_desc[SYS_ERR_NOT_FOUND]`, it reads past the
end of the array and eventually crashes.

Of course, we could fix this by adding the missing entry to the string table.
But that only fixes this specific bug — it does not prevent the same type of
problem from happening again.

The underlying issue is that we are maintaining **two pieces of information that
must always stay consistent**:

-   the enum definition
-   the string table

As long as these are maintained separately and rely on manual synchronization,
mistakes like this are inevitable.

What we really need is a way to eliminate this class of problems entirely.


## X Macros {#x-macros}

While looking for a solution, I came across an article by Randy Meyers
published in _Dr. Dobb's Journal_ titled **The New C: X Macros**&nbsp;[^fn:2].
The article introduces a technique that uses the C preprocessor to generate
code from a single data definition.

This technique is commonly known as **X Macros**.

The idea is simple: maintain a single data list, and generate the necessary
code structures by redefining a macro.

Let's modify the previous example to see how this works.

First, define a **Single source of truth**:

```C
#define SYS_STATE_LIST                         \
    X(SYS_OK,              "System OK")        \
    X(SYS_ERR_TIMEOUT,     "Timeout")          \
    X(SYS_ERR_BUSY,        "System Busy")      \
    X(SYS_ERR_INVALID_ARG, "Invalid Argument") \
    X(SYS_ERR_NOT_FOUND,   "Not Found")
```

This list becomes the master definition of our data.
Each entry is wrapped inside a placeholder macro `X()`.

The name `X` is simply a convention. It has no special meaning and can be
replaced with any name.

Next, we define the macro `X` to generate the enum:

```C
#define X(name, desc) name,
typedef enum {
    SYS_STATE_LIST
} SysState;
#undef X
```

Then we redefine `X` so that the same list generates the string table:

```C
#define X(name, desc) [name] = desc,
const char *state_desc[] = {
    SYS_STATE_LIST
};
#undef X
```

If you come from dynamic languages such as Python or Java, this might look
somewhat like a lightweight form of reflection.

X Macros gather scattered information into a single data list. By redefining
and undefining `X` (`#undef`), the same data can be injected into multiple
code templates. Adding, removing, or reordering entries will not introduce
inconsistencies.

This technique works well in many situations where related data must stay
synchronized, for example:

-   Managing error codes, states, or event types together with their descriptions
-   Generating command tables or protocol definitions
-   Serialization and deserialization
-   Configuration data initialization and access code generating


## Advanced Notes {#advanced-notes}

X Macros are powerful, but they do come with trade-offs. Macro-heavy code can
reduce readability and make debugging more difficult.

Some practical tips:

-   **Improve readability**: Andrew Lucas suggests passing the macro `X` as a
    parameter to the data list to make the structure clearer&nbsp;[^fn:3].

-   **Large data sets**: If the list becomes very long, you may hit compiler
    line-length limits. Randy Meyers recommends placing the list in a separate
    `.def` or `.h` file and using `#include` to reuse it.

-   **Inspect preprocessed output**: You can use `gcc -E` to view the expanded
    source code. Many modern IDEs with language servers can also show this.

-   **Team communication**: Not everyone is familiar with this technique. Good
    comments and documentation help avoid confusion.

-   **Use it sparingly**: Like most macro tricks, X Macros should be used
    thoughtfully rather than everywhere.


## Summary {#summary}

X Macros are essentially a lightweight metaprogramming technique built on top
of the C/C++ preprocessor. In environments without reflection or code
generation tools, they provide a practical way to keep related definitions
synchronized.

By maintaining a **Single Source of Truth** and generating code through macro
expansion, X Macros eliminate the risk of inconsistent duplicated definitions.

The idea dates back to assembly programming in the 1960s. Even today, whenever
large amounts of structured data need to remain consistent, it remains a
surprisingly useful technique — and a valuable tool in the C/C++ programmer's
toolbox.

[^fn:1]: AI translated.  I discovered this link when I tried to find the source of this quote:
    <https://www.quora.com/Einstein-said-that-you-cannot-solve-a-problem-from-the-same-level-of-consciousness-that-created-it-What-did-he-mean-with-it-Can-you-use-a-concrete-example>
[^fn:2]: Randy Meyers. "The New C: X macros", Dr.Dobb's 2001, accessible at:
    <https://jacobfilipp.com/DrDobbs/articles/CUJ/2001/0105/meyers/meyers.htm>
[^fn:3]: Andrew Lucas. ["Reduce C-language coding errors with X macros"](https://www.embedded.com/reduce-c-language-coding-errors-with-x-macros-part-1/), Embedded.com, 2013.
