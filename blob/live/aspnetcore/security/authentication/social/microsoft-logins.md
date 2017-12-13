---
title: "Microsoft 계정 외부 로그인 설정"
author: rick-anderson
description: "이 자습서는 기존 ASP.NET Core 응용 프로그램에 Microsoft 계정 사용자 인증의 통합을 보여 줍니다."
keywords: "ASP.NET Core, Microsoft 계정, 로그인, 인증"
ms.author: riande
manager: wpickett
ms.date: 08/24/2017
ms.topic: article
ms.assetid: 66DB4B94-C78C-4005-BA03-3D982B87C268
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/microsoft-logins
ms.openlocfilehash: 77c16e3ae93c9bfe1f569d0a5888c5b765d04241
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/10/2017
---
# <a name="configuring-microsoft-account-authentication"></a><span data-ttu-id="52763-104">Microsoft 계정 인증 구성</span><span class="sxs-lookup"><span data-stu-id="52763-104">Configuring Microsoft Account authentication</span></span>

<span data-ttu-id="52763-105">작성자: [Valeriy Novytskyy](https://github.com/01binary) 및 [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="52763-105">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="52763-106">이 자습서에서 만든 샘플 ASP.NET 코어 2.0 프로젝트를 사용 하 여 자신의 Microsoft 계정으로 로그인 하 여 사용자가 사용할 수 있도록 하는 방법을 보여 줍니다.는 [이전 페이지](index.md)합니다.</span><span class="sxs-lookup"><span data-stu-id="52763-106">This tutorial shows you how to enable your users to sign in with their Microsoft account using a sample ASP.NET Core 2.0 project created on the [previous page](index.md).</span></span>

## <a name="create-the-app-in-microsoft-developer-portal"></a><span data-ttu-id="52763-107">Microsoft 개발자 포털에서 응용 프로그램 만들기</span><span class="sxs-lookup"><span data-stu-id="52763-107">Create the app in Microsoft Developer Portal</span></span>

* <span data-ttu-id="52763-108">로 이동 [https://apps.dev.microsoft.com](https://apps.dev.microsoft.com) 또는 Microsoft 계정으로 로그인을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="52763-108">Navigate to [https://apps.dev.microsoft.com](https://apps.dev.microsoft.com) and create or sign into a Microsoft account:</span></span>

![대화 상자에 로그인](index/_static/MicrosoftDevLogin.png)

<span data-ttu-id="52763-110">Microsoft 계정이 없는 경우 탭  **[만드세요!](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=13&ct=1478151035&rver=6.7.6643.0&wp=SAPI_LONG&wreply=https%3a%2f%2fapps.dev.microsoft.com%2fLoginPostBack&id=293053&aadredir=1&contextid=D70D4F21246BAB50&bk=1478151036&uiflavor=web&uaid=f0c3de863a914c358b8dc01b1ff49e85&mkt=EN-US&lc=1033&lic=1)**</span><span class="sxs-lookup"><span data-stu-id="52763-110">If you don't already have a Microsoft account, tap **[Create one!](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=13&ct=1478151035&rver=6.7.6643.0&wp=SAPI_LONG&wreply=https%3a%2f%2fapps.dev.microsoft.com%2fLoginPostBack&id=293053&aadredir=1&contextid=D70D4F21246BAB50&bk=1478151036&uiflavor=web&uaid=f0c3de863a914c358b8dc01b1ff49e85&mkt=EN-US&lc=1033&lic=1)**</span></span> <span data-ttu-id="52763-111">로그인 한 후 리디렉션됩니다 **내 응용 프로그램이** 페이지:</span><span class="sxs-lookup"><span data-stu-id="52763-111">After signing in you are redirected to **My applications** page:</span></span>

![Microsoft 개발자 포털에서 Microsoft Edge 열기](index/_static/MicrosoftDev.png)

* <span data-ttu-id="52763-113">탭 **앱 추가** 오른쪽 상단 모서리 고을 입력 하면 **응용 프로그램 이름** 및 **Contact Email**:</span><span class="sxs-lookup"><span data-stu-id="52763-113">Tap **Add an app** in the upper right corner and enter your **Application Name** and **Contact Email**:</span></span>

![새 응용 프로그램 등록 대화 상자](index/_static/MicrosoftDevAppCreate.png)

* <span data-ttu-id="52763-115">이 자습서에서는 선택을 취소는 **단계별 설치** 확인란 합니다.</span><span class="sxs-lookup"><span data-stu-id="52763-115">For the purposes of this tutorial, clear the **Guided Setup** check box.</span></span>

* <span data-ttu-id="52763-116">탭 **만들기** 를 계속 하려면는 **등록** 페이지.</span><span class="sxs-lookup"><span data-stu-id="52763-116">Tap **Create** to continue to the **Registration** page.</span></span> <span data-ttu-id="52763-117">제공는 **이름** 의 값을 확인 하 고는 **응용 프로그램 Id**,으로 사용할 수 있는 `ClientId` 자습서의 뒷부분에 나오는:</span><span class="sxs-lookup"><span data-stu-id="52763-117">Provide a **Name** and note the value of the **Application Id**, which you use as `ClientId` later in the tutorial:</span></span>

![등록 페이지](index/_static/MicrosoftDevAppReg.png)

* <span data-ttu-id="52763-119">탭 **추가 플랫폼** 에 **플랫폼** 선택한 섹션의 **웹** 플랫폼:</span><span class="sxs-lookup"><span data-stu-id="52763-119">Tap **Add Platform** in the **Platforms** section and select the **Web** platform:</span></span>

![추가 플랫폼 대화 상자](index/_static/MicrosoftDevAppPlatform.png)

* <span data-ttu-id="52763-121">새 **웹** 플랫폼 섹션에서와 개발 URL 입력 */signin-microsoft* 에 추가 **리디렉션 Url** 필드 (예: `https://localhost:44320/signin-microsoft`).</span><span class="sxs-lookup"><span data-stu-id="52763-121">In the new **Web** platform section, enter your development URL with */signin-microsoft* appended into the **Redirect URLs** field (for example: `https://localhost:44320/signin-microsoft`).</span></span> <span data-ttu-id="52763-122">이 자습서의 뒷부분에 나오는 구성 된 Microsoft 인증 구성표에는 요청을 자동으로 처리할 */signin-microsoft* OAuth 흐름을 구현 하는 경로:</span><span class="sxs-lookup"><span data-stu-id="52763-122">The Microsoft authentication scheme configured later in this tutorial will automatically handle requests at */signin-microsoft* route to implement the OAuth flow:</span></span>

![웹 플랫폼 섹션](index/_static/MicrosoftRedirectUri.png)

* <span data-ttu-id="52763-124">탭 **URL 추가** URL 추가 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="52763-124">Tap **Add URL** to ensure the URL was added.</span></span>

* <span data-ttu-id="52763-125">필요한 경우 다른 응용 프로그램 설정 작성 하 고 탭 **저장** 앱 구성 변경 내용을 저장 하려면 페이지 맨 아래에 있습니다.</span><span class="sxs-lookup"><span data-stu-id="52763-125">Fill out any other application settings if necessary and tap **Save** at the bottom of the page to save changes to app configuration.</span></span>

* <span data-ttu-id="52763-126">다시 방문 해야 사이트를 배포 하는 경우는 **등록** 페이지 하 고 새 공개 URL을 설정 합니다.</span><span class="sxs-lookup"><span data-stu-id="52763-126">When deploying the site you'll need to revisit the **Registration** page and set a new public URL.</span></span>

## <a name="store-microsoft-application-id-and-password"></a><span data-ttu-id="52763-127">Microsoft 응용 프로그램 Id와 암호를 저장 합니다.</span><span class="sxs-lookup"><span data-stu-id="52763-127">Store Microsoft Application Id and Password</span></span>

* <span data-ttu-id="52763-128">참고는 `Application Id` 에 표시 되는 **등록** 페이지.</span><span class="sxs-lookup"><span data-stu-id="52763-128">Note the `Application Id` displayed on the **Registration** page.</span></span>

* <span data-ttu-id="52763-129">탭 **새 암호를 생성할** 에 **응용 프로그램 암호** 섹션.</span><span class="sxs-lookup"><span data-stu-id="52763-129">Tap **Generate New Password** in the **Application Secrets** section.</span></span> <span data-ttu-id="52763-130">응용 프로그램 암호를 복사할 수 있습니다는 상자가 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="52763-130">This displays a box where you can copy the application password:</span></span>

![새 암호가 생성 대화 상자](index/_static/MicrosoftDevPassword.png)

<span data-ttu-id="52763-132">Microsoft와 같은 중요 한 설정이 연결 `Application ID` 및 `Password` 사용 하 여 응용 프로그램 구성에는 [암호 관리자](../../app-secrets.md)합니다.</span><span class="sxs-lookup"><span data-stu-id="52763-132">Link sensitive settings like Microsoft `Application ID` and `Password` to your application configuration using the [Secret Manager](../../app-secrets.md).</span></span> <span data-ttu-id="52763-133">이 자습서에서는 이름을 토큰 `Authentication:Microsoft:ApplicationId` 및 `Authentication:Microsoft:Password`합니다.</span><span class="sxs-lookup"><span data-stu-id="52763-133">For the purposes of this tutorial, name the tokens `Authentication:Microsoft:ApplicationId` and `Authentication:Microsoft:Password`.</span></span>

## <a name="configure-microsoft-account-authentication"></a><span data-ttu-id="52763-134">Microsoft 계정 인증 구성</span><span class="sxs-lookup"><span data-stu-id="52763-134">Configure Microsoft Account Authentication</span></span>

<span data-ttu-id="52763-135">이 자습서에 사용 된 프로젝트 템플릿을 사용 하면 [Microsoft.AspNetCore.Authentication.MicrosoftAccount](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.MicrosoftAccount) 패키지가 이미 설치 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="52763-135">The project template used in this tutorial ensures that [Microsoft.AspNetCore.Authentication.MicrosoftAccount](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.MicrosoftAccount) package is already installed.</span></span>

* <span data-ttu-id="52763-136">Visual Studio 2017으로이 패키지를 설치 하려면 마우스 오른쪽 단추로 클릭 프로젝트와 선택 **NuGet 패키지 관리**합니다.</span><span class="sxs-lookup"><span data-stu-id="52763-136">To install this package with Visual Studio 2017, right-click on the project and select **Manage NuGet Packages**.</span></span>
* <span data-ttu-id="52763-137">.NET Core CLI를 설치 하려면 다음 프로젝트 디렉터리에 실행 합니다.</span><span class="sxs-lookup"><span data-stu-id="52763-137">To install with .NET Core CLI, execute the following in your project directory:</span></span>

   `dotnet add package Microsoft.AspNetCore.Authentication.MicrosoftAccount`

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="52763-138">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="52763-138">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="52763-139">Microsoft 계정 서비스에 추가 `ConfigureServices` 메서드에서 *Startup.cs* 파일:</span><span class="sxs-lookup"><span data-stu-id="52763-139">Add the Microsoft Account service in the `ConfigureServices` method in *Startup.cs* file:</span></span>

```csharp
services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

services.AddAuthentication().AddMicrosoftAccount(microsoftOptions =>
{
    microsoftOptions.ClientId = Configuration["Authentication:Microsoft:ApplicationId"];
    microsoftOptions.ClientSecret = Configuration["Authentication:Microsoft:Password"];
});
```

[!INCLUDE[default settings configuration](includes/default-settings.md)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="52763-140">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="52763-140">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="52763-141">Microsoft 계정 미들웨어에서 추가 `Configure` 메서드에서 *Startup.cs* 파일:</span><span class="sxs-lookup"><span data-stu-id="52763-141">Add the Microsoft Account middleware in the `Configure` method in *Startup.cs* file:</span></span>

```csharp
app.UseMicrosoftAccountAuthentication(new MicrosoftAccountOptions()
{
    ClientId = Configuration["Authentication:Microsoft:ApplicationId"],
    ClientSecret = Configuration["Authentication:Microsoft:Password"]
});
```

---

<span data-ttu-id="52763-142">하지만 Microsoft 개발자 포털에서 사용 되는 용어 이러한 토큰 이름을 `ApplicationId` 및 `Password`,으로 노출 된 `ClientId` 및 `ClientSecret` 구성 API에 합니다.</span><span class="sxs-lookup"><span data-stu-id="52763-142">Although the terminology used on Microsoft Developer Portal names these tokens `ApplicationId` and `Password`, they are exposed as `ClientId` and `ClientSecret` to the configuration API.</span></span>

<span data-ttu-id="52763-143">참조는 [MicrosoftAccountOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.microsoftaccountoptions) Microsoft 계정 인증에서 지 원하는 구성 옵션에 대 한 자세한 내용은 API 참조 합니다.</span><span class="sxs-lookup"><span data-stu-id="52763-143">See the [MicrosoftAccountOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.microsoftaccountoptions) API reference for more information on configuration options supported by Microsoft Account authentication.</span></span> <span data-ttu-id="52763-144">이 사용 하 여 사용자에 대 한 다른 정보를 요청할 수 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="52763-144">This can be used to request different information about the user.</span></span>

## <a name="sign-in-with-microsoft-account"></a><span data-ttu-id="52763-145">Microsoft 계정으로 로그인</span><span class="sxs-lookup"><span data-stu-id="52763-145">Sign in with Microsoft Account</span></span>

<span data-ttu-id="52763-146">응용 프로그램을 실행 하 고 클릭 **로그인**합니다.</span><span class="sxs-lookup"><span data-stu-id="52763-146">Run your application and click **Log in**.</span></span> <span data-ttu-id="52763-147">Microsoft를 사용 하 여 로그인 하는 옵션이 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="52763-147">An option to sign in with Microsoft appears:</span></span>

![웹 페이지에 응용 프로그램 로그: 인증 되지 않은 사용자](index/_static/DoneMicrosoft.png)

<span data-ttu-id="52763-149">Microsoft을 클릭할 때 리디렉션됩니다 Microsoft 인증에 대 한 합니다.</span><span class="sxs-lookup"><span data-stu-id="52763-149">When you click on Microsoft, you are redirected to Microsoft for authentication.</span></span> <span data-ttu-id="52763-150">(아직 로그인) 하는 경우 Microsoft 계정으로 로그인 한 후 정보에 액세스 하는 앱을 사용 하 라는 메시지가 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="52763-150">After signing in with your Microsoft Account (if not already signed in) you will be prompted to let the app access your info:</span></span>

![Microsoft 인증 대화 상자](index/_static/MicrosoftLogin.png)

<span data-ttu-id="52763-152">탭 **예** 및 전자 메일을 설정할 수 있는 웹 사이트로 다시 리디렉션됩니다.</span><span class="sxs-lookup"><span data-stu-id="52763-152">Tap **Yes** and you will be redirected back to the web site where you can set your email.</span></span>

<span data-ttu-id="52763-153">이제 Microsoft 자격 증명을 사용 하 여 로그인:</span><span class="sxs-lookup"><span data-stu-id="52763-153">You are now logged in using your Microsoft credentials:</span></span>

![웹 응용 프로그램: 인증 된 사용자](index/_static/Done.png)

## <a name="troubleshooting"></a><span data-ttu-id="52763-155">문제 해결</span><span class="sxs-lookup"><span data-stu-id="52763-155">Troubleshooting</span></span>

* <span data-ttu-id="52763-156">Microsoft 계정 공급자 오류 페이지에 로그인 할 사용자를 리디렉션하를 하는 경우 오류 제목 및 설명 쿼리 문자열 매개 변수 바로 다음에 유의 `#` (hashtag) Uri에 있습니다.</span><span class="sxs-lookup"><span data-stu-id="52763-156">If the Microsoft Account provider redirects you to a sign in error page, note the error title and description query string parameters directly following the `#` (hashtag) in the Uri.</span></span>

  <span data-ttu-id="52763-157">가장 일반적인 원인은 Uri의 일치 하지 않는 응용 프로그램 오류 메시지가 Microsoft 인증 문제가 있는 것 처럼 보이지만는 **리디렉션 Uri** 에 대해 지정 된 된 **웹** 플랫폼 .</span><span class="sxs-lookup"><span data-stu-id="52763-157">Although the error message seems to indicate a problem with Microsoft authentication, the most common cause is your application Uri not matching any of the **Redirect URIs** specified for the **Web** platform.</span></span>
* <span data-ttu-id="52763-158">**ASP.NET Core 2.x만:** 경우 Identity를 호출 하 여 구성 되지 않은 `services.AddIdentity` 에 `ConfigureServices`, 인증을 시도 하면 *ArgumentException: 'SignInScheme' 옵션을 제공 해야*합니다.</span><span class="sxs-lookup"><span data-stu-id="52763-158">**ASP.NET Core 2.x only:** If Identity is not configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="52763-159">이 자습서에 사용 된 프로젝트 템플릿을 확인이 수행 되도록 합니다.</span><span class="sxs-lookup"><span data-stu-id="52763-159">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="52763-160">사이트 데이터베이스 초기 마이그레이션을 적용 하 여 생성 되지 않은 경우 발생 합니다 *요청을 처리 하는 동안 데이터베이스 작업이 실패 했습니다* 오류입니다.</span><span class="sxs-lookup"><span data-stu-id="52763-160">If the site database has not been created by applying the initial migration, you will get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="52763-161">탭 **적용 마이그레이션** 는 데이터베이스를 만들고 오류 지 나 새로 고침 합니다.</span><span class="sxs-lookup"><span data-stu-id="52763-161">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="52763-162">다음 단계</span><span class="sxs-lookup"><span data-stu-id="52763-162">Next steps</span></span>

* <span data-ttu-id="52763-163">이 문서를 Microsoft와 인증 방법에 대해 살펴보았습니다.</span><span class="sxs-lookup"><span data-stu-id="52763-163">This article showed how you can authenticate with Microsoft.</span></span> <span data-ttu-id="52763-164">이와 비슷한 방식에 제시 된 다른 공급자를 사용 하 여 인증을 따를 수 있습니다는 [이전 페이지](index.md)합니다.</span><span class="sxs-lookup"><span data-stu-id="52763-164">You can follow a similar approach to authenticate with other providers listed on the [previous page](index.md).</span></span>

* <span data-ttu-id="52763-165">새 만들어야 Azure 웹 앱에 웹 사이트를 게시 한 `Password` Microsoft 개발자 포털에서 합니다.</span><span class="sxs-lookup"><span data-stu-id="52763-165">Once you publish your web site to Azure web app, you should create a new `Password` in the Microsoft Developer Portal.</span></span>

* <span data-ttu-id="52763-166">설정의 `Authentication:Microsoft:ApplicationId` 및 `Authentication:Microsoft:Password` Azure 포털에서 응용 프로그램 설정으로 합니다.</span><span class="sxs-lookup"><span data-stu-id="52763-166">Set the `Authentication:Microsoft:ApplicationId` and `Authentication:Microsoft:Password` as application settings in the Azure portal.</span></span> <span data-ttu-id="52763-167">구성 시스템 환경 변수에서 키를 읽을 수 설정 됩니다.</span><span class="sxs-lookup"><span data-stu-id="52763-167">The configuration system is set up to read keys from environment variables.</span></span>
