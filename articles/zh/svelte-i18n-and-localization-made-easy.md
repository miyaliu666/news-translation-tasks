```markdown 
register('en', () => import('./locales/en.json')); 
register('es', () => import('./locales/es.json')); 
 
init({ 
  fallbackLocale: 'en', 
  initialLocale: navigator.language.split('-')[0] || 'en' 
}); 
``` 
 
这段代码注册了语言文件，并初始化了 Svelte i18n 库。`fallbackLocale` 设置为英语，以防找不到用户的语言。 
 
接下来，在 `App.svelte` 中引入并使用翻译。 
 
```markdown 
<!-- src/App.svelte --> 
<script> 
  import { _, locale } from 'svelte-i18n'; 
  import Welcome from './Welcome.svelte'; 
 
  let username = 'Alex'; 
</script> 
 
<select bind:value={locale}> 
  <option value="en">English</option> 
  <option value="es">Español</option> 
</select> 
 
<Welcome {username}/> 
``` 
 
此代码利用语言选择器组件来切换应用程序界面语言，并在欢迎组件中传递 `username`。 
 
## 更新组件以使用翻译 
 
在 `Welcome.svelte` 组件中，利用翻译功能显示不同语言的欢迎信息。 
 
```markdown 
<!-- src/Welcome.svelte --> 
<script> 
  import { _ } from 'svelte-i18n'; 
  export let username; 
</script> 
 
<div>{$_('hello', { username })}</div> 
``` 
 
使用 `$_('hello', { username })` 函数从翻译文件中获取对应的翻译文本，并用实际的用户名替换占位符。 
 
## 创建语言切换器 
 
我们通过在主应用组件中使用 `<select>` 元素，绑定 `locale` 变量达到这一效果。当用户选择新语言时，`locale` 的值会更新，这将驱动翻译的重新渲染。 
 
## 添加高级功能 
 
在本节中，我们将介绍进一步本地化应用所需的高级特性，比如格式化数字和日期、处理多种形式的复数以及处理缺失的翻译。 
 
## 格式化数字和货币 
 
利用 `svelte-i18n` 中的 `format` 函数，我们可以轻松地根据当前语言环境格式化数字和货币。 
 
```markdown 
// 示例代码 
import { format } from 'svelte-i18n'; 
 
let value = 1234567.89; 
 
// 检查当前语言环境 
$: formattedValue = format(value, { style: 'currency', currency: 'USD' }); 
``` 
 
根据当前选定的语言文化自动格式化提供的数字为带有货币符号的格式。 
 
## 结束语 
 
通过这些方法，您可以将 Svelte 应用程序轻松扩展至支持多语言，确保全球用户的良好体验。 
``` 
 
这段翻译保留了原始 Markdown 的格式和结构。 
 
```markdown 
init({ 
  fallbackLocale: 'en', 
  initialLocale: 'en', 
}); 
``` 
 
我们注册了我们的翻译文件，并将英语设置为初始语言和备用语言。当所选语言中缺少翻译时，将使用备用语言。 
 
## 更新组件以使用翻译 
 
现在我们可以更新欢迎组件以使用 JSON 文件中的文本： 
 
```markdown 
<!-- src/Welcome.svelte --> 
<script> 
  import {  } from 'svelte-i18n'; 
  export let username; 
</script> 
 
<div>{$('hello', { username })}</div> 
``` 
 
`$_` 函数是来自 svelte-i18n 的一个特殊帮助器，用于检索翻译的字符串。 
 
当我们传递 {username} 作为第二个参数时，它会将我们的翻译字符串中的占位符替换为实际的用户名。 
 
## 创建语言切换器 
 
那么如何更改语言呢？让我们创建一个简单的语言选择组件： 
 
```markdown 
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
 
**bind:value** 指令会在用户进行选择时自动更新活动语言。 
 
来自 svelte-i18n 的 locale 存储处理了所有切换语言的幕后工作。 
 
现在让我们在我们的主 **App** 组件中把所有东西结合在一起： 
 
```markdown 
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
 
`waitLocale` 函数确保在显示内容之前加载翻译。这可以防止应用程序首次加载时出现闪烁或缺少翻译的情况。 
 
## 添加高级功能 
 
随着应用程序的发展，您需要处理更复杂的场景。我们来看看一些常见的需求。 
 
### 格式化数字和货币 
 
不同国家对待数字和货币的方式截然不同。例如，如果您向来自法国和美国的人展示十万的数字，您需要以不同的方式显示相同的数字。 
 
法国 → 100 000,00 $ 
 
美国 → $100,000.00 
 
您看到在法国，千位用空格分隔，而小数点用逗号表示了吗？使用美国数字格式会让法国人感到非常困惑。下面是一些其他的例子。 
 
- 美国使用小数点来表示小数 (1,234.56) 
 
- 许多欧洲国家使用逗号表示小数，使用点号表示千位 (1.234,56) 
 
- 有些国家的数字分组方式不同（如印度的 1,23,456） 
 
```javascript 
// src/lib/formatUtils.js 
export function formatCurrency(amount, locale, currency = 'USD') { 
  return new Intl.NumberFormat(locale, { 
    style: 'currency', 
    currency 
  }).format(amount); 
} 
``` 
 
您现在可以在组件中使用这些格式化器： 
 
```markdown 
<!-- src/lib/PriceDisplay.svelte --> 
<script> 
  import { formatCurrency } from './lib/formatUtils'; 
  let price = 1234.56; 
  let locale = 'en'; 
</script> 
 
<p>Price: {formatCurrency(price, locale, 'USD')}</p> 
``` 
 
通过此设置，应用程序现在根据您请求的输出格式适应不同语言环境的货币格式： 
 
- 美国: "$1,234.56", "1.2M", "15%" 
 
- 德国: "1.234,56 €", "1,2 Mio.", "15 %" 
 
### 格式化日期 
 
与货币和数字格式类似，不同国家/地区对日期的格式也是不同的。 
 
因此，在美国使用 MM/DD/YYYY，许多欧洲国家使用 DD/MM/YYYY，而日本通常使用 YYYY年MM月DD日。 
 
幸运的是，我们不需要手动处理此问题。与货币格式化类似，我们有 `Intl.DateTimeFormat` 函数，它接受语言环境和数字格式的日期，并返回适当地格式化的日期。 
 
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
 
您现在可以在 Svelte 应用程序中使用此函数来为每个本地化正确显示日期： 
 
```markdown 
<!-- src/lib/DateDisplay.svelte --> 
<script> 
  import { formatDate } from './lib/dateUtils'; 
  let locale = 'en'; 
  let today = new Date(); 
</script> 
 
<p>Today's date: {formatDate(today, locale)}</p> 
``` 
 
一旦实现，您将看到日期自动格式化，如下所示： 
 
- 英语 (美国): "September 23, 2024" 
 
- 西班牙语: "23 de septiembre de 2024" 
 
- 德语: "23. September 2024" 
 
- 日语: "2024年9月23日" 
 
### 本地化图片 
 
不要忘记，本地化不仅仅是翻译文本，图像也需要关注。具有文化相关性的视觉内容、特定区域的内容（如地图或符号）和可适应的图像格式可以产生重大影响。 
 
#### **动态图像路径** 
 
使用 Svelte 的响应性功能根据当前语言环境动态加载图像。例如，您可以将本地化图片路径存储在 JSON 文件中或直接存储在您的 i18n 配置中： 
``` 
 
然后，在你的 Svelte 组件中： 
 
``` 
<script> 
  import { locale } from 'svelte-i18n'; 
  import translations from './translations.json'; 
 
  $: imagePath = translations[$locale].logo; 
</script> 
 
<img src={imagePath} alt="Localized logo" /> 
``` 
 
#### **集成替代文本本地化** 
 
确保你的图片的 alt 属性也进行本地化。你可以通过在翻译中添加一个额外的字段来实现这一点： 
 
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
 
如果你需要本地化 SVG 中的内容，例如文本或图标，或者希望将其[转换为 SVG][20]格式，可以考虑使用 Svelte 的模板以实现无缝集成。 
 
``` 
<svg> 
  <text x="10" y="20">{$t('svgText')}</text> 
</svg> 
``` 
 
这种方法确保了 SVG 能够直接渲染包含本地化文本。 
 
### 处理多种形式的复数 
 
虽然许多语言有两种复数形式（单数和复数），但也有许多语言有多于两种形式，还有一些语言不进行复数化。例如： 
 
- 阿拉伯语有六种形式 
 
- 日语不以相同的方式进行复数化 
 
我们可以轻松地使用 svelte-i18n 的 ICU 消息语法来处理这些差异。我还添加了一个例子，说明了如何在使用阿拉伯语时处理多于两种形式的情况。 
 
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
 
现在，让我们为 Svelte 应用创建一个复数化组件，其中一个按钮每次点击时都会增加计数。 
 
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
 
实现后，你的应用程序就可以动态处理复数化（通过点击 **Add item** 按钮可以注意到这一点）。 
 
### 处理缺失的翻译 
 
有时，某些键的翻译可能缺失。这可能是由于疏忽、动态内容或翻译文件不完整所致。为了优雅地处理这种情况，使用 **svelte-i18n** 中的 **missingKeyHandler** 选项设置错误处理。 
 
``` 
import { register, init } from 'svelte-i18n'; 
 
register('en', () => import('./locales/en.json')); 
register('es', () => import('./locales/es.json')); 
 
init({ 
  fallbackLocale: 'en', 
  initialLocale: 'en', 
  missingKeyHandler: (locale, key) => { 
    console.warn(`Missing translation: ${key} (${locale})`); 
    return key; // 在缺失翻译时显示键本身 
  } 
}); 
``` 
 
这段代码会在翻译缺失时记录警告，并显示键而不是不显示任何东西。 
 
## 让应用准备好投入生产 
 
在准备面向现实世界用户的应用程序时，通过自动检测用户的语言对其进行本地化优化。 
 
可以使用浏览器的 **navigator.language** 属性来设置初始语言环境： 
 
``` 
import { init, getLocaleFromNavigator } from 'svelte-i18n'; 
 
init({ 
  fallbackLocale: 'en', 
  initialLocale: getLocaleFromNavigator(), 
}); 
``` 
 
## 拓展本地化的最佳实践 
 
- **组织翻译**：逻辑地将相关翻译组合在一起（例如，按钮、菜单、通知），并对键使用一致的命名模式。 
 
- **使用翻译管理平台**：随着应用程序的发展，手动处理翻译文件将变得繁琐。[翻译管理平台][21]专为解决这一问题而设计，能每天节省数小时，使项目协作变得容易，并跟踪进度。 
 
- **彻底测试**：在所有支持的语言中使用真实用户测试应用程序。还需要考虑文本长度变化，以避免布局问题，尤其是德语或阿拉伯语，这些语言可以显著扩展文本。 
 
- **文化考虑**：使应用符合文化规范、阅读方向（例如，阿拉伯语的 RTL）以及区域偏好（例如，日期和货币格式）。 
 
## 总结 
 
虽然一开始本地化可能感觉像是一项大任务，但它是非常值得的。它让你能接触到更多的人，并使应用程序更具包容性。 
 
Svelte 和 svelte-i18n 简化了这个过程，并在构建技能的同时保持了它的乐趣。 
 
为简化起见，可以从基础知识入手，例如添加翻译和语言切换器。随着信心增加，可以依次实现处理日期、货币和复数化等高级功能。 
 
慢慢来，彻底测试，构建一个对所有用户都感觉自然的应用！ 
 
 