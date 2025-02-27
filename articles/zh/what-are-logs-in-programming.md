```markdown
---
title: 实用的真实世界示例
date: 2025-02-27T14:11:07.852Z
author: Syeda Maham Fahim
authorURL: https://www.freecodecamp.org/news/author/syedamahamfahim/
originalURL: https://www.freecodecamp.org/news/what-are-logs-in-programming/
posteditor: ""
proofreader: ""
---

你是否曾经运行过一个程序，却遇到崩溃的情况？没有错误消息，没有提示，只是一片沉默。你如何找出出了什么问题？这时候记录日志就能派上用场。

<!-- more -->

日志记录了代码中正在发生的事情，因此当出现问题时，你无需猜测。它们类似于 `print` 或 `console.log` ，但更强大。

在本教程中，我将使用 Python 创建一些日志代码示例，并引导你了解这些示例。

在讨论日志之前，让我们首先了解一下你可能使用或遇到的不同错误类型。

## 错误类型

在构建生产级应用程序时，你需要根据错误的严重性来显示错误。错误类型有多种，最重要的是：

-   **DEBUG（调试）：** 详细信息，通常用于诊断问题。

-   **INFO（信息）：** 有关程序进程的常规信息。

-   **WARNING（警告）：** 发生了一些意外的事情，但不严重。

-   **ERROR（错误）：** 发生了错误，但程序仍然可运行。

-   **CRITICAL（严重）：** 非常严重的错误，可能会导致程序无法运行。

## 什么是日志记录？

现在，让我们直奔主题，了解什么是日志记录。

简单来说，日志或日志记录是记录有关程序所做一切的信息。记录的信息可以是任何内容，从调用了哪些函数的基本详细信息到跟踪错误或性能问题的详细信息。

### 为什么我们需要日志记录？

你可能会想，“如果日志是打印错误、信息等，我可以直接使用 print 语句。为什么我需要日志记录呢？” 其实 `print` 可以工作，但日志记录给你更多的控制：

↳ 可以将消息存储在文件中。  
↳ 有不同的级别（信息、警告、错误等）。  
↳ 可以根据重要性过滤消息。  
↳ 帮助调试而不会弄乱代码。

这些是 `print` 语句不能有效完成的事情。

## 如何在 Python 中添加日志

在 Python 中，`logging` 模块是专门为日志记录目的而构建的。

让我们设置一些日志，以查看它们的工作方式。

### 步骤 1：导入日志模块

开始使用日志记录，我们需要导入该模块：

```
import logging
```

### 步骤 2：记录消息

现在，你可以开始在程序中记录消息。可以根据消息的重要性使用不同的日志级别。提醒一下，这些级别是（从最不紧急到最紧急）：

-   DEBUG（调试）
    
-   INFO（信息）
    
-   WARNING（警告）
    
-   ERROR（错误）
    
-   CRITICAL（严重）

让我们在每个级别记录一个简单的消息：

```
logging.debug("这是一个调试消息")
logging.info("这是一个信息消息")
logging.warning("这是一个警告消息")
logging.error("这是一个错误消息")
logging.critical("这是一个严重消息")
```

当你运行这个程序时，你会在控制台看到如下所示的消息打印：

![终端显示 Python 日志消息。](https://cdn.hashnode.com/res/hashnode/image/upload/v1738500126070/a2a395c3-5cbe-4f94-bea2-d871cfc1529e.png)

你可能会想知道为什么没有看到 **DEBUG** 和 **INFO** 消息。这是因为默认的日志级别会阻止它们显示。

默认情况下，日志级别设置为 `WARNING`。这意味着只有严重性为 `WARNING` 或更高的消息才会显示（也就是 `WARNING`、`ERROR` 和 `CRITICAL`）。

### **步骤 3：** 设置基本配置

要查看 `debug` 和 `info` 消息，我们需要在运行代码之前设置日志级别为 `DEBUG`。

这意味着我们需要配置日志。为了做到这一点，使用下面的方法 `basicConfig`：

```
logging.basicConfig(level=logging.DEBUG)
```

这个基本配置允许你记录 **DEBUG** 级别或更高级别的消息。你可以根据所需日志类型更改级别。

现在，所有日志都被打印出来：

![日志消息：调试，信息，警告，错误，严重。](https://cdn.hashnode.com/res/hashnode/image/upload/v1738500423798/96b65689-f0e4-4663-9d1a-1dc7147e964e.png)

### 步骤 4：记录到文件

现在，让我们将这些日志保存在文件中，以便我们可以跟踪错误以及它们发生的时间。为此，请更新配置：

```
logging.basicConfig(filename='data_log.log', level=logging.DEBUG, 
                    format='%(asctime)s - %(levelname)s - %(message)s')
```

这里：

-   `asctime` – 事件发生的时间。
    
-   `levelname` – 日志的类型（例如 **DEBUG**、**INFO**）。
    
-   `message` – 我们显示的消息。

现在，当你运行程序时，日志文件将会生成并保存你的日志，显示确切的时间、错误类型和消息。像这样：

![日志文件包含调试、信息、警告、错误、严重消息](https://cdn.hashnode.com/res/hashnode/image/upload/v1738500713832/7895f1db-8740-494a-86dd-86020f4f5569.png)

## 如何使用日志记录器获得更多控制

如果你正在处理一个大项目，你可能希望有一个可以在代码的任何地方使用的实用日志记录器。让我们创建这个自定义记录器。
```

```
logging.basicConfig(
        filename=log_file,
        level=logging.DEBUG,
        format='%(asctime)s - %(levelname)s - %(filename)s:%(lineno)d - %(message)s', 
        filemode='w',
        encoding='utf-8' 
    )
```

解释：

-   `encoding='utf-8'` — 确保特殊字符被记录。
    
-   `%(filename)s:%(lineno)d` — 记录生成日志的文件名和行号。
    

现在，让我们设置一个自定义的控制台记录器：

```
  console_handler = logging.StreamHandler()
  console_handler.setLevel(logging.DEBUG)
  console_formatter = logging.Formatter('%(asctime)s - %(levelname)s - %(filename)s:%(lineno)d - %(message)s')  # 加入行号
  console_handler.setFormatter(console_formatter)


   logging.getLogger().addHandler(console_handler)
```

此设置执行以下操作：

-   `console_handler`: 将日志消息发送到控制台（stdout）。
    
-   `console_formatter`: 使用时间、级别、文件名、行号和消息格式化日志消息。
    
-   `logging.getLogger().addHandler(console_handler)`: 将自定义处理程序添加到根记录器，以便日志消息被打印到控制台。
    

### 完整示例代码

```
import logging
import os
from datetime import datetime

def setup_daily_logger():
    base_dir = os.path.dirname(os.path.abspath(__file__))
    log_dir = os.path.join(base_dir, 'logs')  
    os.makedirs(log_dir, exist_ok=True)


    current_time = datetime.now().strftime("%m_%d_%y_%I_%M_%p")
    log_file = os.path.join(log_dir, f"{current_time}.log")


    logging.basicConfig(
        filename=log_file,
        level=logging.DEBUG,
        format='%(asctime)s - %(levelname)s - %(filename)s:%(lineno)d - %(message)s', 
        filemode='w',
        encoding='utf-8' 
    )


    console_handler = logging.StreamHandler()
    console_handler.setLevel(logging.DEBUG)
    console_formatter = logging.Formatter('%(asctime)s - %(levelname)s - %(filename)s:%(lineno)d - %(message)s')  # 加入行号
    console_handler.setFormatter(console_formatter)


    logging.getLogger().addHandler(console_handler)


    return logging.getLogger(__name__)
```

### 现在会发生什么？

现在，每当您运行程序时，都会在 `logs` 文件夹中创建一个新的日志文件。每次程序执行时，都会生成一个带有唯一时间戳的新日志文件。

像这样：

![在 app.py 中使用自定义日志记录](https://cdn.hashnode.com/res/hashnode/image/upload/v1738503550743/1ad9fb99-762a-4ca9-a189-58d044955617.png)

这些日志将为您提供程序行为的清晰图景，并有助于调试。

我希望这篇文章能够帮助你更清楚地了解日志及其在编程中的重要性。

# 实际的真实世界例子

现在您了解了日志是什么以及如何在 Python 中设置它们，让我们看看实际应用中的用例。

## 1\. 机器人：抓取韩国最大的房地产网站

以下是一个设计用于抓取韩国最大房地产网站的机器人的示例。

-   日志显示了机器人所采取的每一步，使跟踪进度变得更加容易。
    
-   如果在任何步骤中出现错误，它将被记录在日志文件中。
    
-   即使机器人崩溃，我也可以检查日志以找出问题所在。
    

![带有INFO消息的日志文件，显示城市和城镇提取详情。](https://cdn.hashnode.com/res/hashnode/image/upload/v1739037891010/69a8b5ae-d202-4466-add0-bb2ace28230a.png)

![带有INFO消息的日志文件，显示城市和城镇提取详情。](https://cdn.hashnode.com/res/hashnode/image/upload/v1739037833210/bf9ceba0-2caf-48c6-bdb8-ac2d9eb901bd.png)

该机器人的类中的一个方法使用日志记录来跟踪机器人是否正确地选择了省份。

![使用日志记录的 select_province 函数](https://cdn.hashnode.com/res/hashnode/image/upload/v1739038058017/6153c909-477d-4cd6-b493-124b96bc595f.png)

在这里：

-   如果发生错误或警告，它将被保存在日志文件中。
    
-   以后，您可以查看日志并准确查明发生了什么
    

## 2\. 机器人：抓取 Facebook 群组

现在，让我们看看日志记录如何在 Facebook 群组爬虫中提供帮助。

##### 错误跟踪

-   某一时刻，机器人因错误失败。
    
-   由于我们有日志记录，错误被保存在日志文件中。
    
-   这使您可以迅速找到出错的地方。
    

![错误日志文件](https://cdn.hashnode.com/res/hashnode/image/upload/v1739038507530/9662bed7-a124-4dd8-94a9-9d657ec022a1.png)

在这里，您可以看到发生错误的确切文件名和行号。

![日志文件显示成功日志](https://cdn.hashnode.com/res/hashnode/image/upload/v1739038826232/ce717b49-e532-4c5f-a40d-955591aa27a2.png)

一旦确定并解决了问题，机器人又开始正常工作。

它在日志中捕获每个细节，通过精准找出错误发生的位置，节省了调试时间。

##### 调试变得简单

-   日志记录了机器人执行的每一个细节。
    
-   这可以为您节省数小时的调试时间，因为您会知道确切地在哪里发生了错误。
    

## 结论

日志记录是那种当一切正常时没人在意的事情之一。但当问题出现时，日志成为您的得力助手。
```

-   日志记录不仅仅用于错误跟踪——它可以帮助你监控程序的流程。
    
-   不必猜测哪里出了问题，查看日志。答案通常就在日志中。
    

确保在代码中添加日志记录。以后你会感激自己的！

**保持联系 - @syedamahamfahim 🐬**

