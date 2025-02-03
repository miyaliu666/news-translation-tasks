```markdown
---
title: "TypeScript 中的异步编程：Promises、Async/Await 与回调"
date: 2025-02-03T13:40:14.449Z
author: Isaiah Clifford Opoku
authorURL: https://www.freecodecamp.org/news/author/Clifftech/
originalURL: https://www.freecodecamp.org/news/learn-async-programming-in-typescript-promises-asyncawait-and-callbacks/
posteditor: ""
proofreader: ""
---

异步编程是一种允许您编写 `异步` 运行代码的编程范式。与按顺序执行代码的同步编程相对，异步编程允许代码在程序的其余部分继续执行的同时在后台运行。这对于完成可能需要较长时间的任务特别有用，例如从远程 API 获取数据。

<!-- more -->

`异步编程` 对于创建响应迅速且高效的 JavaScript 应用程序至关重要。TypeScript 作为 JavaScript 的超集，使处理异步编程变得更加容易。

在 TypeScript 中有几种方法可以进行 `异步编程`，包括使用 `promises`、`async/await` 和 `callbacks`。我们将详细介绍每种方法，以便您可以根据您的使用场景选择最佳的方法。

## 目录

1.  [为什么异步编程很重要？][1]
    
2.  [TypeScript 如何简化异步编程][2]
    
3.  [如何在 TypeScript 中使用 Promises][3]
    
    -   [如何创建一个 Promise][4]
        
    -   [如何链接 Promise][5]
        
4.  [如何在 TypeScript 中使用 Async / Await][6]
    
5.  [如何在 TypeScript 中使用回调函数][7]
    
6.  [总结][8]
    

## 为什么异步编程很重要？

异步编程对于构建响应迅速和高效的 Web 应用程序至关重要。它允许任务在后台运行的同时程序的其余部分继续运行，从而保持用户界面对输入的响应。此外，异步编程可以通过让多个任务同时运行来提升整体性能。

有许多异步编程的实际例子，例如访问用户相机和麦克风，以及处理用户输入事件。即使您不常创建异步函数，了解如何正确使用它们也很重要，以确保您的应用程序可靠且性能良好。

### TypeScript 如何简化异步编程

TypeScript 提供了几种简化异步编程的功能，包括 `类型安全`、`类型推断`、`类型检查` 和 `类型注释`。

通过类型安全，您可以确保即使在处理异步函数时，您的代码也能按预期工作。例如，TypeScript 可以在编译时捕获与 null 和 undefined 值相关的错误，从而为您节省调试时间和精力。

TypeScript 的类型推断和检查还减少了您需要编写的样板代码，使代码更简洁且易于阅读。

TypeScript 的类型注释为您的代码提供了清晰的文档，这在处理可能复杂的异步函数时尤其有用。

现在让我们深入学习异步编程的三个关键特性：promises, async/await 和 回调。

## 如何在 TypeScript 中使用 Promises

**Promises** 是处理 TypeScript 异步操作的强大工具。例如，您可以使用 promise 从外部 API 获取数据或在后台执行耗时任务，而主线程继续运行。

要使用 Promise，您需要创建一个 `Promise` 类的新实例，并传递一个执行异步操作的函数。当操作成功时，该函数应调用 resolve 方法并传入结果；如果失败，则调用 reject 方法并传入错误。

一旦创建了 Promise，您可以使用 `then` 方法为其附加回调。这些回调会在 Promise 被兑现时触发，并将 resolved 的值作为参数传递。如果 Promise 被拒绝，您可以使用 catch 方法附加错误处理，该方法会收到拒绝的原因。

使用 Promises 相对于传统的基于回调的方法有几个优点。例如，Promises 可以帮助防止“回调地狱”，即异步代码中常见的嵌套回调难以阅读和维护的问题。

Promises 还使异步代码中的错误处理变得更加简单，因为您可以使用 catch 方法来处理 Promise 链中任何地方发生的错误。

最后，Promises 通过提供一致、可组合的方式处理异步操作，无论其底层实现如何，简化了您的代码。

### 如何创建一个 Promise

Promise 语法：

```typescript
const myPromise = new Promise((resolve, reject) => {
  // 执行一些异步操作
  // 如果操作成功，调用 resolve 方法传入结果
  // 如果操作失败，调用 reject 方法传入错误对象
});

myPromise
  .then((result) => {
    // 处理成功的结果
  })
  .catch((error) => {
    // 处理错误
  });
```

```typescript
// 创建一个 promise 的示例 1

function myAsyncFunction(): Promise<string> {
  return new Promise<string>((resolve, reject) => {
    // 一些异步操作
    setTimeout(() => {
      // 成功操作，解决 promise
      const success = true;
```

```markdown
// 使用 promise
myAsyncFunction()
  .then((result) => {
    console.log(result); // 输出 : 结果是成功的，你的操作结果是 4
  })
  .catch((error) => {
    console.error(error); // 输出 : 结果是失败的，你的操作结果是 404
  });
```

在上面的例子中，我们有一个名为 `myAsyncFunction()` 的函数，它返回一个 `promise`。我们使用 `Promise` 构造器来创建 promise，它接受一个带有 `resolve` 和 `reject` 参数的 `回调函数`。如果异步操作成功，我们调用 resolve 函数。如果操作失败，我们调用 reject 函数。

由构造器返回的 promise 对象有一个 `then()` 方法，它接受成功和失败的回调函数。如果 promise 成功解决，成功的回调函数将使用结果调用。如果 promise 被拒绝，将以错误信息调用失败的回调函数。

promise 对象还有一个 `catch()` 方法，用于处理 promise 链中发生的错误。`catch()` 方法接受一个回调函数，如果在 promise 链中发生任何错误，将调用该函数。

现在，让我们继续了解如何在 TypeScript 中串联 promise。

### 如何链式调用 Promise

链式调用 promise 允许你按顺序或并行执行 `多个异步操作`。这在您需要一个接一个地执行多个异步任务或同时执行时非常有用。例如，您可能需要异步获取数据，然后异步处理它。

让我们来看看如何链式调用 promise 的例子：

```markdown
// 链式调用 promise 的示例
// 第一个 promise
const promise1 = new Promise((resolve, reject) => {
  const functionOne: string = "这是第一个 promise 函数";
  setTimeout(() => {
    resolve(functionOne);
  }, 1000);
});

// 第二个 promise
const promise2 = (data: number) => {
  const functionTwo: string = "这是第二个 promise 函数";
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve(` ${data}  '+'  ${functionTwo} `);
    }, 1000);
  });
};

// 将第一个和第二个 promise 链式调用在一起
promise1
  .then(promise2)
  .then((result) => {
    console.log(result); // 输出: 这是第一个 promise 函数 + 这是第二个 promise 函数
  })
  .catch((error) => {
    console.error(error);
  });
```

在上面的例子中，我们有两个 promise：`promise1` 和 `promise2`。`promise1` 在 1 秒后解决，得到字符串 "这是第一个 promise 函数。"  `promise2` 接受一个数字作为输入，并返回一个 promise, 在 1 秒后解决，并组合输入数字和字符串 "这是第二个 promise 函数"

我们使用 `then` 方法链式调用两个 promise。`promise1` 的输出作为输入传递给 `promise2`。最后，我们再次使用 `then` 方法将 `promise2` 的输出记录到控制台。如果 `promise1` 或 `promise2` 中的任何一个被拒绝，则错误将由 `catch` 方法捕获。

恭喜！你已经学会了如何在 TypeScript 中创建和链式调用 promise。你现在可以使用 promise 在 TypeScript 中执行异步操作。现在，让我们探索 TypeScript 中的 `异步/等待` 是如何工作的。

## 如何在 TypeScript 中使用 Async / Await

**异步/等待** 是在 ES2017 中引入的一种语法，可以使对 Promise 的使用更简单。它允许您编写看上去像同步代码的异步代码。

在 TypeScript 中，您可以使用 `async` 关键字定义一个异步函数。它告诉编译器这个函数是异步的，并将返回一个 Promise。

现在，让我们看看如何在 TypeScript 中使用异步/等待。

异步/等待语法：

```markdown
// TypeScript 中的异步/等待语法
async function functionName(): Promise<ReturnType> {
  try {
    const result = await promise;
    // promise 解决后要执行的代码
    return result;
  } catch (error) {
    // 如果 promise 被拒绝，执行的代码
    throw error;
  }
}
```

在上面的例子中，`functionName` 是一个返回 `ReturnType` Promise 的异步函数。`await` 关键字用于等待 promise 解决后再移动到下一行代码。

`try/catch` 块用于处理在异步函数内部运行代码时发生的任何错误。如果发生错误，它将被 catch 块捕获，您可以在那里适当地处理它。

### **使用箭头函数和 Async / Await**

您还可以在 TypeScript 中使用带有异步/等待语法的箭头函数：

```markdown
const functionName = async (): Promise<ReturnType> => {
  try {
    const result = await promise;
    // promise 解决后要执行的代码
    return result;
  } catch (error) {
    // 如果 promise 被拒绝，执行的代码
    throw error;
  }
};
```

在上面的例子中，`functionName` 被定义为返回 `ReturnType` Promise 的箭头函数。async 关键字表示这是一个异步函数，而 await 关键字用于等待 promise 解决后再移动到下一行代码。

### **使用异步/等待进行 API 调用**

现在，让我们超越语法，使用异步/等待从 API 获取一些数据。

```markdown
interface User {
  id: number;
  name: string;
  email: string;
}

const fetchApi = async (): Promise<void> => {
  try {
    const response = await fetch("https://jsonplaceholder.typicode.com/users");

    if (!response.ok) {
      throw new Error(
        `获取用户失败 (HTTP 状态码: ${response.status})`
      );
    }

    const data: User[] = await response.json();
    console.log(data);
  } catch (error) {
    console.error(error);
    throw error;
  }
};
```

```
在这里，我们从 JSONPlaceholder API 获取数据，将其转换为 JSON，然后将其记录到控制台。这是一个如何在 TypeScript 中使用 async/await 的真实示例。

您应该在控制台中看到用户信息。此图像显示了输出结果：

![a1b865ea-0903-4749-a079-c8401be05787](https://cdn.hashnode.com/res/hashnode/image/upload/v1737554438217/a1b865ea-0903-4749-a079-c8401be05787.png)

### **使用 Axios API 调用的 Async/Await**

```
// 示例 2，如何在 TypeScript 中使用 async / await

const fetchApi = async (): Promise<void> => {
  try {
    const response = await axios.get(
      "https://jsonplaceholder.typicode.com/users"
    );
    const data = await response.data;
    console.log(data);
  } catch (error) {
    console.error(error);
  }
};

fetchApi();
```

在上面的示例中，我们使用 async/await 和 `Axios.get()` 方法定义了 `fetchApi()` 函数，以向指定的 URL 发出 HTTP GET 请求。我们使用 await 等待响应，然后使用响应对象的 data 属性提取数据。最后，我们使用 `console.log()` 将数据记录到控制台。任何发生的错误都会被捕获并使用 `console.error()` 记录到控制台。

我们可以使用 Axios 实现这一点，因此您应该在控制台中看到相同的结果。

此图像显示了在控制台中使用 Axios 时的输出结果：

![4f85a12d-6a9b-4eaa-9ab9-910a8a463dc6](https://cdn.hashnode.com/res/hashnode/image/upload/v1737554631796/4f85a12d-6a9b-4eaa-9ab9-910a8a463dc6.png)

注意：在您尝试上述代码之前，您需要使用 npm 或 yarn 安装 Axios。

```

npm install axios
```

```

yarn add axios
```

如果您不熟悉 Axios，您可以[在这里了解更多][9]。

您可以看到，我们使用了 `try` 和 `catch` 块来处理错误。`try` 和 `catch` 块是 TypeScript 中管理错误的方法。因此，无论何时进行像我们刚才这样的 API 调用，请确保使用 `try` 和 `catch` 块来处理任何错误。

现在，让我们探索在 TypeScript 中更高级的 `try` 和 `catch` 块用法：

```
// 示例 3，如何在 TypeScript 中使用 async / await

interface Recipe {
  id: number;
  name: string;
  ingredients: string[];
  instructions: string[];
  prepTimeMinutes: number;
  cookTimeMinutes: number;
  servings: number;
  difficulty: string;
  cuisine: string;
  caloriesPerServing: number;
  tags: string[];
  userId: number;
  image: string;
  rating: number;
  reviewCount: number;
  mealType: string[];
}

const fetchRecipes = async (): Promise<Recipe[] | string> => {
  const api = "https://dummyjson.com/recipes";
  try {
    const response = await fetch(api);

    if (!response.ok) {
      throw new Error(`Failed to fetch recipes: ${response.statusText}`);
    }

    const { recipes } = await response.json();
    return recipes; // 返回 recipes 数组
  } catch (error) {
    console.error("Error fetching recipes:", error);
    if (error instanceof Error) {
      return error.message;
    }
    return "发生了未知错误。";
  }
};

// 获取并记录菜谱
fetchRecipes().then((data) => {
  if (Array.isArray(data)) {
    console.log("Recipes fetched successfully:", data);
  } else {
    console.error("Error message:", data);
  }
});
```

在上面的示例中，我们定义了 `interface Recipe`，该接口概述了我们从 API 预期的数据结构。然后，我们使用 async/await 和 fetch() 方法创建了 `fetchRecipes()` 函数，以向指定的 API 端点发出 HTTP GET 请求。

我们使用 `try/catch` 块来处理在 API 请求期间可能发生的任何错误。如果请求成功，我们使用 await 从响应中提取数据属性并返回它。如果发生错误，我们检查错误消息并在存在时将其作为字符串返回。

最后，我们调用 `fetchRecipes()` 函数并使用 `.then()` 将返回的数据记录到控制台。此示例演示了如何在更高级的场景中使用 `async/await` 和 `try/catch` 块来处理错误，其中我们需要从响应对象中提取数据并返回自定义错误消息。

此图像显示了代码的输出结果：

![922592da-e9a6-4792-9d22-d5f8f8e84889](https://cdn.hashnode.com/res/hashnode/image/upload/v1737557515062/922592da-e9a6-4792-9d22-d5f8f8e84889.png)

### **Async / Await 与 Promise.all**

`Promise.all()` 是一种方法，它将一组 Promise 作为输入（可迭代）并返回一个单个 Promise 作为输出。当所有输入 Promise 已解决或如果输入可迭代对象不包含任何 Promise 时，此 Promise 将解决。如果任何输入 Promise 被拒绝，立即拒绝，或如果非 Promise 抛出错误，则将以第一个拒绝消息或错误进行拒绝。

```
// 使用 async / await 和 Promise.all 的示例
interface User {
  id: number;
  name: string;
  email: string;
  profilePicture: string;
}

interface Post {
  id: number;
  title: string;
  body: string;
}

interface Comment {
  id: number;
  postId: number;
  name: string;
  email: string;
  body: string;
}

const fetchApi = async <T>(url: string): Promise<T> => {
  try {
    const response = await fetch(url);
    if (response.ok) {
      const data = await response.json();
      return data;
    } else {
      throw new Error(`Network response was not ok for ${url}`);
    }
  } catch (error) {
    console.error(error);
    throw new Error(`Error fetching data from ${url}`);
  }
};
```

```
fetchAllApis()
  .then(([users, posts, comments]) => {
    console.log("Users: ", users);
    console.log("Posts: ", posts);
    console.log("Comments: ", comments);
  })
  .catch((error) => console.error(error));
```

在上面的代码中，我们使用 `Promise.all` 同时获取多个 API。如果你有多个 API 要获取，你可以使用 `Promise.all` 一次性获取它们。正如你所见，我们使用 `map` 遍历了 API 的数组，然后将其传递给 `Promise.all` 以同时获取它们。

下面的图片展示了 API 调用的输出：

![14bbecbb-7dad-464e-b412-028f56e9d679](https://cdn.hashnode.com/res/hashnode/image/upload/v1737560380441/14bbecbb-7dad-464e-b412-028f56e9d679.png)

让我们看看如何使用 `Promise.all` 和 Axios：

```
// 使用 async / await 结合 axios 和 Promise.all 的示例

const fetchApi = async () => {
  try {
    const urls = [
      "https://jsonplaceholder.typicode.com/users",
      "https://jsonplaceholder.typicode.com/posts",
    ];
    const responses = await Promise.all(urls.map((url) => axios.get(url)));
    const data = await Promise.all(responses.map((response) => response.data));
    console.log(data);
  } catch (error) {
    console.error(error);
  }
};

fetchApi();
```

在上面的示例中，我们使用 `Promise.all` 同时从两个不同的 URL 获取数据。首先，我们创建了一个 URL 的数组，然后使用 `map` 创建一个从 `axios.get` 调用生成的 Promise 的数组。我们将这个数组传递给 `Promise.all`，它返回一个响应的数组。最后，我们再次使用 `map` 从每个响应中获取数据并将其记录到控制台。

## 如何在 TypeScript 中使用回调

**回调** 是作为参数传递给另一个函数的函数。回调函数在另一个函数内执行。回调确保函数在任务完成之前不会运行，而是在任务完成之后立即运行。它们帮助我们编写异步 JavaScript 代码并防止问题和错误。

```
// 在 typescript 中使用回调的示例

const add = (a: number, b: number, callback: (result: number) => void) => {
  const result = a + b;
  callback(result);
};

add(10, 20, (result) => {
  console.log(result);
});
```

下面的图片展示了回调函数：

![80203145-d053-49b8-a160-a1d72ed17a7a](https://cdn.hashnode.com/res/hashnode/image/upload/v1737560660649/80203145-d053-49b8-a160-a1d72ed17a7a.png)

让我们看看在 TypeScript 中使用回调的另一个示例：

```
// 在 TypeScript 中使用回调函数的示例

type User = {
  name: string;
  email: string;
};

const fetchUserData = (
  id: number,
  callback: (error: Error | null, user: User | null) => void
) => {
  const api = `https://jsonplaceholder.typicode.com/users/${id}`;
  fetch(api)
    .then((response) => {
      if (response.ok) {
        return response.json();
      } else {
        throw new Error("Network response was not ok.");
      }
    })
    .then((data) => {
      const user: User = {
        name: data.name,
        email: data.email,
      };
      callback(null, user);
    })
    .catch((error) => {
      callback(error, null);
    });
};

// 使用回调函数调用 fetchUserData
fetchUserData(1, (error, user) => {
  if (error) {
    console.error(error);
  } else {
    console.log(user);
  }
});
```

在上面的示例中，我们有一个名为 `fetchUserData` 的函数，它接受 `id` 和一个 `callback` 作为参数。这个 `callback` 是一个包含两个参数的函数：一个错误和一个用户。

`fetchUserData` 函数从 JSONPlaceholder API 端点使用 `id` 检索用户数据。如果获取成功，它创建一个 `User` 对象并将其传递给回调函数，并且错误参数为 null。如果获取过程中出现错误，它将错误传递给回调函数，而用户参数为 null。

要使用带有回调的 `fetchUserData` 函数，我们提供一个 `id` 和一个回调函数作为参数。回调函数检查错误，如果没有错误就记录用户数据。

下面的图片展示了 API 调用的输出：

![2b37fa46-1ee4-4dee-8d50-82c09a235aec](https://cdn.hashnode.com/res/hashnode/image/upload/v1737560996613/2b37fa46-1ee4-4dee-8d50-82c09a235aec.png)

### 如何负责任地使用回调

虽然回调是 TypeScript 中异步编程的基础，但它们需要仔细管理以避免 **"回调地狱"**——即形成金字塔形状的、深度嵌套的代码，难以阅读和维护。以下是有效使用回调的方法：

1.  **避免深度嵌套**  
    
    -   通过将复杂操作分解成命名函数来扁平化代码结构
        
    -   对于复杂的异步工作流，使用 Promise 或 async/await（下文将详细介绍）
        
2.  **优先进行错误处理**  
    
    -   始终遵循 Node.js 的 `(error, result)` 参数约定
        
    -   在每一级嵌套回调中检查错误
        
```
    function processData(input: string, callback: (err: Error | null, result?: string) => void) {
      // ... 总是先用错误调用回调
    }
```


```
    type ApiCallback = (error: Error | null, data?: ApiResponse) => void;
```

4.  **考虑使用控制流库**  
    对于复杂的异步操作，使用像 `async.js` 这样的工具：

    -   并行执行
        
    -   顺序执行
        
    -   错误处理管道
        

### 何时使用回调与替代方案

有时候回调是一个很好的选择，而有时候则不是。

当你处理异步操作（单次完成）、与期望回调的旧库或API对接、处理事件监听（如点击监听或WebSocket事件）或创建具有简单异步需求的轻量级工具时，回调会很有帮助。

在其他需要编写具有清晰异步流程的可维护代码的场景中，回调就成了麻烦，你应该更倾向于使用 Promise 或 async-await。例如，当你需要链式多个操作、处理复杂的错误传播、使用现代API（如 Fetch API 或 FS Promises），或使用 `promise.all()` 进行并行执行时。

**从回调迁移到 Promise 的示例：**

```
// 回调版本
function fetchUser(id: number, callback: (err: Error | null, user?: User) => void) {
  // ... 
}

// Promise 版本
async function fetchUserAsync(id: number): Promise<User> {
  // ...
}

// 使用 async/await
try {
  const user = await fetchUserAsync(1);
} catch (error) {
  // 处理错误
}
```

### 异步模式的演变

| 模式 | 优点 | 缺点 |
| --- | --- | --- |
| 回调 | 简单、通用 | 嵌套复杂性 |
| Promise | 可链式调用，更好的错误流 | 需要 .then() 链 |
| Async/Await | 类同步可读性 | 需要转译 |

现代的 TypeScript 项目通常会混合使用：对于事件驱动的模式使用回调，而对复杂的异步逻辑则使用 Promise/async-await。关键是选择适合你具体使用场景的正确工具，同时保持代码清晰。

## 结论

在本文中，我们学习了如何在 TypeScript 中处理异步代码。我们了解了回调、Promise、async/await 以及如何在 TypeScript 中使用它们。我们还学习了这一概念。

如果你想了解更多关于编程的信息并成为更好的软件工程师，可以订阅我的 YouTube 频道 [CliffTech][10]。

感谢阅读我的文章。希望你喜欢它。如果你有任何问题，可以随时联系我。

与我在社交媒体上联系：

-   [Twitter][11]
    
-   [Github][12]
    
-   [Linkedin][13]
    

[1]: #heading-why-is-async-programming-important
[2]: #heading-how-typescript-makes-async-programming-easier
[3]: #heading-how-to-use-promises-in-typescript
[4]: #heading-how-to-create-a-promise
[5]: #heading-how-to-chain-promises
[6]: #heading-how-to-use-async-await-in-typescript
[7]: #heading-how-to-use-callbacks-in-typescript
[8]: #heading-conclusion
[9]: https://www.npmjs.com/package/axios
[10]: https://www.youtube.com/@CliffTech/videos
[11]: https://twitter.com/Clifftech_Dev
[12]: https://github.com/Clifftech123
[13]: https://www.linkedin.com/in/isaiah-clifford-opoku-a506a51b2/
```

