---
title: ASP.NET Core를 사용 하 여 twitter 외부 로그인 설치
author: rick-anderson
description: 이 자습서는 기존 ASP.NET Core 응용 프로그램에 Twitter 계정 사용자 인증의 통합을 보여 줍니다.
manager: wpickett
ms.author: riande
ms.date: 11/01/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/twitter-logins
ms.openlocfilehash: 3f59f7d1bf0280cef8f7757e8cd57d4872769b3d
ms.sourcegitcommit: 545ff5a632e2281035c1becec1f99137298e4f5c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/31/2018
ms.locfileid: "34688998"
---
# <a name="twitter-external-login-setup-with-aspnet-core"></a><span data-ttu-id="47e7c-103">ASP.NET Core를 사용 하 여 twitter 외부 로그인 설치</span><span class="sxs-lookup"><span data-stu-id="47e7c-103">Twitter external login setup with ASP.NET Core</span></span>

<span data-ttu-id="47e7c-104">작성자: [Valeriy Novytskyy](https://github.com/01binary) 및 [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="47e7c-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="47e7c-105">이 자습서에서는 사용자 수 있도록 하는 방법을 보여 줍니다 [Twitter 계정으로 इ न क](https://dev.twitter.com/web/sign-in/desktop-browser) 에서 만든 샘플 ASP.NET 코어 2.0 프로젝트를 사용 하 여 [이전 페이지](xref:security/authentication/social/index)합니다.</span><span class="sxs-lookup"><span data-stu-id="47e7c-105">This tutorial shows you how to enable your users to [sign in with their Twitter account](https://dev.twitter.com/web/sign-in/desktop-browser) using a sample ASP.NET Core 2.0 project created on the [previous page](xref:security/authentication/social/index).</span></span>

## <a name="create-the-app-in-twitter"></a><span data-ttu-id="47e7c-106">Twitter에서 앱을 만들</span><span class="sxs-lookup"><span data-stu-id="47e7c-106">Create the app in Twitter</span></span>

* <span data-ttu-id="47e7c-107">로 이동 [ https://apps.twitter.com/ ](https://apps.twitter.com/) 에 로그인 합니다.</span><span class="sxs-lookup"><span data-stu-id="47e7c-107">Navigate to [https://apps.twitter.com/](https://apps.twitter.com/) and sign in.</span></span> <span data-ttu-id="47e7c-108">Twitter 계정 없는 경우 사용 하 여는 **[지금 등록](https://twitter.com/signup)** 링크를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="47e7c-108">If you don't already have a Twitter account, use the **[Sign up now](https://twitter.com/signup)** link to create one.</span></span> <span data-ttu-id="47e7c-109">에 로그인 한 후의 **응용 프로그램 관리** 페이지가 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="47e7c-109">After signing in, the **Application Management** page is shown:</span></span>

![Microsoft Edge에서 열려 있는 twitter 응용 프로그램 관리](index/_static/TwitterAppManage.png)

* <span data-ttu-id="47e7c-111">탭 **Create New App** 응용 프로그램을 확인 하 고 **이름**, **설명** 및 공용 **웹 사이트** (이 사용 가능 임시 할 때까지 URI 등록 된 도메인 이름):</span><span class="sxs-lookup"><span data-stu-id="47e7c-111">Tap **Create New App** and fill out the application **Name**, **Description** and public **Website** URI (this can be temporary until you register the domain name):</span></span>

![응용 프로그램 페이지 만들기](index/_static/TwitterCreate.png)

* <span data-ttu-id="47e7c-113">개발 URI를 입력으로 */signin-twitter* 에 추가 **유효한 OAuth 리디렉션 Uri** 필드 (예: `https://localhost:44320/signin-twitter`).</span><span class="sxs-lookup"><span data-stu-id="47e7c-113">Enter your development URI with */signin-twitter* appended into the **Valid OAuth Redirect URIs** field (for example: `https://localhost:44320/signin-twitter`).</span></span> <span data-ttu-id="47e7c-114">이 자습서의 뒷부분에 나오는 구성 된 Twitter 인증 구성표에는 요청을 자동으로 처리할 */signin-twitter* OAuth 흐름을 구현 하는 경로입니다.</span><span class="sxs-lookup"><span data-stu-id="47e7c-114">The Twitter authentication scheme configured later in this tutorial will automatically handle requests at */signin-twitter* route to implement the OAuth flow.</span></span>

* <span data-ttu-id="47e7c-115">폼의 나머지 부분을 입력 하 고 탭 **Twitter 응용 프로그램을 만드는**합니다.</span><span class="sxs-lookup"><span data-stu-id="47e7c-115">Fill out the rest of the form and tap **Create your Twitter application**.</span></span> <span data-ttu-id="47e7c-116">새 응용 프로그램 세부 정보가 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="47e7c-116">New application details are displayed:</span></span>

![응용 프로그램 페이지에서 세부 정보 탭](index/_static/TwitterAppDetails.png)

* <span data-ttu-id="47e7c-118">다시 방문 해야 사이트를 배포 하는 경우는 **응용 프로그램 관리** 페이지 하 고 새 공용 URI를 등록 합니다.</span><span class="sxs-lookup"><span data-stu-id="47e7c-118">When deploying the site you'll need to revisit the **Application Management** page and register a new public URI.</span></span>

## <a name="storing-twitter-consumerkey-and-consumersecret"></a><span data-ttu-id="47e7c-119">Twitter ConsumerKey 및 ConsumerSecret 저장</span><span class="sxs-lookup"><span data-stu-id="47e7c-119">Storing Twitter ConsumerKey and ConsumerSecret</span></span>

<span data-ttu-id="47e7c-120">Twitter와 같은 중요 한 설정이 연결 `Consumer Key` 및 `Consumer Secret` 사용 하 여 응용 프로그램 구성에는 [암호 관리자](xref:security/app-secrets)합니다.</span><span class="sxs-lookup"><span data-stu-id="47e7c-120">Link sensitive settings like Twitter `Consumer Key` and `Consumer Secret` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="47e7c-121">이 자습서에서는 이름을 토큰 `Authentication:Twitter:ConsumerKey` 및 `Authentication:Twitter:ConsumerSecret`합니다.</span><span class="sxs-lookup"><span data-stu-id="47e7c-121">For the purposes of this tutorial, name the tokens `Authentication:Twitter:ConsumerKey` and `Authentication:Twitter:ConsumerSecret`.</span></span>

<span data-ttu-id="47e7c-122">이러한 토큰을 확인할 수 있습니다는 **키와 액세스 토큰이** 새 Twitter 응용 프로그램을 만든 후 탭:</span><span class="sxs-lookup"><span data-stu-id="47e7c-122">These tokens can be found on the **Keys and Access Tokens** tab after creating your new Twitter application:</span></span>

![키 및 액세스 토큰 탭](index/_static/TwitterKeys.png)

## <a name="configure-twitter-authentication"></a><span data-ttu-id="47e7c-124">Twitter 인증 구성</span><span class="sxs-lookup"><span data-stu-id="47e7c-124">Configure Twitter Authentication</span></span>

<span data-ttu-id="47e7c-125">이 자습서에 사용 된 프로젝트 템플릿을 사용 하면 [Microsoft.AspNetCore.Authentication.Twitter](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Twitter) 패키지가 이미 설치 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="47e7c-125">The project template used in this tutorial ensures that [Microsoft.AspNetCore.Authentication.Twitter](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Twitter) package is already installed.</span></span>

* <span data-ttu-id="47e7c-126">Visual Studio 2017으로이 패키지를 설치 하려면 마우스 오른쪽 단추로 클릭 프로젝트와 선택 **NuGet 패키지 관리**합니다.</span><span class="sxs-lookup"><span data-stu-id="47e7c-126">To install this package with Visual Studio 2017, right-click on the project and select **Manage NuGet Packages**.</span></span>
* <span data-ttu-id="47e7c-127">.NET Core CLI를 설치 하려면 다음 프로젝트 디렉터리에 실행 합니다.</span><span class="sxs-lookup"><span data-stu-id="47e7c-127">To install with .NET Core CLI, execute the following in your project directory:</span></span>

   `dotnet add package Microsoft.AspNetCore.Authentication.Twitter`

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="47e7c-128">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="47e7c-128">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="47e7c-129">Twitter 서비스에 추가 `ConfigureServices` 메서드에서 *Startup.cs* 파일:</span><span class="sxs-lookup"><span data-stu-id="47e7c-129">Add the Twitter service in the `ConfigureServices` method in *Startup.cs* file:</span></span>

```csharp
services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

services.AddAuthentication().AddTwitter(twitterOptions =>
{
    twitterOptions.ConsumerKey = Configuration["Authentication:Twitter:ConsumerKey"];
    twitterOptions.ConsumerSecret = Configuration["Authentication:Twitter:ConsumerSecret"];
});
```

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](~/includes/chain-auth-providers.md)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="47e7c-130">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="47e7c-130">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="47e7c-131">Twitter 미들웨어에서 추가 된 `Configure` 에서 메서드 *Startup.cs* 파일:</span><span class="sxs-lookup"><span data-stu-id="47e7c-131">Add the Twitter middleware in the `Configure` method in *Startup.cs* file:</span></span>

```csharp
app.UseTwitterAuthentication(new TwitterOptions()
{
    ConsumerKey = Configuration["Authentication:Twitter:ConsumerKey"],
    ConsumerSecret = Configuration["Authentication:Twitter:ConsumerSecret"]
});
```

---

<span data-ttu-id="47e7c-132">참조는 [TwitterOptions](/dotnet/api/microsoft.aspnetcore.builder.twitteroptions) Twitter 인증에서 지 원하는 구성 옵션에 대 한 자세한 내용은 API 참조 합니다.</span><span class="sxs-lookup"><span data-stu-id="47e7c-132">See the [TwitterOptions](/dotnet/api/microsoft.aspnetcore.builder.twitteroptions) API reference for more information on configuration options supported by Twitter authentication.</span></span> <span data-ttu-id="47e7c-133">이 사용 하 여 사용자에 대 한 다른 정보를 요청할 수 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="47e7c-133">This can be used to request different information about the user.</span></span>

## <a name="sign-in-with-twitter"></a><span data-ttu-id="47e7c-134">Twitter를 사용 하 여 로그인</span><span class="sxs-lookup"><span data-stu-id="47e7c-134">Sign in with Twitter</span></span>

<span data-ttu-id="47e7c-135">응용 프로그램을 실행 하 고 클릭 **로그인**합니다.</span><span class="sxs-lookup"><span data-stu-id="47e7c-135">Run your application and click **Log in**.</span></span> <span data-ttu-id="47e7c-136">Twitter를 사용 하 여 로그인 하는 옵션이 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="47e7c-136">An option to sign in with Twitter appears:</span></span>

![웹 응용 프로그램: 인증 되지 않은 사용자](index/_static/DoneTwitter.png)

<span data-ttu-id="47e7c-138">클릭 하면 **Twitter** 인증에 대 한 Twitter에 리디렉션합니다.</span><span class="sxs-lookup"><span data-stu-id="47e7c-138">Clicking on **Twitter** redirects to Twitter for authentication:</span></span>

![Twitter 인증 페이지](index/_static/TwitterLogin.png)

<span data-ttu-id="47e7c-140">Twitter 자격 증명을 입력 한 후 사용자의 전자 메일을 설정할 수 있는 웹 사이트로 다시 리디렉션됩니다.</span><span class="sxs-lookup"><span data-stu-id="47e7c-140">After entering your Twitter credentials, you are redirected back to the web site where you can set your email.</span></span>

<span data-ttu-id="47e7c-141">이제 Twitter 자격 증명을 사용 하 여 로그인:</span><span class="sxs-lookup"><span data-stu-id="47e7c-141">You are now logged in using your Twitter credentials:</span></span>

![웹 응용 프로그램: 인증 된 사용자](index/_static/Done.png)

## <a name="troubleshooting"></a><span data-ttu-id="47e7c-143">문제 해결</span><span class="sxs-lookup"><span data-stu-id="47e7c-143">Troubleshooting</span></span>

* <span data-ttu-id="47e7c-144">**ASP.NET Core 2.x만:** 경우 Identity를 호출 하 여 구성 되지 않았습니다 `services.AddIdentity` 에 `ConfigureServices`, 인증을 시도 하면 *ArgumentException: 'SignInScheme' 옵션을 제공 해야*합니다.</span><span class="sxs-lookup"><span data-stu-id="47e7c-144">**ASP.NET Core 2.x only:** If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="47e7c-145">이 자습서에 사용 된 프로젝트 템플릿을 확인이 수행 되도록 합니다.</span><span class="sxs-lookup"><span data-stu-id="47e7c-145">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="47e7c-146">사이트 데이터베이스 초기 마이그레이션을 적용 하 여 생성 되지 않은 경우 발생 합니다 *요청을 처리 하는 동안 데이터베이스 작업이 실패 했습니다* 오류입니다.</span><span class="sxs-lookup"><span data-stu-id="47e7c-146">If the site database has not been created by applying the initial migration, you will get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="47e7c-147">탭 **적용 마이그레이션** 는 데이터베이스를 만들고 오류 지 나 새로 고침 합니다.</span><span class="sxs-lookup"><span data-stu-id="47e7c-147">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="47e7c-148">다음 단계</span><span class="sxs-lookup"><span data-stu-id="47e7c-148">Next steps</span></span>

* <span data-ttu-id="47e7c-149">이 문서 Twitter와 인증 방법에 대해 살펴보았습니다.</span><span class="sxs-lookup"><span data-stu-id="47e7c-149">This article showed how you can authenticate with Twitter.</span></span> <span data-ttu-id="47e7c-150">이와 비슷한 방식에 제시 된 다른 공급자를 사용 하 여 인증을 따를 수 있습니다는 [이전 페이지](xref:security/authentication/social/index)합니다.</span><span class="sxs-lookup"><span data-stu-id="47e7c-150">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="47e7c-151">다시 설정 해야 Azure 웹 앱에 웹 사이트를 게시 한 후의 `ConsumerSecret` Twitter 개발자 포털에 있습니다.</span><span class="sxs-lookup"><span data-stu-id="47e7c-151">Once you publish your web site to Azure web app, you should reset the `ConsumerSecret` in the Twitter developer portal.</span></span>

* <span data-ttu-id="47e7c-152">설정의 `Authentication:Twitter:ConsumerKey` 및 `Authentication:Twitter:ConsumerSecret` Azure 포털에서 응용 프로그램 설정으로 합니다.</span><span class="sxs-lookup"><span data-stu-id="47e7c-152">Set the `Authentication:Twitter:ConsumerKey` and `Authentication:Twitter:ConsumerSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="47e7c-153">구성 시스템 환경 변수에서 키를 읽을 수 설정 됩니다.</span><span class="sxs-lookup"><span data-stu-id="47e7c-153">The configuration system is set up to read keys from environment variables.</span></span>
