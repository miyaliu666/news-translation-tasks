```markdown
---
title: 如何用 Git 别名简化 Git 命令
date: 2025-01-10T14:23:59.725Z
author: Grant Riordan
authorURL: https://www.freecodecamp.org/news/author/grantdotdev/
originalURL: https://www.freecodecamp.org/news/how-to-simplify-your-git-commands-with-git-aliases/
posteditor: ""
proofreader: ""
---

作为开发者，你可能每天都在使用 Git CLI（命令行界面）。然而，反复输入相同的命令，尤其是冗长的命令，可能会很麻烦。这时候，Git 别名就能帮上忙了。

<!-- more -->

在本文中，你将学习如何使用别名来简化你的 Git 命令。

## 目录

-   [先决条件][1]
    
-   [什么是 Git 别名？][2]
    
-   [如何通过全局 Git 配置文件添加 Git 别名（推荐）][3]
    
    -   [如何设置首选的 Git 编辑器][4]
        
    -   [如何打开 Git 配置文件][5]
        
    -   [如何通过配置文件添加 Git 别名][6]
        
-   [如何在 CLI 中添加别名][7]
    
-   [如何创建用于更复杂快捷方式的自定义命令][8]
    
    -   [如何在所有命令中使用参数][9]
-   [其他有用的别名][10]
    
-   [总结][11]
    

## 先决条件

-   Git 知识。
    
-   安装了 Git Bash（可选但推荐给 Windows 用户）。
    
-   像 VS Code 这样的集成开发环境（这也是可选的）。
    

## 什么是 Git 别名？

Git 别名是现有 Git 命令的自定义快捷方式，使常见任务更快速、更简单。它们允许你定义自己的命令，帮助你定制快捷方式。

你有两个主要选项可以在你的 git 配置中添加/创建 git 别名，使用你的 Git 配置文件或直接通过 CLI（终端/命令行）添加。

## 如何通过全局 Git 配置文件添加 Git 别名（推荐）

此选项需要打开你的全局 git 配置文件，并将你的 git 别名追加到文件的末尾。

### 如何设置首选的 Git 编辑器

设置你的默认 Git 配置编辑器软件，例如，我使用 VS Code 来编辑我的 Git 配置文件，但你可以使用任何你喜欢的文本编辑器/代码编辑器。

运行此命令以在 Windows（CMD/PowerShell）上设置 Notepad 为首选编辑器：

```
git config --global core.editor "notepad"
```

运行此命令以在 Windows 和 MacOS/Linux 上设置 VS Code 为首选编辑器：

```
git config --global core.editor "code --wait"
```

要设置其他默认编辑器，在线搜索“Set {editor} as default Git editor”，并用你喜欢的应用程序替换 `{editor}`。

### 如何打开 Git 配置文件

打开你选择的终端并输入以下命令。这将以编辑模式（`-e`）打开全局 Git 配置文件（`git config —global`）。

```
git config --global -e
```

你可以直接从以下位置打开 git 配置文件：

**Mac Os**: 主目录 → 显示隐藏文件（Cmd + Shift + H） → `.gitconfig`

**Windows**: `C:\Users\YourUsername\` → 然后显示隐藏文件（在视图中）→ 找到 `.gitconfig`

**Linux:** 主目录 → 显示隐藏文件（Ctrl + H） → `.gitconfig`

### 如何通过配置文件添加 Git 别名

如果你是第一次添加 Git 别名，打开你的 `.gitconfig` 文件，在末尾添加 `[alias]`，然后在下面列出你的快捷方式。这会告诉 Git 这些是别名。添加你喜欢的别名（你想运行的简化命令）。

Git 别名的格式是 `<alias> = <command>`，所以我们有：

```
co = checkout
cob = checkout -b
```

**上述例子的说明：**

`co = checkout` 这个将 `git checkout` 命令映射为更短的 `git co` 命令。然后你可以在终端中执行 `git co feature/123`。

你不需要在命令前面输入 `git`，因为配置文件会自动前缀，因为它知道你正在映射的是一个 Git 命令。

**注意**：任何传递给命令的参数将仅应用于别名中调用的最终命令。

可以以这种方式添加更多别名，将快捷方式映射到现有 git 命令。保存并关闭文件后，别名将在你的终端中可用。

## 如何在 CLI 中添加别名

如果你想采用更简洁的方法来添加 Git 别名，你可以直接在终端/命令行中添加它们。

以上面的例子为例，我们可以通过以下方式直接添加这些别名：

命令的格式是：`git config --global alias.{alias} "{original command}"`：

```
git config --global alias.co "checkout"
#或
git config --global alias.cob "checkout -b"
```

就是这么简单！

## 如何创建用于更复杂快捷方式的自定义命令

好吧，这看起来不错，但并不算太惊艳——我们只是移除了一些字符。然而，我们可以让它们更有用，我们可以使用 shell 命令创建我们的命令。

让我们看看以下这个我常用的命令示例！

```
new-work = !git checkout main && git pull && git cob
```

这个别名将多个 Git 命令组合成一个 shell 命令。`!` 字符告诉 Git 将其视为 shell 命令，而不是标准 Git 命令。
```

通过将这些命令链接在一起，我们可以编写更有用的别名。上面的命令将会：

-   首先，切换到 `main` 分支。
    
-   使用 `&&` 操作符，这表示其他命令只有在前一个命令成功时才会运行。
    
-   第二步，它将从 `main` 拉取更改。
    
-   最后，使用我们另一个别名 `git cob` 从 `main` 分支创建一个新分支。
    

最终命令可以接受参数（就像原始的 Git 命令一样），所以可以这样使用：

```
git new-work 'feature/new-work-from-main'
```

### 如何在所有命令中使用参数

到目前为止，我们只能将参数传递给别名中的最终 git 命令。然而，如果我们想将参数传递给别名中的某些命令甚至所有命令，该怎么办？我们可以通过使用 shell 函数来实现这一点。

以下是一个示例：

```
new-work = "!f() { git checkout \"$1\" && git pull && git checkout -b \"$2\"; }; f"
```

上面我们使用了一个处理输入参数的 shell 函数。

**解释：**

1.  `!f()`:
    
    -   `!` 告诉 Git 将别名解释为 shell 命令而不是标准 Git 命令。
        
    -   `f()` 定义了一个 shell 函数 `f`，它允许我们按顺序执行多个命令。
        
2.  大括号 `{ }` 中的所有内容都是将在函数 `f()` 中执行的内容。
    
3.  `git checkout \"$1\"`: 将运行一个参数化的 Git 命令，其中 `$1` 被转义，且将替换为传递给别名的第一个参数。 `\"` 转义序列允许分支名称中包含空格。
    
4.  `&&` 是一个逻辑操作符，确保每个命令只有在前一个成功时才运行。如果 `git checkout \"$1\"` 失败，后续命令将不会运行。
    
5.  `git checkout -b \"$2\"`: 用第二个参数的名称创建一个新分支。
    
6.  `;`: 表示 `f()` 函数的结束；
    
7.  `f`: 最后的 `f` 立即调用别名函数，这意味着当调用别名时，它声明函数并立即调用它。
    

**用法：**

```
git new-work development task/feat-123
```

## 其他有用的别名

```
[alias]
     co = checkout
    cob = checkout -b
    s = status
    tidy-up = !git checkout main && git branch | grep -v "main" | xargs git branch -D
    latest = !git checkout main && git pull
    new-work = "!f() { git checkout \"$1\" && git pull && git checkout -b \"$2\"; }; f"
    done = !git push -u origin HEAD
    save = !git add -A && git commit
    saveM = !git add -A && git commit -m
    br = branch --format='%(HEAD) %(color:yellow)%(refname:short)%(color:reset) - %(contents:subject) %(color:green)(%(committerdate:relative)) [%(authorname)]' --sort=-committerdate
```

## 总结

`co:` 切换到指定分支 → `git co task/feat-123`

`cob`: 从当前分支创建新分支 → `git cob feature/123`

`s`: 调用 `git status` 查看当前 git 分支的状态 → `git s`

`tidy-up`: 删除除 `main` 以外的所有本地分支 → `git tidy-up`

`latest`: 获取远程 `main` 分支的最新更改 → `git latest`

`new-work`: 从第一个参数的分支创建一个新分支（第二个参数） → `git new-work main feat/123`

`git done`: 将当前分支推送到远程存储库 (`origin`) 并将其设置为上游分支。在你首次推送并收到错误时，这可能会很有帮助：  
`fatal: The current branch has no upstream branch. To push the current branch and set the remote as upstream, use git push --set-upstream origin`

`save`: 将所有更改的文件添加并提交，打开默认的 Git 编辑器并请求提交消息 → `git save`

`savem`: 执行与上述相同的操作，但不打开编辑器，你可以在线传递提交消息 → `git savem ‘Task123: add index.html`

`br:` 看起来很复杂，但并不像看起来那么复杂，不过确实展示了别名的强大功能。本质上，它自定义了 `git branch` 的输出格式，以按最近提交日期显示详细、彩色编码的分支列表，类似于下图显示的内容，你本地的每个分支都会显示为这种格式。

![36008ee8-e54e-4b06-8a84-2a20885a1255](https://cdn.hashnode.com/res/hashnode/image/upload/v1730060113591/36008ee8-e54e-4b06-8a84-2a20885a1255.png)

这就是 Git 别名的介绍以及一些你可以添加为配置入门的别名实例。

如果你想讨论这些内容或了解未来的文章，可以随时在 [Twitter][12] 上关注我。

[1]: #heading-prerequisites
[2]: #heading-what-are-git-aliases
[3]: #heading-how-to-add-git-aliases-via-the-global-git-configuration-file-recommended
[4]: #heading-how-to-set-your-preferred-git-editor
[5]: #heading-how-to-open-the-git-config-file
[6]: #heading-how-to-add-a-git-alias-via-your-config-file
[7]: #heading-how-to-add-aliases-in-the-cli
[8]: #heading-how-to-create-custom-commands-for-more-complex-shortcuts
[9]: #heading-how-to-use-parameters-in-all-commands
[10]: #heading-other-useful-aliases
[11]: #heading-summary
[12]: https://x.com/grantdotdev

