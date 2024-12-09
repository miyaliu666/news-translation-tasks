---
title: 如何在 Laravel 应用中设置 Google 身份验证
date: 2024-12-09T07:04:24.268Z
author: Abhijeet Dave
authorURL: https://www.freecodecamp.org/news/author/Abhidave/
originalURL: https://www.freecodecamp.org/news/how-to-set-up-google-auth-in-laravel-apps/
posteditor: ""
proofreader: ""
---

在这个数字化世界中，为您的应用提供一个流畅且安全的身份验证流程是非常重要的。这有助于提升用户体验和应用的整体安全性。

<!-- more -->

Google 身份验证是用户使用他们的 Google 帐号登录网站时最受信任且方便的方法之一。这意味着他们不必记住其他的用户名和密码。

在您的 [Laravel][1] 应用中集成 Google OAuth 简化了登录过程，鼓励用户互动，并提高了您平台的信誉。在本教程中，我将指导您在 Laravel 应用中实现 Google 身份验证的步骤。我们将从设置 Google API 凭据到配置 Laravel 的 Socialite 包开始。

## 目录

-   [先决条件][2]
    
-   [在 Laravel 应用中使用 Google Auth 的好处][3]
    
-   [如何设置 Laravel Google 登录][4]
    
-   [总结][5]
    

### 先决条件

在您开始之前，请确保您具备以下先决条件：

1.  Laravel 11
    
2.  一个 Google 开发者账号。
    
3.  基本的 Laravel 和身份验证知识。
    
4.  用于管理包的 Composer
    

一旦您准备好这些先决条件，就可以开始在 Laravel 应用中集成 Google 身份验证。

### 在 Laravel 应用中使用 Google Auth 的好处

这种设置有许多好处。这里列举几个：

-   与 Socialite 的简化集成
    
-   无缝的用户身份验证
    
-   提升的安全性
    
-   可自定义的用户流程
    
-   提升的可扩展性
    
-   强大的生态系统支持
    
-   更易于维护
    

## 如何设置 Laravel Google 登录

无论您是在进行个人项目还是生产应用，按照以下步骤操作将帮助您顺利集成 Google 身份验证。让我们开始吧。

### 步骤1：设置 Google Cloud 项目

要在 Laravel 应用中使用 Google 身份验证，首先需要配置一个 Google Cloud 项目。按照以下步骤设置您的项目：

1.  访问 [Google Cloud 控制台][6] 并使用您的 Google 帐号登录。
    
    ![Laravel auth step 1](https://cdn.hashnode.com/res/hashnode/image/upload/v1733208476305/836ff373-152a-4b99-93f3-c7684591e5c7.png)
    
2.  点击顶部导航栏的 **“选择项目”** 下拉菜单。在弹出窗口中，点击 **“新建项目”** 来创建一个新项目并填写所需信息。然后点击 **创建项目**。
    
    ![Laravel auth create project](https://cdn.hashnode.com/res/hashnode/image/upload/v1733208488866/409092b9-20b5-4888-9c15-f22eff4226c8.png)
    
3.  创建项目后，打开控制台的左侧菜单并选择 **API 和服务 > 凭据**。
    
    ![APIs & Credentials](https://cdn.hashnode.com/res/hashnode/image/upload/v1733208517526/9b7df08c-8e8f-4db4-b297-a3f320edd0f0.png)
    
4.  在凭据页面，点击 **创建凭据** > **OAuth 客户端 ID**。
    
    ![OAuth Client ID](https://cdn.hashnode.com/res/hashnode/image/upload/v1733208549370/6dcd64f1-1fe4-4a8b-b37e-f74b38bd694a.png)
    
5.  如果这是您第一次创建客户端 ID，它将要求您配置同意屏幕。您可以通过点击 **配置同意屏幕** 进行配置。如果您已配置过同意屏幕，则可以跳过此步骤。
    
    -   如果您的应用是公开使用，请选择 **External**，如果只限于 Google Workspace 组织内用户使用，请选择 **Internal**。
        
        ![OAuth consent](https://cdn.hashnode.com/res/hashnode/image/upload/v1733208660871/53aaf9a2-74d4-4cca-b4e5-9c5cfa9b3066.png)
        
    -   填写所需的详细信息，如 **应用名称**、**用户支持邮箱** 及任何品牌信息。点击 **保存并继续**。
        
        ![OAuth Consent Screen](https://cdn.hashnode.com/res/hashnode/image/upload/v1733208766209/4f458336-9a2a-4238-af89-de954ed2bcf4.png)
        

配置完同意屏幕后，返回 **凭据** 页面并再次选择 **OAuth 客户端 ID**。

6.  选择 **应用类型** 为 **Web 应用** 并为客户端凭据提供一个名称（例如，Laravel Social Login）。
    
7.  在 **授权的重定向 URI** 下，添加您的应用的回调 URL：
    
    -   示例：`http://your-app-url.com/callback/google`
        
    -   如果您在本地测试，请使用：[http://127.0.0.1:8000/api/auth/google/callback][7]
        
        ![OAuth client ID](https://cdn.hashnode.com/res/hashnode/image/upload/v1733208840271/b7c309bc-4880-481b-acda-c0fecf4a0ee5.png)
        
8.  点击 **创建**，Google 将为您的项目生成一个 **客户端 ID** 和 **客户端密钥**。保存这些凭据，因为接下来的步骤将需要它们。
    


如果你还没有准备好项目，你可以使用以下命令创建一个新的 Laravel 项目：

```
composer create-project --prefer-dist laravel/laravel social-auth-example
```

要将 Google 身份验证集成到 Laravel 项目中，我们将使用 [Laravel Socialite][8]。Socialite 是一个由 Laravel 官方提供的包，它简化了与 Google、Facebook、Twitter 等热门服务的 OAuth 身份验证。

要安装 Socialite，请在 Laravel 项目的根目录中打开终端并运行以下命令：

```
composer require laravel/socialite
```

### 第三步：配置环境变量

在这一步中，我们将配置我们的 Laravel 应用以使用在第一步中收集的 Google OAuth 凭据。

在项目根目录中找到你的 `.env` 文件，并添加以下环境变量：

```
GOOGLE_CLIENT_ID=your-client-id
GOOGLE_CLIENT_SECRET=your-client-secret
GOOGLE_REDIRECT_URL=http://your-domain.com/auth/google/callback
```

继续替换所有占位符为你的密钥。

让我们逐一理解每个环境变量：

-   `GOOGLE_CLIENT_ID`：Google 为你的应用提供的唯一标识符。
-   `GOOGLE_CLIENT_SECRET`：用于安全地通过 Google 的 API 验证应用身份的私钥。
-   `GOOGLE_REDIRECT_URL`：用户登录后 Google 重定向用户的 URL。这应该与您在第 1 步创建凭据时指定的重定向 URI匹配。

### 第四步：更新配置文件

要使 Laravel Socialite 使用 Google OAuth 凭据，我们需要在 `config/services.php` 文件中配置提供方详细信息。

在 `services.php` 文件中，为 Google 提供方添加以下配置：

```
'google' => [
    'client_id' => env('GOOGLE_CLIENT_ID'),        // 你的 Google Client ID
    'client_secret' => env('GOOGLE_CLIENT_SECRET'), // 你的 Google Client Secret
    'redirect' => env('GOOGLE_REDIRECT_URL'),      // 你的 Google Redirect URL
]
```

### 第五步：创建控制器和路由以进行身份验证

在这一步中，我们将创建一个控制器来处理 Google OAuth 重定向和回调，并设置触发这些方法的必要路由。

运行以下 Artisan 命令以生成 `GoogleAuthController` 控制器：

```
php artisan make:controller GoogleAuthController
```

这将在 `app/Http/Controllers/GoogleAuthController.php` 处创建一个控制器。

将新创建的 `GoogleAuthController.php` 文件内容替换为以下代码：

```
<?php

namespace App\Http\Controllers;

use App\Http\Controllers\Controller;
use App\Models\User;
use Laravel\Socialite\Facades\Socialite;
use Illuminate\Support\Facades\Auth;
use Illuminate\Support\Str;
use Throwable;

class GoogleAuthController extends Controller
{
    /**
     * 将用户重定向到 Google 的 OAuth 页面。
     */
    public function redirect()
    {
        return Socialite::driver('google')->redirect();
    }

    /**
     * 处理来自 Google 的回调。
     */
    public function callback()
    {
        try {
            // 从 Google 获取用户信息
            $user = Socialite::driver('google')->user();
        } catch (Throwable $e) {
            return redirect('/')->with('error', 'Google 身份验证失败。');
        }

        // 检查用户是否已存在于数据库中
        $existingUser = User::where('email', $user->email)->first();

        if ($existingUser) {
            // 如果用户已存在，则登录
            Auth::login($existingUser);
        } else {
            // 否则，创建一个新用户并登录
            $newUser = User::updateOrCreate([
                'email' => $user->email
            ], [
                'name' => $user->name,
                'password' => bcrypt(Str::random(16)), // 设置一个随机密码
                'email_verified_at' => now()
            ]);
            Auth::login($newUser);
        }

        // 将用户重定向到仪表盘或其他安全页面
        return redirect('/dashboard');
    }
}
```

该控制器包含两个函数：

1.  重定向：将用户重定向到 Google 的 OAuth 页面。
2.  回调：处理来自 Google 的回调并将用户重定向到仪表盘或其他安全页面。

让我们在 `routes/web.php` 文件中定义 `redirect` 和 `callback` 路由：

```
use App\Http\Controllers\GoogleAuthController;

// 路由到 Google 的 OAuth 页面
Route::get('/auth/google/redirect', [GoogleAuthController::class, 'redirect'])->name('auth.google.redirect');

// 处理来自 Google 的回调的路由
Route::get('/auth/google/callback', [GoogleAuthController::class, 'callback'])->name('auth.google.callback');
```

### 第六步：在项目中测试 Laravel Google 身份验证

我们已经设置了 Google 身份验证，现在是时候测试它以确保它能无缝工作。在这一步中，我们将使用一个登录按钮，将用户重定向到 Google 的身份验证页面，并在成功登录后将他们返回到受保护的路由。

首先，我们将添加以下按钮，给予用户使用 Google 登录的选项：

为了测试目的，我定义了一个受保护的路由和一个`dashboard`。这个路由只对经过身份验证的用户开放。在用户登录后，我们将把他们重定向到这个页面。让我们在`web.php`中定义这个路由：

```
Route::get('/dashboard', function () {
    return view('dashboard');
})->middleware('auth')->name('dashboard');
```

接下来，在`resources/views/dashboard.blade.php`中为仪表板创建一个Blade视图文件。以下是仪表板的内容：

```
<html>
    <head>
        <title>Dashboard</title>
    </head>

    <body>
        <h1>Dashboard</h1>
        <p>欢迎来到仪表板，{{ auth()->user()->name }}！</p>
    </body>

</html>
```

在这里，我们使用`auth()->user()`辅助函数来显示登录用户的名称，该名称是从他们用于登录的Google账户中获取的。

现在，让我们尝试登录。

这是登录页面：

![登录界面 - "使用 Google 登录"](https://cdn.hashnode.com/res/hashnode/image/upload/v1732703980184/ecc90d0d-142b-43fe-8457-1b84a54f62d3.png)

点击按钮将重定向到Google的许可屏幕：

![Laravel Google认证示例 - 许可屏幕](https://cdn.hashnode.com/res/hashnode/image/upload/v1732703936372/59f4a5bf-907d-4dda-ba8b-6909cf0a4376.png)

点击继续，您应该会登录到应用中。您将被重定向到如下屏幕。您可以看到欢迎信息和用户的名称。

![Laravel认证示例 - 欢迎屏幕](https://cdn.hashnode.com/res/hashnode/image/upload/v1732703886846/17ab7939-34c1-4d82-9527-99afb78cf3eb.png)

就是这样！您已在Laravel项目中成功实现并测试了Google认证。现在，您的用户可以使用他们的Google帐户登陆，增强了安全性和便利性。

如需参考完整实现，您可以在GitHub上找到该项目的完整源代码：[**Laravel Google登录集成** - GitHub 仓库][9]

## 结论

您现在已经在Laravel应用程序中使用Socialite设置了Google认证！您可以通过在`config/services.php`文件中添加其他配置来扩展此方法以包括其他OAuth提供商，如Facebook、Twitter或GitHub。

Google OAuth集成是现代Web应用程序的常见功能，而Laravel Socialite使其易于实现。

如果您需要更多社交登录选项，如GitHub、Twitter和Facebook，您可以考虑使用现成的Laravel SaaS样板。

大多数预构建的Laravel SaaS样板提供与流行平台（如Google、GitHub、Facebook和Twitter）的无缝集成。例如，有一些高级和开源的资源如：

-   [Laravel启动套件][10] （高级）
    
    -   基于Tailwind CSS
        
    -   提供一键魔法链接设置
        
    -   支持多种身份验证方法，包括传统的电子邮件/密码登录
        
    -   2FA身份验证
        
-   [SaaS样板][11] （开源）
    
    -   单一数据库多租户
        
    -   开发者面板
        
    -   管理个人访问令牌
        
-   [Laranuxt][12] （开源）
    
    -   Nuxt UI是由NuxtJS团队构建的组件集合，由Tailwind CSS提供支持
        
    -   用于帮助用户会话和登录/注销的认证库
        
    -   认证中间件示例
        
-   [Laravel Vue样板][13] （开源）
    
    -   使用Laravel Echo和Pusher的WebSockets。
        
    -   用于更好PWA开发的Workbox。
        
    -   Laravel GraphQL
        

使用这些Laravel SaaS样板之一可以加快您的工作流程，因为您无需从头开始设置一切。

特别感谢经验丰富的Laravel开发人员和爱好者[Deep Kumbhare][14]，他帮助我准备了这篇文章。

希望这篇文章能帮助您设置Laravel的Google登录。

[1]: https://laravel.com/
[2]: #heading-prerequisites
[3]: #heading-benefits-of-using-google-auth-in-a-laravel-app
[4]: #heading-how-to-set-up-laravel-google-login
[5]: #heading-conclusion
[6]: https://console.cloud.google.com/
[7]: http://127.0.0.1:8000/api/auth/google/callback
[8]: https://laravel.com/docs/11.x/socialite
[9]: https://github.com/DeepKumbhare85/social-auth-example
[10]: https://demos.themeselection.com/jetship-laravel-starter-kit/
[11]: https://github.com/miracuthbert/saas-boilerplate
[12]: https://github.com/fumeapp/laranuxt
[13]: https://github.com/alefesouza/laravel-vue-boilerplate
[14]: https://github.com/DeepKumbhare85

