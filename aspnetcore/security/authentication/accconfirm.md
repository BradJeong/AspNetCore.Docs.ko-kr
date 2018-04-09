---
title: 계정 확인 및 ASP.NET 코어에서 암호 복구
author: rick-anderson
description: 전자 메일 확인 및 암호 재설정으로 ASP.NET Core 응용 프로그램을 구축 하는 방법에 알아봅니다.
manager: wpickett
ms.author: riande
ms.date: 2/11/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/accconfirm
ms.openlocfilehash: 8ad2a63ce007a68eac3b607db454c6b4fc834444
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/06/2018
---
# <a name="account-confirmation-and-password-recovery-in-aspnet-core"></a><span data-ttu-id="0b4e5-103">계정 확인 및 ASP.NET 코어에서 암호 복구</span><span class="sxs-lookup"><span data-stu-id="0b4e5-103">Account confirmation and password recovery in ASP.NET Core</span></span>

<span data-ttu-id="0b4e5-104">작성자: [Rick Anderson](https://twitter.com/RickAndMSFT) 및 [Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="0b4e5-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

<span data-ttu-id="0b4e5-105">이 자습서에서는 전자 메일 확인 및 암호 재설정으로 ASP.NET Core 응용 프로그램을 구축 하는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="0b4e5-105">This tutorial shows you how to build an ASP.NET Core app with email confirmation and password reset.</span></span> <span data-ttu-id="0b4e5-106">이 자습서는 **하지** 시작 항목입니다.</span><span class="sxs-lookup"><span data-stu-id="0b4e5-106">This tutorial is **not** a beginning topic.</span></span> <span data-ttu-id="0b4e5-107">에 대해 잘 알고 있어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="0b4e5-107">You should be familiar with:</span></span>

* [<span data-ttu-id="0b4e5-108">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0b4e5-108">ASP.NET Core</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="0b4e5-109">인증</span><span class="sxs-lookup"><span data-stu-id="0b4e5-109">Authentication</span></span>](xref:security/authentication/index)
* [<span data-ttu-id="0b4e5-110">계정 확인 및 비밀번호 복구</span><span class="sxs-lookup"><span data-stu-id="0b4e5-110">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="0b4e5-111">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="0b4e5-111">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

<span data-ttu-id="0b4e5-112">참조 [이 PDF 파일](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) ASP.NET Core MVC 1.1 및 2.x 버전에 대 한 합니다.</span><span class="sxs-lookup"><span data-stu-id="0b4e5-112">See [this PDF file](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) for the ASP.NET Core MVC 1.1 and 2.x versions.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0b4e5-113">전제 조건</span><span class="sxs-lookup"><span data-stu-id="0b4e5-113">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs.md)]

## <a name="create-a-new-aspnet-core-project-with-the-net-core-cli"></a><span data-ttu-id="0b4e5-114">.NET Core CLI 인 새 ASP.NET Core 프로젝트를 만듭니다</span><span class="sxs-lookup"><span data-stu-id="0b4e5-114">Create a new ASP.NET Core project with the .NET Core CLI</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="0b4e5-115">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="0b4e5-115">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```console
dotnet new razor --auth Individual -o WebPWrecover
cd WebPWrecover
```

* <span data-ttu-id="0b4e5-116">`--auth Individual` 개별 사용자 계정 프로젝트 템플릿을 지정합니다.</span><span class="sxs-lookup"><span data-stu-id="0b4e5-116">`--auth Individual` specifies the Individual User Accounts project template.</span></span>
* <span data-ttu-id="0b4e5-117">Windows에서 추가 된 `-uld` 옵션입니다.</span><span class="sxs-lookup"><span data-stu-id="0b4e5-117">On Windows, add the `-uld` option.</span></span> <span data-ttu-id="0b4e5-118">지정 SQLite 대신 LocalDB를 사용 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="0b4e5-118">It specifies LocalDB should be used instead of SQLite.</span></span>
* <span data-ttu-id="0b4e5-119">실행 `new mvc --help` 도움말을 보려면이 명령에 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0b4e5-119">Run `new mvc --help` to get help on this command.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="0b4e5-120">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="0b4e5-120">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="0b4e5-121">CLI 또는 SQLite를 사용 하는 경우 명령 창에서 다음을 실행 합니다.</span><span class="sxs-lookup"><span data-stu-id="0b4e5-121">If you're using the CLI or SQLite, run the following in a command window:</span></span>

```console
dotnet new mvc --auth Individual
```

* <span data-ttu-id="0b4e5-122">`--auth Individual` 개별 사용자 계정 프로젝트 템플릿을 지정합니다.</span><span class="sxs-lookup"><span data-stu-id="0b4e5-122">`--auth Individual` specifies the Individual User Accounts project template.</span></span>
* <span data-ttu-id="0b4e5-123">Windows에서 추가 된 `-uld` 옵션입니다.</span><span class="sxs-lookup"><span data-stu-id="0b4e5-123">On Windows, add the `-uld` option.</span></span> <span data-ttu-id="0b4e5-124">지정 SQLite 대신 LocalDB를 사용 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="0b4e5-124">It specifies LocalDB should be used instead of SQLite.</span></span>
* <span data-ttu-id="0b4e5-125">실행 `new mvc --help` 도움말을 보려면이 명령에 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0b4e5-125">Run `new mvc --help` to get help on this command.</span></span>

---

<span data-ttu-id="0b4e5-126">또는 Visual Studio와 함께 새 ASP.NET Core 프로젝트를 만들 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0b4e5-126">Alternatively, you can create a new ASP.NET Core project with Visual Studio:</span></span>

* <span data-ttu-id="0b4e5-127">Visual Studio에서 만드는 새 **웹 응용 프로그램** 프로젝트.</span><span class="sxs-lookup"><span data-stu-id="0b4e5-127">In Visual Studio, create a new **Web Application** project.</span></span>
* <span data-ttu-id="0b4e5-128">선택 **ASP.NET Core 2.0**합니다.</span><span class="sxs-lookup"><span data-stu-id="0b4e5-128">Select **ASP.NET Core 2.0**.</span></span> <span data-ttu-id="0b4e5-129">**.NET core** 다음 이미지에서 선택한 선택할 수 있지만 **.NET Framework**합니다.</span><span class="sxs-lookup"><span data-stu-id="0b4e5-129">**.NET Core** is selected in the following image, but you can select **.NET Framework**.</span></span>
* <span data-ttu-id="0b4e5-130">선택 **인증 변경** 로 설정 하 고 **개별 사용자 계정**합니다.</span><span class="sxs-lookup"><span data-stu-id="0b4e5-130">Select **Change Authentication** and set to **Individual User Accounts**.</span></span>
* <span data-ttu-id="0b4e5-131">기본값을 그대로 두고 **저장 된 사용자 계정 앱에서**합니다.</span><span class="sxs-lookup"><span data-stu-id="0b4e5-131">Keep the default **Store user accounts in-app**.</span></span>

![선택 하는 "개별 사용자 계정 radio"를 표시 하는 새 프로젝트 대화 상자](accconfirm/_static/2.png)

## <a name="test-new-user-registration"></a><span data-ttu-id="0b4e5-133">새 사용자 등록을 테스트 합니다.</span><span class="sxs-lookup"><span data-stu-id="0b4e5-133">Test new user registration</span></span>

<span data-ttu-id="0b4e5-134">응용 프로그램을 실행, 선택는 **등록** 링크를 선택한 사용자를 등록 합니다.</span><span class="sxs-lookup"><span data-stu-id="0b4e5-134">Run the app, select the **Register** link, and register a user.</span></span> <span data-ttu-id="0b4e5-135">Entity Framework Core 마이그레이션을 실행 하는 지침을 따릅니다.</span><span class="sxs-lookup"><span data-stu-id="0b4e5-135">Follow the instructions to run Entity Framework Core migrations.</span></span> <span data-ttu-id="0b4e5-136">전자 메일에만 유효성 검사와는 시점에서 [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) 특성입니다.</span><span class="sxs-lookup"><span data-stu-id="0b4e5-136">At this point, the only validation on the email is with the [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) attribute.</span></span> <span data-ttu-id="0b4e5-137">등록을 제출한 후 응용 프로그램에 로그인 되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0b4e5-137">After submitting the registration, you are logged into the app.</span></span> <span data-ttu-id="0b4e5-138">자습서의 뒷부분에 나오는 코드 업데이트 되기 때문 자신의 전자 메일의 유효성을 검사 될 때까지 새 사용자가 로그인 할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="0b4e5-138">Later in the tutorial, the code is updated so new users can't log in until their email has been validated.</span></span>

## <a name="view-the-identity-database"></a><span data-ttu-id="0b4e5-139">Id 데이터베이스 보기</span><span class="sxs-lookup"><span data-stu-id="0b4e5-139">View the Identity database</span></span>

<span data-ttu-id="0b4e5-140">참조 [SQLite ASP.NET Core MVC 프로젝트에서 작업할](xref:tutorials/first-mvc-app-xplat/working-with-sql) SQLite 데이터베이스를 확인 하는 방법에 대 한 지침은 합니다.</span><span class="sxs-lookup"><span data-stu-id="0b4e5-140">See [Work with SQLite in an ASP.NET Core MVC project](xref:tutorials/first-mvc-app-xplat/working-with-sql) for instructions on how to view the SQLite database.</span></span>

<span data-ttu-id="0b4e5-141">Visual studio의 경우:</span><span class="sxs-lookup"><span data-stu-id="0b4e5-141">For Visual Studio:</span></span>

* <span data-ttu-id="0b4e5-142">**보기** 메뉴 선택 **SQL Server 개체 탐색기** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="0b4e5-142">From the **View** menu, select **SQL Server Object Explorer** (SSOX).</span></span>
* <span data-ttu-id="0b4e5-143">로 이동 **(localdb) (SQL Server 13) MSSQLLocalDB**합니다.</span><span class="sxs-lookup"><span data-stu-id="0b4e5-143">Navigate to **(localdb)MSSQLLocalDB(SQL Server 13)**.</span></span> <span data-ttu-id="0b4e5-144">마우스 오른쪽 단추로 클릭 **dbo입니다. AspNetUsers** > **데이터를 볼**:</span><span class="sxs-lookup"><span data-stu-id="0b4e5-144">Right-click on **dbo.AspNetUsers** > **View Data**:</span></span>

![SQL Server 개체 탐색기의 AspNetUsers 테이블에 대 한 상황에 맞는 메뉴](accconfirm/_static/ssox.png)

<span data-ttu-id="0b4e5-146">테이블의 참고 `EmailConfirmed` 필드는 `False`합니다.</span><span class="sxs-lookup"><span data-stu-id="0b4e5-146">Note the table's `EmailConfirmed` field is `False`.</span></span>

<span data-ttu-id="0b4e5-147">응용 프로그램에서 확인 전자 메일을 보낼 때 다음 단계에서이 전자 메일에 다시 사용 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0b4e5-147">You might want to use this email again in the next step when the app sends a confirmation email.</span></span> <span data-ttu-id="0b4e5-148">선택한 행을 마우스 오른쪽 단추로 클릭 **삭제**합니다.</span><span class="sxs-lookup"><span data-stu-id="0b4e5-148">Right-click on the row and select **Delete**.</span></span> <span data-ttu-id="0b4e5-149">전자 메일 별칭 삭제 쉽게 다음 단계에 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0b4e5-149">Deleting the email alias makes it easier in the following steps.</span></span>

---

## <a name="require-https"></a><span data-ttu-id="0b4e5-150">HTTPS가 필요</span><span class="sxs-lookup"><span data-stu-id="0b4e5-150">Require HTTPS</span></span>

<span data-ttu-id="0b4e5-151">참조 [HTTPS가 필요한](xref:security/enforcing-ssl)합니다.</span><span class="sxs-lookup"><span data-stu-id="0b4e5-151">See [Require HTTPS](xref:security/enforcing-ssl).</span></span>

<a name="prevent-login-at-registration"></a>
## <a name="require-email-confirmation"></a><span data-ttu-id="0b4e5-152">전자 메일 확인이 필요</span><span class="sxs-lookup"><span data-stu-id="0b4e5-152">Require email confirmation</span></span>

<span data-ttu-id="0b4e5-153">새 사용자 등록의 전자 메일을 확인 하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="0b4e5-153">It's a best practice to confirm the email of a new user registration.</span></span> <span data-ttu-id="0b4e5-154">다른 사람을 가장 하지 하는 것을 확인 하는 데 도움이 확인 전자 메일 (즉, 이러한 하지 않은 등록 된 다른 사용자의 전자 메일).</span><span class="sxs-lookup"><span data-stu-id="0b4e5-154">Email confirmation helps to verify they're not impersonating someone else (that is, they haven't registered with someone else's email).</span></span> <span data-ttu-id="0b4e5-155">토론 포럼을 가지 며 하지 못하도록 하려는 경우를 가정해 볼 "yli@example.com로 등록" 에서"nolivetto@contoso.com."</span><span class="sxs-lookup"><span data-stu-id="0b4e5-155">Suppose you had a discussion forum, and you wanted to prevent "yli@example.com" from registering as "nolivetto@contoso.com."</span></span> <span data-ttu-id="0b4e5-156">전자 메일 확인 하지 않고 "nolivetto@contoso.com" 응용 프로그램에서 원치 않는 전자 메일을 받을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0b4e5-156">Without email confirmation, "nolivetto@contoso.com" could receive unwanted email from your app.</span></span> <span data-ttu-id="0b4e5-157">사용자는 실수로으로 등록 된다고 가정 합니다 "ylo@example.com" "yli"의 잘못 된 철자를 발견 하지 않은 합니다.</span><span class="sxs-lookup"><span data-stu-id="0b4e5-157">Suppose the user accidentally registered as "ylo@example.com" and hadn't noticed the misspelling of "yli".</span></span> <span data-ttu-id="0b4e5-158">앱이 올바른 전자 메일에 없는 때문에 암호 복구를 사용할 수 없게.</span><span class="sxs-lookup"><span data-stu-id="0b4e5-158">They wouldn't be able to use password recovery because the app doesn't have their correct email.</span></span> <span data-ttu-id="0b4e5-159">전자 메일 확인에서 봇만 제한 된 보호를 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="0b4e5-159">Email confirmation provides only limited protection from bots.</span></span> <span data-ttu-id="0b4e5-160">전자 메일 확인 많은 전자 메일 계정 가진 악의적인 사용자 로부터 보호를 제공 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="0b4e5-160">Email confirmation doesn't provide protection from malicious users with many email accounts.</span></span>

<span data-ttu-id="0b4e5-161">일반적으로 새 사용자는 확인 된 전자 메일을 갖기 전에 웹 사이트에 데이터를 게시 하는 것을 방지 하려고 합니다.</span><span class="sxs-lookup"><span data-stu-id="0b4e5-161">You generally want to prevent new users from posting any data to your web site before they have a confirmed email.</span></span>

<span data-ttu-id="0b4e5-162">업데이트 `ConfigureServices` 확인 된 전자 메일을 요구 하도록 합니다.</span><span class="sxs-lookup"><span data-stu-id="0b4e5-162">Update `ConfigureServices` to require a confirmed email:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover/Startup.cs?name=snippet1&highlight=12-17)]

<span data-ttu-id="0b4e5-163">`config.SignIn.RequireConfirmedEmail = true;` 등록된 한 사용자의 전자 메일이 확인 될 때까지에 로그인 하지 못하도록 방지 합니다.</span><span class="sxs-lookup"><span data-stu-id="0b4e5-163">`config.SignIn.RequireConfirmedEmail = true;` prevents registered users from logging in until their email is confirmed.</span></span>

### <a name="configure-email-provider"></a><span data-ttu-id="0b4e5-164">전자 메일 공급자를 구성 합니다.</span><span class="sxs-lookup"><span data-stu-id="0b4e5-164">Configure email provider</span></span>

<span data-ttu-id="0b4e5-165">이 자습서에서는 SendGrid 전자 메일을 보내는 데 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="0b4e5-165">In this tutorial, SendGrid is used to send email.</span></span> <span data-ttu-id="0b4e5-166">SendGrid 계정 및 전자 메일을 보내는 키 필요.</span><span class="sxs-lookup"><span data-stu-id="0b4e5-166">You need a SendGrid account and key to send email.</span></span> <span data-ttu-id="0b4e5-167">다른 전자 메일 공급자를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0b4e5-167">You can use other email providers.</span></span> <span data-ttu-id="0b4e5-168">ASP.NET Core 2.x 포함 `System.Net.Mail`, 응용 프로그램에서 전자 메일을 보낼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0b4e5-168">ASP.NET Core 2.x includes `System.Net.Mail`, which allows you to send email from your app.</span></span> <span data-ttu-id="0b4e5-169">전자 메일을 보내는 SendGrid 또는 다른 전자 메일 서비스를 사용 하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="0b4e5-169">We recommend you use SendGrid or another email service to send email.</span></span> <span data-ttu-id="0b4e5-170">SMTP 보안을 설정 하 고 올바르게 설정 하기가 어렵습니다.</span><span class="sxs-lookup"><span data-stu-id="0b4e5-170">SMTP is difficult to secure and set up correctly.</span></span>

<span data-ttu-id="0b4e5-171">[옵션 패턴](xref:fundamentals/configuration/options) 사용자 계정 및 키 설정에 액세스 하는 데 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="0b4e5-171">The [Options pattern](xref:fundamentals/configuration/options) is used to access the user account and key settings.</span></span> <span data-ttu-id="0b4e5-172">자세한 내용은 참조 [구성](xref:fundamentals/configuration/index)합니다.</span><span class="sxs-lookup"><span data-stu-id="0b4e5-172">For more information, see [configuration](xref:fundamentals/configuration/index).</span></span>

<span data-ttu-id="0b4e5-173">전자 메일 보안 키를 인출 하는 클래스를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="0b4e5-173">Create a class to fetch the secure email key.</span></span> <span data-ttu-id="0b4e5-174">이 샘플은 `AuthMessageSenderOptions` 클래스에 만들어집니다는 *Services/AuthMessageSenderOptions.cs* 파일:</span><span class="sxs-lookup"><span data-stu-id="0b4e5-174">For this sample, the `AuthMessageSenderOptions` class is created in the *Services/AuthMessageSenderOptions.cs* file:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover/Services/AuthMessageSenderOptions.cs?name=snippet1)]

<span data-ttu-id="0b4e5-175">설정의 `SendGridUser` 및 `SendGridKey` 와 [암호 관리자 도구](xref:security/app-secrets)합니다.</span><span class="sxs-lookup"><span data-stu-id="0b4e5-175">Set the `SendGridUser` and `SendGridKey` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="0b4e5-176">예를 들어:</span><span class="sxs-lookup"><span data-stu-id="0b4e5-176">For example:</span></span>

```console
C:\WebAppl\src\WebApp1>dotnet user-secrets set SendGridUser RickAndMSFT
info: Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

<span data-ttu-id="0b4e5-177">Windows에서는 암호 관리자에 있는 키/값 쌍을 저장 한 *secrets.json* 파일에 `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` 디렉터리입니다.</span><span class="sxs-lookup"><span data-stu-id="0b4e5-177">On Windows, Secret Manager stores keys/value pairs in a *secrets.json* file in the `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` directory.</span></span>

<span data-ttu-id="0b4e5-178">콘텐츠는 *secrets.json* 파일이 암호화 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="0b4e5-178">The contents of the *secrets.json* file aren't encrypted.</span></span> <span data-ttu-id="0b4e5-179">*secrets.json* 파일은 다음과 같습니다 (의 `SendGridKey` 값 제거 되었습니다.)</span><span class="sxs-lookup"><span data-stu-id="0b4e5-179">The *secrets.json* file is shown below (the `SendGridKey` value has been removed.)</span></span>

 ```json
  {
    "SendGridUser": "RickAndMSFT",
    "SendGridKey": "<key removed>"
  }
  ```

### <a name="configure-startup-to-use-authmessagesenderoptions"></a><span data-ttu-id="0b4e5-180">AuthMessageSenderOptions를 사용 하는 시작 구성</span><span class="sxs-lookup"><span data-stu-id="0b4e5-180">Configure startup to use AuthMessageSenderOptions</span></span>

<span data-ttu-id="0b4e5-181">추가 `AuthMessageSenderOptions` 끝날 때 서비스 컨테이너에는 `ConfigureServices` 에서 메서드는 *Startup.cs* 파일:</span><span class="sxs-lookup"><span data-stu-id="0b4e5-181">Add `AuthMessageSenderOptions` to the service container at the end of the `ConfigureServices` method in the *Startup.cs* file:</span></span>

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="0b4e5-182">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="0b4e5-182">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)
[!code-csharp[](accconfirm/sample/WebPWrecover/Startup.cs?name=snippet2&highlight=28)]

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="0b4e5-183">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="0b4e5-183">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)
[!code-csharp[](accconfirm/sample/WebApp1/Startup.cs?name=snippet1&highlight=26)]

* * *
### <a name="configure-the-authmessagesender-class"></a><span data-ttu-id="0b4e5-184">AuthMessageSender 클래스 구성</span><span class="sxs-lookup"><span data-stu-id="0b4e5-184">Configure the AuthMessageSender class</span></span>

<span data-ttu-id="0b4e5-185">이 자습서에서는 통해 전자 메일 알림을 추가 하는 방법을 보여 줍니다. [SendGrid](https://sendgrid.com/), 하지만 SMTP 및 다른 메커니즘을 사용 하 여 메일을 보낼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0b4e5-185">This tutorial shows how to add email notifications through [SendGrid](https://sendgrid.com/), but you can send email using SMTP and other mechanisms.</span></span>

<span data-ttu-id="0b4e5-186">설치는 `SendGrid` NuGet 패키지:</span><span class="sxs-lookup"><span data-stu-id="0b4e5-186">Install the `SendGrid` NuGet package:</span></span>

* <span data-ttu-id="0b4e5-187">명령줄:</span><span class="sxs-lookup"><span data-stu-id="0b4e5-187">From the command line:</span></span>

    `dotnet add package SendGrid`

* <span data-ttu-id="0b4e5-188">패키지 관리자 콘솔에서 다음 명령을 입력 합니다.</span><span class="sxs-lookup"><span data-stu-id="0b4e5-188">From the Package Manager Console, enter the following command:</span></span>

  `Install-Package SendGrid`

<span data-ttu-id="0b4e5-189">참조 [SendGrid를 무료로 시작](https://sendgrid.com/free/) 무료 SendGrid 계정을 등록할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0b4e5-189">See [Get Started with SendGrid for Free](https://sendgrid.com/free/) to register for a free SendGrid account.</span></span>

#### <a name="configure-sendgrid"></a><span data-ttu-id="0b4e5-190">SendGrid를 구성 합니다.</span><span class="sxs-lookup"><span data-stu-id="0b4e5-190">Configure SendGrid</span></span>

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="0b4e5-191">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="0b4e5-191">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)
<span data-ttu-id="0b4e5-192">SendGrid를 구성 하려면 코드에서 다음과 같은 추가 *Services/EmailSender.cs*:</span><span class="sxs-lookup"><span data-stu-id="0b4e5-192">To configure SendGrid, add code similar to the following in *Services/EmailSender.cs*:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover/Services/EmailSender.cs)]

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="0b4e5-193">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="0b4e5-193">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)
* <span data-ttu-id="0b4e5-194">에 코드를 추가 *Services/MessageServices.cs* SendGrid를 구성 하는 다음과 비슷합니다.</span><span class="sxs-lookup"><span data-stu-id="0b4e5-194">Add code in *Services/MessageServices.cs* similar to the following to configure SendGrid:</span></span>

[!code-csharp[](accconfirm/sample/WebApp1/Services/MessageServices.cs)]

* * *
## <a name="enable-account-confirmation-and-password-recovery"></a><span data-ttu-id="0b4e5-195">계정 확인 및 암호 복구 사용</span><span class="sxs-lookup"><span data-stu-id="0b4e5-195">Enable account confirmation and password recovery</span></span>

<span data-ttu-id="0b4e5-196">서식 파일에는 계정 확인 및 암호 복구를 위한 코드가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0b4e5-196">The template has the code for account confirmation and password recovery.</span></span> <span data-ttu-id="0b4e5-197">찾을 `OnPostAsync` 메서드에서 *Pages/Account/Register.cshtml.cs*합니다.</span><span class="sxs-lookup"><span data-stu-id="0b4e5-197">Find the `OnPostAsync` method in *Pages/Account/Register.cshtml.cs*.</span></span>

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="0b4e5-198">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="0b4e5-198">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)
<span data-ttu-id="0b4e5-199">새로 등록 된 사용자가 있는 다음 줄을 주석 처리에 자동으로 로그온에서 함:</span><span class="sxs-lookup"><span data-stu-id="0b4e5-199">Prevent newly registered users from being automatically logged on by commenting out the following line:</span></span>

```csharp
await _signInManager.SignInAsync(user, isPersistent: false);
```

<span data-ttu-id="0b4e5-200">전체 메서드 강조 표시 하는 변경 된 줄으로 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="0b4e5-200">The complete method is shown with the changed line highlighted:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover/Pages/Account/Register.cshtml.cs?highlight=16&name=snippet_Register)]

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="0b4e5-201">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="0b4e5-201">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)
<span data-ttu-id="0b4e5-202">계정 확인을 사용 하려면 다음 코드를 주석 처리 제거:</span><span class="sxs-lookup"><span data-stu-id="0b4e5-202">To enable account confirmation, uncomment the following code:</span></span>

[!code-csharp[](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=16-25&name=snippet_Register)]

<span data-ttu-id="0b4e5-203">**참고:** 코드 때문에 새로 등록 된 사용자의 다음 줄을 주석 처리에 자동으로 로그온 합니다.</span><span class="sxs-lookup"><span data-stu-id="0b4e5-203">**Note:** The code is preventing a newly registered user from being automatically logged on by commenting out the following line:</span></span>

```csharp
//await _signInManager.SignInAsync(user, isPersistent: false);
```

<span data-ttu-id="0b4e5-204">코드의 주석 처리 하 여 암호 복구를 사용 하도록 설정 된 `ForgotPassword` 의 동작 *Controllers/AccountController.cs*:</span><span class="sxs-lookup"><span data-stu-id="0b4e5-204">Enable password recovery by uncommenting the code in the `ForgotPassword` action of *Controllers/AccountController.cs*:</span></span>

[!code-csharp[](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=17-23&name=snippet_ForgotPassword)]

<span data-ttu-id="0b4e5-205">Form 요소에 주석 처리 제거 *Views/Account/ForgotPassword.cshtml*합니다.</span><span class="sxs-lookup"><span data-stu-id="0b4e5-205">Uncomment the form element in *Views/Account/ForgotPassword.cshtml*.</span></span> <span data-ttu-id="0b4e5-206">제거 하려는 경우도 `<p> For more information on how to enable reset password ... </p>` 이 문서에 대 한 링크를 포함 하는 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="0b4e5-206">You might want to remove the `<p> For more information on how to enable reset password ... </p>` element, which contains a link to this article.</span></span>

[!code-cshtml[](accconfirm/sample/WebApp1/Views/Account/ForgotPassword.cshtml?highlight=7-10,12,28)]

* * *
## <a name="register-confirm-email-and-reset-password"></a><span data-ttu-id="0b4e5-207">등록 전자 메일을 확인 하 고 암호를 다시 설정</span><span class="sxs-lookup"><span data-stu-id="0b4e5-207">Register, confirm email, and reset password</span></span>

<span data-ttu-id="0b4e5-208">웹 응용 프로그램을 실행 하 고 계정 확인 및 암호 복구 흐름을 테스트 합니다.</span><span class="sxs-lookup"><span data-stu-id="0b4e5-208">Run the web app, and test the account confirmation and password recovery flow.</span></span>

* <span data-ttu-id="0b4e5-209">응용 프로그램을 실행 하 고 새 사용자 등록</span><span class="sxs-lookup"><span data-stu-id="0b4e5-209">Run the app and register a new user</span></span>

  ![웹 응용 프로그램 계정 등록 보기](accconfirm/_static/loginaccconfirm1.png)

* <span data-ttu-id="0b4e5-211">계정 확인 링크에 대 한 전자 메일을 확인 합니다.</span><span class="sxs-lookup"><span data-stu-id="0b4e5-211">Check your email for the account confirmation link.</span></span> <span data-ttu-id="0b4e5-212">참조 [전자 메일을 디버그](#debug) 전자 메일을 얻지 못한 경우.</span><span class="sxs-lookup"><span data-stu-id="0b4e5-212">See [Debug email](#debug) if you don't get the email.</span></span>
* <span data-ttu-id="0b4e5-213">사용자의 전자 메일 확인에 대 한 링크를 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="0b4e5-213">Click the link to confirm your email.</span></span>
* <span data-ttu-id="0b4e5-214">전자 메일 및 암호를 로그인 합니다.</span><span class="sxs-lookup"><span data-stu-id="0b4e5-214">Log in with your email and password.</span></span>
* <span data-ttu-id="0b4e5-215">로그 오프 합니다.</span><span class="sxs-lookup"><span data-stu-id="0b4e5-215">Log off.</span></span>

### <a name="view-the-manage-page"></a><span data-ttu-id="0b4e5-216">관리 페이지 보기</span><span class="sxs-lookup"><span data-stu-id="0b4e5-216">View the manage page</span></span>

<span data-ttu-id="0b4e5-217">브라우저에서 자신의 사용자 이름을 선택: ![사용자 이름으로 브라우저 창](accconfirm/_static/un.png)</span><span class="sxs-lookup"><span data-stu-id="0b4e5-217">Select your user name in the browser: ![browser window with user name](accconfirm/_static/un.png)</span></span>

<span data-ttu-id="0b4e5-218">사용자 이름을 보려면 탐색 모음을 확장 해야 할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0b4e5-218">You might need to expand the navbar to see user name.</span></span>

![탐색 모음](accconfirm/_static/x.png)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="0b4e5-220">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="0b4e5-220">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="0b4e5-221">관리 페이지에 표시 됩니다는 **프로필** 탭이 선택 되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0b4e5-221">The manage page is displayed with the **Profile** tab selected.</span></span> <span data-ttu-id="0b4e5-222">**전자 메일** 확인 전자 메일을 나타내는 확인란을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="0b4e5-222">The **Email** shows a check box indicating the email has been confirmed.</span></span>

![관리 페이지](accconfirm/_static/rick2.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="0b4e5-224">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="0b4e5-224">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="0b4e5-225">이이 자습서의 뒷부분에 나오는 다룹니다.</span><span class="sxs-lookup"><span data-stu-id="0b4e5-225">This is mentioned later in the tutorial.</span></span>
<span data-ttu-id="0b4e5-226">![관리 페이지](accconfirm/_static/rick2.png)</span><span class="sxs-lookup"><span data-stu-id="0b4e5-226">![manage page](accconfirm/_static/rick2.png)</span></span>

---

### <a name="test-password-reset"></a><span data-ttu-id="0b4e5-227">테스트 암호 재설정</span><span class="sxs-lookup"><span data-stu-id="0b4e5-227">Test password reset</span></span>

* <span data-ttu-id="0b4e5-228">로그인 되어 있는 경우 선택 **로그 아웃**합니다.</span><span class="sxs-lookup"><span data-stu-id="0b4e5-228">If you're logged in, select **Logout**.</span></span>
* <span data-ttu-id="0b4e5-229">선택 된 **로그인** 연결 하 고 선택의 **암호를 잊으셨나요?** 링크 합니다.</span><span class="sxs-lookup"><span data-stu-id="0b4e5-229">Select the **Log in** link and select the **Forgot your password?** link.</span></span>
* <span data-ttu-id="0b4e5-230">계정 등록에 사용한 전자 메일을 입력 합니다.</span><span class="sxs-lookup"><span data-stu-id="0b4e5-230">Enter the email you used to register the account.</span></span>
* <span data-ttu-id="0b4e5-231">암호를 다시 설정에 대 한 링크가 포함 된 메일이 전송 됩니다.</span><span class="sxs-lookup"><span data-stu-id="0b4e5-231">An email with a link to reset your password is sent.</span></span> <span data-ttu-id="0b4e5-232">전자 메일을 확인 하 고 암호를 다시 설정에 대 한 링크를 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="0b4e5-232">Check your email and click the link to reset your password.</span></span> <span data-ttu-id="0b4e5-233">암호가 성공적으로 다시 설정, 전자 메일 및 새 암호를 사용 하 여 기록할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0b4e5-233">After your password has been successfully reset, you can log in with your email and new password.</span></span>

<a name="debug"></a>

### <a name="debug-email"></a><span data-ttu-id="0b4e5-234">전자 메일을 디버그</span><span class="sxs-lookup"><span data-stu-id="0b4e5-234">Debug email</span></span>

<span data-ttu-id="0b4e5-235">전자 메일 작업을 가져올 수 없습니다 하는 경우:</span><span class="sxs-lookup"><span data-stu-id="0b4e5-235">If you can't get email working:</span></span>

* <span data-ttu-id="0b4e5-236">만들기는 [전자 메일을 보내는 콘솔 응용 프로그램](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html)합니다.</span><span class="sxs-lookup"><span data-stu-id="0b4e5-236">Create a [console app to send email](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html).</span></span>
* <span data-ttu-id="0b4e5-237">검토는 [전자 메일 작업](https://sendgrid.com/docs/User_Guide/email_activity.html) 페이지.</span><span class="sxs-lookup"><span data-stu-id="0b4e5-237">Review the [Email Activity](https://sendgrid.com/docs/User_Guide/email_activity.html) page.</span></span>
* <span data-ttu-id="0b4e5-238">스팸 폴더를 확인 합니다.</span><span class="sxs-lookup"><span data-stu-id="0b4e5-238">Check your spam folder.</span></span>
* <span data-ttu-id="0b4e5-239">다른 전자 메일 별칭에는 다른 전자 메일 공급자 (Microsoft, Yahoo, Gmail 등)를 시도 하십시오.</span><span class="sxs-lookup"><span data-stu-id="0b4e5-239">Try another email alias on a different email provider (Microsoft, Yahoo, Gmail, etc.)</span></span>
* <span data-ttu-id="0b4e5-240">다른 전자 메일 주소로 보내 보십시오.</span><span class="sxs-lookup"><span data-stu-id="0b4e5-240">Try sending to different email accounts.</span></span>

<span data-ttu-id="0b4e5-241">**보안 모범 사례** 를 **하지** 테스트 및 개발에서 생산 암호를 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="0b4e5-241">**A security best practice** is to **not** use production secrets in test and development.</span></span> <span data-ttu-id="0b4e5-242">Azure에 응용 프로그램을 게시 하는 경우에 Azure 웹 앱 포털에서 응용 프로그램 설정으로 SendGrid 암호를 설정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0b4e5-242">If you publish the app to Azure, you can set the SendGrid secrets as application settings in the Azure Web App portal.</span></span> <span data-ttu-id="0b4e5-243">구성 시스템 환경 변수에서 키를 읽을 수 설정 됩니다.</span><span class="sxs-lookup"><span data-stu-id="0b4e5-243">The configuration system is set up to read keys from environment variables.</span></span>

## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="0b4e5-244">공유 및 로컬 로그인 계정을 결합합니다</span><span class="sxs-lookup"><span data-stu-id="0b4e5-244">Combine social and local login accounts</span></span>

<span data-ttu-id="0b4e5-245">이 섹션을 완료 하려면 외부 인증 공급자를 먼저 활성화 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="0b4e5-245">To complete this section, you must first enable an external authentication provider.</span></span> <span data-ttu-id="0b4e5-246">참조 [Facebook, Google, 및 외부 공급자 인증](xref:security/authentication/social/index)합니다.</span><span class="sxs-lookup"><span data-stu-id="0b4e5-246">See [Facebook, Google, and external provider authentication](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="0b4e5-247">사용자 전자 메일 링크를 클릭 하 여 로컬 및 소셜 계정을 결합할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0b4e5-247">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="0b4e5-248">그러나 다음 순서로 "RickAndMSFT@gmail.com"; 로컬 로그인으로 처음 만들어질를 만들어 계정을 소셜 로그인으로 먼저, 다음 로컬 로그인을 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="0b4e5-248">In the following sequence, "RickAndMSFT@gmail.com" is first created as a local login; however, you can create the account as a social login first, then add a local login.</span></span>

![웹 응용 프로그램: RickAndMSFT@gmail.com 인증 된 사용자](accconfirm/_static/rick.png)

<span data-ttu-id="0b4e5-250">클릭는 **관리** 링크 합니다.</span><span class="sxs-lookup"><span data-stu-id="0b4e5-250">Click on the **Manage** link.</span></span> <span data-ttu-id="0b4e5-251">이 계정에 연결 된 참고 0 외부 (소셜 로그인)</span><span class="sxs-lookup"><span data-stu-id="0b4e5-251">Note the 0 external (social logins) associated with this account.</span></span>

![보기를 관리](accconfirm/_static/manage.png)

<span data-ttu-id="0b4e5-253">다른 로그인 서비스에 대 한 링크를 클릭 하 고 응용 프로그램 요청을 수락 합니다.</span><span class="sxs-lookup"><span data-stu-id="0b4e5-253">Click the link to another login service and accept the app requests.</span></span> <span data-ttu-id="0b4e5-254">다음 그림에서는 Facebook 외부 인증 공급자입니다.</span><span class="sxs-lookup"><span data-stu-id="0b4e5-254">In the following image, Facebook is the external authentication provider:</span></span>

![Facebook을 나열 하 여 외부 로그인 보기를 관리 합니다.](accconfirm/_static/fb.png)

<span data-ttu-id="0b4e5-256">두 개의 계정은 결합 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="0b4e5-256">The two accounts have been combined.</span></span> <span data-ttu-id="0b4e5-257">두 계정으로 로그온 할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0b4e5-257">You are able to log on with either account.</span></span> <span data-ttu-id="0b4e5-258">쓰지만 대개를 분실 했다고 액세스 소셜 자신의 계정으로 되거나 자신의 소셜 로그인 인증 서비스가 다운 된 경우 로컬 계정을 추가 하려면 사용자가 할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0b4e5-258">You might want your users to add local accounts in case their social login authentication service is down, or more likely they've lost access to their social account.</span></span>

## <a name="enable-account-confirmation-after-a-site-has-users"></a><span data-ttu-id="0b4e5-259">사이트에 사용자가 만든 후 계정 확인을 사용 하도록 설정</span><span class="sxs-lookup"><span data-stu-id="0b4e5-259">Enable account confirmation after a site has users</span></span>

<span data-ttu-id="0b4e5-260">사이트에서 사용할 수 있도록 계정 확인 사용자와 모든 기존 사용자가 잠급니다.</span><span class="sxs-lookup"><span data-stu-id="0b4e5-260">Enabling account confirmation on a site with users locks out all the existing users.</span></span> <span data-ttu-id="0b4e5-261">자신의 계정을 확인 하지 때문에 기존 사용자가 잠겨 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0b4e5-261">Existing users are locked out because their accounts aren't confirmed.</span></span> <span data-ttu-id="0b4e5-262">사용자 잠금 종료를 해결 하려면 다음 방법 중 하나를 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="0b4e5-262">To work around exiting user lockout, use one of the following approaches:</span></span>

* <span data-ttu-id="0b4e5-263">기존의 모든 사용자로 확인 되 고 표시 하도록 데이터베이스를 업데이트</span><span class="sxs-lookup"><span data-stu-id="0b4e5-263">Update the database to mark all existing users as being confirmed</span></span>
* <span data-ttu-id="0b4e5-264">기존 사용자를 확인 합니다.</span><span class="sxs-lookup"><span data-stu-id="0b4e5-264">Confirm exiting users.</span></span> <span data-ttu-id="0b4e5-265">예를 들어 일괄 처리 send 확인 링크가 포함 된 전자 메일입니다.</span><span class="sxs-lookup"><span data-stu-id="0b4e5-265">For example, batch-send emails with confirmation links.</span></span>
