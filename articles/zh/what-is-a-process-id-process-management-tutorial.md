```markdown
---
title: 什么是进程 ID？如何使用 PID 进行进程管理
date: 2025-02-03T05:27:46.220Z
author: Syeda Maham Fahim
authorURL: https://www.freecodecamp.org/news/author/syedamahamfahim/
originalURL: https://www.freecodecamp.org/news/what-is-a-process-id-process-management-tutorial/
posteditor: ""
proofreader: ""
---

你是否曾经想过，当多个程序同时运行时，计算机如何知道显示哪个程序的输出？这一切都归功于进程 ID (PID)。

<!-- more -->

PID 是一个唯一标识符，帮助操作系统跟踪和管理运行中的程序。

在本文中，我们将探讨什么是进程 ID (PID)，它为何重要，以及如何使用它来管理进程，包括在必要时终止程序。

## 理解 PID：计算机如何识别正在运行的程序？

让我们考虑两个 Python 脚本：

-   `hello_maham.py` → `print("Hello Maham")`
    
-   `hello_amna.py` → `print("Hello Amna")`
    

计算机如何知道 `hello_maham.py` 应该显示输出 "Hello Maham" 而不是来自 `hello_amna.py` 的 "Hello Amna"？

如果你认为这只是魔法般地发生，那就再想想吧！这都要归功于一个叫做 **进程 ID (PID)** 的东西。

在任何操作系统中，进程都在后台不停地运行以执行任务。无论是你手动启动的程序还是自动运行的系统任务，这些进程都被分配了一个唯一的 PID。

让我们进一步解析这个过程。

## 什么是进程 ID (PID)？

简单来说，

> _一个_ **_进程 ID (PID)_** _是分配给操作系统中每个运行进程的唯一标识符。_

让我们来了解后台发生了什么。

每当一个程序运行时，无论是什么语言，它都需要内存和时间来执行。因此，当你运行一个程序时，操作系统会为它创建一个新的进程。为了识别这个程序，计算机会为它分配一个独特的标识符——**进程 ID**——然后开始执行。

让我们重温之前的例子：

-   当你运行 `hello_maham.py` 时，系统为它分配一个唯一的 PID。
    
-   同样，当你运行 `hello_amna.py` 时，它也会有自己独特的 PID。
    

这就是为什么两个脚本的输出不会重叠！

现在明白了吗？每当创建一个新进程时，系统会确保每个进程获得不同的 PID。系统使用这个 PID 来管理和与进程交互。这就叫做 **PID 的唯一性**

### 计算机如何处理这个 ID？

你可能会想，计算机是否拥有数百万个 PID？毕竟，我们可能同时运行很多程序。

答案是**不**。一旦一个进程结束，PID 会再次可用于重用。这意味着 PID 是可重复使用的，并且不会短缺。

### 但为什么以及何时需要 PID？

现在你知道了 PID 是什么，你可能会想：**我为什么需要这个？**

实际上，PID 对于系统管理员和开发人员非常有用。它们有助于：

**系统管理员和开发人员管理进程**。例如，如果某个东西不正常工作，你需要能够找到并停止引起问题的具体进程，对吧？

PID 对于**资源管理**也很重要。操作系统使用它们来为每个进程分配内存和 CPU 时间，以确保没有一个程序占用所有资源。

## 如何查找正在运行程序的 PID

到目前为止，我们已经讲解了理论概念。现在，你可能会想：我如何在计算机上找到一个程序的 PID？

以下是一些使用终端中各种命令查找正在运行程序的 **PID** 的简单方法。

注意，我使用的是 Bash 来执行 PID 命令，并附上了一个截图。但对于其他终端如 CMD 和 PowerShell，相关命令将在最后提到。

其中一些方法包括：

### 1\. 使用 `ps` 命令

`ps` 命令显示当前正在运行的进程及其 PID 的快照。

```
ps aux | grep <program_name>
```

这里是一个例子：

```
ps aux | grep python
```

此命令将显示系统上所有正在运行的 Python 进程及其 PID。

![运行的 Python 进程及其 PID](https://cdn.hashnode.com/res/hashnode/image/upload/v1737914130884/686568b8-77d5-439d-a08b-61e18d4e5d56.png)

### 2\. 使用 `top` 命令

`top` 命令显示系统上正在运行的进程的实时信息，包括它们的 PID。

```
top
```

在 **PID** 列下查看你感兴趣的进程。

![运行 top 命令的结果](https://cdn.hashnode.com/res/hashnode/image/upload/v1737914189672/43f93890-2e06-42a8-b61b-9103227cf912.png)

## 如何使用 PID 终止进程

如果你想**终止**程序怎么办？无论是 cron 作业还是行为异常或运行时间过长的程序，如何使用 PID 停止它？

让我们了解一下如何做到这一点：

### 1\. 使用 `kill` 命令

要终止进程，请使用 `kill` 命令和 PID：

```
kill <PID>
```

这里是一个例子：

```
kill 1234
```
```

### 2\. 使用 `kill -9` 命令（强制终止）

如果在使用常规 `kill` 命令之后进程仍未停止，可以使用 `kill -9` 命令强制终止它：

```
kill -9 <PID>
```

这是一个例子：

```
kill -9 1234
```

这会强制终止进程并绕过其可能有的任何关闭程序，因此请务必谨慎使用。

## 如何使用 PID 停止 Cron 作业——实际示例

假设你有一个 cron 作业正在运行，并且出现了问题。你该如何停止它？

Cron 作业是指按特定时间间隔自动运行的计划任务。

如果你需要停止正在运行的 cron 作业，可以使用运行该 cron 作业的进程的 PID。

### 停止正在运行的 Cron 作业

如果你需要停止正在运行的 cron 作业，可以使用运行该 cron 作业的进程的 PID。

以下是终止 cron 作业的方法：

1.  **查找 PID**：使用 `ps` 或 `pgrep` 命令查找 cron 作业的 PID。例子：
    
    ```
    ps aux | grep cron
    ```
    
2.  **终止 Cron 作业**：找到 cron 作业的 PID 后，使用 `kill` 或 `kill -9` 命令停止它。
    

## 其他终端的命令：

以下是在不同终端中管理进程的方法：

| 操作 | CMD 命令 | PowerShell 命令 | Bash 命令 |
| --- | --- | --- | --- |
| **列出所有进程** | `tasklist` | `Get-Process` | `ps aux` |
| **按名称查找进程** | `tasklist` | `findstr <name>` | `Get-Process` |
| **通过 PID 终止进程** | `taskkill /PID <PID>` | `Stop-Process -Id <PID>` | `kill <PID>` |
| **强制终止进程** | `taskkill /F /PID <PID>` | `Stop-Process -Id <PID> -Force` | `kill -9 <PID>` |

## 结论

理解 **进程 ID** 是管理计算机上进程的关键。通过简单的命令，你可以轻松找到并停止有问题的进程，确保系统的顺畅运行。

因此，通过使用 PID 来控制你的系统！

**保持联系 — @syedamahamfahim 🐬**

