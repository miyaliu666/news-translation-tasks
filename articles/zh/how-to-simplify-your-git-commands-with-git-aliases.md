---
title: How to Simplify Your Git Commands with Git Aliases
date: 2025-01-10T14:23:59.725Z
author: Grant Riordan
authorURL: https://www.freecodecamp.org/news/author/grantdotdev/
originalURL: https://www.freecodecamp.org/news/how-to-simplify-your-git-commands-with-git-aliases/
posteditor: ""
proofreader: ""
---

As a developer, you probably use the Git CLI (Command Line Interface) daily. However, writing the same old commands repeatedly can be laborious, especially when the commands are lengthy. This is where Git aliases come in to help out.

<!-- more -->

In this article, you’ll learn how to simplify your Git commands by using aliases.

## Table of Contents

-   [Prerequisites][1]
    
-   [What Are Git Aliases?][2]
    
-   [How to Add Git Aliases Via the Global Git Configuration File (Recommended)][3]
    
    -   [How to Set Your Preferred Git Editor][4]
        
    -   [How to Open the Git Config File][5]
        
    -   [How to Add a Git Alias Via Your Config File][6]
        
-   [How to Add Aliases in the CLI][7]
    
-   [How to Create Custom Commands for More Complex Shortcuts][8]
    
    -   [How to Use Parameters in All Commands][9]
-   [Other Useful Aliases][10]
    
-   [Summary][11]
    

## Prerequisites

-   Knowledge of Git.
    
-   Git Bash installed (optional but recommended Windows users).
    
-   An IDE like VS Code (this is also optional).
    

## What Are Git Aliases?

Git aliases are custom shortcuts for existing Git commands, making common tasks quicker and easier. They let you define your commands, allowing you to tailor shortcuts exactly how you want.

You have two main options for adding/creating git aliases in your git configuration, using your Git configuration file or adding them directly via the CLI (terminal/command line).

## How to Add Git Aliases Via the Global Git Configuration File (Recommended)

This option involves opening your global git config file and appending your git aliases to the bottom of the file.

### How to Set Your Preferred Git Editor

Set your default Git config editor software, for example, I use VS Code to edit my Git configuration file, but you can use whatever text editor/code editor you prefer.

Run this command to set Notepad as your preferred editor on Windows (CMD/PowerShell):

```
git config --global core.editor "notepad"
```

Run this command to set VS Code as your preferred editor on Windows & MacOS /Linux:

```
git config --global core.editor "code --wait"
```

To set a different default editor, search online for “Set {editor} as default Git editor,” and replace `{editor}` with your preferred app.

### How to Open the Git Config File

Open your terminal of choice and enter the following command. This will open the global Git config file (`git config —global`), in edit mode (`-e`).

```
git config --global -e
```

You can open the git configuration file directly from the following locations:

**Mac Os**: Home Directory → show hidden (Cmd + Shift + H) → `.gitconfig`

**Windows**: `C:\Users\YourUsername\` → then show hidden files (in View) → and find `.gitconfig`

**Linux:** Home Directory → show hidden (Ctrl + H) → `.gitconfig`

### How to Add a Git Alias Via Your Config File

If you're adding Git aliases for the first time, open your `.gitconfig` file, add `[alias]` at the end, and then list your shortcuts below. This tells Git these are aliases. Add your preferred alias (the shortened command you wish to run).

The format of a git alias is `<alias> = <command>`, so we have:

```
co = checkout
cob = checkout -b
```

**Explanation of the above examples:**

`co = checkout` this maps the `git checkout` command to a shorter `git co` command. You’d then call `git co feature/123` in your terminal.

You do not need to type `git` in front of the command, as the configuration will pre-pend this automatically as it knows the command you’re mapping is a Git command.

**Note**: Any parameters passed to the command will be applied to the final command called within the alias only.

More aliases can be added in this way, mapping shortcuts to existing git commands. Saving and closing the file will then make the aliases available within your terminal.

## How to Add Aliases in the CLI

If you want a more streamlined approach to adding Git aliases, you can add them directly from within the terminal/command line.

Taking the examples above, we can add these directly in the following way:

The format of the command is: `git config --global alias.{alias} "{original command}"`:

```
git config --global alias.co "checkout"
#or
git config --global alias.cob "checkout -b"
```

It’s as easy as that!

## How to Create Custom Commands for More Complex Shortcuts

Ok, this seems great, but it’s not really that impressive – we’re only removing a few characters. However, we can make them much more helpful, we can create our commands using shell commands.

Let’s take the following example, a command I use a lot!

```
new-work = !git checkout main && git pull && git cob
```

This alias combines multiple Git commands into one shell command. The `!` character tells Git to treat it as a shell command, not a standard Git command.

Without `!`, Git treats the alias as a Git command (for example, `checkout` becomes `git checkout`). With `!`, Git knows to run it as a shell command without adding `git` in front.

By chaining these commands, we can write much more useful aliases. The one above will:

-   First, check out the `main` branch.
    
-   Using the `&&` operator, it means the other commands will only run if the previous one has been successful.
    
-   Secondly, it will pull down the changes from `main`.
    
-   Finally, create a new branch from the `main` branch using our other alias `git cob`.
    

The final command can then accept parameters (as the original Git command would), so it can be used like so:

```
git new-work 'feature/new-work-from-main'
```

### How to Use Parameters in All Commands

Up until now, we’ve only been able to pass our parameters to the final git command in our alias. However, what if we want to pass parameters to some, if not all of the commands within the alias? We can achieve this by using a shell function.

Take the following example:

```
new-work = "!f() { git checkout \"$1\" && git pull && git checkout -b \"$2\"; }; f"
```

Above we’re using a shell function that processes input parameters.

**Explanation:**

1.  `!f()`:
    
    -   The `!` tells Git to interpret the alias as a shell command rather than a standard Git command.
        
    -   `f()` defines a shell function `f` that will allow us to execute multiple commands in sequence.
        
2.  Everything inside `{ }` is what will be executed within the `f()` function.
    
3.  `git checkout \”$1”'\`: Will run a parameterized Git command, where `$1` is escaped and will be replaced with the 1st parameter passed to the alias. The `\"` escape sequences around `$1` allow for branch names with spaces.
    
4.  `&&` is a logical operator that ensures each command only runs if the previous one succeeds. If `git checkout "$1"` fails, the commands that follow won’t run.
    
5.  `git checkout -b \”$2”\` : Creates a new branch with the name of the second parameter as before.
    
6.  `;`: Marks the end of the `f()` function;
    
7.  `f`: The final `f` calls the alias function immediately, meaning that when you call the alias, it declares the function and then calls it immediately.
    

**Usage:**

```
git new-work development task/feat-123
```

## Other Useful Aliases

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

## Summary

`co:` Checkout given branch → `git co task/feat-123`

`cob`: Creates a new branch from the current branch → `git cob feature/123`

`s`: Calls `git status` to view the status of the current git branch → `git s`

`tidy-up`: Deletes all local branches other than `main` → `git tidy-up`

`latest`: Gets the latest changes from remote `main` branch → `git latest`

`new-work`: Creates a new branch (2nd param) from 1st param branch → `git new-work main feat/123`

`git done`: Pushes the current branch to the remote repository (`origin`) and sets it as the upstream branch. This can be helpful when pushing your first commit and you get the error:  
`fatal: The current branch has no upstream branch. To push the current branch and set the remote as upstream, use git push --set-upstream origin`

`save`: Will simply add all changed files, and commit them, opening your default Git editor and requesting a commit message → `git save`

`savem`: Will do as above, but instead of opening your editor, you can pass in a commit message inline → `git savem ‘Task123: add index.html`

`br:` This one looks complicated, but it’s not as complicated as it seems but does highlight the power of aliases. In essence, it customizes the output format of `git branch` to display a detailed, color-coded list of branches, sorted by the most recent commit date, it will look something like the image below for each branch you have locally.

![36008ee8-e54e-4b06-8a84-2a20885a1255](https://cdn.hashnode.com/res/hashnode/image/upload/v1730060113591/36008ee8-e54e-4b06-8a84-2a20885a1255.png)

There you have it, an introduction to Git aliases and some useful examples of aliases you can add as a starter to your configuration.

As always if you want to chat about it, or hear about future articles you can follow me on [Twitter][12].

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