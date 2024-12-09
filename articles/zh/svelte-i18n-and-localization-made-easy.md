```markdown
---
title: Svelte i18n 和本地化轻松实现
date: 2024-12-09T05:43:32.864Z
author: Alex Tray
authorURL: https://www.freecodecamp.org/news/author/trayalex812/
originalURL: https://www.freecodecamp.org/news/svelte-i18n-and-localization-made-easy/
posteditor: ""
proofreader: ""
---

应用程序可以在全球范围内访问。这意味着世界上任何地方的人都可以下载你的应用。

<!-- more -->

因此，如果你想要满足世界各地的人们，你的应用就需要支持多种语言。

幸运的是，Svelte 很容易使用，并且它使本地化（l10n）和国际化（i18n）变得相当简单。

但它缺乏内置的 i18n 支持，因此你需要使用像 svelte-i18n 这样的库，或者其他可用的库。让我们创建一个简单的 Svelte 应用来演示如何实现本地化。

我们将创建一个可以用英文和西班牙文使用的欢迎屏幕，然后构建一些更高级的功能。

目录

-   [Svelte 的独特之处是什么？][1]
-   [如何本地化 Svelte 应用程序？][2]
-   [添加语言支持][3]
-   [更新组件以使用翻译][4]
-   [创建语言切换器][5]
-   [添加高级功能][6]
-   [格式化数字和货币][7]
-   [格式化日期][8]
-   [本地化图片][9]
-   [动态图片路径][10]

-   [整合替代文本本地化][11]
-   [动态切换 SVG 内容][12]
-   [处理多种形式的复数][13]
-   [处理缺失的翻译][14]
-   [让你的应用准备好投入生产][15]
-   [扩展本地化的最佳实践][16]
-   [总结][17]

## Svelte 的独特之处是什么？

与 [Vue i18n][18] 相似，Svelte 在构建过程中将代码转换为原生 JS。这意味着你的应用将以最小的代码发布，并提供出色的性能。

虽然像 React 和 Vue 这样的其他框架启动时间较长，但 Svelte 的编译过程改变了这一点。它创建了更小的捆绑包，应用默认运行更快。

Svelte 的响应式语法和轻量特性使其成为渴望高效、现代应用的开发者的绝佳选择。

它的简约性与 [**云开发语言**][19] 所需的简单性相匹配，确保了应用精简、可扩展，并为云环境优化。

这种简约性也意味着你可能并不总是拥有所有所需的内置功能。但几乎所有的限制都可以通过外部库解决。

## 如何本地化 Svelte 应用程序

现在让我们直接开始创建一个 Svelte 项目。我将使用 `npm create` 命令创建项目。逐一运行以下命令：

```
npm create svelte@latest freecodecamp-localization-demo
cd freecodecamp-localization-demo
npm install
npm install svelte-i18n
```

`npm create` 命令会提示你做出一些选择。以下是我选择的内容（但你可以根据项目需求进行调整）：

-   骨架项目：选择 **是**。
  
-   添加 TypeScript 支持：选择 **否**（或者如果你喜欢 TypeScript，可以选择 是）。
  
-   添加 ESLint 进行代码检查：选择 **是**。
  
-   添加 Prettier 进行代码格式化：选择 **是**。
  

现在我们已经设置好了项目，接下来创建一个欢迎组件，稍后我们将通过翻译进行增强。

在你的 src 目录中创建一个名为 Welcome.svelte 的新文件：

```
<!-- src/Welcome.svelte -->
<script>
  export let username;
</script>

<div>欢迎 {username}！</div>
```

![AD_4nXfcaW7fLzjbLa5ysYYu5FplhoGeVNmml9An_l0fcbEwQSxvg5mDbh3PY9b7etS6DESM7ZGdO_R5dqcYaDdgMaaQE2d7w1Q8eU4uAtIfKjHfm58buFwM-KLtJ_bv-x3XyqoYIOgfdQ?key=uXnvRGfJpBUiBb7CkgThtLro](https://lh7-rt.googleusercontent.com/docsz/AD_4nXfcaW7fLzjbLa5ysYYu5FplhoGeVNmml9An_l0fcbEwQSxvg5mDbh3PY9b7etS6DESM7ZGdO_R5dqcYaDdgMaaQE2d7w1Q8eU4uAtIfKjHfm58buFwM-KLtJ_bv-x3XyqoYIOgfdQ?key=uXnvRGfJpBUiBb7CkgThtLro)

此组件接受从 App.svelte 文件传递的 username 属性，并显示欢迎消息。

目前看起来很简单，但如果你的用户说不同的语言呢？让我们添加语言支持。

## 添加语言支持

为此，我们需要为每种语言创建翻译文件。首先，在你的 **src** 文件夹中创建一个名为 **locales** 的新目录。在 **locales** 文件夹中，创建两个 JSON 文件——用于英语的 _en.json_ 和用于西班牙语的 _es.json_。

```json
// src/locales/en.json
{
  "hello": "Hello {username}!",
  "buttons": {
    "save": "Save",
    "cancel": "Cancel"
  }
}

// src/locales/es.json  
{
  "hello": "¡Hola {username}!",
  "buttons": {
    "save": "Guardar",
    "cancel": "Cancelar"
  }
}
```

这些文件包含我们的翻译字符串。

注意我们如何将它们组织成嵌套结构——这有助于在应用增长时管理翻译。

{username} 和 {count} 占位符将在运行时被我们动态（或静态）传递的实际值替换。

接下来，我们需要告诉 Svelte 如何使用这些翻译。为此，我们需要一个 **i18n 配置**文件：

```javascript
// src/i18n.js
import { register, init } from 'svelte-i18n';
```

```md
init({
  fallbackLocale: 'en',
  initialLocale: 'en',
});
```

我们已注册了我们的翻译文件，并将英文设置为初始语言和备用语言。当选定语言中缺少翻译时，将使用备用语言。

## 更新组件以使用翻译

现在我们可以更新欢迎组件以使用 JSON 文件中的文本：

```
<!-- src/Welcome.svelte -->
<script>
  import {  } from 'svelte-i18n';
  export let username;
</script>

<div>{$('hello', { username })}</div>
```

`$_` 函数是 svelte-i18n 的一个特殊辅助函数，用于检索翻译字符串。

当我们传递 { username } 作为第二个参数时，它将用实际的用户名替换翻译字符串中的占位符。

## 创建语言切换器

那么，如何更改语言呢？让我们创建一个简单的语言选择组件：

```
<!-- src/LanguageSelect.svelte -->
<script>
  import { locale } from 'svelte-i18n';

  const languages = [
    { code: 'en', name: 'English' },
    { code: 'es', name: 'Español' }
  ];
</script>

<div>
  {#each languages as { code, name }}
    <button on:click={() => locale.set(code)}>{name}</button>
  {/each}
</div>
```

**bind:value** 指令会在用户选择时自动更新活跃语言。

svelte-i18n 提供的 locale 存储处理了切换语言的所有幕后工作。

现在让我们将所有内容整合到我们的主要 **App** 组件中：

```
<!-- src/App.svelte -->
<script>
  import { waitLocale } from 'svelte-i18n';
  import Welcome from './Welcome.svelte';
  import LanguageSelect from './LanguageSelect.svelte';

  const  username = 'developer';
</script>

{#await waitLocale()}
  <p>Loading...</p>
{:then}
  <main>
    <LanguageSelect />
    <Welcome {username} />
  </main>
{/await}
```

`waitLocale` 函数确保在显示内容之前加载翻译。这可以防止应用首次加载时的闪烁或翻译丢失。

## 添加高级功能

随着您的应用程序的发展，您需要处理更复杂的情况。让我们看看一些常见的需求。

### 格式化数字和货币

不同国家处理数字和货币的方式大相径庭。例如，如果要向法国和美国的人展示十万这一数字，您需要以不同的方式展示相同的数字。

法国 → 100 000,00 $

美国 → $100,000.00

您看到法国的千位用空格分隔，十进制位用逗号分隔了吗？使用美国的数字格式会让法国人感到困惑。以下是一些其他例子。

-   美国使用句点作为小数（1,234.56）

-   许多欧洲国家使用逗号作为小数和句点分隔千位（1.234,56）

-   一些国家的数字分组方式不同（如印度的 1,23,456）

```javascript
// src/lib/formatUtils.js
export function formatCurrency(amount, locale, currency = 'USD') {
  return new Intl.NumberFormat(locale, {
    style: 'currency',
    currency
  }).format(amount);
}
```

您现在可以在组件中使用这些格式化程序：

```
<!-- src/lib/PriceDisplay.svelte -->
<script>
  import { formatCurrency } from './lib/formatUtils';
  let price = 1234.56;
  let locale = 'en';
</script>

<p>Price: {formatCurrency(price, locale, 'USD')}</p>
```

通过此设置，应用程序根据您请求的输出将货币格式适应不同的区域设置：

-   美国: "$1,234.56", "1.2M", "15%"

-   德国: "1.234,56 €", "1,2 Mio.", "15 %"

### 格式化日期

与货币和数字格式类似，不同的区域也会以不同的方式格式化日期。

因此，虽然美国使用 MM/DD/YYYY 格式，但许多欧洲国家使用 DD/MM/YYYY 格式，日本常用 YYYY年MM月DD日。

幸运的是，我们不需要手动处理这些情况。类似于货币格式化，我们有 `Intl.DateTimeFormat` 函数，可以接受区域设置和数字格式日期并返回适当格式化的日期。

```javascript
// src/lib/dateUtils.js
export function formatDate(date, locale) {
  return new Intl.DateTimeFormat(locale, {
    year: 'numeric',
    month: 'long',
    day: 'numeric'
  }).format(date);
}
```

您现在可以在 Svelte 应用中使用此函数来为每个区域设置正确显示日期：

```
<!-- src/lib/DateDisplay.svelte -->
<script>
  import { formatDate } from './lib/dateUtils';
  let locale = 'en';
  let today = new Date();
</script>

<p>Today's date: {formatDate(today, locale)}</p>
```

一旦实现，您将看到日期会被自动格式化，如下所示：

-   英文（美国）："September 23, 2024"

-   西班牙语："23 de septiembre de 2024"

-   德语："23. September 2024"

-   日语："2024年9月23日"

### 本地化图片

别忘了本地化不仅仅是翻译文本——您的图片也需要注意。文化相关的视觉效果、区域特定的内容（如地图或符号）以及可调整的图片格式可能会产生重大影响。

#### **动态图片路径**

使用 Svelte 的响应性根据当前区域动态加载图片。例如，您可以将本地化图片路径存储在 JSON 文件中或直接在 i18n 配置中：
```

然后，在你的 Svelte 组件中:

```
<script>
  import { locale } from 'svelte-i18n';
  import translations from './translations.json';

  $: imagePath = translations[$locale].logo;
</script>

<img src={imagePath} alt="Localized logo" />
```

#### **集成替代文本本地化**

确保你的图片的 alt 属性也实现本地化。你可以通过在翻译文件中添加一个附加字段来实现：

```
{
  "en": { "logoAlt": "Company Logo" },
  "fr": { "logoAlt": "Logo de l'entreprise" }
}

然后动态绑定本地化的 alt 文本
```

```

<img src={imagePath} alt={translations[$locale].logoAlt} />
```

#### **动态切换 SVG 内容**

如果你需要本地化 SVG 内的内容，如文本或图标，或者想要[转换为 SVG][20] 格式，考虑使用 Svelte 的模板化功能实现无缝集成。

```
<svg>
  <text x="10" y="20">{$t('svgText')}</text>
</svg>
```

这种方法确保了你的 SVG 可以直接呈现出本地化文本。

### 处理多种形式的复数

虽然许多语言只有两种形式的复数（单数和复数），但也有很多语言有超过两种形式的复数，还有一些语言不使用复数。例如：

-   阿拉伯语有六种形式
    
-   日语则不以相同方式使用复数
    

我们可以使用 svelte-i18n 的 ICU 消息语法轻松处理这些差异。我还添加了一个示例，以展示如何在使用阿拉伯语时处理多于两种形式的复数。

```
// src/locales/en.json
{
  "items": {
    "count": "{count, plural, =0 {No items} one {1 item} other {{count} items}}"
  }
}

// src/locales/es.json

{
  "items": {
    "count": "{count, plural, =0 {Sin elementos} one {1 elemento} other {{count} elementos}}"
  }
}

// src/locales/ar.json
{
  "items": {
    "count": "{count, plural, =0 {لا عناصر} one {عنصر واحد} two {عنصران} few {# عناصر} many {# عنصر} other {# عنصر}}",
  }
}
```

现在，我们为 Svelte 应用创建一个负责复数处理的组件，当按钮被点击时，计数会增加。

```
<!-- src/lib/ItemCounter.svelte -->
<script>
  import {  } from 'svelte-i18n';
  let count = 0;
</script>

<p>{$('items.count', { count })}</p>
<button on:click={() => count++}>Add Item</button>
<button on:click={() => count--} disabled={count === 0}>Remove Item</button>
```

实现这些后，你的应用将完全准备好动态处理复数（你可以通过点击 **Add item** 按钮来注意到这一点）。

### 处理缺失的翻译

有时，某些键的翻译可能会缺失。这可能是由于忽略、动态内容或不完整的翻译文件导致的。为了优雅地处理这种情况，可以在 **svelte-i18n** 中使用 **missingKeyHandler** 选项设置错误处理。

```
import { register, init } from 'svelte-i18n';

register('en', () => import('./locales/en.json'));
register('es', () => import('./locales/es.json'));

init({
  fallbackLocale: 'en',
  initialLocale: 'en',
  missingKeyHandler: (locale, key) => {
    console.warn(`Missing translation: ${key} (${locale})`);
    return key; // 在缺少翻译时显示键本身
  }
});
```

该代码在缺少翻译时会记录一个警告，并显示键而不是显示空白。

## 让你的应用做好上线准备

当为实际用户准备应用时，通过自动检测用户的语言来优化本地化。

可以使用浏览器的 **navigator.language** 属性来设置初始语言环境：

```
import { init, getLocaleFromNavigator } from 'svelte-i18n';

init({
  fallbackLocale: 'en',
  initialLocale: getLocaleFromNavigator(),
});
```

## 扩展本地化的最佳实践

-   **组织翻译**：逻辑地分组相关翻译（例如，按钮、菜单、通知）并使用一致的键命名模式。
    
-   **使用翻译管理平台**：随着应用的增长，手动处理翻译文件变得繁琐。[翻译管理平台][21] 专为解决此类问题而设计，帮助节省时间，简化项目协作，以及跟踪进度。
    
-   **全面测试**：在所有支持的语言中用真实用户测试应用。还需要考虑文本长度变化以避免布局问题，尤其是在处理像德语或阿拉伯语这样会显著扩展文本的语言时。
    
-   **文化考量**：根据文化规范调整应用程序，例如阅读方向（例如阿拉伯语的 RTL）和地区偏好（例如日期和货币格式）。
    

## 总结

本地化一开始可能感觉是个大任务，但它绝对是值得的。它可以让你接触到更多的人，让你的应用更具包容性。

Svelte 和 svelte-i18n 简化了整个过程，让你在提升技能的同时，保持乐趣。

为了简单起见，先从基础开始，例如添加翻译和语言切换器。随着信心的提升，可以接触日期、货币和复数处理等高级功能。

慢慢来，认真测试，打造一个对所有用户都自然友好的应用！

