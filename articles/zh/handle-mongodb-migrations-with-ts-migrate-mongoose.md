```markdown
---
title: 如何使用 ts-migrate-mongoose 处理 MongoDB 迁移
date: 2024-12-09T07:47:41.957Z
author: Orim Dominic Adah
authorURL: https://www.freecodecamp.org/news/author/orimdominic/
originalURL: https://www.freecodecamp.org/news/handle-mongodb-migrations-with-ts-migrate-mongoose/
posteditor: ""
proofreader: ""
---

数据库迁移是对数据库进行的修改。这些修改可能包括更改表的架构，更新一组记录中的数据，填充数据或删除一系列记录。

<!-- more -->

数据库迁移通常在应用程序启动之前运行，并且对于同一数据库不能成功运行多次。数据库迁移工具会保存数据库中已运行迁移的历史记录，以便将来进行跟踪。

在本文中，你将学习如何在一个极简的 Node.js API 应用程序中设置和运行数据库迁移。我们将使用 [ts-migrate-mongoose][1] 和 npm 脚本在 MongoDB 数据库中创建迁移和填充数据。ts-migrate-mongoose 支持运行来自 TypeScript 代码和 CommonJS 代码的迁移脚本。

ts-migrate-mongoose 是一个针对使用 [mongoose][2] 作为对象数据映射器的 Node.js 项目的迁移框架。它提供了编写迁移脚本的模板，还提供了以编程方式和通过 CLI 运行脚本的配置。

## 目录

-   [如何设置项目][3]
    
-   [如何为项目配置 ts-migrate-mongoose][4]
    
-   [如何使用 ts-migrate-mongoose 填充用户数据][5]
    
-   [如何构建一个 API 端点来获取填充的数据][6]
    
-   [结论][7]
    

## 如何设置项目

要使用 ts-migrate-mongoose 进行数据库迁移，你需要具备以下条件：

1.  一个安装了 mongoose 作为依赖项的 Node.js 项目。
    
2.  连接到项目的 MongoDB 数据库。
    
3.  MongoDB Compass（可选 - 用于查看数据库中的更改）。
    

为了方便起见，我们创建了一个可从 [ts-migrate-mongoose-starter-repo][8] 克隆的启动仓库。克隆该仓库，填写环境变量并通过运行 `npm start` 命令启动应用程序。

使用浏览器或 Postman 等 API 客户端访问 [http://localhost:8000][9]，服务器将返回 "Hello there!" 文本，以显示启动应用程序按预期运行。

## 如何为项目配置 ts-migrate-mongoose

要为项目配置 ts-migrate-mongoose，请使用以下命令安装 ts-migrate-mongoose：

```
npm install ts-migrate-mongoose
```

ts-migrate-mongoose 允许使用 JSON 文件、TypeScript 文件、`.env` 文件或通过 CLI 进行配置。建议使用 `.env` 文件，因为配置内容可能包含数据库密码，不宜公开暴露。`.env` 文件通常通过 `.gitignore` 文件隐藏，因此更安全。该项目将使用 `.env` 文件进行 ts-migrate-mongoose 配置。

文件应包含以下键及其值：

-   `MIGRATE_MONGO_URI` - Mongo 数据库的 URI，与数据库 URL 相同。
    
-   `MIGRATE_MONGO_COLLECTION` - 用于保存迁移的集合（或表）的名称。默认值是 migrations，项目中使用的就是这个值。ts-migrate-mongoose 将迁移保存到 MongoDB。
    
-   `MIGRATE_MIGRATIONS_PATH` - 存储和读取迁移脚本的文件夹路径。默认值是 `./migrations`，项目中使用的就是这个值。
    

## 如何使用 ts-migrate-mongoose 填充用户数据

我们已经创建了一个项目并成功连接到一个 Mongo 数据库。此时，我们希望将用户数据填充到数据库中。我们需要：

1.  创建一个用户集合（或表）
    
2.  使用 ts-migrate-mongoose 创建一个迁移脚本以填充数据
    
3.  在应用程序启动之前，使用 ts-migrate-mongoose 运行迁移以将用户数据填充到数据库中
    

### 1\. 使用 Mongoose 创建用户集合

Mongoose 架构可以用于创建用户集合（或表）。用户文档（或记录）将具有以下字段（或列）：`email`、`favouriteEmoji` 和 `yearOfBirth`。

要为用户集合创建 Mongoose 架构，请在项目根目录下创建一个 `user.model.js` 文件，包含以下代码片段：

```
const mongoose = require("mongoose");

const userSchema = new mongoose.Schema(
  {
    email: {
      type: String,
      lowercase: true,
      required: true,
    },
    favouriteEmoji: {
      type: String,
      required: true,
    },
    yearOfBirth: {
      type: Number,
      required: true,
    },
  },
  {
    timestamps: true,
  }
);

module.exports.UserModel = mongoose.model("User", userSchema);
```

### 2\. 使用 ts-migrate-mongoose 创建迁移脚本

ts-migrate-mongoose 提供了可用于创建迁移脚本的 CLI 命令。

在项目的根目录下运行 `npx migrate create <name-of-script>` 将在 `MIGRATE_MIGRATIONS_PATH` 文件夹（在我们的案例中为 `./migrations`）中创建一个脚本。`<name-of-script>` 是我们希望创建的迁移脚本文件的名称。
```

```
npx migrate create seed-users
```

该命令将在 `./migrations` 文件夹中创建一个文件，文件名格式为 `<timestamp>-seed-users.ts`。该文件将包含以下代码片段：

```
// 在这里导入你的模型

export async function up (): Promise<void> {
  // 在这里编写迁移代码
}

export async function down (): Promise<void> {
  // 在这里编写迁移代码
}
```

`up` 函数用于运行迁移。如果需要，`down` 函数用于逆转 `up` 函数执行的操作。在我们的例子中，我们尝试将用户填充到数据库中。`up` 函数将包含将用户填充到数据库的代码，而 `down` 函数将包含删除 `up` 函数中创建的用户的代码。

如果使用 MongoDB Compass 检查数据库，迁移集合中将有一个看起来像这样的文档：

```
{
  "_id": ObjectId("6744740465519c3bd9c1a7d1"),
  "name": "seed-users",
  "state": "down",
  "createdAt": 2024-11-25T12:56:36.316+00:00,
  "updatedAt": 2024-11-25T12:56:36.316+00:00,
  "__v": 0
}
```

迁移文档的 `state` 字段设置为 `down`。运行成功后，它会变为 `up`。

你可以将 `./migrations/<timestamp>-seed-users.ts` 文件中的代码更新为下面片段中的代码：

```
require("dotenv").config() // 加载环境变量
const db = require("../db.js")
const { UserModel } = require("../user.model.js");

const seedUsers = [
  { email: "john@email.com", favouriteEmoji: "🏃", yearOfBirth: 1997 },
  { email: "jane@email.com", favouriteEmoji: "🍏", yearOfBirth: 1998 },
];

export async function up (): Promise<void> {
  await db.connect(process.env.MONGO_URI)
  await UserModel.create(seedUsers);
}

export async function down (): Promise<void> {
  await db.connect(process.env.MONGO_URI)
  await UserModel.delete({
    email: {
      $in: seedUsers.map((u) => u.email),
    },
  });
}
```

### 3\. 在应用程序启动前运行迁移

ts-migrate-mongoose 为我们提供了 CLI 命令来运行迁移脚本的 `up` 和 `down` 函数。

使用 `npx migrate up <name-of-script>` 我们可以运行特定脚本的 `up` 函数。使用 `npx migrate up` 我们可以运行数据库中 `state` 为 `down` 的所有脚本的 `up` 函数。

要在应用程序启动前运行迁移，我们使用 npm 脚本。带有 `pre` 前缀的 npm 脚本将在不带 `pre` 前缀的脚本之前运行。例如，如果有一个 `dev` 脚本和一个 `predev` 脚本，那么每当使用 `npm run dev` 运行 `dev` 脚本时，`predev` 脚本将自动在 `dev` 脚本之前运行。

我们将利用 npm 脚本的这个特性，将 ts-migrate-mongoose 命令放入 `prestart` 脚本中，以便迁移在 `start` 脚本之前运行。

更新 `package.json` 文件，使其包含运行项目中迁移脚本的 `up` 函数的 ts-migrate-mongoose 命令的 `prestart` 脚本。

```
  "scripts": {
    "prestart": "npx migrate up",
    "start": "node index.js"
  },
```

通过此设置，当执行 `npm run start` 来启动应用程序时，`prestart` 脚本将运行，以使用 ts-migrate-mongoose 执行迁移，并在应用程序启动之前为数据库填充数据。

在运行 `npm run start` 后，你应该看到类似以下代码段的内容：

```
同步文件系统迁移与数据库...
MongoDB 连接成功
up: 1732543529744-seed-users.ts 
所有迁移均成功完成

> ts-migrate-mongoose-starter-repo@1.0.0 start
> node index.js

MongoDB 连接成功                      
服务器监听端口 8000
```

查看仓库的 [seed-users][10] 分支，以了解本文中代码库在此时的当前状态。

## 如何构建一个 API 端点以获取填充的数据

我们可以构建一个 API 端点，以获取数据库中已填充的用户数据。在 `server.js` 文件中，将代码更新为以下片段中的内容：

```
const { UserModel } = require("./user.model.js")

module.exports = async function (req, res) {
  const users = await UserModel.find({}) // 获取数据库中的所有用户

  res.writeHead(200, { "Content-Type": "application/json" });
  return res.end(JSON.stringify({ // 返回获取的用户数据的 JSON 表示
    users: users.map((u) => ({
      email: u.email,
      favouriteEmoji: u.favouriteEmoji,
      yearOfBirth: u.yearOfBirth,
      createdAt: u.createdAt
    }))
  }, null, 2));
};
```

如果我们启动应用程序并使用 Postman 或浏览器访问 [http://localhost:8000][11]，我们将获得类似于下面的 JSON 响应：

```
{
  "users": [
    {
      "email": "john@email.com",
      "favouriteEmoji": "🏃",
      "yearOfBirth": 1997,
      "createdAt": "2024-11-25T14:18:55.416Z"
    },
    {
      "email": "jane@email.com",
      "favouriteEmoji": "🍏",
      "yearOfBirth": 1998,
      "createdAt": "2024-11-25T14:18:55.416Z"
    }
  ]
}
```

请注意，如果再次运行应用程序，迁移脚本将不再运行，因为迁移的 `state` 在成功运行后现在将为 `up`。

## 结论

在构建应用程序时，迁移非常有用，因为需要通过添加或移除列来为测试播种初始数据、播种管理用户、更新数据库架构以及一次更新多个记录的列值。

如果您使用 Mongoose 和 MongoDB，ts-migrate-mongoose 可以帮助提供一个框架来运行 Node.js 应用程序的迁移。

[1]: https://www.npmjs.com/package/ts-migrate-mongoose
[2]: https://www.npmjs.com/package/mongoose
[3]: #heading-how-to-set-up-the-project
[4]: #heading-how-to-configure-ts-migrate-mongoose-for-the-project
[5]: #heading-how-to-seed-user-data-with-ts-migrate-mongoose
[6]: #heading-how-to-build-an-api-endpoint-to-fetch-seeded-data
[7]: #heading-conclusion
[8]: https://github.com/orimdominic/ts-migrate-mongoose-starter-repo
[9]: http://localhost:8000
[10]: https://github.com/orimdominic/ts-migrate-mongoose-starter-repo/tree/seed-users
[11]: http://localhost:8000
[12]: https://github.com/orimdominic/ts-migrate-mongoose-starter-repo/tree/fetch-users

