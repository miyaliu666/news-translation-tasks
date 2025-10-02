---
title: 如何成为 AI 辅助编码专家 – 开发者手册
date: 2025-10-02T10:40:08.430Z
author: Mrugesh Mohapatra
authorURL: https://www.freecodecamp.org/news/author/mrugesh/
originalURL: https://www.freecodecamp.org/news/how-to-become-an-expert-in-ai-assisted-coding-a-handbook-for-developers/
posteditor: ""
proofreader: ""
---

在过去的七年里，我一直负责 freeCodeCamp 的基础设施。现在我确信，经验丰富的开发者可以在保持代码质量的同时提高 3-4 倍的编码速度。这就是 AI 辅助开发能够提供的优势。简单来说，使用像 GitHub Copilot 这样的 AI 工具作为你的编码伙伴可以提高效率。它们可以建议代码、帮助你调试以及加快重复性任务的进度。

<!-- more -->

### 为什么这很重要

传统编码时，你需要自己编写每一行代码、查找文档、弄清语法。有了 AI，你可以：

-   专注于解决问题，而不是记住语法
    
-   通过实时看到优秀的代码示例更快地学习
    
-   快速构建项目而不牺牲质量
    

对于有经验的开发者来说，在 AI 帮助下可以更快地完成任务。但关键是：**你需要了解如何有效使用这些工具**。你也需要有编程背景来做到这一点。

感兴趣吗？让我们深入了解这些已经风靡全球的 AI 编码工具。

## 目录

-   [基础 AI 术语][1]
    
-   [何时使用 AI 以及何时自己编码][2]
    
-   [前提条件][3]
    
-   [你的完整学习旅程][4]
    
-   [如何生成你的第一个 AI 辅助代码（快速入门）][5]
    
-   [阶段 1：基础 – 开始 AI 编码][6]
    
-   [阶段 2：高级 GitHub Copilot 功能][7]
    
-   [阶段 3：基于 CLI 的 AI 代理（Claude Code & Gemini）][8]
    
-   [阶段 4：掌握 – 结合工具和高级工作流程][9]
    
-   [常见 AI 问题][10]
    
-   [完成所有阶段后接下来做什么？][11]
    
-   [结论][12]
    

## 基础 AI 术语

在开始之前，确保你了解以下关键术语：

-   **Tokens（令牌）：** 把令牌想象成“词片”——AI 阅读你的代码和文本的方式。每个字符、单词或符号都会使用令牌。免费版本会限制你可以使用的令牌数量。
    
-   **Context Window（上下文窗口）：** AI 能一次性“记住”多少代码/对话。就像短期记忆，更大的窗口意味着对项目的更好理解。
    
-   **Hallucinations（幻觉）：** 当 AI 自信地建议错误信息时——例如，捏造不存在的函数。务必验证 AI 的建议！
    
-   **Prompt（提示）：** 你给 AI 的指令——评论、问题或请求，用以指导其生成的代码。
    

## 何时使用 AI 以及何时自己编码

**使用 AI 的场合：**

-   编写模板代码（getters、setters、基本的 CRUD）
    
-   学习新的框架或语法
    
-   编写测试和文档
    
-   重构重复模式
    
-   在语法错误上解开困境
    

**自己编码的场合：**

-   设计系统架构
    
-   做出安全关键性决策
    
-   编写复杂的业务逻辑
    
-   学习新概念（首次）
    
-   进行性能关键的优化
    

**黄金法则：** 使用 AI 加速实施，但将架构决策留给自己。AI 擅长“如何”，但你决定“什么”和“为什么”。

## 前提条件

在开始本教程之前，你应该具备：

-   **基本编程经验** – 你可以用任何语言编写简单程序
    
-   **安装有代码编辑器** – 推荐使用 VS Code（从 [code.visualstudio.com][13] 免费获取）
    
-   **基本 Git 知识** – 你知道如何提交和推送代码
    
-   **免费起步** – 许多工具现在都有慷慨的免费版本，付费计划大约从每月 10-20 美元起
    

## 你的完整学习旅程

这篇全面的教程结构化为一个循序渐进的计划，旨在将你转变为 AI 辅助开发的专家：

注意：为了保持教程的易用性，我们将只关注一些核心工具。但你应该研究和探索更多适合你特定需求的工具，超出我们在此使用的工具。

### 学习路径：

你将经过 4 个阶段：掌握 GitHub Copilot 基础，解锁高级功能如聊天模式和代理，探索 CLI 工具（Claude Code 和 Gemini），最终战略性地结合多个工具进行完整的项目工作流程。

首先，让我们快速看看如何生成你的第一个 AI 代码片段。

## 如何生成你的第一个 AI 辅助代码（快速入门）

让我们从最基础的开始。别担心选择“完美”的工具——你总可以后续切换。以下是开始的方法：

### GitHub Copilot（推荐初学者使用）

你可以通过以下步骤安装 GitHub Copilot：

1.  打开 VS Code
    
2.  点击扩展图标（或按 Ctrl+Shift+X）
    
3.  搜索“GitHub Copilot”
    
4.  点击“安装”
    
5.  使用你的 GitHub 账户登录
    

**提示：** 学生、教师和开源软件维护者 [可以免费获得 Pro 计划][14]，这可以提供无限制的使用，而不是免费的使用限制。

### 您的第一个 AI 建议

安装后，创建一个名为 `test.js` 的新文件，并输入：

```
// function to calculate the area of a circle
```

按下 Enter 键等待。您会看到灰色文本出现——这是您的 AI 建议！按 Tab 键接受它。

就这样！您刚刚获得了第一个 AI 建议！是不是很酷？

## 阶段 1：基础 – 开始使用 AI 编码

### 第 1 步：了解您的选项

将 AI 编码助手想象成不同类型的有帮助的朋友和同事。让我们来浏览几个：

**基于 IDE 的：** 一些工具设计为可以与熟悉的代码编辑器一起工作，或作为编辑器如 VS Code 的独立分支。例如：

-   **GitHub Copilot (VS Code 扩展)** – 来自 GitHub 的 AI 编码助手，直接在 VS Code 中工作，提供 Tab 补全和聊天功能
    
-   **Cursor (独立)** – VS Code 分支，具有增强的代理模式、更快的自主编码和更好的大代码库重构处理
    
-   **Windsurf (独立或 VS Code 扩展)** – 专注于实时建议和团队功能的协作 AI 开发
    
-   **Zed** – 高性能编辑器，内置 AI 辅助功能和快速渲染
    

**基于 CLI 的：** 一些工具是基于 CLI 的，您可以在终端应用中启动：

-   **Claude Code** – Anthropic 的终端 AI，用于自主开发会话和复杂推理
    
-   **Gemini** – Google 的 CLI 工具，具有大上下文窗口和多模态功能（图像、文档）
    
-   **OpenCode** – 开源替代方案，具有可自定义模型和本地处理选项
    
-   **Cursor CLI** – Cursor 的终端版本，用于命令行 AI 辅助
    

**基于 UI 和后台代理的工具：** 除此之外，还有一些后台代理和工具可以完全在后台运行，例如执行拉取请求审查等。

例如，如果您设置了它们，ChatGPT 和 Claude 的桌面应用程序都可以编辑本地文件系统上的文件。同样，一些基于云的代理可以“在后台运行”以完成您的指令。我们将在本指南范围内排除这些。

### 第 2 步：做出选择并学习自动建议（Tab 补全）

对于您的第一阶段，我建议从 GitHub Copilot 开始。您可以在学习完基础知识后随时切换到适合您需求的工具。

### 第 3 步：逐步设置

#### 如何设置 GitHub Copilot（如果您之前已经遵循过快速入门，可以跳过此步骤）

1.  **打开 VS Code。** 如果您没有，请从 [code.visualstudio.com][15] 下载。
    
2.  **安装扩展**
    
    -   按 `Ctrl+Shift+X`（Windows/Linux）或 `Cmd+Shift+X`（Mac）
        
    -   在搜索框中输入“GitHub Copilot”
        
    -   点击蓝色的“安装”按钮
        
    -   您会看到一个弹出窗口要求您登录
        
3.  **登录**
    
    -   点击“登录 GitHub”
        
    -   您的浏览器会打开
        
    -   使用您的 GitHub 帐户登录（如果需要，可以在 [github.com][16] 免费创建一个）
        
    -   点击“授权 GitHub Copilot”
        
4.  **开始使用 Copilot**
    
    -   返回 VS Code，您会看到“GitHub Copilot 已准备好使用”

### 第 4 步：掌握 Tab 补全

让我们确保它正常工作。创建一个新文件：`hello.py`。输入此注释并按 Enter：

```
# function to greet a user by name
```

等待 1-2 秒。您应该会看到灰色文本出现。只需按 `Tab` 接受建议。

**您应该看到的内容：**

```
# function to greet a user by name
def greet_user(name):
    return f"Hello, {name}!"
```

如果您看到了这个，恭喜您！您现在正在使用 AI 帮助您编写代码。

如果您遇到设置问题，可以查看 [疑难解答快速参考][17] 寻求解决方案。

### 第 5 步：基本键盘快捷键和第一次练习

以下是您第一周需要的唯一快捷键：

**基础：**

-   `Tab` – 接受 AI 建议（这个用得最多！）
    
-   `Esc` – 拒绝建议（当您不需要时）
    

当您准备好了解更多时，尝试这些：

**Windows/Linux：**

-   `Alt+]` – 查看下一个建议
    
-   `Alt+[` – 查看上一个建议
    
-   `Ctrl+Enter` – 在面板中查看所有建议
    

**macOS：**

-   `Option+]` （或 `Alt+]`） – 查看下一个建议
    
-   `Option+[` （或 `Alt+[`) – 查看上一个建议
    
-   `Ctrl+Enter` – 在面板中查看所有建议
    

### 阶段 1 实践练习

#### 练习：构建一个简单的待办事项应用

1.  创建一个名为 `todo.js` 的新文件
    
2.  以此注释开始：`// TODO app with add, remove, and list functions`
    
3.  添加此注释并等待 AI 建议：`// function to add a new todo item`
    
4.  如果建议看起来不错，按 Tab 键接受
    
5.  继续为移除和列出函数添加注释
    
6.  测试您的函数以确保它们工作正常
    

**目标：** 学习通过清晰的注释与 AI “对话”，并建立接受/拒绝建议的信心。

### 准备好进入下一阶段了吗？在继续之前，请确保您能够：

```
- [ ] 通过输入注释获取 AI 建议
- [ ] 使用 Tab 接受建议，使用 Esc 拒绝建议
- [ ] 使用 Alt+] 和 Alt+[ 查看不同的建议
- [ ] 在 AI 帮助下编写基本函数
```

如果您对这些基本内容感到满意，就可以学习更强大的 Copilot 功能了。

## 阶段 2：高级 GitHub Copilot 功能

### 第 6 步：获取更好的 AI 建议

现在您已经了解了基础知识，让我们学习如何从您的 AI 那里得到_更好_的建议。诀窍在于了解您的 AI 能看到什么。

#### 您的 AI 助手能看到什么

把您的 AI 助手想象成一个能在您肩头上方看到的好朋友。它能看到：

1.  **您当前正在输入的内容**——您的当前文件
    
2.  **其他打开的标签页**——您打开的文件（这很重要！）
    
3.  **您的项目结构**——文件夹和文件名
    
4.  **您的注释**——这是您与 AI "对话" 的方式
    

#### "相邻标签页"技巧

这是一个能为您节省数小时的专业提示：**在标签页中打开相关文件**。

**示例：** 如果您在编写一个 React 组件：

-   打开您的组件文件 (`Button.jsx`)
    
-   还要打开 CSS 文件 (`Button.css`)
    
-   同时也让测试文件可见 (`Button.test.js`)
    

然后，您可以通过几种方式与 AI 共享这些额外文件的上下文：

-   **@提及文件：** 在聊天中键入 `@filename.js` 以引用特定文件
    
-   **使用 @workspace：** 这个聊天参与者可以看到您项目中的所有文件
    
-   **拖放：** 只需将文件从资源管理器窗口拖动到聊天窗口
    
-   **选择代码：** 高亮代码并右键单击"询问 Copilot"以将其包括在上下文中
    

AI 使用这些打开的文件来理解您的项目结构，并建议更相关的代码以匹配您现有的模式。

### 第 7 步：质量控制与最佳实践

#### 了解 AI 的局限性

AI 功能强大，但并不完美。以下是需要注意的关键事项：

**常见的 AI 错误：**

1.  虚构的函数：例如，`const result = array.superSort();` 并不存在！
    
2.  错误的参数：例如，`greetUser("John", "Doe");` 当函数期望 `greetUser(name)` 时
    
3.  过于复杂的解决方案：例如，`const isEven = (num) => num.toString(2).slice(-1) === "0";` - 只需使用 `num % 2 === 0`
    

快速质量检查清单：

```
- [ ] 测试代码 - 它真的有效吗？
- [ ] 阅读 - 逻辑上是否合理？
- [ ] 检查基础 - 所有函数/变量是否已定义？
- [ ] 信任直觉 - 如果感觉不对，进行调查
```

#### 安全要点

在接受 AI 建议之前，请确保检查这些安全问题：

```
- [ ] 没有硬编码的密码或 API 密钥
- [ ] 用户输入已验证
- [ ] 没有对用户数据使用 eval()
- [ ] 错误消息不暴露敏感信息
```

#### 更好的提示写作

这里有一个编写可靠提示的公式：是什么 + 如何 + 返回类型。

```
// ❌ 含糊： "make function"
// ✅ 清晰： "function to validate email format using regex, returns boolean"
```

#### 使用 Copilot 指令进行存储库级自定义

GitHub Copilot 现在支持通过 `.github/copilot-instructions.md` 文件进行存储库级别的自定义。此功能帮助 Copilot 理解您项目的特定模式和约定。

以下是设置 Copilot 指令的方法：

```
# 如果不存在，创建 GitHub 目录
mkdir -p .github
touch .github/copilot-instructions.md
```

示例 [copilot-instructions.md][19] 文件：

```
# Copilot 指令

## 代码风格

- 使用具有 hooks 的 React 函数组件
- 为新文件更偏好使用 TypeScript 而不是 JavaScript
- 使用 Tailwind CSS 进行样式设计
- 遵循 `/src/components` 中的现有文件结构

## 测试

- 使用 React Testing Library 编写测试
- 将测试文件放在 `__tests__` 目录中
- 使用描述性测试名称来解释行为

## API 模式

- 使用自定义钩子进行 API 调用
- 一致地处理加载和错误状态
- 使用 React Query 进行数据提取

## 命名约定

- 组件：PascalCase（例如，`UserProfile.tsx`）
- 钩子：以“use”开头的 camelCase（例如，`useUserData.ts`）
- 工具：camelCase（例如，`formatDate.ts`）
```

**这项功能实现了：**

-   Copilot 建议符合您项目模式的代码
    
-   自动遵循您的命名约定
    
-   建议合适的测试方法
    
-   理解您偏好的库和框架
    

**最佳实践：**

-   保持指令清晰和具体
    
-   随着项目标准的发展及时更新
    
-   包括首选模式的示例
    
-   提到您使用的库和框架
    

### 第 8 步：解锁高级 Copilot 功能

#### 了解您的选择

GitHub Copilot 提供多种方式来获得 AI 帮助：

1.  **Tab 补全**（您一直在使用的）——在输入时提供建议
    
2.  **聊天模式**——与 AI 就您的代码进行对话
    
3.  **编辑模式**——请求 AI 对现有代码进行修改
    
4.  **代理模式**——让 AI 自主完成大型任务
    

#### 模型选择

Copilot 现在提供不同的 AI 模型以满足不同需求：

订阅免费：

- **GPT-4.1** – 默认模型，综合性能强大

- **GPT-4** – 对大多数编码任务来说可靠

高级模型（有限的每月使用量）：

- **Claude 3.5 Sonnet** – 适合复杂逻辑

- **GPT-5** – 最新且最强大

- **Gemini 2.0 Flash** – 响应非常快速

**如何切换模型：** 点击聊天视图中的模型下拉菜单

**提示：** 学习时从免费模型（GPT-4.1）开始，对于复杂问题保留高级模型使用。

#### GitHub Copilot 的局限性

以下是在使用 AI 来帮助您进行编码时需要考虑的一些重要事项：

- **互联网依赖性** – 需要稳定的连接以获得建议

- **上下文限制** – 只能看到打开的文件，而不是整个项目结构

- **免费层限制** – 每月 2,000 次完成和 50 次聊天请求

- **代码质量参差不齐** – 始终审查建议，特别是涉及安全性敏感的代码时

- **学习曲线** – 编写有效的提示需要时间，特别是对于复杂任务

- **隐私考虑** – 您的代码会被发送到 GitHub 的服务器（请检查您组织的政策）

#### 基础聊天与建议

您可能想知道 - 什么时候应该使用制表符完成，什么时候应该使用聊天？最好是用制表符完成来编写新函数、快速的语法帮助和模式完成。可以使用聊天来解释现有代码、获取错误帮助和规划解决问题的方法。

**尝试一下：** 打开聊天（Ctrl+Shift+I）并询问：“这个函数有什么作用？”同时选择代码。

### 第9步：掌握聊天和代理模式

#### 三种聊天模式

1. **询问模式（默认）** – 用于问题和解释：

```
“这个函数有什么作用？”
“如何优化这段代码？”
“解释此错误消息”
```

2. **编辑模式** – 用于对现有代码进行更改：

```
“重构这段代码以使用 async/await”
“为所有 API 调用添加错误处理”
“将其转换为 TypeScript”
```

- 显示内联差异后应用更改

- 可跨多个文件工作

- 适合系统化重构

3. **代理模式** – 用于自主开发：

```
“创建一个带身份验证的 REST API”
“使用React和测试构建一个待办应用”
“将此代码库从Vue 2迁移到Vue 3”
```

- 按 `Cmd+Shift+I` （Mac）或 `Ctrl+Shift+Alt+I`（Linux）或 `Ctrl+Shift+I`（Windows）

- 可独立工作数小时

- 自动安装包、创建文件、运行测试

#### 何时使用各模式

每种模式都有特定的使用场景。当您学习新概念时，希望理解现有代码、获取解释和去计划方法时，使用询问模式。

当您正在重构现有代码、应用一致的更改、为现有功能添加特性或进行样式/模式更新时，使用编辑模式。

代理模式在构建完整特性（30分钟以上的工作）、设置新项目、大规模重构时有用，并且在您希望在 AI 编码时处理其他事情时使用。

#### 代理模式示例

小型代理任务（15分钟）：

```
“为我的 Express 应用添加用户身份验证”
```

代理生成的内容：

```
// middleware/auth.js
const jwt = require('jsonwebtoken');

const authenticateToken = (req, res, next) => {
  const authHeader = req.headers['authorization'];
  const token = authHeader && authHeader.split(' ')[1];

  if (!token) return res.sendStatus(401);

  jwt.verify(token, process.env.ACCESS_TOKEN_SECRET, (err, user) => {
    if (err) return res.sendStatus(403);
    req.user = user;
    next();
  });
};

// routes/auth.js
router.post('/login', async (req, res) => {
  // 使用 bcrypt 进行身份验证的逻辑
  const accessToken = jwt.sign({username: user.username}, process.env.ACCESS_TOKEN_SECRET);
  res.json({accessToken: accessToken});
});
```

**我发现的关键问题：** 代理最初忘记对密码进行哈希处理，并且没有包括刷新令牌。这需要一次迭代来修复安全漏洞并添加适当的错误处理。

大型代理任务（4小时以上）：

```
“将此基于类的 React 应用更新为使用 TypeScript 的 hooks”
```

代理生成的内容：

```
// 之前（类组件）
class UserProfile extends React.Component {
  constructor(props) {
    this.state = { user: null, loading: true };
  }
  // ... 生命周期方法
}

// 之后（Hooks + TypeScript）
interface User {
  id: number;
  name: string;
  email: string;
}

const UserProfile: React.FC = () => {
  const [user, setUser] = useState<User | null>(null);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    fetchUser().then(setUser).finally(() => setLoading(false));
  }, []);

  return <div>{loading ? 'Loading...' : user?.name}</div>;
};
```

**我发现的关键问题：**代理成功更新了47个文件，但最初在事件处理程序中存在类型问题，并且需要改进泛型类型。自动化测试还需要人工检查以确保适当的 TypeScript 覆盖。

Chat 参与者是专门的 AI 助手，可以访问您的开发环境中的特定部分。可以将它们视为不同领域的专家，能够帮助完成有针对性的任务。

它们基本上是以 `@` 作为前缀的 AI 助手，具有特殊的知识和能力：

-   **@workspace** 可以访问您整个项目的结构，可以搜索文件并理解组件之间的关系。当您需要项目范围的分析时，请使用 `@workspace`："找出此项目中的所有 API 端点" 或 "显示用户身份验证是在哪里实现的。"

-   **@terminal** 了解命令行操作，可以建议 shell 命令并解释终端输出。对于命令行帮助，请使用 `@terminal`："哪个命令可以运行测试？" 或 "如何为生产构建此项目？"

-   **@vscode** 是 VS Code 功能方面的专家，可以帮助进行设置、调试和编辑器配置。对于编辑器帮助，请使用 `@vscode`："为 Node.js 设置调试" 或 "为此项目配置自动格式化。"

**示例用法:**

```
@workspace 你能找到这个项目中的所有数据库模型吗？
@terminal 安装依赖项并启动开发服务器的命令是什么？
@vscode 如何设置断点以调试这个 Express 应用？
```

### 步骤 10：高级用户功能和高级工作流程

除了您已经了解的 Copilot 核心功能外，还有一些专门的工具和命令可以极大地提高您的生产力。这些功能超越基本的聊天模式和模型选择，专注于复杂的多文件操作和高级自动化。

#### 高级斜线命令

```
/doc - 生成文档
/explain - 详细的代码说明
/fix - 修复选定代码中的错误
/tests - 生成单元测试
/new - 创建新的项目结构
```

#### 多文件操作

**使用 # 引用：**

`#` 符号创建特定的引用，告诉 Copilot 精确关注哪里。这些引用就像是指向项目不同部分的精准指针：

-   **#file:filename**: 引用特定文件：`#file:UserModel.js`
    
-   **#codebase**: 引用整个项目代码库以进行搜索
    
-   **#selection**: 引用当前选定的代码
    
-   **#editor**: 引用当前活动文件
    

```
"更新 #file:UserModel.js 以包含时间戳"
"在 #codebase 中搜索所有数据库查询"  
"将 #selection 重构为现代 JavaScript 语法"
"为所有 API 调用添加错误处理到 #editor"
```

这些引用帮助 Copilot 准确理解要查看和更改的位置，使多文件操作更加精确。

**拖放操作：**

拖放是提供给 Copilot 的最直观的上下文方式之一。您可以将文件从 VS Code 浏览器中直接拖入聊天窗口，Copilot 会立即了解其内容和结构。

当您处理相关组件并需要 AI 理解不同文件如何相互连接时，此功能特别有用。Copilot 会在整个对话中记住这些文件关系，因此在继续相同的讨论时无需再次上传文件。

这种上下文保留在多个聊天会话中有效，使您可以轻松在复杂的多文件项目中继续之前的工作。

### 阶段 2 实践练习

#### 练习 1：聊天模式实践

1.  使用提问模式理解一个复杂的函数
    
2.  切换到编辑模式进行重构
    
3.  比较不同的方法
    

#### 练习 2：代理模式项目

1.  启动代理模式 (`Shift+Cmd+I`)
    
2.  请求："创建一个带测试的简单待办应用"
    
3.  观察自主开发过程
    
4.  查看生成的代码
    

#### 练习 3：高级功能

1.  使用 @ 参与者进行项目问题
    
2.  试验斜线命令
    
3.  练习多文件操作
    

### 准备好使用 CLI 工具了吗？

您现在已经学习了在 VS Code 中使用 GitHub Copilot 的基础知识！像 Claude Code 和 Gemini 这样的 CLI 工具为基于终端的开发提供了更强大的功能。

如果你对终端 AI 感兴趣，可以继续学习下方的第三阶段。如果你更愿意继续使用 VS Code，请跳到第四阶段以获取高级工作流程。

## 第三阶段：基于 CLI 的 AI 代理（Claude Code & Gemini）

### 步骤 11：认识 Claude Code——您的终端 AI 助手

#### 什么是 Claude Code？

还记得 GitHub Copilot 如何在 VS Code 中帮助你吗？Claude Code 在终端中为你提供相同的帮助。

不用在 VS Code 中键入和获取建议，你可以在终端中键入并与 AI 对话。这就像在命令行上有一个编程伙伴。

#### 简单例子：

在 VS Code 中使用 Copilot：

```
// 创建一个函数来验证电子邮件
[AI 提供建议代码]
```

在终端中使用 Claude Code：

```
claude
> 创造一个函数来验证电子邮件地址
[AI 为你编写代码]
```

那么，什么时候应该使用 VS Code/Copilot，什么时候应该使用 Claude Code 呢？

**如果你：**

-   喜欢在终端工作
    
-   想要与 AI 进行关于代码的对话
    
-   需要有关命令行任务的帮助
    
-   想要更多控制 AI 互动
    
Claude Code 非常适合你。

-   偏好可视化编辑器

-   对当前的工作流程感到满意

-   不花太多时间在终端

#### 定价

Claude Code 需要 Claude Pro（每月 20 美元）或 ClaudeMax（每月 100 美元）订阅，或使用 API 积分按使用量付费。

#### Claude Code 限制

如果您计划使用 Claude Code，请考虑以下重要事项：

-   **仅付费** – 没有免费版本，需要 Claude Pro 订阅或 API 积分

-   **基于终端** – 不如 IDE 集成工具可视化

-   **学习曲线** – 需要熟悉命令行界面

-   **上下文管理** – 需要手动管理对话上下文

-   **依赖互联网** – 所有操作都需要稳定的连接

-   **会话限制** – 长时间的自主会话会消耗大量 API 积分

#### 安装

推荐（所有平台）：

```
npm install -g @anthropic-ai/claude-code
```

其他安装方式：

-   **macOS/Linux**: `curl -fsSL https://claude.ai/install.sh | bash`

-   **Windows**: `irm https://claude.ai/install.ps1 | iex`

#### 基本用法

**交互模式（推荐）：**

交互模式是 Claude Code 的主要界面，您可以与 AI 实时对话。不同于一次性命令，交互模式创建了一个持久的会话，您可以提出后续问题、迭代解决方案，并随着时间的推移构建复杂的项目。

推荐使用交互模式，因为：

-   **上下文持久性**：Claude 会记住整个对话和项目上下文

-   **迭代开发**：您可以优化请求，并基于先前的回应进行改进

-   **实时协作**：在工作时提问、获取解释并修改方法

-   **会话恢复**：使用 `claude --resume` 继续之前的对话

**其他可用模式：**

-   **一次性模式**：单个命令执行（下文说明）

-   **代理模式**：可独立工作数小时的自主开发会话

1.  导航到您的项目：

```
cd your-project
claude
```

2.  自然地开始对话：

```
Claude Code > 分析这个代码库并提出改进建议

Claude Code > 现在帮我重构用户认证

Claude Code > 为支付模块添加单元测试
```

3.  继续之前的会话：

```
claude --resume
```

**一次性命令（用于快速任务）：**

一次性命令是执行特定任务后即退出的单次执行命令。与交互模式不同，这些命令不保留对话上下文 —— 非常适合快速、独立的任务。

**什么是一次性命令？**

这些是在您的终端中直接运行带有特定指令的命令，而不进入交互会话。Claude 将执行请求并立即提供结果。

**何时使用一次性命令：**

-   快速分析或代码审查

-   简单文件修改

-   自动化脚本和 CI/CD 集成

-   需要单一特定答案时

**示例：**

```
claude "分析这个代码库并提出改进建议"
claude "修复 src/ 中的所有 TypeScript 错误"
claude "为 utils.js 生成单元测试"
claude "解释这个函数的作用" --file src/auth.js
```

关键区别在于一次性命令在运行之间不记住上下文，而交互模式保持完整的对话历史记录和项目理解。

**交互式 vs 自主会话：**

在交互模式内，您可以选择协作和自主的方法：

**交互式会话（协作）：**

```
Claude Code > 我正在构建用户认证。我们应该采用什么方法？

你：使用 JWT 令牌和刷新令牌轮换

Claude Code > 实现带刷新令牌的 JWT 认证
[逐步向您展示实现方案]

Claude Code > 我还需要添加密码重置功能吗？

你：是的，使用基于邮件的重置
```

**自主会话（免动手开发）：**

```
Claude Code > 构建一个完整的用户管理系统，包括认证、个人资料、偏好设置和管理功能。使用安全和测试的最佳实践。

[Claude 自主工作数小时，定期提供更新]
[最终结果：完整的用户管理系统，准备投入生产]
```

**何时使用每种模式：** 在学习时或希望对决策进行控制时使用交互式会话。对于定义明确的任务，在您信任 Claude 能够独立做出良好选择时使用自主会话。

#### 关键特性

**思维模式（在交互式会话中使用）：**

思维模式是特定的命令，可以告诉 Claude 在响应前要进行多深的分析。您可以根据问题的复杂程度手动选择这些模式。

**何时使用每种模式：**

-   `think` – 对简单任务进行快速分析："think: review this function for bugs"

-   `think hard` – 对复杂逻辑进行深入推理："think hard: optimize this algorithm"

-   `think harder` – 复杂问题解决，考虑多重因素："think harder: design a scalable database schema"

-   `ultrathink` – 对架构决策进行最大深度分析："ultrathink: evaluate microservices vs monolith for this project"

Claude展示其通过较长的思考模式进行推理的过程。您将在得出最终答案之前看到逐步分析。更高的思考模式花费更多时间，但会提供更全面的解决方案。

**选择正确的模式：**

使用`think`进行快速代码审查，使用`think hard`调试复杂问题，使用`think harder`处理系统设计问题，使用`ultrathink`进行影响整个项目的重大架构决策。

#### 使用Claude.md进行项目级定制

Claude Code最强大的功能之一是使用`.claude/CLAUDE.md`文件进行项目级定制。这让你可以为Claude提供有关特定项目、编码标准和偏好的上下文。

像这样设置CLAUDE.md：

```
# 创建项目级配置
mkdir -p .claude
touch .claude/CLAUDE.md
```

以下是一个CLAUDE.md文件的示例：

```
# 项目上下文

这是一个使用Express和PostgreSQL的Node.js REST API。

## 编码标准

- 使用async/await，绝不使用回调
- 所有数据库查询使用Prisma ORM
- 为所有新功能使用Jest编写测试
- 遵循RESTful约定

## 项目结构

- `/src/routes` - API端点
- `/src/models` - 数据库模型
- `/src/middleware` - Express中间件
- `/tests` - 单元和集成测试

## 偏好

- 为所有新文件使用TypeScript
- 更喜欢函数式编程模式
- 为所有函数包含JSDoc注释
```

这有助于Claude自动理解您的项目结构。还帮助Claude遵循您的特定编码标准，为您的技术栈建议合适的模式，并在多次会话中记住您的偏好。

**专业提示：** 随着项目的发展更新你的CLAUDE.md，以保持Claude的建议相关和准确。

#### Claude Code自主开发会话

这是一个多小时的自主开发会话示例：

```
cd your-project
claude
```

**你的提示：**“构建一个可扩展的具有实时订阅、身份验证、速率限制和全面测试的GraphQL API”

Claude的6小时自主过程：

```
Claude Code > ultrathink: 分析需求并设计架构

[创建详细的技术规格和开发计划]

Claude Code > 实现核心GraphQL服务器与Apollo
[运行：npm install apollo-server-express graphql]
[创建：server.js, schema/typeDefs.js, resolvers/index.js]

Claude Code > 添加身份验证和授权中间件
[创建：middleware/auth.js, utils/jwt.js, models/User.js]
[实现具有适当错误处理的JWT策略]

Claude Code > 设置具有Redis的实时订阅
[运行：npm install graphql-redis-subscriptions]
[配置WebSocket连接，创建订阅解析器]

Claude Code > 实施速率限制和安全措施
[添加helmet, express-rate-limit, 查询复杂度分析]

Claude Code > 编写全面的测试套件
[创建单元测试、集成测试、负载测试]
[运行：npm test - 看到8个失败的测试]

Claude Code > 自动修复测试失败
[识别异步时间问题，修复解析器逻辑]
[反复重新运行测试直到94/94个测试通过]

Claude Code > 性能优化和缓存
[实现Redis缓存，优化数据库查询]
[添加性能监控和日志记录]

Claude Code > 生成全面的文档
[创建带示例的API文档，README，部署指南]

Claude Code > 最终验证和清理
[运行完整的测试套件，检查代码覆盖率：96%]
[创建生产构建和部署脚本]
```

这代表了6小时的自主工作（在此期间你可以处理其他项目）。结果是一个生产就绪的GraphQL API，具有身份验证、实时功能和全面的测试。

为什么这有效：

- **自主反馈循环：**Claude运行测试，看到失败，自动修复
    
- **上下文感知：**保持对整个项目结构的理解
    
- **自我纠正：**反复迭代解决方案直至正确工作
    
- **工具集成：**无缝使用git, npm, 测试框架
    

**网络搜索集成：**

Claude Code可以搜索网络以获取当前信息，这对于AI训练数据有截断日期特别有用。该功能帮助您保持与最新文档、最佳实践和解决方案的同步。

```
Claude Code > 搜索最新的React 19功能并更新我的组件

[Claude搜索网络，然后以发现继续对话]

Claude Code > 现在将这些新功能应用于UserProfile组件
```

**网络搜索何时有帮助：**

- 获取新库版本的当前文档
    
- 找到最近错误消息或漏洞的解决方案
    
- 研究最新的最佳实践和模式
    
- 比较当前问题的解决方案
    

当Claude检测到需要当前信息时，网络搜索会自动发生，或者你可以通过在提示中提到“搜索”或“最新”来显式请求。

#### Claude Code键盘快捷键

你可以使用这些键盘快捷键提高工作效率：

-   `Ctrl+C` – 取消当前输入或生成

-   `Ctrl+D` – 退出 Claude Code 会话

-   `Ctrl+L` – 清除终端屏幕

-   `Up/Down arrows` – 导航命令历史

-   `Esc` + `Esc` – 编辑上一条消息

**多行输入：**

-   `\` + `Enter` – 快速转义以创建新行（适用于所有终端）

-   `Option+Enter` (Mac) / `Shift+Enter` (已配置) – 插入新行

### 步骤12：Google Gemini 命令行界面

#### 何时使用 Gemini 与 Claude Code：

Gemini 是另一种基于命令行界面的 AI 工具，它与 Claude Code 相辅相成，而不是竞争。尽管 Claude Code 擅长深度推理和复杂的开发任务，但 Gemini 提供了独特的优势：大规模上下文窗口（超过100万个标记）、慷慨的免费限制以及强大的多模态能力。

**使用 Gemini，当您：**

-   需要一次分析整个大型代码库
    
-   希望处理图像、图表或草图
    
-   在预算限制内工作（慷慨的免费套餐）
    
-   需要极大的上下文窗口来处理复杂项目
    

**使用 Claude Code，当您：**

-   需要复杂的推理和问题解决
    
-   希望进行自主开发会话
    
-   偏爱用于复杂分析的高级思维模式
    
-   正在构建需要详细规划的生产系统
    

**最佳方法：** 许多开发人员策略性地使用这两种工具——Gemini用于分析和视觉输入，Claude Code用于复杂的开发任务。

Gemini 将 Google 的 AI 带到您的终端，并提供慷慨的免费限制。

#### 安装

使用 npx (推荐试用)：

```
npx @google/gemini-cli
```

全局安装：

```
npm install -g @google/gemini-cli
gemini  # 开始交互式会话
```

#### 认证

1.  使用 Google 登录：

```
gemini auth login
```

2.  检查状态：

```
gemini auth status
```

免费限制：

-   60 次请求/分钟
    
-   使用 Google 帐户每日 1,000 次请求
    

内置工具：

-   `/memory` – 管理对话记忆
    
-   `/stats` – 查看使用统计
    
-   `/tools` – 列出可用工具
    
-   `/mcp` – 配置模型上下文协议服务器
    

#### Gemini CLI 的限制

如果您计划使用 Gemini，请考虑以下重要事项：

-   **速率限制** – 免费套餐 60 次请求/分钟，1,000次/天
    
-   **Google 依赖性** – 需要 Google 帐户和互联网连接
    
-   **较新的工具** – 相较于 GitHub Copilot，社区较小，资源较少
    
-   **面向终端** – 与流行的 IDE 整合较少
    
-   **多模态处理** – 图像上传有大小限制 (20MB)
    
-   **测试版功能** – 某些高级功能可能不稳定
    

#### 独特的 Gemini 特性

**庞大的上下文窗口：**  
Gemini 可以在单个会话中处理超过 100 万个标记，这意味着它可以同时分析整个大型代码库。这对于理解复杂系统架构和多个文件之间的关系特别有用。

**多模态能力：**  
Gemini 能够处理和理解各种类型的视觉内容以及代码，使其在设计到代码的工作流程和视觉调试中具有独特的优势。

#### 将您的草图转化为代码

这真的很酷：您可以在纸上绘制一些东西，然后 Gemini 将其转化为可工作的代码！

操作方法如下：

1.  **创建您的草图：** 在纸上、白板或数位板上绘制您的想法
    
2.  **拍照或截图：** 使用手机拍摄或截图，将草图数字化
    
3.  **保存图像：** 保存为 JPG、PNG 或 WebP 格式（小于 20MB）
    
4.  **通过命令行将其展示给 Gemini：**
    

```
gemini -p "将此草图转化为具有良好样式的 React 组件" sketch.jpg
```

**替代方法：**

```
# 如果您处于交互会话中，您可以引用文件：
gemini
> 分析此UI草图并创建HTML/CSS：@sketch.jpg

# 或在支持的终端中拖放
gemini
> 将此设计实现为 Vue 组件
[将 sketch.jpg 拖入终端]
```

然后，Gemini 会查看您的图纸并生成：

-   与您的草图匹配的工作 React 组件
    
-   使其看起来不错的精美 CSS 样式
    
-   如果您绘制了表单则提供表单验证
    
-   使其工作的所有代码
    

这就像拥有一个能读懂您心思的设计师和开发人员！

#### 通过展示图像来修复 Gemini 中的错误

UI 出现了 Bug？您可以向 Gemini 展示视觉信息以帮助调试：

```
gemini -p "该 UI 看起来有问题。有什么问题，我该如何解决？" image.png
```

Gemini 可以分析视觉信息并告诉您：

-   问题的原因是什么
    
-   需要修改哪些代码
    
-   有时还有更好的解决方法
    

#### 将架构图转化为代码

画出系统架构图，Gemini 可以构建它：

```
gemini -p "使用 Docker 和数据库构建此系统架构" diagram.jpg
```

Gemini 将：

-   理解您的图示
    
-   创建您所需的所有 Docker 文件
    
-   设置数据库和连接
    
-   根据您的设计提供一个可工作的系统

将设计翻译成代码不再需要耗费数小时，您可以：

1.  向 Gemini 展示您的草图或设计
    
2.  让 Gemini 构建它
    
3.  在几分钟内获得工作代码，而不是几个小时，只需根据需要进行细化
    

大多数情况下，Gemini 初次尝试就能很接近您的期望。即使它并不完美，它也为您提供了一个很好的起点，节省了大量时间。

### 第13步：比较 CLI 工具

这里有一个简易表格来帮助您比较 Claude Code 和 Gemini CLI 的功能：

| **功能** | **Claude Code** | **Gemini CLI** |
| --- | --- | --- |
| **上下文窗口** | 大 | 1M+ 令牌 |
| **网络搜索** | 内置 | 谷歌搜索集成 |
| **文件编辑** | 直接编辑 | 基于差异 |
| **思维模式** | 4 个等级 | ReAct 循环 |
| **IDE 集成** | VS Code 快捷键 | 终端优先 |
| **免费层** | 有限 | 慷慨 (1000/天) |
| **开源** | 否 | 是 |
| **多模态** | 否 | 是 (图像, PDF) |

### 第14步：高级 CLI 工作流程

#### 工作流程1：Claude Code 的互动代码审查

```
Claude Code > review my recent git changes

[Claude 分析差异]

Claude Code > fix the security issue you found in the login function

Claude Code > now create a pull request with a good description
```

#### 工作流程2：Gemini 的对话式架构分析

```
Gemini > analyze this codebase architecture and identify technical debt

[Gemini 提供全面的分析]

Gemini > create a migration plan for the database issues you found

Gemini > generate API documentation for the endpoints
```

#### 工作流程3：互动测试驱动开发

```
Claude Code > I need to add payment processing. Start by writing comprehensive tests

[Claude 创建测试套件]

Claude Code > now implement the payment service to pass these tests

Claude Code > add error handling and edge cases
```

### 结合 VS Code 和 CLI 工具

#### 混合工作流程的力量：

最具生产力的开发者通常不会只选择一个 AI 工具——他们策略性地将 VS Code 扩展与 CLI 工具结合起来，以最大化效率。每个工具都有独特的优势，结合它们会创造出一个大于其各部分总和的工作流程。

**结合工具的好处：**

-   **无缝的上下文切换：** 从 Copilot 开始快速开发，然后无缝移动到 Claude Code 进行复杂的分析，而不失去动力
    
-   **互补的优势：** 利用每个工具的最佳功能，如 Copilot 的实时建议 + Claude 的深入推理 + Gemini 的视觉处理
    
-   **持续的工作流：** 无需在工具之间复制/粘贴代码—在项目中直接工作，并根据需要获得不同的 AI 支持
    
-   **减少心理负担：** 工具处理不同的认知任务，让您专注于创造性的问题解决
    

#### 如何实际组合工具：

示例工作流程——构建用户仪表板：

1.  **在 VS Code 中使用 Copilot 开始：** 使用制表符补全快速构建基本组件结构
    
2.  **保持 VS Code 打开，启动 Claude Code：** 获取架构建议和重构建议，同时保持编辑器上下文
    
3.  **切换到 Gemini 以获取视觉元素：** 上传 UI 模型以生成匹配的样式
    
4.  **返回到 VS Code：** 应用所有建议，同时有 Copilot 协助实现细节
    

**关键集成点：**

-   **共享项目上下文：** 所有工具在同一个目录中工作，了解您的项目结构
    
-   **文件系统协调：** CLI 工具进行的更改会立即在 VS Code 中可见
    
-   **版本控制集成：** 使用 CLI 工具进行 git 操作，而 VS Code 显示视觉差异
    

### 快速切换设置

#### 什么是快速切换？

快速切换设置指的是配置你的开发环境，让你可以快速在不同的 AI 工具之间切换，而不产生摩擦。通过创建快捷方式，而不是输入长命令或通过多个设置步骤进行导航，您可以立即访问当前任务所需的 AI 工具。

在您的 shell 配置文件中添加（`.zshrc` 或 `.bashrc`）：

```
# 快速 AI 命令用于交互模式
alias cc="claude"
alias gc="gemini"

# 在需要时进行快速单次命令
alias think="claude 'think hard:'"
alias analyze="gemini -p 'analyze:'"
```

### 阶段3实践练习

#### 练习1：互动 Claude Code 项目设置

1.  创建一个新项目目录
    
2.  启动：`claude`
    
3.  开始对话："set up a Node.js Express API with PostgreSQL"
    
4.  继续聊天："add authentication middleware"
    
5.  继续："now add comprehensive error handling"
    
6.  查看生成的代码并提问
    

#### 练习2：互动 Gemini 代码分析

1.  导航到现有项目
    
2.  启动：`gemini`
    
3.  从以下内容开始："analyze this codebase and identify potential security vulnerabilities"
    
4.  后续："explain the most critical issue in detail"
    
5.  继续："create a fix for the authentication vulnerability"
    
6.  询问："what other improvements should I prioritize?"
    

1. 在 VS Code 中使用 Copilot 开始初始开发
    
2. 切换到交互式 Claude Code 会话进行复杂的代码重构
    
3. 使用交互式 Gemini 会话进行代码库分析和文档撰写
    
4. 熟练地在工具之间无缝切换
    

需要 CLI 工具的帮助吗？请参阅[故障排除快速参考][20]以了解设置和常见问题。

## 第四阶段：掌握工具与高级工作流程的结合

### 第 15 步：工具选择策略

#### 何时使用每种工具

那么，在你的工作流程中，什么时候应该使用每个工具呢？

当速度至关重要时，你可以将 GitHub Copilot 作为一名在线的结对程序员使用。它可以帮助你快速编写新函数、在输入时获得实时建议，并即时掌握不熟悉的 API 或框架。它也方便于在不中断你的工作流程的情况下快速查找文档。

然后，你可以转而使用 Claude Code 来处理更大、更复杂的任务：复杂的多文件重构、起草全面的测试以及“大声思考”关于架构和权衡的问题。在这里，它还可以帮助完成 Git 任务，比如指导你进行操作和组装拉取请求。

最后，当需要端到端分析大型代码库或将视觉输入（如截图/图表）整合到工作流程中时，可以从终端调用 Gemini CLI。由于可以免费使用，所以适合大量运行的场景，并且它适用于需要可定制、脚本友好的设置的场合。

### 第 16 步：理解 MCP —— 让 AI 工具协同工作

#### 什么是 MCP？

MCP（模型上下文协议）是一种让你的 AI 工具增强能力的简单方法。可以把它想象成给你的手机增添应用程序——每个 MCP 服务器都会为你的 AI 增加新的功能。

#### 为什么初学者需要关注 MCP？

没有 MCP 会遇到的问题是：你的 AI 只能处理它所知道的和你告诉它的内容。它无法：

- 搜索网络上的当前信息
    
- 自动测试你的网站
    
- 在会话之间记住你的项目细节
    
- 连接到你的数据库或 API
    

但有了 MCP 服务器，你的 AI 就能突然：

- **获取当前信息** —— 从 Google 搜索最新的文档和解决方案
    
- **测试你的代码** —— 自动检查你的网站是否正常工作
    
- **记住你的项目** —— 跟踪你的架构和决策
    
- **连接到工具** —— 使用 GitHub、数据库等
    

所以，相比于手动重复劳动，你的 AI 可以自动处理这些任务。这意味着你将花更少的时间去谷歌搜索错误消息、手动测试代码，以及在每个会话中向 AI 解释你的项目，而是花更多时间实际构建东西。

#### 初学者简单的 MCP 示例

以下是 MCP 能为你做的一些初学者友好的示例：

**示例 1：无需谷歌搜索的帮助**

```
你：“这个 CSS 不工作。找出原因并修复它”

没有 MCP：你需要谷歌搜索错误，阅读文档，尝试解决方案
有了 MCP：AI 搜索当前的 CSS 文档，找出问题，并自动修复
```

**示例 2：自动测试你的网站**

```
你：“检查我的联系表单是否正常工作”

没有 MCP：你需要手动填写表单，检查邮件，测试边缘案例
有了 MCP：AI 填写表单，确认邮件已发送，测试不同的输入
```

**示例 3：AI 记住你的项目**

```
你：“为我的待办事项应用程序添加一项新功能”

没有 MCP：你需要解释数据库结构、API 路线、前端框架
有了 MCP：AI 已记住一切并直接构建功能
```

#### 准备尝试 MCP 吗？

如果这看起来很复杂，不用担心！你可以从一个简单的 MCP 服务开始，在适应后再添加更多。

#### 初学者简单的 MCP 设置

我们将从 VS Code 开始（因为这是最简单的选项）：

1. 打开 VS Code
    
2. 转到扩展（Ctrl+Shift+X）
    
3. 搜索“GitHub Copilot MCP”或类似的 MCP 扩展
    
4. 点击“安装”
    

这样就完成了！扩展会自动处理一切。

通过这样做，你可以为你的 AI 获取网络搜索功能、基本项目记忆力和简单的自动化功能。

要测试它，试着让你的 AI：“搜索最新的 React 最佳实践并给我一个示例”。 如果它能搜索并返回当前的信息，MCP 就正常工作了！

#### 想要更多的 MCP 功能吗？

一旦你适应了基本的 MCP，可以探索下面更高级的设置：

- 自定义 MCP 服务器安装
    
- 高级配置选项
    
- 构建自己的 MCP 集成
    

目前，上述的 VS Code 扩展方法将为你提供充足的 AI 超能力以便入门！

**这就是 MCP 的精要！**从上面的简单 VS Code 扩展方法开始，你将迅速看到你的 AI 变得多么强大。

#### 下一步

- 尝试基础的 VS Code MCP 扩展
    
- 使用简单的请求进行测试，比如“搜索 X 并实现它”
    
- 熟悉后，探索更多的第 4 阶段 MCP 服务器
    

MCP 将你的 AI 从代码建议工具转变为真正的开发伙伴。最好的部分是？一旦你用一个工具设置好，它就会与所有工具协同工作！

如果 AI 提示无法搜索网络，可以尝试以下几种方法。

首先，检查 MCP 扩展是否确实安装在 VS Code 中。然后尝试重启 VS Code。最后，要确保你的提问方式是 AI 能理解的，比如：“搜索 X 并展示 Y”。

如果 VS Code 扩展无法安装，尝试检查你的网络连接或将 VS Code 更新到最新版本。你也可以尝试使用不同名称搜索“MCP”或“Model Context Protocol”扩展。

如果仍然遇到问题，我们将在下文中介绍高级故障排除方法。或者你也可以询问你的 AI：“帮助我排查 MCP 设置故障”。

### 高级 MCP 设置与集成

#### 手动 MCP 服务器安装

对于希望完全控制其 MCP 设置的高级用户：

**步骤 1：安装 MCP 服务器**

大多数 MCP 服务器可以通过 npm 安装：

```
# 用于网络自动化和测试
npm install -g @modelcontextprotocol/server-puppeteer

# 用于无需 API 密钥的网络搜索
npm install -g @mcp-servers/duckduckgo

# 用于数据库访问
npm install -g @modelcontextprotocol/server-postgres
```

某些服务器（如 GitHub）则使用 Docker：

```
docker pull ghcr.io/github/github-mcp-server
```

**步骤 2：配置你的工具**

**理解层次化配置：**

每个 AI 工具在多个位置检查 MCP 配置，更具体的设置优先级高于一般设置。这意味着你可以有全局默认值，但可以针对特定项目进行覆盖。这就像 CSS——更具体的规则覆盖一般规则。

**Claude Code 具有最灵活的设置：**

Claude Code 配置层次结构（按顺序检查）：

1.  **项目级别**：`.claude/mcp.json`（最高优先级）
    
2.  **本地设置**：`.claude/settings.local.json`
    
3.  **全局配置**：`~/.claude/mcp.json`（后备）
    

其他工具：

-   **VS Code**：`.vscode/mcp.json`（仅项目级别）
    
-   **Cursor**：`.cursor/mcp.json`（仅项目级别）
    
-   **Windsurf**：使用 VS Code 的配置格式
    

以下是一个示例配置（适用于任何工具，只需调整文件位置）：

```
{
  "mcpServers": {
    "puppeteer": {
      "command": "npx",
      "args": ["@modelcontextprotocol/server-puppeteer"]
    },
    "duckduckgo": {
      "command": "npx",
      "args": ["@mcp-servers/duckduckgo"]
    },
    "github": {
      "command": "docker",
      "args": [
        "run",
        "-i",
        "--rm",
        "-e",
        "GITHUB_PERSONAL_ACCESS_TOKEN",
        "ghcr.io/github/github-mcp-server"
      ],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "your_token_here"
      }
    }
  }
}
```

#### 生产环境 MCP 服务器

**1\. 颠覆性认知工具：**

**序列思维服务器：**  
该服务器通过将复杂问题分解为逻辑步骤来改变 AI 处理复杂问题的方式。当你要求实现一个大型功能时，AI 不会直接跳到代码实施，而是首先创建一个详细的计划，其中包含阶段、依赖关系和决策点。

这对于重构遗留系统或构建新功能来说是无价的，尤其是在操作顺序很重要的情况下。服务器在整个开发会话期间保持这种规划上下文，确保决策的一致性。

**记忆库服务器：**  
消除了每次会话都需要重新解释项目结构的烦恼。该服务器会创建有关你的架构选择、编码标准、团队偏好和项目目标的持久性记忆。当你几天后返回工作时，AI 可以立即知道你的数据库架构、API 模式，甚至为什么做出某些决策。它就像一个随着你的开发工作完美同步的项目文档系统。

**知识图谱服务器：**  
创建代码库关系的动态地图——不仅仅是文件依赖，还有功能、共享工具和架构模式之间的概念连接。当你修改一个组件时，AI 可以立即识别出可能需要更新的所有相关区域。这可以防止由于遗漏相关更改而导致的错误，并有助于重构期间的影响分析。

**2\. 网页自动化与测试服务器：**

**Puppeteer Server：**  
提供无头浏览器的控制，用于综合测试工作流。AI 可以自动浏览你的 Web 应用程序，填写表单、点击按钮，并验证预期行为。

这在回归测试中特别强大——AI 可以重播用户工作流并在部署前捕获更改。此外，它还启用基于截图的测试和性能监控自动化。

**Playwright Server：**  
同时扩展 Chrome、Firefox 和 Safari 浏览器的自动化功能。该服务器对于跨浏览器兼容性测试至关重要，并允许 AI 在开发初期捕获特定浏览器的问题。

与手动测试不同，AI 可以在所有浏览器上并行运行相同的测试场景，生成关于功能和性能差异的比较报告。

**3\. 开发集成服务器：**

**GitHub Server：**  
将终端转变为具有 AI 智能的完整 GitHub 界面。AI 可以自动创建分支、管理拉取请求、分析代码审查评论，甚至根据代码更改生成 PR 描述。它还可以根据内容分析分配标签，并通过理解问题与实际代码更改之间的关系来维护项目板。

**PostgreSQL 服务器：**  
支持直接的数据库分析和优化。人工智能可以检查查询性能、建议索引优化、分析数据模式，甚至生成迁移脚本。此服务器在调试生产问题时特别有价值，因为人工智能需要理解实际的数据分布和查询执行模式，而不仅仅是理论上的数据库设计。

**4\. 辅助工具：**

**MCP Compass**  
帮助您为任何任务找到合适的 MCP 服务器。

这些服务器将您的人工智能从代码建议者转变为能够测试、搜索、记忆和自动化的真正开发合作伙伴！

### 步骤 17：高级提示工程

#### 上下文提示

提供示例：

```
// 替代用语："创建一个验证函数"
// 使用："创建一个类似这样的验证函数但用于电子邮件：
// function validatePhone(phone) { return /^\d{10}$/.test(phone); }"
```

指定约束条件：

```
claude "将此代码重构为使用函数式编程，不使用循环，使用 map/filter/reduce"
```

包括边缘案例：

```
gemini -p "实现用户认证，处理以下情况：过期令牌、并发登录、速率限制"
```

### 步骤 18：构建 AI 辅助开发流水线

#### 自动化代码审查流水线

1.  使用 Copilot 进行预提交检查：

```
// .copilot-instructions
"审核所有更改以发现：安全问题、性能问题、代码风格"
```

2.  使用 Claude 进行 PR 审查：

```
claude "审查此 PR：git diff main..feature-branch"
```

3.  使用 Gemini 完成文档：

```
gemini -p "为这些更改生成更新日志并更新 README"
```

#### 测试驱动的 AI 开发

1.  编写测试规范：

```
claude "为支付处理系统编写全面的测试规范"
```

2.  生成测试代码：

```
gemini -p "使用 Jest 实现这些测试规范"
```

3.  使用 Copilot 实现：
    
    -   使用代理模式实现功能
        
    -   测试指导实现过程
        

### 步骤 19：创建您的个人 AI 工作流

#### 设置您的环境

1\. VS Code 设置（`settings.json`）：

```
{
  "github.copilot.enable": {
    "*": true
  },
  "github.copilot.advanced": {
    "inlineCompletions.enable": true,
    "chat.enabled": true
  }
}
```

2\. Claude 代码配置（`~/.claude/settings.json`）：

```
{
  "cleanupPeriodDays": 7,
  "permissions": {
    "allow": [
      "Bash(fd:*)",
      "Bash(rg:*)",
      "Bash(ls:*)",
      "WebFetch(domain:github.com)",
      "WebFetch(domain:stackoverflow.com)"
    ],
    "deny": ["WebFetch(domain:medium.com)"]
  }
}
```

3\. Gemini 设置（`~/.gemini/config.json`）：

```
{
  "defaultModel": "gemini-2.5-pro",
  "contextWindow": "large",
  "safetyMode": "interactive"
}
```

#### 自定义命令和别名

为常见任务设置 Shell 别名：

```
# 启动互动会话
alias cc='claude'
alias gc='gemini'

# 快速一次性命令（在需要时）
alias aicommit='claude "创建一个带有描述性信息的 git 提交"'
alias aireview='claude "审查我的未提交更改"'
alias complexity='gemini -p "分析代码复杂性并建议简化"'
alias security='claude "更深入思考：检查安全漏洞"'
alias aidocs='gemini -p "生成全面的文档"'
```

### 最终项目：构建一个完整的应用程序与 AI

#### 项目要求

构建一个任务管理 API，具备：

-   用户认证
    
-   CRUD 操作
    
-   实时更新
    
-   测试套件
    
-   文档
    

#### 建议工作流程

阶段 1：互动规划

```
# 启动 Claude 代码会话
claude

Claude Code > 超级思考：设计一个可扩展的任务管理 API 架构

[Claude 提供详细分析]

Claude Code > 现在将其分解为实施阶段

# 切换到 Gemini 进行规格说明
gemini

Gemini > 为此任务管理 API 创建详细的技术规格

Gemini > 包括数据库架构和 API 端点规格
```

阶段 2：互动实现

1.  使用 Copilot 代理模式进行初始设置
    
2.  使用内联 Copilot 实现功能
    
3.  切换到互动 Claude 代码会话处理复杂逻辑：
    

```
Claude Code > 实施我们计划的用户认证系统

Claude Code > 现在添加任务 CRUD 操作

Claude Code > 用 WebSockets 集成实时更新
```

阶段 3：互动测试与文档

```
# Claude 代码会话用于测试
claude

Claude Code > 为所有 API 端点编写全面测试

Claude Code > 为身份验证流程添加集成测试

Claude Code > 为高负载场景创建性能测试

# Gemini 生成文档
gemini

Gemini > 生成包含示例的全面 API 文档

Gemini > 创建开发者入职指南
```

阶段 4：互动优化

```
# Claude 代码进行性能优化
claude

Claude Code > 分析并优化我们的数据库查询

Claude Code > 为经常访问的数据实现缓存

Claude Code > 添加监控和日志

# Gemini 进行最终审查
gemini

Gemini > 审查整个代码库以进行改进
```

```
### 测量你的进步

#### 阶段 1 里程碑

-   熟悉标签补全
    
-   可以编写有效的提示语
    
-   理解 AI 的局限性
    

#### 阶段 2 里程碑

-   有效使用多个模型
    
-   掌握聊天模式和代理
    
-   使用高级聊天功能
    

#### 阶段 3 里程碑

-   熟练使用 CLI 工具
    
-   可以将 VS Code 和终端工作流结合起来
    
-   了解工具的优势
    

#### 阶段 4 里程碑

-   创建自定义 AI 工作流
    
-   使用 AI 构建完整应用程序
    
-   可以教别人 AI 辅助开发
    

### 阶段 4 练习

#### 练习 1：工具选择精通

1.  选择一个中等复杂的编码任务（例如，“构建一个 URL 缩短服务 API”）
    
2.  计划每个阶段使用的工具（设计、编码、测试、部署）
    
3.  按照你选择的工作流程执行
    
4.  记录哪些工作顺利，哪些需要改进
    

#### 练习 2：创建自定义工作流

1.  识别工作中的重复性开发任务
    
2.  设计一个使用多个工具的 AI 辅助工作流
    
3.  测试并改进该工作流
    
4.  为团队同事创建相关文档
    

#### 练习 3：完整项目构建

1.  使用 AI 辅助构建一个小型但完整的应用程序
    
2.  战略性地使用至少两种不同 AI 工具
    
3.  包括测试、文档和部署
    
4.  反思比起传统开发的效率提升
    

### 继续你的旅程

#### 持续更新

-   关注工具的更新说明
    
-   加入 AI 编码社区
    
-   试验新功能
    

#### 探索高级主题

-   自定义 MCP 服务器开发
    
-   AI 模型微调
    
-   企业部署策略
    
-   团队协作模式
    

#### 持续学习资源

-   每个工具的官方文档
    
-   社区论坛和 Discord 服务器
    
-   开源 AI 编码项目
    
-   会议演讲和教程
    

## 常见 AI 问题

即便使用最好的 AI 工具，你仍会遇到挑战。一旦你了解模式，这些问题是正常且可管理的。以下是开发者面临的最常见问题以及实际可行的解决方案。

### “我的 AI 建议很糟！”

**问题：** AI 给出的建议无关或错误

**解决方案：**

-   写更清晰的注释
    
-   打开相关文件以提供上下文
    
-   从简单任务开始
    
-   确保你在正确的文件类型中
    

**示例修正：**

```
// 原先: "make function"
// 尝试: "create function to validate US phone number format (xxx) xxx-xxxx"
```

### “AI 太慢”

**问题：** 等待建议时间过长

**解决方案：**

-   检查你的网络连接
    
-   关闭不必要的程序
    
-   尝试较轻量级的 AI 工具
    
-   要有耐心 —— 复杂建议需要时间
    

### “我害怕过于依赖 AI”

**问题：** 担心丧失编码能力

**解决方案：**

-   将 AI 作为学习工具，而非依赖的工具
    
-   在接受之前务必理解代码
    
-   定期练习不使用 AI 编码
    
-   专注于解决问题，而非语法
    

### “它建议使用过时代码”

**问题：** AI 建议的模式老旧或使用了废弃的方法

**解决方案：**

-   在注释中指定版本
    
-   保持工具的更新
    
-   学会识别过时的模式
    

**示例：**

```
// create React functional component using hooks (not class component)
```

### 故障排除快速参考

#### 常见问题（所有工具）

| **问题** | **快捷修复** |
| --- | --- |
| 没有 AI 建议 | 检查网络连接，重启编辑器，验证登录 |
| “需要付款”消息 | 检查免费层限制，验证账户状态 |
| 建议效果差 | 使用更清晰的注释，打开相关文件提供上下文 |
| 工具无法安装 | 更新编辑器，检查网络，尝试不同安装方法 |

#### GitHub Copilot 问题

| **问题** | **解决方案** |
| --- | --- |
| VS Code 无建议 | 检查右下角“GitHub Copilot”状态 |
| 免费层过期 | 查看 [学生/维护者免费访问][21] |
| 代理模式不起作用 | 尝试 `Shift+Cmd+I`（Mac）或 `Ctrl+Shift+I`（Windows/Linux） |
| 聊天无响应 | 尝试重启 VS Code，检查网络连接 |

#### Claude Code 问题

| **问题** | **解决方案** |
| --- | --- |
| “命令未找到” | 重新安装：`npm uninstall -g @anthropic-ai/claude-code && npm install -g @anthropic-ai/claude-code` |
| 认证失败 | 运行 `claude auth login`，检查剩余 API 信用额度 |
| 响应缓慢 | 网络检查：`ping api.anthropic.com`，尝试轻量模型：`--model claude-3-haiku` |
| MCP 服务器不起作用 | 检查 `~/.claude/mcp.json` 语法，测试服务器：`npx @mcp/server-github --help` |
| 命令挂起/卡住 | 按 `Ctrl+C` 取消，重启终端，检查后台进程 |

#### Gemini CLI 问题

| **问题** | **解决方案** |
| --- | --- |
| 需要认证 | 运行 `gemini auth login`，检查 Google 账户权限 |
| 超过速率限制 | 检查使用情况：`gemini /stats`，等待 1 分钟或升级计划 |
| 无法安装 | 尝试 `npx @google/gemini-cli`，检查 Node.js 16+ |
| 图片上传失败 | 检查格式（JPG/PNG/WebP），大小小于 20MB，验证文件路径 |
| 上下文窗口错误 | 将大请求分割成小部分，清除历史记录 |
```

如果一切无效，请按顺序尝试以下方法：

1.  重启你的编辑器/终端
    
2.  检查互联网连接
    
3.  确认你已登录到正确的账户
    
4.  更新工具至最新版本
    
5.  尝试不同的工具（如果一个失败了，通常其他的会有效）
    
6.  询问 AI 自己：“帮助我排查 troubleshoottool_tool_setup”
    

## 完成所有阶段后有哪些后续步骤？

一旦你掌握了基础知识，这里有一些简单的后续步骤：

### 与团队合作

#### 团队 AI 工作流基础

**共享提示库：**

构建团队提示库可以改变你整个团队使用 AI 的方式。首先创建一个共享的仓库，让开发者记录适用于你特定领域和代码库的有效提示。

例如，如果你正在构建电子商务软件，为常见任务创建标准化提示，如“生成符合我们 REST 约定的产品目录 API 端点”或“使用我们的标准模式创建支付处理错误处理”。

记录成功的代理模式工作流供团队成员重复使用。某位开发者可能会发现 Claude Code 在给定有关模式演化实践的特定上下文时，特别适合数据库迁移。通过共享这些工作流，你可以避免每个团队成员独立地发现有效的方法。

**工具标准化：**

当每个人都使用兼容的 AI 工具时，团队生产力会倍增。根据团队的需要确定主要工具——例如，所有开发者都使用 GitHub Copilot，以确保一致的内联支持，再加上 Claude Code 以处理需要深入推理的复杂架构任务。制定清晰的指南，规定何时使用自主的代理模式与协作会话，以防止冲突并确保代码质量。

配置共享的 MCP 服务器设置，以便所有团队成员都能访问相同的增强型 AI 功能。这可能包括内部 API 的团队专用服务器、共享数据库访问或了解你的部署管道的定制工具。当每个人都拥有相同的 AI 功能时，协作就会变得顺畅无比。

**AI 生成的代码审查：**

将你的代码审查过程转变为与 AI 生成的代码有效协作。为在拉取请求中标记 AI 生成的部分建立惯例——这有助于审查者将注意力集中在适当的地方。审查者可以专注于架构决策、业务逻辑正确性和需要人工判断的集成模式，而不是挑剔 AI 通常处理得很好的语法。

对 AI 生成的代码实施严格的测试，因为自动化测试比手动审查更可靠地捕捉 AI 错误。为测试 AI 输出创建团队标准，包括 AI 可能遗漏的边缘案例和集成场景。这样你就能在享受 AI 带来的速度的同时，通过系统的验证保持质量。

**在提交信息中记录 AI 工具决策。**

#### 简单的团队设置

从小处着手，逐步建立：

-   首先让每个人使用相同的 AI 工具
    
-   创建一个适合你项目的提示共享文档
    
-   找出团队何时应该使用代理模式与常规协助模式
    
-   为你最重要的团队工具设置 MCP 服务器
    

### 对于更大的项目

随着项目的发展，你可能会想要：

-   为不同的任务尝试不同的 AI 模型（简单代码用快速的，复杂问题用强大的）
    
-   为你经常执行的任务创建快捷方式
    
-   将 AI 工具与现有开发工作流连接
    

### 持续学习

AI 编码工具每个月都在进步！通过以下方式保持最新：

-   关注工具的发行说明（他们会发送更新邮件）
    
-   加入 AI 编码的 Discord 社区
    
-   尝试新功能，因为它们不断出现
    

## 结论

恭喜你！你现在已经拥有了开启 AI 辅助编码之旅的所有必需品。记住，每个专家都曾是初学者，而有 AI 作为你的编码伙伴，你可以比以往更快地学习和成长。

**记住：**

-   AI 不会取代你的创造力——它会放大它
    
-   每个建议都是一次学习的机会
    
-   错误是旅程的一部分
    
-   社区在这里帮助你
    

你不仅仅是在学习用 AI 编码——你是在了解软件开发的未来。几个月后，你会想知道自己过去是如何在没有它的情况下编码的。今天拥抱 AI 辅助的开发者将成为明日的领袖。

快乐编码！🚀

[1]: #heading-essential-ai-terminology
[2]: #heading-when-to-use-ai-vs-when-to-code-yourself
[3]: #heading-prerequisites
[4]: #heading-your-complete-learning-journey
[5]: #heading-how-to-generate-your-first-ai-assisted-code-quick-start
[6]: #heading-stage-1-foundation-getting-started-with-ai-coding
[7]: #heading-stage-2-advanced-github-copilot-features
[8]: #heading-stage-3-cli-based-ai-agents-claude-code-amp-gemini
[9]: #heading-stage-4-mastery-combining-tools-and-advanced-workflows
[10]: #heading-common-ai-issues
[11]: #heading-whats-next-after-completing-all-stages
[12]: #heading-conclusion
[13]: http://code.visualstudio.com/
[14]: https://docs.github.com/en/copilot/how-tos/manage-your-account/getting-free-access-to-copilot-pro-as-a-student-teacher-or-maintainer
[15]: https://code.visualstudio.com/
[16]: http://github.com/
[17]: http://localhost:3333/#troubleshooting-quick-reference
[18]: http://localhost:3333/#troubleshooting-quick-reference
[19]: http://copilot-instructions.md/
[20]: http://localhost:3333/#troubleshooting-quick-reference
[21]: https://docs.github.com/en/copilot/how-tos/manage-your-account/getting-free-access-to-copilot-pro-as-a-student-teacher-or-maintainer

