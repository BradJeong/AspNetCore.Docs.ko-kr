---
title: ASP.NET Core에서 multi-factor authentication
author: damienbod
description: ASP.NET Core 앱에서 MFA (multi-factor authentication)를 설정 하는 방법에 대해 알아봅니다.
monikerRange: '>= aspnetcore-3.1'
ms.author: rick-anderson
ms.custom: mvc
ms.date: 03/17/2020
no-loc:
- Identity
uid: security/authentication/mfa
ms.openlocfilehash: 6220688d53f0718ca5be5f63dd5d9539d37e2391
ms.sourcegitcommit: d64ef143c64ee4fdade8f9ea0b753b16752c5998
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/18/2020
ms.locfileid: "79520148"
---
# <a name="multi-factor-authentication-in-aspnet-core"></a><span data-ttu-id="01251-103">ASP.NET Core에서 multi-factor authentication</span><span class="sxs-lookup"><span data-stu-id="01251-103">Multi-factor authentication in ASP.NET Core</span></span>

<span data-ttu-id="01251-104">[Damien Bowden](https://github.com/damienbod)</span><span class="sxs-lookup"><span data-stu-id="01251-104">By [Damien Bowden](https://github.com/damienbod)</span></span>

<span data-ttu-id="01251-105">MFA (multi-factor authentication)는 추가 형태의 식별을 위해 로그인 이벤트 중에 사용자가 요청 되는 프로세스입니다.</span><span class="sxs-lookup"><span data-stu-id="01251-105">Multi-factor authentication (MFA) is a process in which a user is requested during a sign-in event for additional forms of identification.</span></span> <span data-ttu-id="01251-106">이 메시지는 휴대폰에서 코드를 입력 하거나, FIDO2 키를 사용 하거나, 지문 검색을 제공 하는 것일 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="01251-106">This prompt could be to enter a code from a cellphone, use a FIDO2 key, or to provide a fingerprint scan.</span></span> <span data-ttu-id="01251-107">두 번째 인증 형식이 필요한 경우 보안이 강화 됩니다.</span><span class="sxs-lookup"><span data-stu-id="01251-107">When you require a second form of authentication, security is enhanced.</span></span> <span data-ttu-id="01251-108">공격자가 추가 요소를 쉽게 얻거나 중복 하는 것은 아닙니다.</span><span class="sxs-lookup"><span data-stu-id="01251-108">The additional factor isn't easily obtained or duplicated by an attacker.</span></span>

<span data-ttu-id="01251-109">이 문서에서는 다음 영역에 대해 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="01251-109">This article covers the following areas:</span></span>

* <span data-ttu-id="01251-110">MFA 정의 및 권장 되는 MFA 흐름</span><span class="sxs-lookup"><span data-stu-id="01251-110">What is MFA and what MFA flows are recommended</span></span>
* <span data-ttu-id="01251-111">ASP.NET Core Identity를 사용 하 여 관리 페이지에 대 한 MFA 구성</span><span class="sxs-lookup"><span data-stu-id="01251-111">Configure MFA for administration pages using ASP.NET Core Identity</span></span>
* <span data-ttu-id="01251-112">Openid connect Connect 서버에 MFA 로그인 요구 사항 보내기</span><span class="sxs-lookup"><span data-stu-id="01251-112">Send MFA sign-in requirement to OpenID Connect server</span></span>
* <span data-ttu-id="01251-113">강제로 ASP.NET Core Openid connect Connect 클라이언트에 MFA를 요구 합니다.</span><span class="sxs-lookup"><span data-stu-id="01251-113">Force ASP.NET Core OpenID Connect client to require MFA</span></span>

## <a name="mfa-2fa"></a><span data-ttu-id="01251-114">MFA, 2FA</span><span class="sxs-lookup"><span data-stu-id="01251-114">MFA, 2FA</span></span>

<span data-ttu-id="01251-115">MFA를 사용 하려면 사용자가 알고 있는 것과 같은 id에 대 한 둘 이상의 증명 유형이 필요 하거나 사용자가 인증을 받을 수 있도록 생체 인식 유효성 검사가 필요 합니다.</span><span class="sxs-lookup"><span data-stu-id="01251-115">MFA requires at least two or more types of proof for an identity like something you know, something you possess, or biometric validation for the user to authenticate.</span></span>

<span data-ttu-id="01251-116">2 단계 인증 (2FA)은 MFA의 하위 집합과 유사 하지만 MFA가 id를 증명 하는 두 개 이상의 요인을 요구할 수 있다는 차이점이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="01251-116">Two-factor authentication (2FA) is like a subset of MFA, but the difference being that MFA can require two or more factors to prove the identity.</span></span>

### <a name="mfa-totp-time-based-one-time-password-algorithm"></a><span data-ttu-id="01251-117">MFA TOTP (시간 기반 일회용 암호 알고리즘)</span><span class="sxs-lookup"><span data-stu-id="01251-117">MFA TOTP (Time-based One-time Password Algorithm)</span></span>

<span data-ttu-id="01251-118">TOTP를 사용 하는 MFA는 ASP.NET Core Identity를 사용 하는 지원 되는 구현입니다.</span><span class="sxs-lookup"><span data-stu-id="01251-118">MFA using TOTP is a supported implementation using ASP.NET Core Identity.</span></span> <span data-ttu-id="01251-119">다음을 포함 하 여 모든 규격 인증자 앱과 함께 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="01251-119">This can be used together with any compliant authenticator app, including:</span></span>

* <span data-ttu-id="01251-120">Microsoft Authenticator 앱</span><span class="sxs-lookup"><span data-stu-id="01251-120">Microsoft Authenticator App</span></span>
* <span data-ttu-id="01251-121">Google Authenticator 앱</span><span class="sxs-lookup"><span data-stu-id="01251-121">Google Authenticator App</span></span>

<span data-ttu-id="01251-122">구현 세부 정보는 다음 링크를 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="01251-122">See the following link for implementation details:</span></span>

[<span data-ttu-id="01251-123">ASP.NET Core에서 TOTP authenticator 앱에 대 한 QR 코드 생성 사용</span><span class="sxs-lookup"><span data-stu-id="01251-123">Enable QR Code generation for TOTP authenticator apps in ASP.NET Core</span></span>](xref:security/authentication/identity-enable-qrcodes)

### <a name="mfa-fido2-or-passwordless"></a><span data-ttu-id="01251-124">MFA FIDO2 또는 암호 없는</span><span class="sxs-lookup"><span data-stu-id="01251-124">MFA FIDO2 or passwordless</span></span>

<span data-ttu-id="01251-125">FIDO2은 현재 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="01251-125">FIDO2 is currently:</span></span>

* <span data-ttu-id="01251-126">MFA를 가장 안전 하 게 달성 하는 방법입니다.</span><span class="sxs-lookup"><span data-stu-id="01251-126">The most secure way of achieving MFA.</span></span>
* <span data-ttu-id="01251-127">피싱 공격 으로부터 보호 하는 유일한 MFA 흐름입니다.</span><span class="sxs-lookup"><span data-stu-id="01251-127">The only MFA flow that protects against phishing attacks.</span></span>

<span data-ttu-id="01251-128">현재 ASP.NET Core는 FIDO2을 직접 지원 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="01251-128">At present, ASP.NET Core doesn't support FIDO2 directly.</span></span> <span data-ttu-id="01251-129">FIDO2는 MFA 또는 암호 없는 흐름에 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="01251-129">FIDO2 can be used for MFA or passwordless flows.</span></span>

<span data-ttu-id="01251-130">Azure Active Directory는 FIDO2 및 암호 없는 흐름에 대 한 지원을 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="01251-130">Azure Active Directory provides support for FIDO2 and passwordless flows.</span></span> <span data-ttu-id="01251-131">자세한 내용은 [Azure Active Directory에 대 한 암호 없는 인증 옵션](/azure/active-directory/authentication/concept-authentication-passwordless)을 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="01251-131">For more information, see [Passwordless authentication options for Azure Active Directory](/azure/active-directory/authentication/concept-authentication-passwordless).</span></span>

### <a name="mfa-sms"></a><span data-ttu-id="01251-132">MFA SMS</span><span class="sxs-lookup"><span data-stu-id="01251-132">MFA SMS</span></span>

<span data-ttu-id="01251-133">SMS를 사용 하는 MFA는 암호 인증 (단일 요소)과 비교 하 여 보안 대규모 높아집니다.</span><span class="sxs-lookup"><span data-stu-id="01251-133">MFA with SMS increases security massively compared with password authentication (single factor).</span></span> <span data-ttu-id="01251-134">그러나 SMS를 두 번째 요소로 사용 하는 것은 더 이상 권장 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="01251-134">However, using SMS as a second factor is no longer recommended.</span></span> <span data-ttu-id="01251-135">이 형식의 구현에 대해 알려진 공격 벡터가 너무 많습니다.</span><span class="sxs-lookup"><span data-stu-id="01251-135">Too many known attack vectors exist for this type of implementation.</span></span>

[<span data-ttu-id="01251-136">NIST 지침</span><span class="sxs-lookup"><span data-stu-id="01251-136">NIST guidelines</span></span>](https://pages.nist.gov/800-63-3/sp800-63b.html)

## <a name="configure-mfa-for-administration-pages-using-aspnet-core-opno-locidentity"></a><span data-ttu-id="01251-137">ASP.NET Core Identity를 사용 하 여 관리 페이지에 대 한 MFA 구성</span><span class="sxs-lookup"><span data-stu-id="01251-137">Configure MFA for administration pages using ASP.NET Core Identity</span></span>

<span data-ttu-id="01251-138">사용자가 ASP.NET Core Identity 앱 내에서 중요 한 페이지에 액세스할 수 있도록 MFA를 강제할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="01251-138">MFA could be forced on users to access sensitive pages within an ASP.NET Core Identity app.</span></span> <span data-ttu-id="01251-139">이는 다양 한 id에 대해 서로 다른 수준의 액세스 권한이 있는 앱에 유용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="01251-139">This could be useful for apps where different levels of access exist for the different identities.</span></span> <span data-ttu-id="01251-140">예를 들어 사용자가 암호 로그인을 사용 하 여 프로필 데이터를 볼 수 있지만 관리자가 MFA를 사용 하 여 관리 페이지에 액세스 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="01251-140">For example, users might be able to view the profile data using a password login, but an administrator would be required to use MFA to access the administrative pages.</span></span>

### <a name="extend-the-login-with-an-mfa-claim"></a><span data-ttu-id="01251-141">MFA 클레임을 사용 하 여 로그인 확장</span><span class="sxs-lookup"><span data-stu-id="01251-141">Extend the login with an MFA claim</span></span>

<span data-ttu-id="01251-142">데모 코드는 Identity 및 Razor Pages와 ASP.NET Core를 사용 하 여 설정 됩니다.</span><span class="sxs-lookup"><span data-stu-id="01251-142">The demo code is setup using ASP.NET Core with Identity and Razor Pages.</span></span> <span data-ttu-id="01251-143">`AddIdentity` 메서드는 하나 `AddDefaultIdentity` 대신 사용 되므로 성공적인 로그인 후에 `IUserClaimsPrincipalFactory` 구현을 사용 하 여 클레임을 id에 추가할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="01251-143">The `AddIdentity` method is used instead of `AddDefaultIdentity` one, so an `IUserClaimsPrincipalFactory` implementation can be used to add claims to the identity after a successful login.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseSqlite(
            Configuration.GetConnectionString("DefaultConnection")));
    
    services.AddIdentity<IdentityUser, IdentityRole>(
            options => options.SignIn.RequireConfirmedAccount = false)
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

    services.AddSingleton<IEmailSender, EmailSender>();
    services.AddScoped<IUserClaimsPrincipalFactory<IdentityUser>, 
        AdditionalUserClaimsPrincipalFactory>();

    services.AddAuthorization(options =>
        options.AddPolicy("TwoFactorEnabled",
            x => x.RequireClaim("amr", "mfa")));

    services.AddRazorPages();
}
```

<span data-ttu-id="01251-144">`AdditionalUserClaimsPrincipalFactory` 클래스는 로그인에 성공한 후에만 사용자 클레임에 `amr` 클레임을 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="01251-144">The `AdditionalUserClaimsPrincipalFactory` class adds the `amr` claim to the user claims only after a successful login.</span></span> <span data-ttu-id="01251-145">클레임의 값을 데이터베이스에서 읽습니다.</span><span class="sxs-lookup"><span data-stu-id="01251-145">The claim's value is read from the database.</span></span> <span data-ttu-id="01251-146">Id가 MFA로 로그인 한 경우에는 사용자가 더 높은 수준의 보호 된 보기에만 액세스 해야 하므로 클레임은 여기에 추가 됩니다.</span><span class="sxs-lookup"><span data-stu-id="01251-146">The claim is added here because the user should only access the higher protected view if the identity has logged in with MFA.</span></span> <span data-ttu-id="01251-147">클레임을 사용 하는 대신 데이터베이스에서 직접 데이터베이스 뷰를 읽는 경우 MFA를 활성화 한 후 MFA를 사용 하지 않고 보기에 직접 액세스할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="01251-147">If the database view is read from the database directly instead of using the claim, it's possible to access the view without MFA directly after activating the MFA.</span></span>

```csharp
using Microsoft.AspNetCore.Identity;
using Microsoft.Extensions.Options;
using System.Collections.Generic;
using System.Security.Claims;
using System.Threading.Tasks;

namespace IdentityStandaloneMfa
{
    public class AdditionalUserClaimsPrincipalFactory : 
        UserClaimsPrincipalFactory<IdentityUser, IdentityRole>
    {
        public AdditionalUserClaimsPrincipalFactory( 
            UserManager<IdentityUser> userManager,
            RoleManager<IdentityRole> roleManager, 
            IOptions<IdentityOptions> optionsAccessor) 
            : base(userManager, roleManager, optionsAccessor)
        {
        }

        public async override Task<ClaimsPrincipal> CreateAsync(IdentityUser user)
        {
            var principal = await base.CreateAsync(user);
            var identity = (ClaimsIdentity)principal.Identity;

            var claims = new List<Claim>();

            if (user.TwoFactorEnabled)
            {
                claims.Add(new Claim("amr", "mfa"));
            }
            else
            {
                claims.Add(new Claim("amr", "pwd"));
            }

            identity.AddClaims(claims);
            return principal;
        }
    }
}
```

<span data-ttu-id="01251-148">`Startup` 클래스에서 Identity 서비스 설정이 변경 되었으므로 Identity의 레이아웃을 업데이트 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="01251-148">Because the Identity service setup changed in the `Startup` class, the layouts of the Identity need to be updated.</span></span> <span data-ttu-id="01251-149">Identity 페이지를 앱에 스 캐 폴드.</span><span class="sxs-lookup"><span data-stu-id="01251-149">Scaffold the Identity pages into the app.</span></span> <span data-ttu-id="01251-150">*Identity/Account/Manage/_Layout* 파일에 레이아웃을 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="01251-150">Define the layout in the *Identity/Account/Manage/_Layout.cshtml* file.</span></span>

```cshtml
@{
    Layout = "/Pages/Shared/_Layout.cshtml";
}
```

<span data-ttu-id="01251-151">또한 Identity 페이지에서 모든 관리 페이지에 대 한 레이아웃을 할당 합니다.</span><span class="sxs-lookup"><span data-stu-id="01251-151">Also assign the layout for all the manage pages from the Identity pages:</span></span>

```cshtml
@{
    Layout = "_Layout.cshtml";
}
```

### <a name="validate-the-mfa-requirement-in-the-administration-page"></a><span data-ttu-id="01251-152">관리 페이지에서 MFA 요구 사항의 유효성을 검사 합니다.</span><span class="sxs-lookup"><span data-stu-id="01251-152">Validate the MFA requirement in the administration page</span></span>

<span data-ttu-id="01251-153">관리 Razor 페이지에서는 사용자가 MFA를 사용 하 여 로그인 했는지 확인 합니다.</span><span class="sxs-lookup"><span data-stu-id="01251-153">The administration Razor Page validates that the user has logged in using MFA.</span></span> <span data-ttu-id="01251-154">`OnGet` 메서드에서 id는 사용자 클레임에 액세스 하는 데 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="01251-154">In the `OnGet` method, the identity is used to access the user claims.</span></span> <span data-ttu-id="01251-155">`mfa`값에 대해 `amr` 클레임이 확인 됩니다.</span><span class="sxs-lookup"><span data-stu-id="01251-155">The `amr` claim is checked for the value `mfa`.</span></span> <span data-ttu-id="01251-156">Id가이 클레임을 누락 하거나 `false`된 경우 페이지는 MFA 사용 페이지로 리디렉션됩니다.</span><span class="sxs-lookup"><span data-stu-id="01251-156">If the identity is missing this claim or is `false`, the page redirects to the Enable MFA page.</span></span> <span data-ttu-id="01251-157">사용자가 이미 로그인 했지만 MFA를 사용 하지 않았기 때문에이를 수행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="01251-157">This is possible because the user has logged in already, but without MFA.</span></span>

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Mvc;
using Microsoft.AspNetCore.Mvc.RazorPages;

namespace IdentityStandaloneMfa
{
    public class AdminModel : PageModel
    {
        public IActionResult OnGet()
        {
            var claimTwoFactorEnabled = 
                User.Claims.FirstOrDefault(t => t.Type == "amr");

            if (claimTwoFactorEnabled != null && 
                "mfa".Equals(claimTwoFactorEnabled.Value))
            {
                // You logged in with MFA, do the administrative stuff
            }
            else
            {
                return Redirect(
                    "/Identity/Account/Manage/TwoFactorAuthentication");
            }

            return Page();
        }
    }
}
```

### <a name="ui-logic-to-toggle-user-login-information"></a><span data-ttu-id="01251-158">사용자 로그인 정보를 설정/해제 하는 UI 논리</span><span class="sxs-lookup"><span data-stu-id="01251-158">UI logic to toggle user login information</span></span>

<span data-ttu-id="01251-159">시작 시 권한 부여 정책이 추가 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="01251-159">An authorization policy was added at startup.</span></span> <span data-ttu-id="01251-160">정책에 `mfa`값이 `amr` 클레임이 필요 합니다.</span><span class="sxs-lookup"><span data-stu-id="01251-160">The policy requires the `amr` claim with the value `mfa`.</span></span>

```csharp
services.AddAuthorization(options =>
    options.AddPolicy("TwoFactorEnabled",
        x => x.RequireClaim("amr", "mfa")));
```

<span data-ttu-id="01251-161">그런 다음 `_Layout` 보기에서이 정책을 사용 하 여 경고와 함께 **관리자** 메뉴를 표시 하거나 숨길 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="01251-161">This policy can then be used in the `_Layout` view to show or hide the **Admin** menu with the warning:</span></span>

```cshtml
@using Microsoft.AspNetCore.Authorization
@using Microsoft.AspNetCore.Identity
@inject SignInManager<IdentityUser> SignInManager
@inject UserManager<IdentityUser> UserManager
@inject IAuthorizationService AuthorizationService
```

<span data-ttu-id="01251-162">Id가 MFA를 사용 하 여 로그인 한 경우에는 도구 설명 경고 없이 **Admin** 메뉴가 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="01251-162">If the identity has logged in using MFA, the **Admin** menu is displayed without the tooltip warning.</span></span> <span data-ttu-id="01251-163">사용자가 MFA 없이 로그인 하면 **관리자 (사용 안 함)** 메뉴가 사용자에 게 알리는 도구 설명 (경고 설명)과 함께 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="01251-163">When the user has logged in without MFA, the **Admin (Not Enabled)** menu is displayed along with the tooltip that informs the user (explaining the warning).</span></span>

```cshtml
@if (SignInManager.IsSignedIn(User))
{
    @if ((AuthorizationService.AuthorizeAsync(User, "TwoFactorEnabled")).Result.Succeeded)
    {
        <li class="nav-item">
            <a class="nav-link text-dark" asp-area="" asp-page="/Admin">Admin</a>
        </li>
    }
    else
    {
        <li class="nav-item">
            <a class="nav-link text-dark" asp-area="" asp-page="/Admin" 
               id="tooltip-demo"  
               data-toggle="tooltip" 
               data-placement="bottom" 
               title="MFA is NOT enabled. This is required for the Admin Page. If you have activated MFA, then logout, login again.">
                Admin (Not Enabled)
            </a>
        </li>
    }
}
```

<span data-ttu-id="01251-164">사용자가 MFA 없이 로그인 하는 경우 경고가 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="01251-164">If the user logs in without MFA, the warning is displayed:</span></span>

![관리자 MFA 인증](mfa/_static/identitystandalonemfa_01.png)

<span data-ttu-id="01251-166">**관리자** 링크를 클릭 하면 사용자가 MFA 사용 보기로 리디렉션됩니다.</span><span class="sxs-lookup"><span data-stu-id="01251-166">The user is redirected to the MFA enable view when clicking the **Admin** link:</span></span>

![관리자가 MFA 인증 활성화](mfa/_static/identitystandalonemfa_02.png)

## <a name="send-mfa-sign-in-requirement-to-openid-connect-server"></a><span data-ttu-id="01251-168">Openid connect Connect 서버에 MFA 로그인 요구 사항 보내기</span><span class="sxs-lookup"><span data-stu-id="01251-168">Send MFA sign-in requirement to OpenID Connect server</span></span> 

<span data-ttu-id="01251-169">`acr_values` 매개 변수를 사용 하 여 인증 요청에서 클라이언트의 `mfa` 필수 값을 서버로 전달할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="01251-169">The `acr_values` parameter can be used to pass the `mfa` required value from the client to the server in an authentication request.</span></span>

> [!NOTE]
> <span data-ttu-id="01251-170">이 작업을 수행 하려면 Open ID Connect 서버에서 `acr_values` 매개 변수를 처리 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="01251-170">The `acr_values` parameter needs to be handled on the Open ID Connect server for this to work.</span></span>

### <a name="openid-connect-aspnet-core-client"></a><span data-ttu-id="01251-171">Openid connect Connect ASP.NET Core 클라이언트</span><span class="sxs-lookup"><span data-stu-id="01251-171">OpenID Connect ASP.NET Core client</span></span>

<span data-ttu-id="01251-172">ASP.NET Core Razor Pages Open ID Connect 클라이언트 앱은 `AddOpenIdConnect` 메서드를 사용 하 여 Open ID Connect 서버에 로그인 합니다.</span><span class="sxs-lookup"><span data-stu-id="01251-172">The ASP.NET Core Razor Pages Open ID Connect client app uses the `AddOpenIdConnect` method to login to the Open ID Connect server.</span></span> <span data-ttu-id="01251-173">`acr_values` 매개 변수는 `mfa` 값으로 설정 되 고 인증 요청과 함께 전송 됩니다.</span><span class="sxs-lookup"><span data-stu-id="01251-173">The `acr_values` parameter is set with the `mfa` value and sent with the authentication request.</span></span> <span data-ttu-id="01251-174">`OpenIdConnectEvents`를 추가 하는 데 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="01251-174">The `OpenIdConnectEvents` is used to add this.</span></span>

<span data-ttu-id="01251-175">권장 `acr_values` 매개 변수 값은 [인증 방법 참조 값](https://tools.ietf.org/html/draft-ietf-oauth-amr-values-08)을 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="01251-175">For recommended `acr_values` parameter values, see [Authentication Method Reference Values](https://tools.ietf.org/html/draft-ietf-oauth-amr-values-08).</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddAuthentication(options =>
    {
        options.DefaultScheme =
            CookieAuthenticationDefaults.AuthenticationScheme;
        options.DefaultChallengeScheme =
            OpenIdConnectDefaults.AuthenticationScheme;
    })
    .AddCookie()
    .AddOpenIdConnect(options =>
    {
        options.SignInScheme =
            CookieAuthenticationDefaults.AuthenticationScheme;
        options.Authority = "<OpenID Connect server URL>";
        options.RequireHttpsMetadata = true;
        options.ClientId = "<OpenID Connect client ID>";
        options.ClientSecret = "<>";
        // Code with PKCE can also be used here
        options.ResponseType = "code id_token";
        options.Scope.Add("profile");
        options.Scope.Add("offline_access");
        options.SaveTokens = true;
        options.Events = new OpenIdConnectEvents
        {
            OnRedirectToIdentityProvider = context =>
            {
                context.ProtocolMessage.SetParameter("acr_values", "mfa");
                return Task.FromResult(0);
            }
        };
    });
```

### <a name="example-openid-connect-identityserver-4-server-with-aspnet-core-opno-locidentity"></a><span data-ttu-id="01251-176">예 Openid connect Connect IdentityServer 4 server with ASP.NET Core Identity</span><span class="sxs-lookup"><span data-stu-id="01251-176">Example OpenID Connect IdentityServer 4 server with ASP.NET Core Identity</span></span>

<span data-ttu-id="01251-177">MVC 뷰와 ASP.NET Core Identity를 사용 하 여 구현 되는 Openid connect Connect 서버에서 *ErrorEnable2FA* 라는 새 보기가 만들어집니다.</span><span class="sxs-lookup"><span data-stu-id="01251-177">On the OpenID Connect server, which is implemented using ASP.NET Core Identity with MVC views, a new view named *ErrorEnable2FA.cshtml* is created.</span></span> <span data-ttu-id="01251-178">보기:</span><span class="sxs-lookup"><span data-stu-id="01251-178">The view:</span></span>

* <span data-ttu-id="01251-179">Identity MFA를 요구 하는 앱에서 제공 되었지만 사용자가 Identity에서 활성화 되지 않은 경우 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="01251-179">Displays if the Identity comes from an app that requires MFA but the user hasn't activated this in Identity.</span></span>
* <span data-ttu-id="01251-180">사용자에 게 알리고이를 활성화 하는 링크를 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="01251-180">Informs the user and adds a link to activate this.</span></span>

```cshtml
@{
    ViewData["Title"] = "ErrorEnable2FA";
}

<h1>The client application requires you to have MFA enabled. Enable this, try login again.</h1>

<br />

You can enable MFA to login here:

<br />

<a asp-controller="Manage" asp-action="TwoFactorAuthentication">Enable MFA</a>
```

<span data-ttu-id="01251-181">`Login` 메서드에서는 `IIdentityServerInteractionService` interface 구현 `_interaction`를 사용 하 여 Open ID Connect request 매개 변수에 액세스 합니다.</span><span class="sxs-lookup"><span data-stu-id="01251-181">In the `Login` method, the `IIdentityServerInteractionService` interface implementation `_interaction` is used to access the Open ID Connect request parameters.</span></span> <span data-ttu-id="01251-182">`acr_values` 매개 변수는 `AcrValues` 속성을 사용 하 여 액세스할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="01251-182">The `acr_values` parameter is accessed using the `AcrValues` property.</span></span> <span data-ttu-id="01251-183">클라이언트에서 `mfa` 집합을 사용 하 여이를 보낸 후에는이를 확인할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="01251-183">As the client sent this with `mfa` set, this can then be checked.</span></span>

<span data-ttu-id="01251-184">MFA가 필요 하 고 ASP.NET Core Identity의 사용자에 게 MFA를 사용 하도록 설정 되어 있으면 로그인이 계속 됩니다.</span><span class="sxs-lookup"><span data-stu-id="01251-184">If MFA is required, and the user in ASP.NET Core Identity has MFA enabled, then the login continues.</span></span> <span data-ttu-id="01251-185">사용자가 MFA를 사용 하도록 설정 하지 않은 경우 사용자는 사용자 지정 보기 *ErrorEnable2FA*리디렉션됩니다.</span><span class="sxs-lookup"><span data-stu-id="01251-185">When the user has no MFA enabled, the user is redirected to the custom view *ErrorEnable2FA.cshtml*.</span></span> <span data-ttu-id="01251-186">그런 다음 ASP.NET Core에서 사용자에 게 로그인 Identity 합니다.</span><span class="sxs-lookup"><span data-stu-id="01251-186">Then ASP.NET Core Identity signs the user in.</span></span>

```csharp
//
// POST: /Account/Login
[HttpPost]
[AllowAnonymous]
[ValidateAntiForgeryToken]
public async Task<IActionResult> Login(LoginInputModel model)
{
    var returnUrl = model.ReturnUrl;
    var context = 
        await _interaction.GetAuthorizationContextAsync(returnUrl);
    var requires2Fa = 
        context?.AcrValues.Count(t => t.Contains("mfa")) >= 1;

    var user = await _userManager.FindByNameAsync(model.Email);
    if (user != null && !user.TwoFactorEnabled && requires2Fa)
    {
        return RedirectToAction(nameof(ErrorEnable2FA));
    }

    // code omitted for brevity
```

<span data-ttu-id="01251-187">`ExternalLoginCallback` 메서드는 로컬 Identity 로그인과 같은 방식으로 작동 합니다.</span><span class="sxs-lookup"><span data-stu-id="01251-187">The `ExternalLoginCallback` method works like the local Identity login.</span></span> <span data-ttu-id="01251-188">`mfa` 값에 대해 `AcrValues` 속성이 확인 됩니다.</span><span class="sxs-lookup"><span data-stu-id="01251-188">The `AcrValues` property is checked for the `mfa` value.</span></span> <span data-ttu-id="01251-189">`mfa` 값이 있는 경우 로그인이 완료 되기 전에 MFA가 강제로 수행 됩니다 (예: `ErrorEnable2FA` 보기로 리디렉션 됨).</span><span class="sxs-lookup"><span data-stu-id="01251-189">If the `mfa` value is present, MFA is forced before the login completes (for example, redirected to the `ErrorEnable2FA` view).</span></span>

```csharp
//
// GET: /Account/ExternalLoginCallback
[HttpGet]
[AllowAnonymous]
public async Task<IActionResult> ExternalLoginCallback(
    string returnUrl = null,
    string remoteError = null)
{
    var context =
        await _interaction.GetAuthorizationContextAsync(returnUrl);
    var requires2Fa =
        context?.AcrValues.Count(t => t.Contains("mfa")) >= 1;

    if (remoteError != null)
    {
        ModelState.AddModelError(
            string.Empty,
            _sharedLocalizer["EXTERNAL_PROVIDER_ERROR", 
            remoteError]);
        return View(nameof(Login));
    }
    var info = await _signInManager.GetExternalLoginInfoAsync();

    if (info == null)
    {
        return RedirectToAction(nameof(Login));
    }

    var email = info.Principal.FindFirstValue(ClaimTypes.Email);

    if (!string.IsNullOrEmpty(email))
    {
        var user = await _userManager.FindByNameAsync(email);
        if (user != null && !user.TwoFactorEnabled && requires2Fa)
        {
            return RedirectToAction(nameof(ErrorEnable2FA));
        }
    }

    // Sign in the user with this external login provider if the user already has a login.
    var result = await _signInManager
        .ExternalLoginSignInAsync(
            info.LoginProvider, 
            info.ProviderKey, 
            isPersistent: 
            false);

    // code omitted for brevity
```

<span data-ttu-id="01251-190">사용자가 이미 로그인 한 경우 클라이언트 앱은 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="01251-190">If the user is already logged in, the client app:</span></span>

* <span data-ttu-id="01251-191">는 여전히 `amr` 클레임의 유효성을 검사 합니다.</span><span class="sxs-lookup"><span data-stu-id="01251-191">Still validates the `amr` claim.</span></span>
* <span data-ttu-id="01251-192">ASP.NET Core Identity 보기에 대 한 링크를 사용 하 여 MFA를 설정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="01251-192">Can set up the MFA with a link to the ASP.NET Core Identity view.</span></span>

![acr_values-1](mfa/_static/acr_values-1.png)

## <a name="force-aspnet-core-openid-connect-client-to-require-mfa"></a><span data-ttu-id="01251-194">강제로 ASP.NET Core Openid connect Connect 클라이언트에 MFA를 요구 합니다.</span><span class="sxs-lookup"><span data-stu-id="01251-194">Force ASP.NET Core OpenID Connect client to require MFA</span></span>

<span data-ttu-id="01251-195">이 예제에서는 Openid connect Connect를 사용 하 여 로그인 하는 ASP.NET Core Razor 페이지 앱이 사용자에 게 MFA를 사용 하 여 인증 되도록 요구할 수 있는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="01251-195">This example shows how an ASP.NET Core Razor Page app, which uses OpenID Connect to sign in, can require that users have authenticated using MFA.</span></span>

<span data-ttu-id="01251-196">MFA 요구 사항의 유효성을 검사 하기 위해 `IAuthorizationRequirement` 요구 사항이 생성 됩니다.</span><span class="sxs-lookup"><span data-stu-id="01251-196">To validate the MFA requirement, an `IAuthorizationRequirement` requirement is created.</span></span> <span data-ttu-id="01251-197">MFA를 요구 하는 정책을 사용 하 여 페이지에 추가 됩니다.</span><span class="sxs-lookup"><span data-stu-id="01251-197">This will be added to the pages using a policy that requires MFA.</span></span>

```csharp
using Microsoft.AspNetCore.Authorization;
 
namespace AspNetCoreRequireMfaOidc
{
    public class RequireMfa : IAuthorizationRequirement{}
}
```

<span data-ttu-id="01251-198">`amr` 클레임을 사용 하 고 `mfa`값을 확인 하는 `AuthorizationHandler` 구현 됩니다.</span><span class="sxs-lookup"><span data-stu-id="01251-198">An `AuthorizationHandler` is implemented that will use the `amr` claim and check for the value `mfa`.</span></span> <span data-ttu-id="01251-199">`amr`는 성공적인 인증의 `id_token`에 반환 되며 [인증 방법 참조 값](https://tools.ietf.org/html/draft-ietf-oauth-amr-values-08) 사양에 정의 된 대로 다양 한 값을 가질 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="01251-199">The `amr` is returned in the `id_token` of a successful authentication and can have many different values as defined in the [Authentication Method Reference Values](https://tools.ietf.org/html/draft-ietf-oauth-amr-values-08) specification.</span></span>

<span data-ttu-id="01251-200">반환 되는 값은 id가 인증 된 방법과 Open ID Connect 서버 구현에 따라 달라 집니다.</span><span class="sxs-lookup"><span data-stu-id="01251-200">The returned value depends on how the identity authenticated and on the Open ID Connect server implementation.</span></span>

<span data-ttu-id="01251-201">`AuthorizationHandler` `RequireMfa` 요구 사항을 사용 하 고 `amr` 클레임의 유효성을 검사 합니다.</span><span class="sxs-lookup"><span data-stu-id="01251-201">The `AuthorizationHandler` uses the `RequireMfa` requirement and validates the `amr` claim.</span></span> <span data-ttu-id="01251-202">Openid connect Connect 서버는 ASP.NET Core Identity있는 IdentityServer4을 사용 하 여 구현할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="01251-202">The OpenID Connect server can be implemented using IdentityServer4 with ASP.NET Core Identity.</span></span> <span data-ttu-id="01251-203">사용자가 TOTP를 사용 하 여 로그인 하면 `amr` 클레임이 MFA 값으로 반환 됩니다.</span><span class="sxs-lookup"><span data-stu-id="01251-203">When a user logs in using TOTP, the `amr` claim is returned with an MFA value.</span></span> <span data-ttu-id="01251-204">다른 Openid connect Connect 서버 구현 또는 다른 MFA 유형을 사용 하는 경우 `amr` 클레임은 다른 값을 가질 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="01251-204">If using a different OpenID Connect server implementation or a different MFA type, the `amr` claim will, or can, have a different value.</span></span> <span data-ttu-id="01251-205">또한이를 허용 하려면 코드를 확장 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="01251-205">The code must be extended to accept this as well.</span></span>

```csharp
using Microsoft.AspNetCore.Authorization;
using System;
using System.Linq;
using System.Threading.Tasks;

namespace AspNetCoreRequireMfaOidc
{
    public class RequireMfaHandler : AuthorizationHandler<RequireMfa>
    {
        protected override Task HandleRequirementAsync(
            AuthorizationHandlerContext context, 
            RequireMfa requirement)
        {
            if (context == null)
                throw new ArgumentNullException(nameof(context));
            if (requirement == null)
                throw new ArgumentNullException(nameof(requirement));

            var amrClaim =
                context.User.Claims.FirstOrDefault(t => t.Type == "amr");

            if (amrClaim != null && amrClaim.Value == Amr.Mfa)
            {
                context.Succeed(requirement);
            }

            return Task.CompletedTask;
        }
    }
}
```

<span data-ttu-id="01251-206">`Startup.ConfigureServices` 메서드에서 `AddOpenIdConnect` 메서드가 기본 챌린지 체계로 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="01251-206">In the `Startup.ConfigureServices` method, the `AddOpenIdConnect` method is used as the default challenge scheme.</span></span> <span data-ttu-id="01251-207">`amr` 클레임을 확인 하는 데 사용 되는 권한 부여 처리기는 제어 컨테이너의 반전에 추가 됩니다.</span><span class="sxs-lookup"><span data-stu-id="01251-207">The authorization handler, which is used to check the `amr` claim, is added to the Inversion of Control container.</span></span> <span data-ttu-id="01251-208">그런 다음 `RequireMfa` 요구 사항을 추가 하는 정책이 생성 됩니다.</span><span class="sxs-lookup"><span data-stu-id="01251-208">A policy is then created which adds the `RequireMfa` requirement.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.ConfigureApplicationCookie(options =>
        options.Cookie.SecurePolicy =
            CookieSecurePolicy.Always);

    services.AddSingleton<IAuthorizationHandler, RequireMfaHandler>();

    services.AddAuthentication(options =>
    {
        options.DefaultScheme =
            CookieAuthenticationDefaults.AuthenticationScheme;
        options.DefaultChallengeScheme =
            OpenIdConnectDefaults.AuthenticationScheme;
    })
    .AddCookie()
    .AddOpenIdConnect(options =>
    {
        options.SignInScheme =
            CookieAuthenticationDefaults.AuthenticationScheme;
        options.Authority = "https://localhost:44352";
        options.RequireHttpsMetadata = true;
        options.ClientId = "AspNetCoreRequireMfaOidc";
        options.ClientSecret = "AspNetCoreRequireMfaOidcSecret";
        options.ResponseType = "code id_token";
        options.Scope.Add("profile");
        options.Scope.Add("offline_access");
        options.SaveTokens = true;
    });

    services.AddAuthorization(options =>
    {
        options.AddPolicy("RequireMfa", policyIsAdminRequirement =>
        {
            policyIsAdminRequirement.Requirements.Add(new RequireMfa());
        });
    });

    services.AddRazorPages();
}
```

<span data-ttu-id="01251-209">이 정책은 필요에 따라 Razor 페이지에서 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="01251-209">This policy is then used in the Razor page as required.</span></span> <span data-ttu-id="01251-210">정책은 전체 앱에 대해서도 전역적으로 추가할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="01251-210">The policy could be added globally for the entire app as well.</span></span>

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Authorization;
using Microsoft.AspNetCore.Mvc;
using Microsoft.AspNetCore.Mvc.RazorPages;
using Microsoft.Extensions.Logging;

namespace AspNetCoreRequireMfaOidc.Pages
{
    [Authorize(Policy= "RequireMfa")]
    public class IndexModel : PageModel
    {
        private readonly ILogger<IndexModel> _logger;

        public IndexModel(ILogger<IndexModel> logger)
        {
            _logger = logger;
        }

        public void OnGet()
        {
        }
    }
}
```

<span data-ttu-id="01251-211">사용자가 MFA 없이 인증 하는 경우 `amr` 클레임에 `pwd` 값이 있을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="01251-211">If the user authenticates without MFA, the `amr` claim will probably have a `pwd` value.</span></span> <span data-ttu-id="01251-212">이 요청에는 페이지에 대 한 액세스 권한이 부여 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="01251-212">The request won't be authorized to access the page.</span></span> <span data-ttu-id="01251-213">기본값을 사용 하 여 사용자가 *계정/s i d/거부* 페이지로 리디렉션됩니다.</span><span class="sxs-lookup"><span data-stu-id="01251-213">Using the default values, the user will be redirected to the *Account/AccessDenied* page.</span></span> <span data-ttu-id="01251-214">이 동작을 변경 하거나 사용자 고유의 사용자 지정 논리를 구현할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="01251-214">This behavior can be changed or you can implement your own custom logic here.</span></span> <span data-ttu-id="01251-215">이 예제에서는 유효한 사용자가 계정에 대해 MFA를 설정할 수 있도록 링크가 추가 됩니다.</span><span class="sxs-lookup"><span data-stu-id="01251-215">In this example, a link is added so that the valid user can set up MFA for their account.</span></span>

```cshtml
@page
@model AspNetCoreRequireMfaOidc.AccessDeniedModel
@{
    ViewData["Title"] = "AccessDenied";
    Layout = "~/Pages/Shared/_Layout.cshtml";
}

<h1>AccessDenied</h1>

You require MFA to login here

<a href="https://localhost:44352/Manage/TwoFactorAuthentication">Enable MFA</a>
```

<span data-ttu-id="01251-216">이제 MFA로 인증 하는 사용자만 페이지 또는 웹 사이트에 액세스할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="01251-216">Now only users that authenticate with MFA can access the page or website.</span></span> <span data-ttu-id="01251-217">서로 다른 MFA 유형을 사용 하거나 2FA가 양호 하면 `amr` 클레임은 다른 값을 가지 며 올바르게 처리 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="01251-217">If different MFA types are used or if 2FA is okay, the `amr` claim will have different values and needs to be processed correctly.</span></span> <span data-ttu-id="01251-218">다른 Open ID Connect 서버는이 클레임에 대해 서로 다른 값을 반환 하며 [인증 방법 참조 값](https://tools.ietf.org/html/draft-ietf-oauth-amr-values-08) 사양을 따르지 않을 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="01251-218">Different Open ID Connect servers also return different values for this claim and might not follow the [Authentication Method Reference Values](https://tools.ietf.org/html/draft-ietf-oauth-amr-values-08) specification.</span></span>

<span data-ttu-id="01251-219">MFA 없이 로그인 하는 경우 (예: 암호 사용):</span><span class="sxs-lookup"><span data-stu-id="01251-219">When logging in without MFA (for example, using just a password):</span></span>

* <span data-ttu-id="01251-220">`amr`에 `pwd` 값이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="01251-220">The `amr` has the `pwd` value:</span></span>

    ![require_mfa_oidc_02 .png](mfa/_static/require_mfa_oidc_02.png)

* <span data-ttu-id="01251-222">액세스가 거부 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="01251-222">Access is denied:</span></span>

    ![require_mfa_oidc_03 .png](mfa/_static/require_mfa_oidc_03.png)

<span data-ttu-id="01251-224">또는 Identity와 함께 OTP를 사용 하 여 로그인 합니다.</span><span class="sxs-lookup"><span data-stu-id="01251-224">Alternatively, logging in using OTP with Identity:</span></span>

![require_mfa_oidc_01 .png](mfa/_static/require_mfa_oidc_01.png)

## <a name="additional-resources"></a><span data-ttu-id="01251-226">추가 리소스</span><span class="sxs-lookup"><span data-stu-id="01251-226">Additional resources</span></span>

* [<span data-ttu-id="01251-227">ASP.NET Core에서 TOTP authenticator 앱에 대 한 QR 코드 생성 사용</span><span class="sxs-lookup"><span data-stu-id="01251-227">Enable QR Code generation for TOTP authenticator apps in ASP.NET Core</span></span>](xref:security/authentication/identity-enable-qrcodes)
* [<span data-ttu-id="01251-228">Azure Active Directory에 대 한 암호 없는 인증 옵션</span><span class="sxs-lookup"><span data-stu-id="01251-228">Passwordless authentication options for Azure Active Directory</span></span>](/azure/active-directory/authentication/concept-authentication-passwordless)
* [<span data-ttu-id="01251-229">.NET을 사용 하 여 FIDO2/WebAuthn 증명 및 어설션에 대 한 FIDO2 .NET 라이브러리</span><span class="sxs-lookup"><span data-stu-id="01251-229">FIDO2 .NET library for FIDO2 / WebAuthn Attestation and Assertion using .NET</span></span>](https://github.com/abergs/fido2-net-lib)
* [<span data-ttu-id="01251-230">WebAuthn 놀라운</span><span class="sxs-lookup"><span data-stu-id="01251-230">WebAuthn Awesome</span></span>](https://github.com/herrjemand/awesome-webauthn)