```markdown 
--- 
title: 如何使用 Git 别名简化 Git 命令 
date: 2025-01-10T13:45:08.074Z 
author: Grant Riordan 
authorURL: https://www.freecodecamp.org/news/author/grantdotdev/ 
originalURL: https://www.freecodecamp.org/news/how-to-simplify-your-git-commands-with-git-aliases/ 
posteditor: "" 
proofreader: "" 
--- 
 
作为开发者，你可能每天都在使用 Git CLI（命令行界面）。然而，重复地编写相同的老旧命令可能会很费力，特别是当这些命令很长时。在这里，Git 别名可以帮上忙。 
 
<!-- more --> 
 
在本文中，你将学习如何通过使用别名来简化 Git 命令。 
 
## 目录 
 
-   [先决条件][1] 
     
-   [什么是 Git 别名？][2] 
     
-   [如何通过全局 Git 配置文件添加 Git 别名（推荐）][3] 
     
    -   [如何设置你偏好的 Git 编辑器][4] 
         
    -   [如何打开 Git 配置文件][5] 
         
    -   [如何通过配置文件添加 Git 别名][6] 
         
-   [如何在 CLI 中添加别名][7] 
     
-   [如何创建更复杂的自定义命令][8] 
     
    -   [如何在所有命令中使用参数][9] 
-   [其他有用的别名][10] 
     
-   [总结][11] 
     
 
## 先决条件 
 
-   了解 Git。 
     
-   已安装 Git Bash（可选，但推荐 Windows 用户使用）。 
     
-   像 VS Code 这样的集成开发环境（这也是可选的）。 
     
 
## 什么是 Git 别名？ 
 
Git 别名是现有 Git 命令的自定义快捷方式，使常见任务更快更容易。它们让你可以定义自己的命令，允许你根据需要精确设定快捷方式。 
 
你有两种主要的方式可以在你的 git 配置中添加/创建 git 别名，可以使用 Git 配置文件或直接通过 CLI（终端/命令行）添加。 
 
## 如何通过全局 Git 配置文件添加 Git 别名（推荐） 
 
此选项涉及打开你的全局 git 配置文件，并在文件末尾添加你的 git 别名。 
 
### 如何设置你偏好的 Git 编辑器 
 
设置你的默认 Git 配置编辑器软件，例如，我使用 VS Code 来编辑我的 Git 配置文件，但你可以使用你偏好的任何文本编辑器/代码编辑器。 
 
运行以下命令在 Windows (CMD/PowerShell) 上设置 Notepad 作为你的偏好编辑器： 
 
``` 
git config --global core.editor "notepad" 
``` 
 
运行以下命令在 Windows & MacOS / Linux 上设置 VS Code 作为你的偏好编辑器： 
 
``` 
git config --global core.editor "code --wait" 
``` 
 
要设置其他默认编辑器，可以在线搜索“将 {editor} 设为默认 Git 编辑器”，并将 `{editor}` 替换为你偏好的应用程序。 
 
### 如何打开 Git 配置文件 
 
打开你选择的终端并输入以下命令。这将打开全球 Git 配置文件（`git config —global`），处于编辑模式（`-e`）。 
 
``` 
git config --global -e 
``` 
 
你可以从以下位置直接打开 git 配置文件： 
 
**Mac Os**: 主目录 → 显示隐藏文件 (Cmd + Shift + H) → `.gitconfig` 
 
**Windows**: `C:\Users\YourUsername\` → 然后显示隐藏文件（在视图中）→ 找到 `.gitconfig` 
 
**Linux:** 主目录 → 显示隐藏文件 (Ctrl + H) → `.gitconfig` 
 
### 如何通过配置文件添加 Git 别名 
 
如果你第一次添加 Git 别名，打开你的 `.gitconfig` 文件，在末尾添加 `[alias]`，然后在下面列出你的快捷方式。这告诉 Git 这些是别名。添加你偏好的别名（你希望运行的缩写命令）。 
 
Git 别名的格式是 `<alias> = <command>`，因此我们有： 
 
``` 
co = checkout 
cob = checkout -b 
``` 
 
**上述示例的解释：** 
 
`co = checkout` 这将 `git checkout` 命令映射为更短的 `git co` 命令。你可以在终端中调用 `git co feature/123`。 
 
你不需要在命令前输入 `git`，因为配置文件会自动在前面加上这部分，因为它知道你在映射的命令是 Git 命令。 
 
**注意**：任何传递给该命令的参数只会应用于别名中调用的最终命令。 
 
更多的别名可以通过这种方式添加，将快捷方式映射到现有的 git 命令。保存并关闭文件后，这些别名将在你的终端中可用。 
 
## 如何在 CLI 中添加别名 
 
如果你想要更简洁的方式来添加 Git 别名，你可以直接在终端/命令行中添加它们。 
 
以上述示例为例，我们可以通过以下方式直接添加这些： 
 
命令的格式是：`git config --global alias.{alias} "{original command}"`： 
 
``` 
git config --global alias.co "checkout" 
#或 
git config --global alias.cob "checkout -b" 
``` 
 
就是这么简单！ 
 
## 如何创建更复杂的自定义命令 
 
好的，这看起来不错，但实际上并不算惊艳——我们只是减少了一些字符。然而，我们可以让它们变得更有用，我们可以使用 shell 命令创建自己的命令。 
 
让我们来看一个我经常使用的命令示例！ 
 
``` 
new-work = !git checkout main && git pull && git cob 
``` 
 
此别名将多个 Git 命令组合为一个 shell 命令。`!` 字符告诉 Git 将其视为 shell 命令，而不是标准 Git 命令。 
``` 
 
通过将这些命令链接在一起，我们可以编写更有用的别名。上面的例子将会： 
 
-   首先，切换到 `main` 分支。 
     
-   使用 `&&` 运算符，这意味着其他命令只有在前一个命令成功后才会运行。 
     
-   第二步，从 `main` 分支拉取更新。 
     
-   最后，使用我们另一个别名 `git cob` 从 `main` 分支创建一个新分支。 
     
 
最后的命令可以接受参数（就像原始的 Git 命令一样），因此可以像这样使用： 
 
``` 
git new-work 'feature/new-work-from-main' 
``` 
 
### 如何在所有命令中使用参数 
 
到目前为止，我们只能将参数传递给别名中的最终 git 命令。然而，如果我们希望将参数传递给别名中的某些命令甚至所有命令呢？我们可以通过使用 shell 函数来实现。 
 
以下是一个例子： 
 
``` 
new-work = "!f() { git checkout \"$1\" && git pull && git checkout -b \"$2\"; }; f" 
``` 
 
上面我们使用的是一个处理输入参数的 shell 函数。 
 
**解释：** 
 
1.  `!f()`: 
     
    -   `!` 告诉 Git 将别名解释为 shell 命令而非标准 Git 命令。 
         
    -   `f()` 定义了一个 shell 函数 `f`，它允许我们按顺序执行多个命令。 
         
2.  `{ }` 内的所有内容将在 `f()` 函数中执行。 
     
3.  `git checkout \”$1”'\`: 将运行一个参数化的 Git 命令，其中 `$1` 被转义并将替换为传递给别名的第一个参数。`\"` 包围 `$1` 的转义序列允许分支名称中包含空格。 
     
4.  `&&` 是一个逻辑运算符，确保只有前一个命令成功时才运行后续命令。如果 `git checkout "$1"` 失败，则后续命令不会运行。 
     
5.  `git checkout -b \”$2”\` : 像之前一样，以第二个参数的名称创建一个新分支。 
     
6.  `;`: 标记 `f()` 函数的结束； 
     
7.  `f`: 最后的 `f` 立即调用别名函数，这意味着当您调用别名时，它首先声明函数，然后立即调用它。 
     
 
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
 
`cob`: 从当前分支创建一个新分支 → `git cob feature/123` 
 
`s`: 调用 `git status` 查看当前 git 分支的状态 → `git s` 
 
`tidy-up`: 删除除 `main` 之外的所有本地分支 → `git tidy-up` 
 
`latest`: 从远程 `main` 分支获取最新更改 → `git latest` 
 
`new-work`: 从第一个参数分支创建第二个参数的新分支 → `git new-work main feat/123` 
 
`git done`: 将当前分支推送到远程仓库 (`origin`) 并设置为上游分支。这在您推送第一次提交并出现错误时非常有用：   
`fatal: 当前分支没有上游分支。要推送当前分支并设置远程为上游，请使用 git push --set-upstream origin` 
 
`save`: 将所有更改的文件添加并提交，打开默认 Git 编辑器并请求提交信息 → `git save` 
 
`savem`: 将执行上述操作，但不打开编辑器，您可以在命令行传递提交信息 → `git savem ‘Task123: add index.html` 
 
`br:` 这个看起来复杂，但其实不如看起来那么复杂，反而突出了别名的强大功能。实际上，它定制了 `git branch` 的输出格式，以显示详细的、颜色编码的分支列表，按最近的提交日期排序，看起来每个您在本地拥有的分支将像下面的图片一样。 
 
![36008ee8-e54e-4b06-8a84-2a20885a1255](https://cdn.hashnode.com/res/hashnode/image/upload/v1730060113591/36008ee8-e54e-4b06-8a84-2a20885a1255.png) 
 
这就是对 Git 别名的介绍以及一些可以作为您配置起点的别名示例。 
 
正如往常，如果您想讨论或了解未来的文章，您可以在 [Twitter][12] 上关注我。 
 
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
 
 