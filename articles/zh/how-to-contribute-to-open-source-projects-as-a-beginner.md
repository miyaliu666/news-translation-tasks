```markdown
---
title: 如何作为初学者贡献开源项目
date: 2024-12-09T06:22:12.732Z
author: Fanny Nyayic
authorURL: https://www.freecodecamp.org/news/author/nyayicfanny/
originalURL: https://www.freecodecamp.org/news/how-to-contribute-to-open-source-projects-as-a-beginner/
posteditor: ""
proofreader: ""
---

最近，我参加了一次开源峰会，并被意外的启示震撼。在一个关于社区贡献的小组讨论中，一个问题被抛向观众：“你们中有多少人曾经为开源项目做过贡献？”只有少数人举起了手。看到房间里对开源的热情如此之高，却有如此多的与会者不确定如何迈出贡献的第一步，令人惊讶。

<!-- more -->

## 目录

-   [充满潜力的房间带来的灵感][1]
    
-   [介绍][2]
    
-   [什么是开源？][3]
    
-   [贡献开源的益处][4]
    
-   [如何开始开源贡献][5]
    
-   [非技术类开源贡献][6]
    
-   [最后的想法][7]
    

## 充满潜力的房间带来的灵感

会后的交谈揭示出了一个共同的主题，许多人感到胆怯，他们认为需要成为专业的编码人员才能做出有意义的贡献。这种经历启发我写下这篇指南，分解开源项目贡献的过程，展示无论你的技术能力如何，任何人都可以在开源生态系统中发挥重要作用。

## 介绍

开源软件是我们日常使用的许多工具和服务的基础。无论是你使用的网页浏览器、计算机上的操作系统，还是驱动你喜欢的应用的库，开源项目对科技领域的贡献是显著的。

然而，作为一个初学者，有时进入开源贡献领域可能会有挑战。许多新手被开源项目的规模和复杂性所压倒，不知道如何开始或如何做出有意义的贡献。

本文将一步步引导你如何为开源项目做出贡献。到达文章末尾，你将拥有开始参与项目的知识和信心，无论你的技能水平如何。

## 什么是开源？

在我们深入探讨如何贡献之前，让我们澄清一下“开源”是什么意思。开源软件是指那些通过许可证公开提供的任何人都可以查看、修改和分发代码的软件。这种协作模式允许从爱好者到大型企业的任何人都可以为项目做出贡献。一些流行的开源项目包括：

-   **Linux**：驱动许多操作系统的内核。
    
-   **Python**：一个广泛使用的编程语言。
    
-   **React**：用于构建用户界面的 JavaScript 库。
    
-   **Mozilla Firefox**：一个流行的网页浏览器。
    

这些项目通常在像 GitHub 和 GitLab 这样的平台上维护，贡献者可以提交代码、报告问题和审查变更。

## 贡献开源的益处

贡献开源项目可以带来许多益处：

-   技能发展：通过实战项目学习新的编程语言、工具和最佳实践。
    
-   社区参与：开源项目通常拥有欢迎的社区，可以帮助你作为开发者和个人成长。
    
-   社交网络：当你为开源做贡献时，你将与其他开发者、潜在雇主和科技领域志同道合的人建立联系。
    
-   建立你的作品集：为开源项目贡献是建立你的作品集并向潜在雇主展示你的技能的绝佳途径。
    
-   做出改变：你的贡献可以直接影响全球数千用户，帮助改进他们依赖的软件。
    

## 如何开始开源贡献

开始接触开源可以分解成几个可管理的步骤。这些步骤将指导你完成整个过程，从寻找可以贡献的项目、理解如何做出贡献、到提交这些贡献以供审核。

### 第一步：设置你的开发环境

在你贡献开源项目之前，你需要设置你的本地开发环境。所需工具取决于项目使用的语言或技术。以下是大多数项目适用的基本设置：

1.  **Git**：Git 是一个版本控制系统，允许你跟踪代码中的更改并与他人协作。你可以从 [git-scm.com][8] 安装 Git。
    
    ![git-scm.com ](https://cdn.hashnode.com/res/hashnode/image/upload/v1733214111859/74f48b46-fee4-4bea-9c63-6ae24fdbf311.png)
    
    安装后，设置你的 Git 用户名和邮箱：
    
    ```
     git config --global user.name "Your Name"
     git config --global user.email "youremail@example.com"
    ```
    
2.  **GitHub 账号**：大多数开源项目托管在 GitHub 上，因此请在 [github.com][9] 创建一个账号。
    
    ![GitHub 首页](https://cdn.hashnode.com/res/hashnode/image/upload/v1733214199999/142c4c47-8ed8-4fb8-a4ee-e57105ff35d4.png)
    
3.  **文本编辑器**：选择一个文本编辑器或 IDE（集成开发环境），你将在其中编写代码。流行的选择包括 [Visual Studio Code][10]、[Sublime Text][11] 和 [JetBrains IDEs][12]。
    
    ![visual studio code](https://cdn.hashnode.com/res/hashnode/image/upload/v1733214312343/e1062a2c-381d-41f0-b643-65ff82f01c88.png)
    
4.  **编程语言**：根据项目的要求，你需要安装相应的编程语言。例如，如果你在一个 Python 项目工作，确保你的系统中安装了 Python。
    
```

版本控制是开源贡献的支柱。Git 允许多个开发者在同一个项目上工作而不互相冲突。在贡献之前，理解以下 Git 概念是很重要的：

-   **仓库 (repo)**：项目的代码和文件存储的目录。
    
-   **Forking**：Fork 一个项目会创建仓库的个人副本，允许你进行更改而不影响原始项目。
    
-   **克隆 (Clone)**：克隆会将整个仓库复制到你的本地计算机上，以便你可以脱机工作。
    
-   **分支 (Branch)**：分支用于将你的更改与主代码库隔离（通常称为 `main` 或 `master`）。
    
-   **拉取请求 (PR)**：拉取请求是将你的分支中的更改合并到原始仓库代码库中的提议。
    

克隆仓库：

```
git clone https://github.com/username/repository.git
```

为你的更改创建一个新分支：

```
git checkout -b my-feature-branch
```

### 步骤 3：寻找一个可以贡献的项目

找到合适的项目是开始的关键。这里有一些方法来寻找欢迎初学者的项目：

1.  **GitHub 探索**：GitHub 有一个 [Explore][13] 页面，在那里你可以找到热门仓库或按语言或兴趣搜索项目。
    
    ![GitHub explore](https://cdn.hashnode.com/res/hashnode/image/upload/v1733214628002/440e7fcc-8cb9-4bbb-8d2f-fd6505c4139f.png)
    
2.  **良好的首个问题**：许多开源项目使用标签 "good first issue" 来标记适合初学者的问题。你可以通过在 GitHub 或其他平台上搜索 "good first issue" 来找到这些问题。
    
    ![finding good first issues](https://cdn.hashnode.com/res/hashnode/image/upload/v1733214567620/eee17260-2f94-455b-a925-60a8bec0cbc5.png)
    
3.  **开源社区**：像 [First Timers Only][14] 和 [Up For Grabs][15] 这样的网站列出了积极寻找初学者贡献者的开源项目。
    
    ![first timers only](https://cdn.hashnode.com/res/hashnode/image/upload/v1733214482927/3de47afd-d9cf-440b-92eb-ff7abcfb2f3b.png)
    
4.  **检查文档**：寻找那些有良好文档的项目。文档完善的项目更有可能指导你完成贡献过程。
    

例如，如果你是一名 Python 开发者，你可以为 Python 文档本身或像 `Requests`、`Flask` 或 `Django` 这样的库做贡献。

### 步骤 4：了解项目

一旦你找到了感兴趣的项目，下一步就是熟悉它。

1.  **阅读 README**：项目的 README 文件是你应该首先查看的地方。它提供了项目的概览、如何设置以及通常会概述贡献指南。
    
    ![an example of README](https://cdn.hashnode.com/res/hashnode/image/upload/v1733214859166/b762f2c2-ffde-43e8-8d6f-50697a2b6078.png)
    
2.  **查看问题 (Issues)**：查看项目 GitHub 仓库中的问题。问题通常是记录错误、功能请求或任务的地方。寻找例如 `good first issue` 或 `beginner-friendly` 这样的标签。
    
    ![The issues tab on GitHub](https://cdn.hashnode.com/res/hashnode/image/upload/v1733214763497/28f9dda5-8abb-45ef-aff9-dd3f2ab1f22d.png)
    
3.  **在本地设置项目**：克隆仓库并在你的本地计算机上设置项目，以确保一切按 README 所述正常工作。
    
    例如，如果你正在处理一个 Python 项目，你可能需要通过 `pip` 安装依赖项：
    
    ```
     pip install -r requirements.txt
    ```
    
4.  **阅读贡献指南**：许多项目都有贡献指南。这些指南可能涵盖编码风格、测试要求以及如何格式化提交信息等内容。务必阅读并理解这些指南。
    
    ![contribution guidelines](https://cdn.hashnode.com/res/hashnode/image/upload/v1733214985183/cb9921b7-c3eb-4ec4-bf6d-93083cdb416a.png)
    

### 步骤 5：进行你的首次贡献

现在到了有趣的部分，贡献！以下是如何操作的步骤：

1.  **Fork 仓库**：在 GitHub 上，点击 "Fork" 按钮创建项目的个人副本。
    
    ![fork a repo](https://cdn.hashnode.com/res/hashnode/image/upload/v1733215091342/648dd025-f162-45dd-9edb-8d6fa8e7bff4.png)
    
2.  **克隆你的 Fork**：将你的 Fork 克隆到本地计算机：
    
    ```
     git clone https://github.com/your-username/repository.git
    ```
    
3.  **创建新分支**：为每次贡献创建新分支是一种好的做法：
    
    ```
     git checkout -b my-branch
    ```
    
4.  **进行更改**：现在，进行你想要贡献的更改。例如，如果你正在修复一个错误，可以在适当的文件中编辑代码。如果你正在更新文档，可以编辑 `README.md`。
    
    假设你正在修复 README 中的一个拼写错误：
    
    ```
     # 错误文本
     This is a sampe of a typo.
    ```
    
    你会将其更改为：
    
    ```
     # 更正后的文本
     This is an example of a typo.
    ```
    
5.  **提交你的更改**：一旦你进行了更改，使用清晰简明的信息提交它们：
    
    ```
     git add .
     git commit -m "Fix typo in README"
    ```
    
6.  **推送你的更改**：将你的更改推送到 GitHub 上的 Fork：

    ```
     git push origin my-branch
    ```

现在您的更改已推送到 GitHub，是时候提交它们以供审核了。

1.  **转到原始仓库**：导航到原始仓库（而不是您的 fork 仓库）。
    
    ![创建一个新的 pull request](https://cdn.hashnode.com/res/hashnode/image/upload/v1733215228523/f76e02bc-fd29-4416-a56e-c5c9232efe71.png)
    
2.  **创建一个 Pull Request**：GitHub 通常会显示一个横幅，提示您的分支已准备好创建 pull request。点击“Compare & pull request”按钮。
    
3.  **写一个描述**：提供一个清晰的说明，描述您所做的工作及其原因。具体说明您的更改解决了什么问题。
    

一旦提交 pull request，项目维护者将审核您的更改。他们可能会要求修改或批准您的更改。

### 步骤 7：回应反馈

维护者可能会对您的 pull request 提供反馈。请务必及时回应。如果他们要求更改，请在本地进行这些更改，提交它们，然后推送到您的 fork 仓库。

例如：

```
git commit --amend
git push --force
```

一旦更改获得批准，您的 pull request 将合并到主项目中。

## 非技术性开源贡献

虽然技术贡献如编码通常与开源项目相关，但有很多不需要编程技能的宝贵贡献方式。

### 1\. **文档编写**

清晰、全面的文档对任何开源项目都至关重要，但往往被忽视。作为一名非技术贡献者，您可以为项目改进或编写文档，使新用户和贡献者更容易理解和使用软件。

-   **改进 README**：澄清安装说明、使用示例和安装流程。
    
-   **编写教程**：创建逐步指南或视频教程，帮助初学者入门项目。
    
-   **修正错别字**：纠正现有文档中的拼写、语法和格式错误。
    

### 2\. **社区支持和参与**

许多开源项目依赖于活跃的社区来蓬勃发展。为社区做贡献可以包括回答问题、管理讨论和为新用户提供支持。

-   通过在 GitHub 问题或社区论坛中回答问题，帮助遇到项目问题的用户。
    
-   确保论坛、邮件列表或社交媒体上的讨论具有建设性并在主题之内。
    
-   汇总常见问题及其答案，以帮助用户解决常见问题。
    

### 3\. **设计和用户界面 (UI) 贡献**

项目通常需要帮助以使其用户界面在视觉上更吸引人、更易于使用。如果您有设计背景，可以通过创建模型、改进布局或提出 UI/UX 改进建议进行贡献。

-   设计项目的徽标、图标或插图。
    
-   创建线框图或提供建议以使界面更直观、更易于使用。
    

### 4\. **项目翻译**

使开源项目对全球观众可访问至关重要。将项目内容翻译成不同语言可帮助非英语使用者使用和参与项目。

-   将项目文档转换为其他语言以扩大用户基础。
    
-   调整软件界面、错误消息或网站以适应不同地区和文化。
    

### 5\. **营销和推广**

开源项目需要被新用户和贡献者发现，而这就是营销的作用。非技术贡献者可以通过各种渠道帮助提高项目的知名度。

-   在 Twitter、LinkedIn、Facebook 和其他平台上分享关于项目的帖子、更新和亮点。
    
-   撰写关于项目的文章，介绍如何使用或如何解决特定问题。
    
-   制作视频教程或博客文章教授新用户如何使用或贡献于项目。
    

### 6\. **活动组织和筹款**

组织活动或筹集资金对于开源项目的可持续性可能至关重要。非技术贡献如活动策划或财务支持可以产生重大影响。

-   帮助组织社区活动、黑客马拉松或会议，以汇集开发人员和用户。
    
-   协助筹款工作，以确保项目的财务未来，不论是通过众筹活动还是申请资助。
    

### 7\. **质量保证 (QA) 和测试**

虽然测试软件可能听起来像是一项技术任务，但非开发人员可以通过测试可用性和报告问题来提供帮助。非技术用户可以提供项目用户体验的宝贵反馈。

-   识别和报告使用软件时遇到的错误或问题。
    
-   对软件的易用性提供反馈并提出改进建议。
    

### 8\. **法律事务和许可证贡献**

开源项目常常需要在法律方面的帮助，如许可证、服务条款以及确保项目遵循相关法律。

这些非技术贡献对于开源项目的成功至关重要，往往被新手忽视，因为他们可能会认为技术技能是唯一的贡献方式。开源社区依赖于协作，而非技术贡献者在培育这种精神方面发挥了重要作用。

## 最后的想法

为开源项目做贡献是一种有益的体验，能够帮助你成长为一名开发者，与你志同道合的人建立联系，并在软件领域带来改变。

记住，每位贡献者都是从某个地方开始的。如果你最初的贡献很小，或者在过程中遇到挑战，不要气馁。开源社区是包容的，你贡献得越多，你将学到和成长得越多。

[1]: #heading-inspired-by-a-room-full-of-potential
[2]: #heading-introduction
[3]: #heading-what-is-open-source
[4]: #heading-benefits-of-contributing-to-open-source
[5]: #heading-how-to-get-started-with-open-source-contributions
[6]: #heading-non-tech-open-source-contributions
[7]: #heading-final-thoughts
[8]: https://git-scm.com/
[9]: https://github.com/
[10]: https://code.visualstudio.com/
[11]: https://www.sublimetext.com/
[12]: https://www.jetbrains.com/
[13]: https://github.com/explore
[14]: https://www.firsttimersonly.com/
[15]: http://up-for-grabs.net/

