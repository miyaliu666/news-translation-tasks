```markdown
---
title: 如何在 Laravel 应用中设置谷歌身份验证
date: 2024-12-09T07:16:44.820Z
author: Abhijeet Dave
authorURL: https://www.freecodecamp.org/news/author/Abhidave/
originalURL: https://www.freecodecamp.org/news/how-to-set-up-google-auth-in-laravel-apps/
posteditor: ""
proofreader: ""
---

在这个数字化的世界中，为您的应用提供流畅而安全的身份验证过程至关重要。这有助于改善用户体验和提升应用的总体安全性。

<!-- more -->

谷歌身份验证是用户利用其谷歌账户登录网站的最值得信赖和方便的方法之一。这意味着他们无需记住另一个用户名和密码。

将谷歌 OAuth 集成到您的 [Laravel][1] 应用中简化了登录过程，鼓励用户参与，提高平台的可信度。在本教程中，我将指导您在 Laravel 应用中实现谷歌身份验证的步骤。我们将从设置谷歌 API 凭据到配置 Laravel 的 Socialite 包。

## 目录

-   [先决条件][2]
    
-   [在 Laravel 应用中使用谷歌身份验证的好处][3]
    
-   [如何设置 Laravel 谷歌登录][4]
    
-   [结论][5]
    

### 先决条件

在开始之前，请确保您具备以下先决条件：

1.  Laravel 11
    
2.  一个谷歌开发者账户。
    
3.  基本的 Laravel 和身份验证知识。
    
4.  用于管理包的 Composer
    

一旦准备好这些先决条件，您就可以开始将谷歌身份验证集成到您的 Laravel 应用中。

### 在 Laravel 应用中使用谷歌身份验证的好处

这种设置有许多好处。以下是其中的一些：

-   简化通过 Socialite 的集成
    
-   无缝的用户身份验证
    
-   提升的安全性
    
-   可定制的用户流程
    
-   改善的可扩展性
    
-   稳固的生态系统支持
    
-   更容易的维护
    

## 如何设置 Laravel 谷歌登录

无论您是在工作个人项目还是生产就绪的应用，遵循这些步骤将帮助您顺利地集成谷歌身份验证。让我们开始吧。

### 步骤1：设置谷歌云项目

要在您的 Laravel 应用中使用谷歌身份验证，首先需要配置一个谷歌云项目。请按照以下步骤设置您的项目：

1.  访问 [谷歌云控制台][6] 并使用您的谷歌账户登录。
    
    ![Laravel auth step 1](https://cdn.hashnode.com/res/hashnode/image/upload/v1733208476305/836ff373-152a-4b99-93f3-c7684591e5c7.png)
    
2.  点击顶部导航栏中的 **“选择一个项目”** 下拉菜单。在弹出的窗口中，点击 **“新建项目”** 创建一个新项目并提供所需的详细信息。然后点击 **创建项目**。
    
    ![Laravel auth create project](https://cdn.hashnode.com/res/hashnode/image/upload/v1733208488866/409092b9-20b5-4888-9c15-f22eff4226c8.png)
    
3.  创建项目后，打开控制台左侧菜单并选择 **API 和服务 > 凭据**。
    
    ![APIs & Credentials](https://cdn.hashnode.com/res/hashnode/image/upload/v1733208517526/9b7df08c-8e8f-4db4-b297-a3f320edd0f0.png)
    
4.  在凭据页面，点击 **创建凭据** > **OAuth 客户端 ID**。
    
    ![OAuth Client ID](https://cdn.hashnode.com/res/hashnode/image/upload/v1733208549370/6dcd64f1-1fe4-4a8b-b37e-f74b38bd694a.png)
    
5.  如果这是您第一次创建客户端 ID，则会要求您配置同意屏幕。您可以通过点击 **配置同意屏幕** 进行配置。如果您已经配置过同意屏幕，则可以跳过此步骤。
    
    -   如果您的应用是公开使用的，选择 **外部**，如果限于您谷歌工作区组织内的用户，则选择 **内部**。
        
        ![OAuth consent](https://cdn.hashnode.com/res/hashnode/image/upload/v1733208660871/53aaf9a2-74d4-4cca-b4e5-9c5cfa9b3066.png)
        
    -   填写所需的详细信息，如 **应用名称**、**用户支持邮箱**和任何品牌信息。点击 **保存并继续**。
        
        ![OAuth Consent Screen](https://cdn.hashnode.com/res/hashnode/image/upload/v1733208766209/4f458336-9a2a-4238-af89-de954ed2bcf4.png)
        

配置完同意屏幕后，返回到 **凭据** 页面并再次选择 **OAuth 客户端 ID**。

6.  选择 **应用程序类型** 为 **Web 应用程序**，并为客户端凭据提供一个名称（例如，Laravel Social Login）。
    
7.  在 **授权重定向 URI** 下，添加您的应用的回调 URL：
    
    -   示例：`http://your-app-url.com/callback/google`
        
    -   如果您在本地测试，使用：[http://127.0.0.1:8000/api/auth/google/callback][7]
        
        ![OAuth client ID](https://cdn.hashnode.com/res/hashnode/image/upload/v1733208840271/b7c309bc-4880-481b-acda-c0fecf4a0ee5.png)
        
8.  点击 **创建**，谷歌将为您的项目生成一个 **客户端 ID** 和 **客户端密钥**。保存这些凭据，因为在接下来的步骤中将需要用到。
    
```

如果没有准备好的项目，可以使用以下命令创建一个新的 Laravel 项目：

```
composer create-project --prefer-dist laravel/laravel social-auth-example
```

要将 Google 身份验证集成到 Laravel 项目中，我们将使用 [Laravel Socialite][8]。Socialite 是一个由 Laravel 提供的官方包，它简化了与 Google、Facebook、Twitter 等流行服务的 OAuth 身份验证。

要安装 Socialite，请在 Laravel 项目的根目录中打开终端并运行以下命令：

```
composer require laravel/socialite
```

### 步骤 3：配置环境变量

在这一步中，我们将配置 Laravel 应用程序以使用我们在步骤 1 中收集的 Google OAuth 凭据。

找到位于项目根目录的 `.env` 文件，并添加以下环境变量：

```
GOOGLE_CLIENT_ID=your-client-id
GOOGLE_CLIENT_SECRET=your-client-secret
GOOGLE_REDIRECT_URL=http://your-domain.com/auth/google/callback
```

继续替换所有占位符为实际的密钥。

让我们逐一理解每个环境变量：

- `GOOGLE_CLIENT_ID`：由 Google 提供的应用程序的唯一标识符。
  
- `GOOGLE_CLIENT_SECRET`：应用程序用于安全地与 Google 的 API 进行身份验证的私钥。
  
- `GOOGLE_REDIRECT_URL`：用户登录后 Google 重定向的 URL。此 URL 应与创建凭据时指定的重定向 URI 匹配。
  

### 步骤 4：更新配置文件

要使 Laravel Socialite 使用 Google OAuth 凭据，我们需要在 `config/services.php` 文件中配置提供者详情。

在 `services.php` 文件中，为 Google 提供者添加以下配置：

```
'google' => [
    'client_id' => env('GOOGLE_CLIENT_ID'),        // 您的 Google 客户端 ID
    'client_secret' => env('GOOGLE_CLIENT_SECRET'), // 您的 Google 客户端密钥
    'redirect' => env('GOOGLE_REDIRECT_URL'),      // 您的 Google 重定向 URL
]
```

### 步骤 5：创建用于身份验证的控制器和路由。

在这一步中，我们将创建一个控制器来处理 Google OAuth 的重定向和回调，并设置必要的路由以触发这些方法。

运行以下 Artisan 命令以生成 `GoogleAuthController` 控制器：

```
php artisan make:controller GoogleAuthController
```

这将在 `app/Http/Controllers/GoogleAuthController.php` 创建一个控制器。

用以下代码替换新创建的 `GoogleAuthController.php` 的内容：

```php
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
            return redirect('/')->with('error', 'Google 验证失败。');
        }

        // 检查用户是否已存在于数据库中
        $existingUser = User::where('email', $user->email)->first();

        if ($existingUser) {
            // 如果用户已存在，则登录该用户
            Auth::login($existingUser);
        } else {
            // 否则，创建新用户并登录
            $newUser = User::updateOrCreate([
                'email' => $user->email
            ], [
                'name' => $user->name,
                'password' => bcrypt(Str::random(16)), // 设置随机密码
                'email_verified_at' => now()
            ]);
            Auth::login($newUser);
        }

        // 将用户重定向到仪表板或其他安全页面
        return redirect('/dashboard');
    }
}
```

此控制器包含两个函数：

1. Redirect：将用户重定向到 Google 的 OAuth 页面。
  
2. Callback：处理来自 Google 的回调，并将用户重定向到仪表板或其他安全页面。
  

让我们在 `routes/web.php` 文件中定义 `redirect` 和 `callback` 的路由：

```php
use App\Http\Controllers\GoogleAuthController;

// 路由以重定向到 Google 的 OAuth 页面
Route::get('/auth/google/redirect', [GoogleAuthController::class, 'redirect'])->name('auth.google.redirect');

// 路由以处理来自 Google 的回调
Route::get('/auth/google/callback', [GoogleAuthController::class, 'callback'])->name('auth.google.callback');
```

### 步骤 6：在项目中测试 Laravel Google 身份验证。

我们已经设置好了 Google 身份验证，现在是测试它以确保其无缝运行的时候了。在这一步中，我们将使用一个登录按钮，将用户重定向到 Google 的身份验证页面，并在成功登录后将其返回到受保护的路由。

首先，我们将添加以下按钮，允许用户选择使用 Google 登录：

为了测试，我定义了一个受保护的路由和一个 `dashboard`。此路由仅对经过身份验证的用户可访问。登录后，我们将用户重定向到此页面。让我们在 `web.php` 中定义此路由：

```
Route::get('/dashboard', function () {
    return view('dashboard');
})->middleware('auth')->name('dashboard');
```

接下来，在 `resources/views/dashboard.blade.php` 创建一个用于 dashboard 的 Blade 视图文件。以下是 dashboard 的内容：

```
<html>
    <head>
        <title>Dashboard</title>
    </head>

    <body>
        <h1>Dashboard</h1>
        <p>欢迎来到仪表盘，{{ auth()->user()->name }}!</p>
    </body>

</html>
```

在这里，我们使用 `auth()->user()` 辅助函数来显示登录用户的名称，该名称是从他们用于登录的 Google 账户中获取的。

现在，让我们尝试登录。

这是登录页面：

![登录界面 - "通过 Google 登录"](https://cdn.hashnode.com/res/hashnode/image/upload/v1732703980184/ecc90d0d-142b-43fe-8457-1b84a54f62d3.png)

点击按钮将重定向到 Google 的同意屏幕：

![Laravel Google 身份验证示例 - 同意屏幕](https://cdn.hashnode.com/res/hashnode/image/upload/v1732703936372/59f4a5bf-907d-4dda-ba8b-6909cf0a4376.png)

点击继续，然后你应该可以登录到应用程序。你将被重定向到如下屏幕。你可以看到显示用户名称的欢迎消息。

![Laravel 身份验证示例 - 欢迎屏幕](https://cdn.hashnode.com/res/hashnode/image/upload/v1732703886846/17ab7939-34c1-4d82-9527-99afb78cf3eb.png)

就是这样！你已经在 Laravel 项目中成功实现并测试了 Google 身份验证。现在你的用户可以通过他们的 Google 账户登录，提高了安全性和便利性。

要参考完整的实现，你可以在 GitHub 上找到这个项目的完整源代码：[**Laravel 的 Google 登录集成** - GitHub 仓库][9]

## 总结

你现在已经在 Laravel 应用程序中使用 Socialite 配置了 Google 身份验证！你可以通过向 `config/services.php` 文件添加其他配置，扩展此方法以包含其他 OAuth 提供商，如 Facebook、Twitter 或 GitHub。

Google OAuth 集成是现代 Web 应用程序的常见功能，而 Laravel Socialite 使其易于实现。

如果你需要更多的社交登录选项，比如 GitHub、Twitter 和 Facebook，那么可以考虑使用现成的 Laravel SaaS 模板。

大多数预构建的 Laravel SaaS 模板提供与 Google、GitHub、Facebook 和 Twitter 等热门平台的无缝集成。例如，这里有一些高级和开源资源：

-   [Laravel Starter Kit][10]（高级）
    
    -   基于 Tailwind CSS
        
    -   拥有一键魔法链接设置
        
    -   支持多种身份验证方法，包括传统的邮箱/密码登录
        
    -   2FA 身份验证
        
-   [SaaS Boilerplate][11]（开源）
    
    -   单一数据库多租户
        
    -   开发者面板
        
    -   管理个人访问令牌
        
-   [Laranuxt][12]（开源）
    
    -   Nuxt UI 是由 NuxtJS 团队构建的组件集合，基于 Tailwind CSS
        
    -   身份验证库以协助用户会话和登录/注销
        
    -   示例身份验证中间件
        
-   [Laravel Vue Boilerplate][13]（开源）
    
    -   使用 Laravel Echo 和 Pusher 的 WebSockets。
        
    -   为更好的 PWA 开发提供 Workbox。
        
    -   Laravel GraphQL
        

使用这些 Laravel SaaS 模板之一可以加速工作流程，因为不需要从头开始设置所有内容。

特别感谢 [Deep Kumbhare][14]，一位经验丰富的 Laravel 开发人员和爱好者，帮助我准备了这篇文章。

希望这篇文章能帮助你在 Laravel 中设置 Google 登录。

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

