```markdown
---
title: 如何在红帽企业 Linux 中调度任务
date: 2025-07-01T12:34:15.216Z
author: Hang Hu
authorURL: https://www.freecodecamp.org/news/author/huhuhang/
originalURL: https://www.freecodecamp.org/news/how-to-schedule-tasks-in-red-hat-enterprise-linux/
posteditor: ""
proofreader: ""
---

红帽企业 Linux（RHEL）是一种领先的企业级 Linux 发行版，被广泛认为是关键任务服务器环境的黄金标准。它为从小型企业到财富 500 强公司提供强大、安全和可扩展的解决方案，为包括 Web 服务器、数据库、云基础设施和容器化应用在内的各种应用提供动力。

<!-- more -->

您可以在自动化系统维护（例如日志轮换或备份操作）、管理常规管理任务（如用户帐户清理或安全更新）或在企业环境中编排复杂工作流等场景中使用 RHEL 的任务调度功能。这些调度工具对于维护系统健康并确保关键操作无需人工干预即可运行至关重要。

对于系统管理员来说，可以将任务调度视为自动化系统管理的骨干，允许您设置可靠地在后台运行的进程，同时您专注于更具战略意义的举措。其强大的功能在于其灵活性和可靠性，使其成为任何生产环境中管理 Linux 系统的不可或缺的技能。

在本教程中，您将学习如何使用各种内置工具和技术在红帽企业 Linux 中调度任务。此内容是**计划未来任务**的一部分，它是 [Red Hat 系统管理 (RH134) 课程][1] 的第 2 章。 RH134 是获得红帽认证系统管理员 (RHCSA) 认证的基础课程之一，这是 Linux 管理领域最受认可的认证之一。

本次动手教程提供了 RH134 课程中所涵盖调度概念的实践经验，使您拥有在企业 RHEL 环境中有效自动化任务的技能。

### 我们将涵盖以下内容：

-   [如何使用 'at' 调度一次性作业][2]
    
-   [如何管理 'at' 作业][3]
    
-   [如何使用 'crontab' 调度周期性用户作业][4]
    
-   [如何管理用户 'crontab' 条目][5]
    
-   [如何使用 cron 目录调度周期性系统作业][6]
    
-   [如何配置 systemd 定时器以进行周期性任务][7]
    
-   [如何使用 systemd-tmpfiles 管理临时文件][8]
    

## **先决条件**

本教程旨在适合初学者！您只需对使用 Linux 命令行有基本的了解。如果您可以导航目录并运行简单命令，那么您就可以开始了。

对于希望加深 RHEL 知识的人， [RHEL 技能树][9] 提供了包括 [RH124][10]、[RH134][11]、[RH294][12] 以及其他 RHCSA 和 RHCE 认证课程的综合动手实验室。

不用担心如果您是红帽企业 Linux 的新手 - 我会一步步解释所有内容，并且这些概念也适用于大多数 Linux 发行版。

## **如何使用 'at' 调度一次性作业**

首先，让我们学习如何使用 `at` 命令安排在未来某个时间运行一次的作业。 `at` 命令对于执行不需要重复运行的命令很有用。我们将安排一个简单的作业，检查其详细信息，然后移除它。

在本教程中，我们将直接在本地系统上进行任务调度学习。您将在当前终端环境中执行所有命令。

让我们安排一个作业，将当前日期和时间打印到您的主目录中的一个名为 `~/myjob.txt` 的文件中。我们将安排其在 3 分钟后运行：

```
at now + 3 minutes << EOF
date > ~/myjob.txt
EOF
```

`warning: commands will be executed using /bin/sh` 信息是正常的。 `job N at ...` 输出指示作业编号和计划的执行时间。请记下作业编号，因为您稍后将需要它。

接下来，让我们以交互方式安排另一个作业。这种方法对于输入多个命令或更复杂的脚本很有用。我们将安排一个作业，在 5 分钟后将“Hello from at job!” 附加到 `~/at_output.txt`：

```
at now + 5 minutes
```

输入命令并按 Enter 后，您将看到 `at>` 提示符。输入您的命令，然后按 `Ctrl+d` 完成：

```
at > echo "Hello from at job!" >> ~/at_output.txt
at > Ctrl+d
```

要查看当前在 `at` 队列中的作业，请使用 `atq` 命令。此命令列出了当前用户的所有待处理的 `at` 作业。

```
atq
```

输出将显示作业编号、计划时间、队列和安排作业的用户。

![显示已调度作业的 atq 命令输出](https://cdn.hashnode.com/res/hashnode/image/upload/v1750726190789/d2dd54c0-80a0-4bb2-8561-3114bb279387.png)

您可以使用 `at -c` 命令加上作业编号来检查特定 `at` 作业将运行的命令。将 `N` 替换为您之前记下的作业编号之一。

```
at -c N
```

此命令将显示 `at` 将为该作业执行的 shell 脚本。您应该在输出中看到 `date > ~/myjob.txt` 或 `echo "Hello from at job!" >> ~/at_output.txt` 命令。
```

```
atrm N
```

删除任务后，可以再次使用 `atq` 以确认队列中已不再存在该任务。

```
atq
```

您现在应该只看到第二个任务（如果它尚未执行）或者一个空的队列（如果两个任务已被移除或执行过）。

这就完成了使用 `at` 命令调度一次性任务的第一步。

## **如何管理 'at' 任务**

现在，让我们深入了解如何管理 `at` 任务，包括使用不同队列调度任务和验证它们的执行。了解 `at` 队列对于任务的优先级排序或分离不同类型的一次性任务很有用。

我们将在本地系统上继续工作，探索更高级的 `at` 任务管理功能。

`at` 命令允许你使用 `-q` 选项指定一个队列。队列是从 `a` 到 `z` 的单个字母。队列 `a` 是默认的，队列 `a` 到 `z` 的任务按照递减的优先度执行。队列 `a` 的优先级最高，而队列 `z` 的优先级最低。队列 `b` 保留用于批处理任务。

让我们在队列 `g`（较低优先级的队列）中调度一个任务，它将在 2 分钟后运行。此任务将创建一个名为 `~/queue_g_job.txt` 的文件，并带有时间戳：

```
at -q g now + 2 minutes << EOF
date > ~/queue_g_job.txt
EOF
```

您将会看到类似 `job N at ...` 的输出。记下这个任务编号。

接下来，让我们安排另一个任务，这次在队列 `b`（批处理队列），它通常用于系统负载较低时可以运行的任务。此任务将附加 "Batch job executed!" 到 `~/batch_job.txt`。我们将安排它在 4 分钟后运行：

```
at -q b now + 4 minutes << EOF
echo "Batch job executed!" >> ~/batch_job.txt
EOF
```

再次记下任务编号。

要查看所有挂起的任务，包括那些在不同队列中的任务，请使用 `atq`。

```
atq
```

您现在应该看到列出的两个任务及其各自的队列字母（`g` 和 `b`）。

![atq 命令输出显示的计划任务](https://cdn.hashnode.com/res/hashnode/image/upload/v1750726218380/bcc9d551-0530-48d1-bf7f-46073c6f77a6.png)

现在，等待您的计划任务执行。至少等待 5 分钟以允许所有任务完成。您可以检查由 `at` 任务创建的文件是否存在并包含预期的内容。

检查 `~/queue_g_job.txt`：

```
cat ~/queue_g_job.txt
```

您应该看到一个日期和时间字符串。

检查 `~/batch_job.txt`：

```
cat ~/batch_job.txt
```

您应该看到 "Batch job executed!"。

如果文件不存在或为空，则可能意味着任务尚未执行，或命令有问题。您可以重新检查 `atq` 看它们是否仍在等待中。

## **如何使用 'crontab' 调度用户的定期任务**

接下来，您将学习如何为特定用户调度定期任务，使用 `crontab`。与单次运行的 `at` 任务不同，`cron` 任务在指定的时间间隔反复运行。非常适合例行维护、数据备份或生成报告。

我们将继续在本地系统上学习用户 `crontab` 管理。

`crontab` 命令允许用户创建、编辑和查看他们自己的 `cron` 任务。每个用户都有自己的 `crontab` 文件。

要编辑您的 `crontab` 文件，使用命令 `crontab -e`。这将打开默认文本编辑器（通常是 `vim`）中的 `crontab` 文件。

```
crontab -e
```

**Vim 编辑器说明：**

-   按 `i` 进入插入模式（您将在底部看到 `-- INSERT --`）
    
-   使用方向键导航
    
-   保存并退出：按 `Esc` 退出插入模式，然后输入 `:wq` 并按 `Enter`
    
-   不保存直接退出：按 `Esc`，然后输入 `:q!` 并按 `Enter`
    

在编辑器中，您将添加一行来定义您的 `cron` 任务。`cron` 条目有五个时间和日期字段，后面是要执行的命令。这些字段是：

-   **分钟 (0-59)**
    
-   **小时 (0-23)**
    
-   **月中的天 (1-31)**
    
-   **月 (1-12)**
    
-   **周中的天 (0-7，其中 0 或 7 是星期天)**
    

您可以使用 `*` 作为通配符，表示一个字段的 "每" 或者 `/` 指定步长值（例如，`*/5` 表示每 5 分钟）。

让我们安排一个任务，每分钟追加当前的日期和时间到一个名为 `~/my_cron_log.txt` 的文件中。这将让我们快速观察到 `cron` 任务的运行。

在 Vim 中按以下步骤操作：

1.  按 `i` 进入插入模式
    
2.  向 `crontab` 文件添加以下行：
    

```
* * * * * /usr/bin/date >> ~/my_cron_log.txt
```

3.  按 `Esc` 退出插入模式
    
4.  输入 `:wq` 并按 `Enter` 保存并退出
    

您应该看到一条消息，表示已安装新的 `crontab`：

```
crontab: installing new crontab
```

要验证您的 `cron` 任务是否已成功添加，您可以使用 `crontab -l` 命令列出您的 `crontab` 条目：

```
crontab -l
```

您应该看到刚刚添加的那一行：

```
* * * * * /usr/bin/date >> ~/my_cron_log.txt
```

现在，等待一到两分钟，以让 `cron` 任务至少执行一次。您可以查看当前时间以看到下一个整分的时间点：

```
date
```

等待至少两分钟，以让 `cron` 任务执行几次，然后检查 `~/my_cron_log.txt` 文件的内容。

您应该看到一行或多行，每行包含一个日期和时间，指示您的 `cron` 任务已执行。

![日志文件中的 Cron 任务输出](https://cdn.hashnode.com/res/hashnode/image/upload/v1750726409656/bfd85cf0-316a-4c1d-89c2-0d60c30cd33f.png)

```
Mon Apr 8 10:30:01 AM EDT 2025
Mon Apr 8 10:31:01 AM EDT 2025
```

## **如何管理用户 'crontab' 条目**

现在，您将学习更高级的技术来管理用户 `crontab` 条目，包括编辑现有任务、添加多个任务以及理解特殊的 `cron` 字符串。有效的 `crontab` 管理对于自动化例行任务至关重要。

我们将继续在本地系统上进行操作，以探索高级的 crontab 管理技术。

让我们从添加一个新的 `cron` 任务开始。这个任务将每两分钟将 "Hello from cron!" 附加到 `~/cron_messages.txt` 文件中。

打开您的 `crontab` 进行编辑：

```
crontab -e
```

在 Vim 中：

1. 按 `i` 进入插入模式
    
2. 在 `crontab` 文件中添加以下行：
    

```
*/2 * * * * echo "Hello from cron!" >> ~/cron_messages.txt
```

3. 按 `Esc` 退出插入模式
    
4. 输入 `:wq` 并按 `Enter` 保存并退出
    

验证该条目已被添加：

```
crontab -l
```

您应该看到新添加的行。

现在，让我们添加另一个每天 08:00 AM 运行的 `cron` 任务。此任务将记录您的主目录的磁盘使用情况到 `~/disk_usage.log`。

再次打开您的 `crontab` 进行编辑：

```
crontab -e
```

在 Vim 中：

1. 按 `i` 进入插入模式
    
2. 在前一行下方添加以下行：
    

```
0 8 * * * du -sh ~ >> ~/disk_usage.log
```

3. 按 `Esc` 退出插入模式
    
4. 输入 `:wq` 并按 `Enter` 保存并退出
    

验证两个条目都已存在：

```
crontab -l
```

您现在应该看到列出的两个 `cron` 任务。

`cron` 还支持可以简化常见时间表的特殊字符串。这些包括 `@reboot`、`@yearly`、`@annually`、`@monthly`、`@weekly`、`@daily`、`@midnight` 和 `@hourly`。例如，`@hourly` 等同于 `0 * * * *`。

让我们添加一个每小时运行并将系统运行时间记录到 `~/uptime_log.txt` 的任务。

打开您的 `crontab` 进行编辑：

```
crontab -e
```

在 Vim 中：

1. 按 `i` 进入插入模式
    
2. 添加以下行：
    

```
@hourly uptime >> ~/uptime_log.txt
```

3. 按 `Esc` 退出插入模式
    
4. 输入 `:wq` 并按 `Enter` 保存并退出
    

验证所有三个条目：

```
crontab -l
```

您现在应该看到所有三个 `cron` 任务。

为了展示这些任务的效果，我们将等待一小段时间。由于任务安排在不同的时间间隔，我们不会立即看到所有任务执行，但我们可以验证设置。

等待至少 3 分钟，以使 `*/2` 任务至少运行一次。

检查 `~/cron_messages.txt` 文件：

```
cat ~/cron_messages.txt
```

您应该看到至少一个 "Hello from cron!" 消息。

```
Hello from cron!
```

`~/disk_usage.log` 和 `~/uptime_log.txt` 文件可能尚未创建，具体取决于当前时间，因为它们分别安排在每天和每小时执行。重要的是，它们的条目在您的 `crontab` 中已正确配置。

## **如何使用 `cron` 目录安排重复的系统任务**

在此步骤中，您将学习如何使用 `cron` 目录安排重复的系统范围内的任务。与用户 `crontab` 条目不同，用户 `crontab` 条目是特定于用户的，系统 `cron` 任务由 root 用户管理并影响整个系统。它们通常用于系统维护、日志轮换和其他管理任务。

我们将继续在本地系统上进行操作，以探索系统范围内的 cron 任务配置。

系统范围内的 `cron` 任务在 `/etc/crontab` 中定义，或者通过将脚本放置在特定目录中：

- `/etc/cron.hourly/`：此目录中的脚本每小时运行一次。
    
- `/etc/cron.daily/`：此目录中的脚本每天运行一次。
    
- `/etc/cron.weekly/`：此目录中的脚本每周运行一次。
    
- `/etc/cron.monthly/`：此目录中的脚本每月运行一次。
    

这些目录由 `run-parts` 实用程序处理，该实用程序会执行其中的所有可执行文件。

要管理系统 `cron` 任务，您需要 root 权限。由于 labex 用户具有 sudo 访问权限，我们可以使用 `sudo` 执行所需的命令。

让我们创建一个简单的脚本，将消息记录到系统日志中。我们将此脚本放在 `/etc/cron.hourly/` 中，使其每小时运行。

首先，创建脚本文件 `/etc/cron.hourly/my_hourly_script`：

```
sudo nano /etc/cron.hourly/my_hourly_script
```

将以下内容添加到该文件中：

```
#!/bin/bash
logger "Hourly cron job executed at $(date)"
```

保存并退出编辑器（在 `nano` 中按 `Ctrl+o`，`Enter`，`Ctrl+x`）。

接下来，您需要将脚本设为可执行文件。如果没有执行权限，`run-parts` 将忽略它。

```
sudo chmod +x /etc/cron.hourly/my_hourly_script
```

现在，让我们验证脚本是可执行的：

```
ls -l /etc/cron.hourly/my_hourly_script
```

您应该在权限中看到 `x`，例如：`-rwxr-xr-x`。

由于 `cron.hourly` 任务每小时运行一次，我们不能在本教程中等待整整一个小时来验证其执行。但我们可以手动触发 `run-parts` 命令以模拟其执行。

该命令会执行 `/etc/cron.hourly/` 目录下的所有可执行脚本。我们创建的脚本使用 `logger` 命令将消息写入系统日志。

在一个真实的 RHEL 系统中，你可以使用 `journalctl` 或 `/var/log/messages` 检查系统日志，以验证脚本是否成功执行。

这就完成了系统计划任务管理步骤。脚本将保留在合适的位置，并在真实系统环境中每小时执行一次。

## **如何为重复任务配置** `systemd` **计时器**

接下来，你将学习 `systemd` 计时器，它是计划 Linux 系统任务的现代替代方案。`systemd` 计时器提供了更多的灵活性，并与 `systemd` 生态系统有更好的集成。

`systemd` 计时器与 `systemd` 服务单元一起工作。一个计时器单元文件 (`.timer` 文件) 定义任务何时运行，而一个服务单元文件 (`.service` 文件) 定义要执行的任务。

我们将在本地系统上继续工作，探索 systemd 计时器配置。

你需要 root 特权来在系统目录中创建 `systemd` 单元文件。因为 labex 用户有 sudo 访问权限，我们可以使用 `sudo` 来执行所需命令。

让我们创建一个简单的日志消息到文件的服务。我们将把该服务单元文件放置在 `/etc/systemd/system/` 中，这是自定义服务单元通常存储的地方。

创建服务单元文件 `/etc/systemd/system/my-custom-task.service`：

```
sudo nano /etc/systemd/system/my-custom-task.service
```

将以下内容添加到文件中：

```
[Unit]
Description=My Custom Scheduled Task

[Service]
Type=oneshot
ExecStart=/bin/bash -c 'echo "My custom task executed at $(date)" >> /var/log/my-custom-task.log'
```

保存并退出编辑器（在 `nano` 中 `Ctrl+o`, `Enter`, `Ctrl+x`）。

接下来，创建计时器单元文件 `/etc/systemd/system/my-custom-task.timer`。这个计时器将每 5 分钟激活一次我们的服务。

```
sudo nano /etc/systemd/system/my-custom-task.timer
```

将以下内容添加到文件中：

```
[Unit]
Description=Run My Custom Scheduled Task every 5 minutes

[Timer]
OnCalendar=*:0/5
Persistent=true

[Install]
WantedBy=timers.target
```

保存并退出编辑器。

**解释** `OnCalendar`：

- `*:0/5` 意思是“每 5 分钟”。
  - `*` 表示年、月、日、小时（任意值）。
  - `0/5` 表示分钟，从 0 分钟开始，每 5 分钟一次（0, 5, 10, ..., 55）。

在典型的 `systemd` 环境中，你现在要运行 `systemctl daemon-reload` 使 `systemd` 识别新的单元文件，然后执行 `systemctl enable --now my-custom-task.timer` 启动计时器。

让我们验证创建的文件是否存在：

```
ls -l /etc/systemd/system/my-custom-task.service
ls -l /etc/systemd/system/my-custom-task.timer
```

你应该看到输出表明这两个文件都存在。

要模拟服务的执行，你可以手动运行 `ExecStart` 中定义的命令：

```
sudo /bin/bash -c 'echo "My custom task executed at $(date)" >> /var/log/my-custom-task.log'
```

现在，查看日志文件以查看输出：

```
sudo cat /var/log/my-custom-task.log
```

你应该看到刚刚记录的消息：

```
My custom task executed at Tue Jun 10 06:54:40 UTC 2025
```

这就完成了 systemd 计时器的配置步骤。服务和计时器单元文件将保留在合适的位置以供参考。

## **如何使用** `systemd-tmpfiles` **管理临时文件**

现在你将学习如何使用 `systemd-tmpfiles` 管理临时文件和目录。这个实用程序是 `systemd` 的一部分，负责创建、删除和清理易失性和临时文件及目录。它通常用于管理 `/tmp`, `/var/tmp` 和其他临时存储位置，以确保定期删除旧文件。

我们将在本地系统上继续工作，探索 systemd-tmpfiles 配置。

你需要 root 特权来配置 `systemd-tmpfiles`。因为 labex 用户有 sudo 访问权限，我们可以使用 `sudo` 来执行所需命令。

`systemd-tmpfiles` 从 `/etc/tmpfiles.d/` 和 `/usr/lib/tmpfiles.d/` 中读取配置文件。这些文件定义了创建、删除和管理文件及目录的规则。

让我们创建一个自定义配置文件来管理一个新的临时目录。我们将创建一个目录 `/run/my_temp_dir` 并配置 `systemd-tmpfiles` 清理其中超过 1 分钟的文件。

创建配置文件 `/etc/tmpfiles.d/my_temp_dir.conf`：

```
sudo nano /etc/tmpfiles.d/my_temp_dir.conf
```

将以下内容添加到文件中：

```
d /run/my_temp_dir 0755 labex labex 1m
```

**行的解释：**

- `d`: 指定此条目定义一个目录。
- `/run/my_temp_dir`: 目录的路径。
- `0755`: 目录的权限。
- `labex labex`: 目录的拥有者和群组。
- `1m`: 文件在这个目录中应被删除的年龄（1分钟）。

保存并退出编辑器（在 `nano` 中 `Ctrl+o`, `Enter`, `Ctrl+x`）。

现在，让我们告诉 `systemd-tmpfiles` 应用此配置。`--create` 选项将创建目录（如果不存在）。

确认目录已使用正确的权限和所有权创建：

```
ls -ld /run/my_temp_dir
```

你应该会看到类似的输出：

```
drwxr-xr-x 2 labex labex 6 Jun 10 06:55 /run/my_temp_dir
```

接下来，让我们在这个新的临时目录中创建一个测试文件：

```
sudo touch /run/my_temp_dir/test_file.txt
```

验证文件是否存在：

```
ls -l /run/my_temp_dir/test_file.txt
```

现在，我们需要等待超过1分钟，使文件根据我们的配置变为“旧”文件。等待至少70秒（1分10秒）。

等待超过1分钟后，我们将手动运行`systemd-tmpfiles`，并使用`--clean`选项根据我们的配置触发清理过程。

```
sudo systemd-tmpfiles --clean /etc/tmpfiles.d/my_temp_dir.conf
```

最后，检查`test_file.txt`是否已被删除：

```
ls -l /run/my_temp_dir/test_file.txt
```

你应该会收到“No such file or directory”（没有这样的文件或目录）错误，表明`systemd-tmpfiles`已成功清理了旧文件。

这完成了systemd-tmpfiles的配置。配置文件和临时目录将保留以供参考。

## **总结**

在本教程中，您学习了如何使用 `at` 命令调度和管理一次性任务，包括交互式和非交互式调度作业，使用 `atq` 查看 `at` 队列，以及使用 `atrm` 删除待定作业。 您还学习了如何使用 `crontab` 调度用户特定的定期任务，包括如何编辑、列出和删除 cron 作业，以及学习了用于指定执行时间的 cron 语法。

我们还演示了如何通过在标准 cron 目录（`/etc/cron.hourly`、`/etc/cron.daily` 等）中放置脚本来调度系统范围的定期任务，以及如何在 `/etc/cron.d` 中创建自定义 cron 作业。

最后，您探索了使用 `systemd` 定时器的高级任务调度，学习创建和启用服务和定时器单元以执行重复任务，以及如何使用 `systemd-tmpfiles` 管理临时文件和目录以实现自动清理。

本综合教程提供了在 RHEL 系统上管理多种任务调度需求的实践经验，从简单的一次性命令到复杂的重复系统进程。

要练习本教程中的操作，请尝试交互式动手实验：[Schedule Tasks in Red Hat Enterprise Linux][13]。

[1]: https://labex.io/courses/red-hat-system-administration-rh134-labs
[2]: #heading-how-to-schedule-a-one-time-job-with-at
[3]: #heading-how-to-manage-at-jobs
[4]: #heading-how-to-schedule-recurring-user-jobs-with-crontab
[5]: #heading-how-to-manage-user-crontab-entries
[6]: #heading-how-to-schedule-recurring-system-jobs-with-cron-directories
[7]: #heading-how-to-configure-systemd-timers-for-recurring-tasks
[8]: #heading-how-to-manage-temporary-files-with-systemd-tmpfiles
[9]: https://labex.io/skilltrees/rhel
[10]: https://labex.io/courses/red-hat-system-administration-rh124-labs
[11]: https://labex.io/courses/red-hat-system-administration-rh134-labs
[12]: https://labex.io/courses/red-hat-enterprise-linux-automation-with-ansible-rh294
[13]: https://labex.io/labs/rhel-schedule-tasks-in-red-hat-enterprise-linux-588897?course=red-hat-system-administration-rh134-labs

