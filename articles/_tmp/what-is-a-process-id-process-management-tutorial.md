---
title: What is a Process ID? How to Use PIDs for Process Management
date: 2025-02-03T05:27:46.220Z
author: Syeda Maham Fahim
authorURL: https://www.freecodecamp.org/news/author/syedamahamfahim/
originalURL: https://www.freecodecamp.org/news/what-is-a-process-id-process-management-tutorial/
posteditor: ""
proofreader: ""
---

Have you ever wondered how a computer knows which program’s output to display, especially when multiple programs are running simultaneously? This is possible because of the Process ID (PID).

<!-- more -->

A PID is a unique identifier that helps the operating system track and manage running programs.

In this article, we’ll explore what a Process ID (PID) is, why it’s important, and how you can use it to manage processes, including terminating a program when necessary.

## Understanding PIDs: How Does a Computer Identify Running Programs?

Let’s consider two Python scripts:

-   `hello_maham.py` → `print("Hello Maham")`
    
-   `hello_amna.py` → `print("Hello Amna")`
    

How does the computer know that `hello_maham.py` should show the output "Hello Maham" and not "Hello Amna" from `hello_amna.py`?

If you think it just happens magically, then think again! This happens because of something called a **Process ID (PID)**.

In any operating system, processes are constantly running in the background to execute tasks. Whether it’s a program you launched manually or a system task running automatically, each of these processes is assigned a unique PID.

Let’s break this down further.

## What is a Process ID (PID)?

Simply put,

> _A_ **_Process ID (PID)_** _is a unique identifier assigned to each process running in an operating system._

Let’s understand what’s going on in the background.

Whenever a program runs, no matter the language, it needs memory and time to execute. So, when you run a program, the operating system creates a new process for it. To identify the program, the computer assigns it a unique identifier – the **Process ID** – and then it begins execution.

Let’s revisit our previous example:

-   When you run `hello_maham.py`, the system assigns a unique PID to it.
    
-   Similarly, when you run `hello_amna.py`, it gets its own unique PID.
    

This is why the outputs of both scripts don’t overlap!

Now, you got it? Each time a new process is created, the system ensures that every process gets a different PID. This PID is used by the system to manage and interact with processes. Its called **Uniqueness Of PIDs**

### How Does the Computer Handle This ID?

Now you may wonder, does the computer have millions of PIDs? After all, we could be running many programs at once.

The answer is **no**. Once a process ends, the PID becomes available again for reuse. This means that PIDs are reusable, and there is no shortage of them.

### But Why and When Do I Need PIDs?

Now that you know what a PID is, you might be wondering: **Why do I need this?**

Well, PIDs are actually very useful for system administrators and developers. They help in:

**System admins and developers manage processes**. Like, if something’s not working properly, you need to be able to find and stop the specific process causing the issue, right?

PIDs are also important for **resource management_._** The operating system uses them to allocate memory and CPU time to each process, so no one program hogs everything.

## How to Find the PID of a Running Program

Up until now, we've covered the theoretical concepts. Now, you might be wondering: How do I actually find the PID of a program on my computer?

Well, here are some simple ways to find the **PID** of a running program using various commands in the terminal.

Note, that I’ve used Bash to execute the PID command and included a screenshot for it. But for other terminals like CMD and PowerShell, the respective commands are mentioned at the end.

Some of these methods include:

### 1\. Using the `ps` Command

The `ps` command shows a snapshot of the current running processes and their PIDs.

```
ps aux | grep <program_name>
```

Here’s an example:

```
ps aux | grep python
```

This command will display all the Python processes running on the system, along with their PIDs.

![Python processes running plus their PIDs](https://cdn.hashnode.com/res/hashnode/image/upload/v1737914130884/686568b8-77d5-439d-a08b-61e18d4e5d56.png)

### 2\. Using the `top` Command

The `top` command shows real-time information about processes running on the system, including their PIDs.

```
top
```

Look under the **PID** column to find the process you're interested in.

![Result of running the top command](https://cdn.hashnode.com/res/hashnode/image/upload/v1737914189672/43f93890-2e06-42a8-b61b-9103227cf912.png)

## How to Kill a Process Using its PID

What if you want to **kill** the program? Whether it’s a cron job or a program that is misbehaving or running for too long, how can you stop it using the PID?

Let’s go through how you can do that:

### 1\. Using the `kill` Command

To terminate a process, use the `kill` command followed by the PID:

```
kill <PID>
```

Here’s an example:

```
kill 1234
```

This command will gracefully terminate the process with PID `1234`.

### 2\. Using the `kill -9` Command (Force Kill)

If the process does not stop after using the regular `kill` command, you can force kill it using the `kill -9` command:

```
kill -9 <PID>
```

Here’s an example:

```
kill -9 1234
```

This forcefully terminates the process and bypasses any shutdown procedures it may have, so make sure you use it with caution.

## How to Stop Cron Jobs Using a PID – Practical Example

So, let’s say you’ve got a cron job running, and it’s acting up. How do you stop it?

Cron jobs are scheduled tasks that run automatically at specific intervals.

If you need to stop a running cron job, you can use the PID of the process running the cron job.

### Stop a Running Cron Job

If you need to stop a running cron job, you can use the PID of the process running the cron job.

Here’s how to kill a cron job:

1.  **Find the PID**: Use the `ps` or `pgrep` command to find the PID of the cron job. Example:
    
    ```
     ps aux | grep cron
    ```
    
2.  **Kill the Cron Job**: Once you find the PID of the cron job, use the `kill` or `kill -9` command to stop it.
    

## Commands For Other Terminals:

Here’s how to manage processes in different terminals:

| Action | CMD Command | PowerShell Command | Bash Command |
| --- | --- | --- | --- |
| **List all processes** | `tasklist` | `Get-Process` | `ps aux` |
| **Find process by name** | `tasklist` | `findstr <name>` | `Get-Process` |
| **Kill process by PID** | `taskkill /PID <PID>` | `Stop-Process -Id <PID>` | `kill <PID>` |
| **Force kill process** | `taskkill /F /PID <PID>` | `Stop-Process -Id <PID> -Force` | `kill -9 <PID>` |

## Conclusion

Understanding **Process IDs** is key to managing processes on your computer. With simple commands, you can easily find and stop problematic processes, ensuring smooth system operation.

So, stay in control of your system with PIDs!

**Stay connected — @syedamahamfahim 🐬**