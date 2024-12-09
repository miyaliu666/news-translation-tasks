```markdown
---
title: 如何编写易读代码 —— 开发者提示
date: 2024-12-09T06:36:36.982Z
author: Orim Dominic Adah
authorURL: https://www.freecodecamp.org/news/author/orimdominic/
originalURL: https://www.freecodecamp.org/news/how-to-write-code-thats-easy-to-read/
posteditor: ""
proofreader: ""
---

> 程序是为人类阅读而写的，并且只是顺带让计算机去执行。 - 唐纳德·克努斯

<!-- more -->

你是否听说过，程序员花更多的时间阅读代码而不是编写代码？我发现这往往是真的：作为一名开发者，你会花更多的时间阅读和思考代码，而不是实际写代码。

这意味着，无论你打算如何优化你的代码运行，它也重要的是要让它令人愉快且易于阅读。

在本文中，我们将查看一个示例函数：`createOrUpdateUserOnLogin`。它存在于一个遥远的 JavaScript 代码库中，迫切需要让其变得易于阅读。我们将查看 `createOrUpdateUserOnLogin`，突出其难以阅读之处及原因，并最终重构它以使其更易于阅读和理解。

该函数用 JavaScript 编写，并使用 [JSDoc][1] 来记录其参数。JavaScript 的知识不是必须的，因为函数中的逻辑将会详细解释。JSDoc 仅用于记录函数参数的含义。

## 有问题的函数

这个函数并不是虚构的。它是一个拥有超过一千用户的应用程序代码库中的真实函数。以下是它的代码：

```javascript
/**
 * @param {Object} dto
 * @param {string} dto.email
 * @param {string} dto.firstName
 * @param {string} dto.lastName
 * @param {string} [dto.photoUrl]
 * @param {'apple' | 'google'} [dto.loginProvider]
 * @param {string} [dto.appleId]
 * @returns {string} token - 访问令牌
 */
async function createOrUpdateUserOnLogin(dto) {
  let user;

  if (dto.loginProvider == "apple") {
    user = await findOneByAppleId(dto.appleId);
    if (user?.isDisabled) {
      throw new Error("Unable to login");
    }

    if (user && !user.verified) {
      user = await setUserAsVerified(user.email);
    }

    if (!user) {
      user = await findOneByEmail(dto.email);

      if (user && dto.appleId) {
        user = await updateUserAppleId(user, dto.appleId);
      }

      if (user && !user.verified) {
        user = await setUserAsVerified(user.email);
      }
    }
  } else {
    user = await findOneByEmail(dto.email);
    if (user?.isDisabled) {
      throw new Error("Unable to login");
    }

    if (user && !user.photoUrl && dto.photoUrl) {
      user.photoUrl = dto.photoUrl;
      user = await updateUserDetails(user._id, user);
    }

    if (user && !user.verified) {
      user = await setUserAsVerified(user.email);
    }
  }

  if (!user) {
    user = await this.usersService.create(loginProviderDto);
  }

  return await this.createToken(user);
}
```

通过阅读和研究代码，你或许能够看出它相当难以跟随。如果你在阅读这个函数后离开你的计算机休息一下，那么返回时你很可能不会记得它究竟做了什么。

但这不应该，也不应该是你在阅读好故事时的情况，无论其长度如何。你可以轻松地跟随并在听完后记住基本细节。

该函数在用户尝试登录或注册时执行。用户可以使用他们的谷歌账户或苹果账户进行身份验证，函数必须在成功尝试后返回访问令牌。

有些用户已禁用他们的账户。这些用户不允许成功进行身份验证。函数的逻辑还包括根据某些条件更新已经注册用户的数据。

该函数执行以下两件事情之一：

1.  为现有账户创建一个身份验证令牌，并在更新账户详细信息后返回该令牌，或
    
2.  如果不存在账户则创建一个，并返回一个身份验证令牌。
    

这违反了单一职责原则 —— 但修复这个问题是另一篇文章的挑战。

这里的目标是重构这个函数，让它如此易读，即使是非程序员也可以读懂并明白它的功能。更好的是，我们还希望他们在一段时间没接触后仍能记住它。

该函数经过充分测试，因此在重构时不用担心破坏任何功能。测试会报告任何破坏性更改。

### 是什么让这段代码难以阅读？

有几个因素让这段代码难以阅读。以下是主要的几个：

1.  **深度嵌套** (`if` 语句嵌套在 `if` 语句中) 使得跟踪代码执行过程中的变化变得困难。在 `createOrUpdateUserOnLogin` 的情况下，是嵌套条件。其他情况可能包括逻辑如在 `while` 循环内的 `if` 语句，嵌套在另一个 `if` 语句中。
    
    深度嵌套增加了阅读和理解代码的复杂性。其流动性不讨人喜欢，并且使编写测试更加复杂，因为你必须考虑嵌套代码块内的操作。
    
2.  **复杂条件** 如 `user && !user.photoUrl && dto.photoUrl` 包含很多逻辑，必须保留在你的短期记忆中，并在继续阅读时记住。
    
3.  **杂乱无章的流程** 使得你难以一眼看出函数在做什么。函数似乎做了很多事情，但实际上并没有。两次执行阻止禁用用户登录和三次更新用户验证状态。通过电子邮件查找用户也重复了两次。
```


在检查了该函数中使其难以阅读的问题后，您可能希望实施以下几个更改：

**首先处理失败情况**：考虑优先处理失败情况，并将其从内容中剔除，以便该函数可以专注于成功情形，从而顺畅地叙述代码逻辑。

这涉及在函数中尽早使用 `return` 语句或抛出错误，以阻止那些妨碍函数目标实现的操作。

**重新安排流程**：如果某些操作可以在其他操作之前进行，并且这样排列会使代码流更易记忆、更加愉快阅读，同时仍能实现函数目的，那么您应该据此重新安排。

**使用日常语法**：这涉及更新标识符并将复杂的条件压缩为易于记忆的标识符名称。日常语法易于阅读，因为它是熟悉且贴切的。

**避免嵌套代码块**：在心理调试或尝试理解代码时，跟踪嵌套代码块中标识符值的变化是困难的。这是因为随着每个嵌套条件，逻辑执行更新一个标识符的路径数至少增加 2 倍——如果有多个标识符要更新，情况会更糟。

这意味着您必须跟踪这些路径，这可能会迅速升级为精神记忆负担，当更新代码时可能导致心理负担和错误。

嵌套代码的视觉效果也不讨人喜欢，并且这使得编写测试比实际应该的复杂。

在使用上述指导原则重构代码后，我们得到以下片段（我已编号代码的不同部分以便于在下面的说明中参考）：

```javascript
async function updateUserOnLogin(dto) {
  let user = await findUserByEmail(dto.email); // 1
  if (!user) {
    user = await createUser(dto);
  }

  if (user.isDisabled) { // 2a
    throw new Error("Unable to login"); // 2b
  }

  const userIsNotVerified = Boolean(user.isVerified) == false // 3a
  if (userIsNotVerified) { // 3b
    await setUserAsVerified(user.email);
  }

  const shouldUpdateAppleId = dto.loginProvider == "apple" && dto.appleId // 4a
  if (shouldUpdateAppleId) { // 4b
    await setUserAppleId(user.email, dto.appleId);
  }

  const shouldUpdatePhotoUrl = !user.photoUrl && dto.photoUrl // 5a
  if (shouldUpdatePhotoUrl) { // 5b
    await updateUserDetails(user._id, { photoUrl: dto.photoUrl });
  }

  return await this.createToken(user);
}
```

好了，现在让我们看看为了使代码更愉快阅读，我们在这里具体做了什么。

### 1\. 重新安排流程

根据函数上方的 JSDoc 注释，`email` 是必需的参数字段。无论他们的登录提供者如何，现有账户都有一个电子邮箱地址。我们可以先通过 `email` 获取一个账户，并在不存在账户时选择创建一个新账户（代码部分 1）。通过这样做，失败情况得到了早期处理。

选择在开始时抛出错误（部分 2b）如果帐户被禁用，也是为了早期处理失败情况。这不会影响新账户，因为新账户默认不被禁用。

早期处理失败情境有助于我们更容易理解代码，因为我们可以自由地只考虑将会发生的事情，而不必跟踪之前的错误情境（例如在继续阅读时不必记住 `user` 对象是否有值（部分 5））。

重构后的代码也消除了嵌套条件，并且仍按预期工作。

### 2\. 使用日常语法

在尝试使代码读起来像日常语法时，我们使用了清晰且贴切的变量名称（见部分 2a、3、4、5）。以这种方式书写，即使是非程序员如产品经理也可以阅读代码并理解正在发生的事情。

日常语法读起来像伪代码—“如果用户未验证，则将用户设为已验证”和“如果应该更新苹果 ID，则更新苹果 ID”。

使用日常语法是使代码读起来像故事的关键。

## 结论

可读性高的代码有助于提高可维护性，从而提高软件的寿命。贡献者可以轻松阅读、理解和最终进行更新。像阅读一部写得很好的故事一样，阅读代码也可以是一种愉快的活动。

图片来源：[Storyset 的工作插图][2]

[1]: https://jsdoc.app/
[2]: https://storyset.com/work

