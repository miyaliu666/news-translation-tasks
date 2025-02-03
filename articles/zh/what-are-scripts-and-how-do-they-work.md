
---
title: 脚本是什么以及它们如何工作？通过脚本提高您的生产力
date: 2025-02-03T06:12:40.553Z
author: Arunachalam B
authorURL: https://www.freecodecamp.org/news/author/arunachalamb/
originalURL: https://www.freecodecamp.org/news/what-are-scripts-and-how-do-they-work/
posteditor: ""
proofreader: ""
---

拥有丰富经验的开发人员通常通过编写脚本来自动化大部分工作。这些脚本从简单的 alias bash 命令到在服务器上运行的重复 cron 触发器。

<!-- more -->

在本教程中，你将了解脚本是什么，它的多种使用案例，以及使用脚本的一些优缺点。我们还会通过一些示例脚本来展示它们的实际应用。

## 什么是脚本？

脚本是一组用任何脚本语言（例如 Bash、Python、JavaScript 等）编写的指令，可以帮助您自动化任务或控制流程。与已编译的程序不同，脚本通常是[解释执行][1]的，这意味着它们直接由运行时环境执行，而无需提前编译。

脚本是自动化重复任务、管理工作流程以及高效解决小问题（有时是大问题）的强大工具。无论您是初学者还是经验丰富的开发人员，了解如何编写脚本都可以提高您的生产力并拓宽您的技术能力。

## 为什么要编写脚本？

我已经提到过您可以使用脚本做些什么。所以现在让我们看看它们的优点（以及挑战）以便您了解它们为什么如此强大——以及何时使用它们。

### 脚本的优点

1. 自动化：脚本可以帮助您简化数据处理或文件管理等重复性任务。
    
2. 效率：它们还可以通过自动化您本来需要手动执行的任务来为您节省时间。
    
3. 错误减少：通过一致执行指令，脚本可以帮助减少人为错误。
    
4. 灵活性：脚本可以通过最小的修改适应各种任务。
    
5. 集成：它们还可以与其他系统、工具或工作流程无缝集成。
    

### 脚本的挑战

1. 性能：由于解释开销，脚本可能比编译程序要慢。
    
2. 可扩展性：它们并不总是适合大型或高度复杂的任务。
    
3. 调试：由于脚本的动态性，调试有时可能具有挑战性。
    
4. 安全风险：编写不当的脚本可能会暴露漏洞，特别是如果它们执行系统级命令。
    

### 何时使用脚本

脚本非常适合：

1. 任务简单、明确或一次性
    
2. 原型设计或快速自动化一个过程
    
3. 范围足够小以避免复杂性
    

脚本不适合：

1. 对性能要求高的任务。这种情况下，应尝试使用专用的 ETL（提取、转换、加载）工具或消息代理，或适合您用例的类似替代工具。
    
2. 具有广泛用户界面的应用程序。相反，您可以构建一个具有适当日志记录、测试和文档的小型应用程序或模块化系统。
    
3. 需要长期维护的场景，这时编译程序可能更稳定。相反，使用任务调度器或工作流程管理器，如 CRON、Airflow、AWS Lambda/GCP Functions。
    

## 如何编写有效的脚本

这是我用来编写有效脚本的方法。在我们了解这个过程后，我们将看到不同语言的示例脚本，以便您可以进行一些实践。

1. 定义问题：在编写脚本之前，明确它要解决的问题。明确要自动化的任务和预期的结果。
    
2. 选择合适的语言：
    
    - **Bash：** 适合系统级任务，如文件操作或服务器管理。
        
    - **Python：** 非常适合数据处理、网页抓取和更复杂的自动化。
        
    - **JavaScript：** 适合 Web 开发和基于浏览器的自动化。
        
3. 编写脚本：使用文本编辑器或集成开发环境 (IDE)，确保遵循最佳实践，如使用注释、有意义的变量名和模块化代码。我们将在下面讨论这些。
    
4. 测试脚本：在受控环境中测试脚本，以确保其按预期执行且不会导致错误。
    
5. 执行和部署：在目标环境中运行脚本。如有必要，使用 Cron（对于 Bash）或任务调度器来安排其执行。
    

## 示例脚本

现在您已经了解了基础知识，让我们来练习一下。假设您有大约 100 个文件，文件名为“book-part-1.pdf”、“book-part-2.pdf”、...、“book-part-100.pdf”。您想将所有文件名中的连字符（-）替换为下划线（\_），因为您尝试上传这些文档的网站不允许文件名中包含连字符。

这里有三种不同语言编写的脚本，它们都执行相同的操作。流程如下：
```

以下是翻译内容，保留了源文件的 markdown 排版布局：

这里是开始的文件名（包含连字符）：

![带有连字符的文件名](https://cdn.hashnode.com/res/hashnode/image/upload/v1737563509852/e9b1e671-465d-43ed-a831-3034852de624.png)

### Bash 脚本

我们将从一个 bash 脚本开始。如下所示：

```
#!/bin/bash
# 将文件名中的“-”替换为“_”
DIRECTORY="/path/to/your/folder"
for FILE in "$DIRECTORY"/*; do
    if [[ "$FILE" == *-* ]]; then
        NEW_NAME=$(echo "$FILE" | sed 's/-/_/g')
        mv "$FILE" "$NEW_NAME"
        echo "Renamed: $FILE -> $NEW_NAME"
    fi
done
```

我们在顶部定义了文件所在的目录。对于目录中的每个文件，我们检查名称是否包含 `-`。在这种情况下，我们通过使用 `echo` 命令复制旧文件名，并使用 `sed` 命令将 `-` 替换为 `_`，创建一个新文件名并存储在变量 `NEW_NAME` 中。最后，我们使用移动命令 `mv` 以旧文件名和新文件名作为参数。

### Python 脚本

接下来，让我们看看用 Python 实现会是什么样子：

```
import os
# 将文件名中的“-”替换为“_”
directory = "/path/to/your/folder"
for filename in os.listdir(directory):
    if "-" in filename:
        old_path = os.path.join(directory, filename)
        new_filename = filename.replace("-", "_")
        new_path = os.path.join(directory, new_filename)
        os.rename(old_path, new_path)
        print(f"Renamed: {filename} -> {new_filename}")
```

步骤在 Python 中非常相似。首先，我们定义目录，然后迭代遍历该目录中的每个文件。为了找到目录中的所有文件，我们必须使用 `os` 模块中的 `listdir` 方法。

然后，我们在下一行检查文件名是否包含 `-`。在这种情况下，我们通过合并目录及其文件名来找到文件的当前路径 (`old_path`) 。我们可以使用 `replace` 方法将 `-` 替换为 `_`，创建新的文件名。

然后我们以类似于生成 `old_path` 的方式来生成新的文件路径 (`new_path`) 。最后，我们在 `os` 模块中调用 `rename` 方法，以旧路径和新路径作为参数。

### JavaScript 脚本

现在让我们看看用 JavaScript 实现会是什么样子：

```
const fs = require('fs');
const path = require('path');
const directory = '/path/to/your/folder';

fs.readdir(directory, (err, files) => {
    if (err) {
        console.error('Error reading directory:', err);
        return;
    }
    files.forEach(file => {
        if (file.includes('-')) {
            const oldPath = path.join(directory, file);
            const newFilename = file.replace(/-/g, '_');
            const newPath = path.join(directory, newFilename);
            fs.rename(oldPath, newPath, err => {
                if (err) {
                    console.error(`Error renaming ${file}:`, err);
                } else {
                    console.log(`Renamed: ${file} -> ${newFilename}`);
                }
            });
        }
    });
});
```

JavaScript 实现与 Python 实现有些类似——但需要编写更多代码。一般来说，开发者不倾向于使用 JavaScript 来编写这些类型的脚本。大多数人依赖 Bash/Python。JavaScript 更适合用于基于浏览器的自动化脚本。

不过，让我们看看我们这里有什么。在这段 JavaScript 代码中，您需要使用两个不同的模块：`fs` 和 `path`。我们在顶部定义目录，使用 `fs` 模块中的 `readdir` 方法读取目录中的文件，并将目录作为参数传递。除了目录之外，我们还传递一个回调函数，该函数将在读取文件后执行。

在回调函数中，我们遍历每个文件，检查文件名是否包含连字符（`-`）。如果是，我们使用 `path` 模块通过目录和文件名作为参数找到旧路径。然后通过使用 `replace` 方法将所有连字符替换为下划线来构造新的文件名。

与旧路径类似，我们使用新文件名作为参数找到新路径。然后我们使用 `fs` 模块中的 `rename` 方法，通过传递旧文件名和新文件名来重命名文件。如果在重命名或读取目录中的文件时出现错误，我们会记录错误消息。否则，我们记录成功消息。

#### 如何运行这些脚本

下面是如何实际使用这些脚本：

1.  将 `/path/to/your/folder` 替换为包含文件的实际目录。
    
2.  在相应的环境中运行脚本：
    
    -   **Bash:** 保存为 `.sh` 文件，然后使用 `bash script.sh` 执行
        
    -   **Python:** 保存为 `.py` 文件，然后使用 `python script.py` 执行
        
    -   **JavaScript:** 保存为 `.js` 文件，然后使用 `node script.js` 执行
        

下面的截图显示了如何运行 bash 脚本来更改文件的名称。

![使用 bash 脚本更改文件名](https://cdn.hashnode.com/res/hashnode/image/upload/v1737563774216/f31158ab-da77-4b18-8625-ee0b2522e3e6.png)

![运行脚本后，文件名中的连字符被替换成下划线](https://cdn.hashnode.com/res/hashnode/image/upload/v1737563640766/4aa508af-1f0e-4fad-8b2c-ac2369cbe337.png)

定期脚本旨在定期执行，例如每周检查系统状态、清理日志或获取数据更新。这些脚本通常使用某种形式的任务调度程序。

### 常见方法

1.  CRON 任务：大多数操作系统都支持 CRON，它可以根据定义的时间计划触发脚本。
    
2.  任务队列：像 Celery（Python）、Bull（Node.js）或 Sidekiq（Ruby）这样的工具可以更灵活地处理定时任务。
    
3.  云调度器：AWS Lambda 的 EventBridge、Google Cloud Scheduler 或 Azure Logic Apps 等服务允许您在无服务器架构中设置定期脚本。
    

定期脚本的一个很好示例用例是发送每日/每周的系统使用/性能报告。您可以编写一个脚本，找出加入和订阅您产品的用户数量，并将该报告每天/每周通过电子邮件发送。

## 编写脚本的最佳实践

在编写脚本时需要记住以下几点：

**1\. 使用注释**：用注释解释脚本的复杂部分。

在下面的示例中，如果没有注释，可能需要额外时间来弄清楚为什么税率是小数而不是百分比。

```
# 计算含税总价
def calculate_price_with_tax(price, tax_rate):
    # 税率以小数表示（例如，7% 为 0.07）
    return price + (price * tax_rate)
```

2\. **错误处理**：考虑可能的错误并优雅地处理它们。

在下面的示例中，如果文件丢失，脚本不会崩溃——而是会显示一个有用的错误消息。

```
try:
    with open('data.csv', 'r') as file:
        data = file.readlines()
except FileNotFoundError:
    print("错误：未找到 'data.csv' 文件。请确保文件存在后再运行脚本。")
except Exception as e:
    print(f"发生意外错误：{e}")
```

3\. **模块化设计**：将脚本分解为可重用的函数或模块。

在下面的示例中，通过将功能划分为较小的可重用函数，您可以独立调试或重用脚本的部分。

```
def fetch_data_from_api(api_url):
    # 从给定的 API 获取数据
    pass

def process_data(data):
    # 将数据处理成所需格式
    pass

def save_to_file(data, filename):
    # 将处理后的数据保存到文件
    pass

# 主脚本
if __name__ == "__main__":
    data = fetch_data_from_api("https://example.com/api")
    processed_data = process_data(data)
    save_to_file(processed_data, "output.json")
```

4\. **输入验证**：验证用户输入以防止意外错误或安全风险。

如果没有验证，有人可能会输入无效或恶意数据（例如，某些情况下的 SQL 注入字符串）。

```
import re

# 验证输入是有效的电子邮件地址
def validate_email(email):
    pattern = r'^[a-zA-Z0-9_.+-]+@[a-zA-Z0-9-]+\.[a-zA-Z0-9-.]+$'
    if not re.match(pattern, email):
        raise ValueError("无效的电子邮件地址格式")
    return email

# 示例用法
try:
    user_email = validate_email(input("请输入您的电子邮件："))
    print(f"有效的电子邮件：{user_email}")
except ValueError as e:
    print(e)
```

5\. **版本控制**：使用 Git 或其他版本控制工具来跟踪更改。

如果某个更改导致脚本出错，您可以轻松地使用 `git checkout` 恢复到之前的提交。此外，您可以与团队成员无缝协作。

```
git init
git add script.py
git commit -m "Initial commit"
```

## 结论

编写脚本是一项可以显著提高您的生产力和解决问题能力的技能。通过理解 Bash、Python 和 JavaScript 等脚本语言的基础知识，您可以自动化任务、简化工作流程，并节省宝贵的时间。从小处入手，逐步构建，练习为不同用例编写脚本以掌握这一无价技能。

我给你一个练习。要运行和验证这个示例脚本，你可能认为需要手动创建 100 个文件。这会消耗大量时间。

我写了一个脚本来生成这 100 个文件。我也建议你试着写一个脚本来生成 100 个文件，文件名中带有连字符。然后尝试运行示例脚本将连字符转换为下划线。

这在开始时可能听起来很困难，但相信我，你只需要写 5 行 bash 代码就能生成 100 个文件。不仅仅是 100 个——你甚至可以只用 5 行代码生成百万/十亿/万亿的文件。

如果您希望了解更多关于脚本的信息，请订阅我的 [电子邮件通讯 (https://5minslearn.gogosoon.com/)][2] 并关注我的社交媒体。

祝你编写脚本愉快！

[1]: https://www.freecodecamp.org/news/compiled-versus-interpreted-languages/
[2]: https://5minslearn.gogosoon.com/?ref=fcc_what_are_scripts

