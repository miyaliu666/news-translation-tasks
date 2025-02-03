---
title: 如何在 React 中构建依赖下拉菜单
date: 2025-02-03T05:36:36.722Z
author: Timothy Olanrewaju
authorURL: https://www.freecodecamp.org/news/author/SmoothTech/
originalURL: https://www.freecodecamp.org/news/how-to-build-dependent-dropdowns-in-react/
posteditor: ""
proofreader: ""
---

在许多 Web 应用程序中，我们经常遇到这样的表单：在一个下拉菜单中选择一个选项会解锁另一个下拉菜单中的新选项。这些相互关联的下拉菜单，通常被称为依赖下拉菜单或级联下拉菜单，在创建无缝且直观的表单填写体验中起着关键作用。

<!-- more -->

无论是选择一个国家以显示相应的州，还是选择一个产品类别以显示特定项目，这些下拉菜单都可以简化复杂的选择。对于开发者而言，实现依赖下拉菜单是一个将逻辑、可用性和动态数据处理相结合的实用挑战。

在本教程中，你将学习如何在你的 React 应用中实现这种类型的下拉菜单。

## 目录

-   [什么是依赖下拉菜单？][1]
    
-   [依赖下拉菜单是如何工作的？][2]
    
-   [在 React 中创建依赖下拉菜单的步骤][3]
    
    -   [步骤 1：设置 React 项目][4]
        
    -   [步骤 2：构建组件结构][5]
        
    -   [步骤 3：使用组件][6]
        
-   [处理动态数据（API 请求）][7]
    
-   [结论][8]
    

## **什么是依赖下拉菜单？**

依赖下拉菜单是一种 UI 元素，其中一个下拉菜单中的可用选项由另一个下拉菜单中的选项决定。例如，考虑如下场景：

1.  国家下拉菜单：用户选择一个国家。
    
2.  城市下拉菜单：根据选择的国家，第二个下拉菜单中可用城市的列表会相应过滤。
    

这种交互对于需要复杂、上下文敏感的数据输入的表单来说至关重要。

## **依赖下拉菜单是如何工作的？**

依赖下拉菜单通过根据第一个下拉菜单中选择的值动态更新第二个下拉菜单的选项来工作。这种动态更改通常通过以下方式实现：

1.  **监听用户输入：** 当用户在第一个下拉菜单中选择一个选项时，通常由 onChange 触发的事件会调用一个函数来更新状态。
    
2.  **获取新数据：** 这个更新的状态可以用来过滤现有数据或进行 API 调用以获取新的选项列表。
    
3.  **渲染新数据：** 然后使用新选项更新第二个下拉菜单，为用户提供相关的选择。
    

## **在 React 中创建依赖下拉菜单的步骤**

### **步骤 1：设置 React 项目**

如果你是 React 新手并希望一起学习，[查看 Vite 文档][9]并按照步骤创建你的 React 项目。完成后，再回来继续构建。

如果你已经有一个想使用的 React 项目，那就太好了。

### **步骤 2：构建组件结构**

为了简单起见，我们假设正在构建一个两级依赖的下拉菜单，第一个下拉菜单让你选择国家，第二个下拉菜单根据选定的国家显示城市。

此外，在国家下拉菜单中，我们将有另一个选项用于输入不包括在国家选项中的国家名称。用户可以继续在文本输入中输入他们的国家。

首先，创建一个名为 `DependentDropdown.js` 或 `DependentDropdown.jsx` 的新文件。在这个文件中，定义一个名为 `DependentDropdown` 的函数组件。

现在我们将通过以下步骤构建我们的依赖下拉菜单：

**声明用于存储数据的变量**

我们需要为国家和城市的值创建静态数据：

```javascript
  // 静态国家数据
  const countries = [
    { id: 1, name: 'USA' },
    { id: 2, name: 'Canada' },
    { id: 3, name: 'Other' },
  ];

  // 对应国家的静态城市数据
  const cities = {
    USA: ['New York', 'Los Angeles', 'Chicago'],
    Canada: ['Toronto', 'Vancouver', 'Montreal'],
  };
```

-   `countries` 是一个对象数组。每个对象都有 `id` 和 `name` 属性。
    
-   `cities` 是一个对象，以国家名称作为键，城市数组作为值。
    

**声明状态变量**

对于每次选择国家或城市，我们都希望能够跟踪所选的值。我们还希望能够在选择国家后填充城市选项。为此，我们需要声明一些状态。

如果你对状态的概念不熟悉，可以阅读我关于状态的文章[这里][10]。

```javascript
  const [selectedCountry, setSelectedCountry] = useState('');
  const [availableCities, setAvailableCities] = useState([]);
  const [selectedCity, setSelectedCity] = useState('');
  const [otherCountry, setOtherCountry] = useState('');
```

-   `selectedCountry` 状态被声明，初始值设为空字符串。
    
-   `availableCities` 状态被声明，初始值设为空数组。
    
-   `selectedCity` 状态被声明，初始值设为空字符串。
    
-   `otherCountry` 状态被声明，初始值设为空字符串。
    

在下拉菜单中进行选择的过程中，我们希望执行一些操作。事件处理程序使我们能够在事件发生时（在本例中是 `onChange` 事件）做到这一点。

```javascript
  const handleCountryChange = (e) => {
    const country = e.target.value;
    setSelectedCountry(country);
    setAvailableCities(cities[country] || []);
    setSelectedCity(''); 
    if (country !== 'Other') {
      setOtherCountry('');
    }
  };
```

以下是 `handleCountryChange` 函数中发生的事情：

-   获取下拉菜单中选定选项的值（所选国家）。
    
-   `setSelectedCountry` 用新选择的国家更新状态变量（selectedCountry）。
    
-   `cities[country]` 从 `cities` 对象中查找选定国家的城市列表。
    
    -   如果找到 `cities[country]`，则将该城市列表设置为可用城市。
        
    -   如果未找到选定国家的城市（`cities[country]` 未定义），`|| []` 确保使用空数组 (`[]`) 作为回退，以防止在尝试显示城市时出现错误。
        
-   当用户更改国家选择时，`setSelectedCity` 函数将 `selectedCity` 重置为空字符串。
    
-   如果选择的国家不是 “Other”，则 `otherCountry` 状态被重置为空字符串。这确保如果用户之前在 "Other" 输入中键入了内容，那么一旦他们选择其他国家（例如 “USA” 或 “Canada”），这些文本将被清除。
    

对于 “Other” 国家选择，我们只需要跟踪输入中输入的值。`setOtherCountry` 函数更新输入的值。以下是实现方法：

```javascript
  const handleOtherCountryChange = (e) => {
    setOtherCountry(e.target.value);
  };
```

对于城市的变化，我们不需要做太多工作，因为所选国家决定了显示哪些城市。我们只需要将 `selectedCity` 更新为下拉菜单中所选选项的值，即所选择的城市。

在 React 中，更新函数负责更新状态变量，因此在这种情况下，`setSelectedCity` 处理这个问题。

`handleCityChange` 函数如下：

```javascript
  const handleCityChange = (e) => {
    setSelectedCity(e.target.value);
  };
```

**返回 JSX**

`DependentDropdown` 组件渲染三个主要元素：国家下拉菜单、城市下拉菜单和国家文本输入。

HTML 中的下拉菜单是 `<select>` 和 `<option>` 元素的组合。为了跟踪元素的值，我们将状态变量附加到它们，以便我们可以控制它们。这样做叫做 '控制元素'，而这些元素在 React 中被称为 '受控元素'。

为了控制国家 `<select>` 元素，我们给它一个 `selectedCountry` 的 `value` 属性，并附加 `handleCountryChange` 函数。

```javascript
     <label htmlFor="country" className='font-bold'>Select Country: </label>
      <select id="country" value={selectedCountry} onChange={handleCountryChange}>
        <option value="">Select a country</option>
        {countries.map((country) => (
          <option key={country.id} value={country.name}>
            {country.name}
          </option>
        ))}
      </select>
```

还有，

-   在 `<option>` 里面，我们遍历 `countries` 数组，并动态为数组中的每个国家对象创建一个 `<option>`。
    
-   每个国家的 `name` 被显示为选项的文本。
    
-   每个选项的 `key` 设置为国家的 `id`，`value` 设置为国家的 `name`。
    
-   `key` 帮助 React 在重新渲染时有效地管理列表。
    

城市下拉菜单根据所选国家来有条件地渲染。如果选择了 "Other" 国家选项，则显示一个文本输入字段供用户指定国家。否则，如果选择了一个有效的国家，则显示一个具有相关选项的城市下拉菜单。

```javascript
{selectedCountry === 'Other' ? (
        <>
          <label htmlFor="other-country" className='font-bold'>Please specify the country: </label>
          <input
            id="other-country"
            type="text"
            value={otherCountry}
            onChange={handleOtherCountryChange}
            placeholder="Enter country name"
          />
        </>
      ) : (
        selectedCountry && (
          <>
            <label htmlFor="city" className='font-bold'>Select City: </label>
            <select id="city" value={selectedCity} onChange={handleCityChange}>
              <option value="">Select a city</option>
              {availableCities.map((city, index) => (
                <option key={index} value={city}>
                  {city}
                </option>
              ))}
            </select>
          </>
        )
      )
}
```

另外：

-   我们检查 `selectedCountry` 是否为 “Other” 选项，并显示一个文本输入。
    
-   文本输入具有 `otherCountry` 状态，并附加了 `handleOtherCountryChange` 处理程序函数。
    
-   我们使用 `value` 属性控制城市 `<select>` 元素，将其设置为 `selectedCity` 的状态变量。事件处理程序 `handleCityChange` 也被附加来处理 `onChange` 事件。
    
-   我们遍历 `availableCities` 数组，并动态为数组中的每个城市创建一个 `<option>`。
    
-   每个选项的 `key` 被设置为一个 `index`，`value` 被设置为 `city`。
    
-   每个城市被显示为选项的文本。
    

这里是所有代码的组合：

```javascript
import React, { useState } from 'react';

const DependentDropdown = () => {
  // 静态国家数据
  const countries = [
    { id: 1, name: '美国' },
    { id: 2, name: '加拿大' },
    { id: 3, name: '其他' },
  ];

  // 对应国家的静态城市数据
  const cities = {
    美国: ['纽约', '洛杉矶', '芝加哥'],
    加拿大: ['多伦多', '温哥华', '蒙特利尔'],
  };

  // 保存选定国家、城市和其他国家文本的状态
  const [selectedCountry, setSelectedCountry] = useState('');
  const [availableCities, setAvailableCities] = useState([]);
  const [selectedCity, setSelectedCity] = useState('');
  const [otherCountry, setOtherCountry] = useState(''); 

  // 处理国家变化
  const handleCountryChange = (e) => {
    const country = e.target.value;
    setSelectedCountry(country);
    setAvailableCities(cities[country] || []);
    setSelectedCity(''); 
    if (country !== '其他') {
      setOtherCountry('');
    }
  };

  // 处理城市变化
  const handleCityChange = (e) => {
    setSelectedCity(e.target.value);
  };

  // 处理其他国家输入变化
  const handleOtherCountryChange = (e) => {
    setOtherCountry(e.target.value);
  };

  return (
    <div className='text-center text-3xl'>
      <h1 className='font-extrabold text-5xl p-10'>依赖下拉菜单示例</h1>

      {/* 国家下拉菜单 */}
      <label htmlFor="country" className='font-bold'>选择国家: </label>
      <select id="country" value={selectedCountry} onChange={handleCountryChange}>
        <option value="">选择一个国家</option>
        {countries.map((country) => (
          <option key={country.id} value={country.name}>
            {country.name}
          </option>
        ))}
      </select>

      {/* 城市或其他国家输入 */}
      {selectedCountry === '其他' ? (
        <>
          <label htmlFor="other-country" className='font-bold'>请指定国家: </label>
          <input
            id="other-country"
            type="text"
            value={otherCountry}
            onChange={handleOtherCountryChange}
            placeholder="输入国家名称"
          />
        </>
      ) : (
        selectedCountry && (
          <>
            <label htmlFor="city" className='font-bold'>选择城市: </label>
            <select id="city" value={selectedCity} onChange={handleCityChange}>
              <option value="">选择一个城市</option>
              {availableCities.map((city, index) => (
                <option key={index} value={city}>
                  {city}
                </option>
              ))}
            </select>
          </>
        )
      )}
    </div>
  );
};

export default DependentDropdown;
```

### 第三步：使用组件

要得到最终结果，你需要将 `DependentDropdown` 组件导入到你的 `App.js` 或 `App.jsx` 中并将它放在 App 组件的返回部分。

```javascript
import DependentDropdown from './DependentDropdown'

function App() {

  return (
    <DependentDropdown/>
  )
}

export default App
```

不要忘记通过输入以下任一命令来运行应用程序：

```bash
npm start //用于 create react app
npm run dev //用于 react vite app
```

最后，你的浏览器上应该呈现如下所示的内容：

![38ff328c-09bd-4f74-b458-423ff1216e48](https://cdn.hashnode.com/res/hashnode/image/upload/v1737899898480/38ff328c-09bd-4f74-b458-423ff1216e48.gif)

## 处理动态数据（API 请求）

在现实世界的应用中，下拉列表的选项可能不是静态的，而是从一个 API 或作为 API 的 JSON 文件中获取的。

在这个例子中，我们将从一个 JSON 文件中读取数据来填充我们的依赖下拉菜单。这种做法有一些好处：

- **减少数据库负载：** 使用静态 JSON 文件（或预加载文件）可以减少为填充下拉菜单而需要的数据库查询次数。特别是在下拉选项相对静态且不经常变更的情况下。
  
- **更快的 UI 渲染：** 由于数据已经在客户端，无需每次用户与下拉菜单交互时都请求服务器。这可以使界面更加快速响应。
  

我们的 JSON 文件包含州和地方政府区域（Local Government Areas，LGA），这相当于国家和城市。

JSON 文件中的数据表示为对象数组，每个对象有 **state**（状态）、**alias**（别名）和 **lgas**（地方政府区域）键。'lgas' 键包含一个数组。

如下所示：

```json
[
  {
    "state": "Adamawa",
    "alias": "adamawa",
    "lgas": [
      "Demsa",
      "Fufure",
      "Toungo",
      "Yola North",
      "Yola South"
    ]
  },
  {
    "state": "Akwa Ibom",
    "alias": "akwa_ibom",
    "lgas": [
      "Abak",
      "Uruan",
      "Urue-Offong/Oruko",
      "Uyo"
    ]
  }
  //其余对象
]
```

从 API 创建动态依赖下拉菜单的方法与前面的例子没有太大区别，只需进行一些小的修改。

```
// 导入 React 及其钩子
import React, { useEffect, useState } from "react";

function DependentDropdown() {
// 声明全局状态变量
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);

// 使用 useEffect 钩子获取数据
  useEffect(() => {
    fetch("nigeria-state-and-lgas.json") // JSON文件作为URL
      .then((res) => res.json())
      .then((data) => {
        setData(data);
        setLoading(false);
      })
      .catch((error) => {
        console.error("获取数据出错:", error);
        setLoading(false);
      });
  }, []);
  return loading ? <div>加载中...</div> : <Form data={data} />;

}
// 表单接收数据作为属性
function Form({ data }) {

// 声明本地状态变量
  const [selectedState, setSelectedState] = useState("");
  const [selectedLga, setSelectedLga] = useState("");
  const [showList, setShowList] = useState(false);
  let sortedData = data.slice().sort((a, b) => a.state.localeCompare(b.state));
  const selectedData = sortedData.find((item) => item.state === selectedState);

// 状态的事件处理函数
  function handleClickState(e) {
    setSelectedState(e.target.value);
    setShowList(true);
  }
// LGA的事件处理函数
  function handleClickLga(e) {
    setSelectedLga(e.target.value);
  }

  return (
    <div>
  <form onSubmit={handleFormSubmit}>
    <div>
      {/* 名字 */}
      <div>
        <label htmlFor="firstName">名字</label>
        <input type="text"
          id="firstName"
          name="firstName"
          placeholder="请输入你的名字"/>
      </div>

      {/* 姓氏 */}
      <div>
        <label htmlFor="lastName">
          姓氏
        </label>
        <input
          type="text"
          id="lastName"
          name="lastName"
          placeholder="请输入你的姓氏"/>
      </div>
    </div>

    <div>
      <div>
        <select value={selectedState} onChange={handleClickState} name="state">
          <option value="" disabled>请选择你的州</option>
          {sortedData.map((data) => (
            <option key={data.alias} value={data.state}>
              {data.state}
            </option>
          ))}
        </select>
      </div>
      {selectedData && showList && (
        <select value={selectedLga} onChange={handleClickLga} name="lga">
          <option value="" disabled>{`请选择你所在的 ${selectedState} 区`}</option>
          {selectedData.lgas.map((lgass) => (
            <option key={lgass} value={lgass}>
              {lgass}
            </option>
          ))}
        </select>
      )}

    </div>
    <div>
        <button type="submit">
          提交
        </button>
      </div>
  </form>
</div>
  );
}

export default DependentDropdown;
```

这里的关键修改是使用 `useEffect` 钩子获取数据，它在初始渲染时只获取州和LGA数据。

以下是它在浏览器上的渲染效果：

![ada964c4-a2a4-4012-869f-c0bbf53761a7](https://cdn.hashnode.com/res/hashnode/image/upload/v1737856956995/ada964c4-a2a4-4012-869f-c0bbf53761a7.gif)

## 结论

在这个教程中，你学会了如何在 React 中使用静态和动态数据创建依赖下拉菜单。现在你可以在你的 React 应用程序中使用这种类型的下拉菜单了。

如果你觉得这篇文章有帮助，可以在 [LinkedIn][11] 上与我联系，获取更多编程相关的文章和帖子。

下一篇再见！

[1]: #heading-what-is-a-dependent-dropdown
[2]: #heading-how-does-a-dependent-dropdown-work
[3]: #heading-steps-to-create-dependent-dropdowns-in-react
[4]: #heading-step-1-set-up-your-react-project
[5]: #heading-step-2-structure-the-component
[6]: #heading-step-3-use-the-component
[7]: #heading-handling-dynamic-data-api-requests
[8]: #heading-conclusion
[9]: https://vite.dev/guide/
[10]: https://www.freecodecamp.org/news/react-state-management/
[11]: https://linkedin.com/in/timothy-olanrewaju750
```

