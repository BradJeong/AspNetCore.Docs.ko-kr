---
title: "마이그레이션 인증 및 Id"
author: ardalis
description: 
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/identity
ms.openlocfilehash: f02d9472ea0aa1dceae3f53c812776aab85ab54e
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/30/2018
---
# <a name="migrating-authentication-and-identity"></a><span data-ttu-id="1c1dc-102">마이그레이션 인증 및 Id</span><span class="sxs-lookup"><span data-stu-id="1c1dc-102">Migrating Authentication and Identity</span></span>

<a name="migration-identity"></a>

<span data-ttu-id="1c1dc-103">으로 [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="1c1dc-103">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="1c1dc-104">이전 문서에서는 [ASP.NET MVC 프로젝트에서 ASP.NET Core MVC 구성을 마이그레이션할](configuration.md)합니다.</span><span class="sxs-lookup"><span data-stu-id="1c1dc-104">In the previous article we [migrated configuration from an ASP.NET MVC project to ASP.NET Core MVC](configuration.md).</span></span> <span data-ttu-id="1c1dc-105">이 문서에서는 등록, 로그인 및 사용자 관리 기능을 마이그레이션할 했습니다.</span><span class="sxs-lookup"><span data-stu-id="1c1dc-105">In this article, we migrate the registration, login, and user management features.</span></span>

## <a name="configure-identity-and-membership"></a><span data-ttu-id="1c1dc-106">Id 및 멤버 자격을 구성 합니다.</span><span class="sxs-lookup"><span data-stu-id="1c1dc-106">Configure Identity and Membership</span></span>

<span data-ttu-id="1c1dc-107">ASP.NET MVC에서 인증 및 id 기능 Startup.Auth.cs 및 App_Start 폴더에 있는 IdentityConfig.cs ASP.NET Identity를 사용 하 여 구성 됩니다.</span><span class="sxs-lookup"><span data-stu-id="1c1dc-107">In ASP.NET MVC, authentication and identity features are configured using ASP.NET Identity in Startup.Auth.cs and IdentityConfig.cs, located in the App_Start folder.</span></span> <span data-ttu-id="1c1dc-108">ASP.NET Core MVC에서 이러한 기능에 구성 된 *Startup.cs*합니다.</span><span class="sxs-lookup"><span data-stu-id="1c1dc-108">In ASP.NET Core MVC, these features are configured in *Startup.cs*.</span></span>

<span data-ttu-id="1c1dc-109">설치는 `Microsoft.AspNetCore.Identity.EntityFrameworkCore` 및 `Microsoft.AspNetCore.Authentication.Cookies` NuGet 패키지 합니다.</span><span class="sxs-lookup"><span data-stu-id="1c1dc-109">Install the `Microsoft.AspNetCore.Identity.EntityFrameworkCore` and `Microsoft.AspNetCore.Authentication.Cookies` NuGet packages.</span></span>

<span data-ttu-id="1c1dc-110">그런 다음 Startup.cs 및 update를 열고는 `ConfigureServices()` Entity Framework 및 Id 서비스를 사용 하는 메서드:</span><span class="sxs-lookup"><span data-stu-id="1c1dc-110">Then, open Startup.cs and update the `ConfigureServices()` method to use Entity Framework and Identity services:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
  // Add EF services to the services container.
  services.AddEntityFramework(Configuration)
    .AddSqlServer()
    .AddDbContext<ApplicationDbContext>();

  // Add Identity services to the services container.
  services.AddIdentity<ApplicationUser, IdentityRole>(Configuration)
    .AddEntityFrameworkStores<ApplicationDbContext>();

  services.AddMvc();
}
```

<span data-ttu-id="1c1dc-111">이 시점에서 ASP.NET MVC 프로젝트에서 사용한 아직 하지 않은 위의 코드에서 참조 하는 형식은 두 가지가: `ApplicationDbContext` 및 `ApplicationUser`합니다.</span><span class="sxs-lookup"><span data-stu-id="1c1dc-111">At this point, there are two types referenced in the above code that we haven't yet migrated from the ASP.NET MVC project: `ApplicationDbContext` and `ApplicationUser`.</span></span> <span data-ttu-id="1c1dc-112">새 *모델* 폴더에서 ASP.NET Core 프로젝트를 하 고 이러한 형식에 해당를 두 개의 클래스를 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="1c1dc-112">Create a new *Models* folder in the ASP.NET Core project, and add two classes to it corresponding to these types.</span></span> <span data-ttu-id="1c1dc-113">ASP.NET MVC에서 이러한 클래스의 버전을 찾이 됩니다 `/Models/IdentityModels.cs`, 있지만 명확 하 게 되므로 마이그레이션된 프로젝트에서 클래스 마다 파일 하나를 사용 하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="1c1dc-113">You will find the ASP.NET MVC versions of these classes in `/Models/IdentityModels.cs`, but we will use one file per class in the migrated project since that's more clear.</span></span>

<span data-ttu-id="1c1dc-114">ApplicationUser.cs:</span><span class="sxs-lookup"><span data-stu-id="1c1dc-114">ApplicationUser.cs:</span></span>

```csharp
using Microsoft.AspNetCore.Identity.EntityFrameworkCore;

namespace NewMvc6Project.Models
{
  public class ApplicationUser : IdentityUser
  {
  }
}
```

<span data-ttu-id="1c1dc-115">ApplicationDbContext.cs:</span><span class="sxs-lookup"><span data-stu-id="1c1dc-115">ApplicationDbContext.cs:</span></span>

```csharp
using Microsoft.AspNetCore.Identity.EntityFramework;
using Microsoft.Data.Entity;

namespace NewMvc6Project.Models
{
  public class ApplicationDbContext : IdentityDbContext<ApplicationUser>
  {
    public ApplicationDbContext()
    {
      Database.EnsureCreated();
    }

    protected override void OnConfiguring(DbContextOptionsBuilder options)
    {
      options.UseSqlServer();
    }
  }
}
```

<span data-ttu-id="1c1dc-116">ASP.NET Core MVC 시작 웹 프로젝트는 사용자의 많은 사용자 지정 또는 ApplicationDbContext 포함 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="1c1dc-116">The ASP.NET Core MVC Starter Web project doesn't include much customization of users, or the ApplicationDbContext.</span></span> <span data-ttu-id="1c1dc-117">실제 응용 프로그램을 마이그레이션할 때는 응용 프로그램 (예를 들어 프로그램 DbContext에 경우 DbSet활용모델클래스뿐만아니라응용프로그램의사용자및DbContext클래스의메서드와사용자지정속성을모두마이그레이션할해야합니다.<Album>, 앨범 클래스를 마이그레이션해야 할 물론).</span><span class="sxs-lookup"><span data-stu-id="1c1dc-117">When migrating a real application, you will also need to migrate all of the custom properties and methods of your application's user and DbContext classes, as well as any other Model classes your application utilizes (for example, if your DbContext has a DbSet<Album>, you will of course need to migrate the Album class).</span></span>

<span data-ttu-id="1c1dc-118">사용 하 여 해당 업데이트 하 여 컴파일하는 데 만들 있습니다 Startup.cs 파일 위치에 이러한 파일을 통해 문:</span><span class="sxs-lookup"><span data-stu-id="1c1dc-118">With these files in place, the Startup.cs file can be made to compile by updating its using statements:</span></span>

```csharp
using Microsoft.Framework.ConfigurationModel;
using Microsoft.AspNetCore.Hosting;
using NewMvc6Project.Models;
using Microsoft.AspNetCore.Identity;
```

<span data-ttu-id="1c1dc-119">응용 프로그램 인증 및 id 서비스를 지원 하도록 준비 되었습니다.-사용자에 게 이러한 기능을 갖추고 하기만 합니다.</span><span class="sxs-lookup"><span data-stu-id="1c1dc-119">Our application is now ready to support authentication and identity services - it just needs to have these features exposed to users.</span></span>

## <a name="migrate-registration-and-login-logic"></a><span data-ttu-id="1c1dc-120">등록 및 로그인 논리 마이그레이션</span><span class="sxs-lookup"><span data-stu-id="1c1dc-120">Migrate Registration and Login Logic</span></span>

<span data-ttu-id="1c1dc-121">응용 프로그램 및 Entity Framework 및 SQL Server를 사용 하 여 구성 하는 데이터 액세스에 대해 구성 하는 id 서비스를 우리 준비가 이제 응용 프로그램에 등록 및 로그인에 대 한 지원을 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="1c1dc-121">With identity services configured for the application and data access configured using Entity Framework and SQL Server, we are now ready to add support for registration and login to the application.</span></span> <span data-ttu-id="1c1dc-122">이전에 설명한 대로 [마이그레이션 프로세스의 앞부분에 나오는](mvc.md#migrate-layout-file) _LoginPartial _Layout.cshtml에 대 한 참조를 주석으로 했습니다.</span><span class="sxs-lookup"><span data-stu-id="1c1dc-122">Recall that [earlier in the migration process](mvc.md#migrate-layout-file) we commented out a reference to _LoginPartial in _Layout.cshtml.</span></span> <span data-ttu-id="1c1dc-123">이제 해당 코드를 반환 합니다. 주석 처리를 제거, 로그인 기능을 지 원하는 데 필요한 컨트롤러를 추가 하는 시간입니다.</span><span class="sxs-lookup"><span data-stu-id="1c1dc-123">Now it's time to return to that code, uncomment it, and add in the necessary controllers and views to support login functionality.</span></span>

<span data-ttu-id="1c1dc-124">_Layout.cshtml; 업데이트 주석 처리 제거는 @Html.Partial 줄:</span><span class="sxs-lookup"><span data-stu-id="1c1dc-124">Update _Layout.cshtml; uncomment the @Html.Partial line:</span></span>

```cshtml
      <li>@Html.ActionLink("Contact", "Contact", "Home")</li>
    </ul>
    @*@Html.Partial("_LoginPartial")*@
  </div>
</div>
```

<span data-ttu-id="1c1dc-125">이제, _LoginPartial 호출 뷰/공유 폴더를 새 MVC 뷰 페이지를 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="1c1dc-125">Now, add a new MVC View Page called _LoginPartial to the Views/Shared folder:</span></span>

<span data-ttu-id="1c1dc-126">다음 코드로 _LoginPartial.cshtml 업데이트 (모든 내용을 바꾸기):</span><span class="sxs-lookup"><span data-stu-id="1c1dc-126">Update _LoginPartial.cshtml with the following code (replace all of its contents):</span></span>

```cshtml
@inject SignInManager<User> SignInManager
@inject UserManager<User> UserManager

@if (SignInManager.IsSignedIn(User))
{
    <form asp-area="" asp-controller="Account" asp-action="LogOff" method="post" id="logoutForm" class="navbar-right">
        <ul class="nav navbar-nav navbar-right">
            <li>
                <a asp-area="" asp-controller="Manage" asp-action="Index" title="Manage">Hello @UserManager.GetUserName(User)!</a>
            </li>
            <li>
                <button type="submit" class="btn btn-link navbar-btn navbar-link">Log off</button>
            </li>
        </ul>
    </form>
}
else
{
    <ul class="nav navbar-nav navbar-right">
        <li><a asp-area="" asp-controller="Account" asp-action="Register">Register</a></li>
        <li><a asp-area="" asp-controller="Account" asp-action="Login">Log in</a></li>
    </ul>
}
```

<span data-ttu-id="1c1dc-127">이 시점에서 브라우저에서 사이트를 새로 고칠 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="1c1dc-127">At this point, you should be able to refresh the site in your browser.</span></span>

## <a name="summary"></a><span data-ttu-id="1c1dc-128">요약</span><span class="sxs-lookup"><span data-stu-id="1c1dc-128">Summary</span></span>

<span data-ttu-id="1c1dc-129">ASP.NET Core에서는 ASP.NET Id 기능을 변경 합니다.</span><span class="sxs-lookup"><span data-stu-id="1c1dc-129">ASP.NET Core introduces changes to the ASP.NET Identity features.</span></span> <span data-ttu-id="1c1dc-130">이 문서에서 ASP.NET Core에는 ASP.NET Identity의 인증 및 사용자 관리 기능을 마이그레이션하는 방법에 알아보았습니다.</span><span class="sxs-lookup"><span data-stu-id="1c1dc-130">In this article, you have seen how to migrate the authentication and user management features of an ASP.NET Identity to ASP.NET Core.</span></span>
