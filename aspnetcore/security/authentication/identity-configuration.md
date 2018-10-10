---
title: ASP.NET Core Id를 구성 합니다.
author: AdrienTorris
description: ASP.NET Core Id 기본 값을 이해 하 고 사용자 지정 값을 사용 하도록 Id 속성을 구성 하는 방법을 알아봅니다.
ms.author: riande
ms.date: 08/14/2018
uid: security/authentication/identity-configuration
ms.openlocfilehash: 02441cd28c2a99eda7b50ed54f4437d4b52ca5d9
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/10/2018
ms.locfileid: "48911948"
---
# <a name="configure-aspnet-core-identity"></a><span data-ttu-id="e5cf9-103">ASP.NET Core Id를 구성 합니다.</span><span class="sxs-lookup"><span data-stu-id="e5cf9-103">Configure ASP.NET Core Identity</span></span>

<span data-ttu-id="e5cf9-104">ASP.NET Core Id는 암호 정책, 잠금 쿠키 구성과 같은 설정에 대 한 기본값을 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="e5cf9-104">ASP.NET Core Identity uses default values for settings such as password policy, lockout, and cookie configuration.</span></span> <span data-ttu-id="e5cf9-105">이러한 설정을 재정의할 수 있습니다는 `Startup` 클래스입니다.</span><span class="sxs-lookup"><span data-stu-id="e5cf9-105">These settings can be overridden in the `Startup` class.</span></span>

## <a name="identity-options"></a><span data-ttu-id="e5cf9-106">Id 옵션</span><span class="sxs-lookup"><span data-stu-id="e5cf9-106">Identity options</span></span>

<span data-ttu-id="e5cf9-107">합니다 [IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) 클래스 Id 시스템을 구성 하려면 사용할 수 있는 옵션을 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="e5cf9-107">The [IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) class represents the options that can be used to configure the Identity system.</span></span> <span data-ttu-id="e5cf9-108">`IdentityOptions` 설정 해야 합니다 **한 후** 호출 `AddIdentity` 또는 `AddDefaultIdentity`합니다.</span><span class="sxs-lookup"><span data-stu-id="e5cf9-108">`IdentityOptions` must be set **after** calling `AddIdentity` or `AddDefaultIdentity`.</span></span>

### <a name="claims-identity"></a><span data-ttu-id="e5cf9-109">클레임 Id</span><span class="sxs-lookup"><span data-stu-id="e5cf9-109">Claims Identity</span></span>

<span data-ttu-id="e5cf9-110">[IdentityOptions.ClaimsIdentity](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.claimsidentity) 지정 된 [ClaimsIdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions) 표에 표시 된 속성을 사용 하 여 합니다.</span><span class="sxs-lookup"><span data-stu-id="e5cf9-110">[IdentityOptions.ClaimsIdentity](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.claimsidentity) specifies the [ClaimsIdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions) with the properties shown in the following table.</span></span>

| <span data-ttu-id="e5cf9-111">속성</span><span class="sxs-lookup"><span data-stu-id="e5cf9-111">Property</span></span> | <span data-ttu-id="e5cf9-112">설명</span><span class="sxs-lookup"><span data-stu-id="e5cf9-112">Description</span></span> | <span data-ttu-id="e5cf9-113">기본</span><span class="sxs-lookup"><span data-stu-id="e5cf9-113">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="e5cf9-114">RoleClaimType</span><span class="sxs-lookup"><span data-stu-id="e5cf9-114">RoleClaimType</span></span>](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.roleclaimtype) | <span data-ttu-id="e5cf9-115">역할 클레임에 사용 되는 클레임 형식을 가져오거나 설정 합니다.</span><span class="sxs-lookup"><span data-stu-id="e5cf9-115">Gets or sets the claim type used for a role claim.</span></span> | [<span data-ttu-id="e5cf9-116">ClaimTypes.Role</span><span class="sxs-lookup"><span data-stu-id="e5cf9-116">ClaimTypes.Role</span></span>](/dotnet/api/system.security.claims.claimtypes.role) |
| [<span data-ttu-id="e5cf9-117">SecurityStampClaimType</span><span class="sxs-lookup"><span data-stu-id="e5cf9-117">SecurityStampClaimType</span></span>](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.securitystampclaimtype) | <span data-ttu-id="e5cf9-118">보안 스탬프 클레임에 사용 되는 클레임 형식을 가져오거나 설정 합니다.</span><span class="sxs-lookup"><span data-stu-id="e5cf9-118">Gets or sets the claim type used for the security stamp claim.</span></span> | `AspNet.Identity.SecurityStamp` |
| [<span data-ttu-id="e5cf9-119">UserIdClaimType</span><span class="sxs-lookup"><span data-stu-id="e5cf9-119">UserIdClaimType</span></span>](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.useridclaimtype) | <span data-ttu-id="e5cf9-120">사용자 식별자 클레임에 사용 되는 클레임 형식을 가져오거나 설정 합니다.</span><span class="sxs-lookup"><span data-stu-id="e5cf9-120">Gets or sets the claim type used for the user identifier claim.</span></span> | [<span data-ttu-id="e5cf9-121">ClaimTypes.NameIdentifier</span><span class="sxs-lookup"><span data-stu-id="e5cf9-121">ClaimTypes.NameIdentifier</span></span>](/dotnet/api/system.security.claims.claimtypes.nameidentifier) |
| [<span data-ttu-id="e5cf9-122">UserNameClaimType</span><span class="sxs-lookup"><span data-stu-id="e5cf9-122">UserNameClaimType</span></span>](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.usernameclaimtype) | <span data-ttu-id="e5cf9-123">사용자 이름 클레임에 사용 되는 클레임 형식을 가져오거나 설정 합니다.</span><span class="sxs-lookup"><span data-stu-id="e5cf9-123">Gets or sets the claim type used for the user name claim.</span></span> | [<span data-ttu-id="e5cf9-124">ClaimTypes.Name</span><span class="sxs-lookup"><span data-stu-id="e5cf9-124">ClaimTypes.Name</span></span>](/dotnet/api/system.security.claims.claimtypes.name) |

### <a name="lockout"></a><span data-ttu-id="e5cf9-125">잠금</span><span class="sxs-lookup"><span data-stu-id="e5cf9-125">Lockout</span></span>

<span data-ttu-id="e5cf9-126">잠금에 설정 된 [PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync#Microsoft_AspNetCore_Identity_SignInManager_1_PasswordSignInAsync_System_String_System_String_System_Boolean_System_Boolean_) 메서드:</span><span class="sxs-lookup"><span data-stu-id="e5cf9-126">Lockout is set in the [PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync#Microsoft_AspNetCore_Identity_SignInManager_1_PasswordSignInAsync_System_String_System_String_System_Boolean_System_Boolean_) method:</span></span>

[!code-csharp[](identity-configuration/sample/Areas/Identity/Pages/Account/Login.cshtml.cs?name=snippet&highlight=9)]

<span data-ttu-id="e5cf9-127">위의 코드는 기반을 `Login` Identity 템플릿.</span><span class="sxs-lookup"><span data-stu-id="e5cf9-127">The preceding code is based on the `Login` Identity template.</span></span> 

<span data-ttu-id="e5cf9-128">잠금 옵션 설정이 `StartUp.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="e5cf9-128">Lockout options are set in `StartUp.ConfigureServices`:</span></span>

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_lock)]

<span data-ttu-id="e5cf9-129">위의 코드 집합을 [IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) [LockoutOptions](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions) 기본값을 사용 하 여 합니다.</span><span class="sxs-lookup"><span data-stu-id="e5cf9-129">The preceding code sets the [IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) [LockoutOptions](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions) with default values.</span></span>

<span data-ttu-id="e5cf9-130">실패 한 액세스 시도 횟수를 다시 설정 하 고 시계를 다시 설정 하는 성공적으로 인증 합니다.</span><span class="sxs-lookup"><span data-stu-id="e5cf9-130">A successful authentication resets the failed access attempts count and resets the clock.</span></span>

<span data-ttu-id="e5cf9-131">[IdentityOptions.Lockout](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.lockout) 지정 된 [LockoutOptions](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions) 표에 표시 된 속성을 사용 하 여 합니다.</span><span class="sxs-lookup"><span data-stu-id="e5cf9-131">[IdentityOptions.Lockout](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.lockout) specifies the [LockoutOptions](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions) with the properties shown in the table.</span></span>

| <span data-ttu-id="e5cf9-132">속성</span><span class="sxs-lookup"><span data-stu-id="e5cf9-132">Property</span></span> | <span data-ttu-id="e5cf9-133">설명</span><span class="sxs-lookup"><span data-stu-id="e5cf9-133">Description</span></span> | <span data-ttu-id="e5cf9-134">기본</span><span class="sxs-lookup"><span data-stu-id="e5cf9-134">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="e5cf9-135">AllowedForNewUsers</span><span class="sxs-lookup"><span data-stu-id="e5cf9-135">AllowedForNewUsers</span></span>](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.allowedfornewusers) | <span data-ttu-id="e5cf9-136">하는 경우 새 사용자를 잠글 수를 결정 합니다.</span><span class="sxs-lookup"><span data-stu-id="e5cf9-136">Determines if a new user can be locked out.</span></span> | `true` |
| [<span data-ttu-id="e5cf9-137">DefaultLockoutTimeSpan</span><span class="sxs-lookup"><span data-stu-id="e5cf9-137">DefaultLockoutTimeSpan</span></span>](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan) | <span data-ttu-id="e5cf9-138">시간을 사용자가 잠겨 잠금 발생 하는 경우.</span><span class="sxs-lookup"><span data-stu-id="e5cf9-138">The amount of time a user is locked out when a lockout occurs.</span></span> | <span data-ttu-id="e5cf9-139">5 분</span><span class="sxs-lookup"><span data-stu-id="e5cf9-139">5 minutes</span></span> |
| [<span data-ttu-id="e5cf9-140">MaxFailedAccessAttempts</span><span class="sxs-lookup"><span data-stu-id="e5cf9-140">MaxFailedAccessAttempts</span></span>](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) | <span data-ttu-id="e5cf9-141">잠금이 설정 된 경우, 사용자가 차단 될 때까지 실패 한 액세스 시도 횟수입니다.</span><span class="sxs-lookup"><span data-stu-id="e5cf9-141">The number of failed access attempts until a user is locked out, if lockout is enabled.</span></span> | <span data-ttu-id="e5cf9-142">5</span><span class="sxs-lookup"><span data-stu-id="e5cf9-142">5</span></span> |

### <a name="password"></a><span data-ttu-id="e5cf9-143">암호</span><span class="sxs-lookup"><span data-stu-id="e5cf9-143">Password</span></span>

<span data-ttu-id="e5cf9-144">기본적으로 Identity 암호에 대문자, 소문자, 숫자 및 영숫자가 아닌 문자를 포함 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="e5cf9-144">By default, Identity requires that passwords contain an uppercase character, lowercase character, a digit, and a non-alphanumeric character.</span></span> <span data-ttu-id="e5cf9-145">암호는 최소 6 자 여야 합니다.</span><span class="sxs-lookup"><span data-stu-id="e5cf9-145">Passwords must be at least six characters long.</span></span> <span data-ttu-id="e5cf9-146">[PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions) 에서 설정할 수 있습니다 `Startup.ConfigureServices`합니다.</span><span class="sxs-lookup"><span data-stu-id="e5cf9-146">[PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions) can be set in `Startup.ConfigureServices`.</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_pw)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-37,50-52)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-65,84)]

::: moniker-end

<span data-ttu-id="e5cf9-147">[IdentityOptions.Password](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.password) 지정 된 [PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions) 표에 표시 된 속성을 사용 하 여 합니다.</span><span class="sxs-lookup"><span data-stu-id="e5cf9-147">[IdentityOptions.Password](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.password) specifies the [PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions) with the properties shown in the table.</span></span>

::: moniker range=">= aspnetcore-2.0"

| <span data-ttu-id="e5cf9-148">속성</span><span class="sxs-lookup"><span data-stu-id="e5cf9-148">Property</span></span> | <span data-ttu-id="e5cf9-149">설명</span><span class="sxs-lookup"><span data-stu-id="e5cf9-149">Description</span></span> | <span data-ttu-id="e5cf9-150">기본</span><span class="sxs-lookup"><span data-stu-id="e5cf9-150">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="e5cf9-151">RequireDigit</span><span class="sxs-lookup"><span data-stu-id="e5cf9-151">RequireDigit</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredigit) | <span data-ttu-id="e5cf9-152">암호에 0-9 사이의 숫자를 여야 합니다.</span><span class="sxs-lookup"><span data-stu-id="e5cf9-152">Requires a number between 0-9 in the password.</span></span> | `true` |
| [<span data-ttu-id="e5cf9-153">RequiredLength</span><span class="sxs-lookup"><span data-stu-id="e5cf9-153">RequiredLength</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredlength) | <span data-ttu-id="e5cf9-154">암호의 최소 길이입니다.</span><span class="sxs-lookup"><span data-stu-id="e5cf9-154">The minimum length of the password.</span></span> | <span data-ttu-id="e5cf9-155">6</span><span class="sxs-lookup"><span data-stu-id="e5cf9-155">6</span></span> |
| [<span data-ttu-id="e5cf9-156">RequireLowercase</span><span class="sxs-lookup"><span data-stu-id="e5cf9-156">RequireLowercase</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirelowercase) | <span data-ttu-id="e5cf9-157">암호에 소문자가 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="e5cf9-157">Requires a lowercase character in the password.</span></span> | `true` |
| [<span data-ttu-id="e5cf9-158">RequireNonAlphanumeric</span><span class="sxs-lookup"><span data-stu-id="e5cf9-158">RequireNonAlphanumeric</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirenonalphanumeric) | <span data-ttu-id="e5cf9-159">암호의 영숫자가 아닌 문자가 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="e5cf9-159">Requires a non-alphanumeric character in the password.</span></span> | `true` |
| [<span data-ttu-id="e5cf9-160">RequiredUniqueChars</span><span class="sxs-lookup"><span data-stu-id="e5cf9-160">RequiredUniqueChars</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireduniquechars) | <span data-ttu-id="e5cf9-161">ASP.NET Core 2.0 이상에 적용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="e5cf9-161">Only applies to ASP.NET Core 2.0 or later.</span></span><br><br> <span data-ttu-id="e5cf9-162">암호에 고유 문자 수가 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="e5cf9-162">Requires the number of distinct characters in the password.</span></span> | <span data-ttu-id="e5cf9-163">1</span><span class="sxs-lookup"><span data-stu-id="e5cf9-163">1</span></span> |
| [<span data-ttu-id="e5cf9-164">RequireUppercase</span><span class="sxs-lookup"><span data-stu-id="e5cf9-164">RequireUppercase</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireuppercase) | <span data-ttu-id="e5cf9-165">암호에 대문자가 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="e5cf9-165">Requires an uppercase character in the password.</span></span> | `true` |

::: moniker-end

::: moniker range="< aspnetcore-2.0"

| <span data-ttu-id="e5cf9-166">속성</span><span class="sxs-lookup"><span data-stu-id="e5cf9-166">Property</span></span> | <span data-ttu-id="e5cf9-167">설명</span><span class="sxs-lookup"><span data-stu-id="e5cf9-167">Description</span></span> | <span data-ttu-id="e5cf9-168">기본</span><span class="sxs-lookup"><span data-stu-id="e5cf9-168">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="e5cf9-169">RequireDigit</span><span class="sxs-lookup"><span data-stu-id="e5cf9-169">RequireDigit</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredigit) | <span data-ttu-id="e5cf9-170">암호에 0-9 사이의 숫자를 여야 합니다.</span><span class="sxs-lookup"><span data-stu-id="e5cf9-170">Requires a number between 0-9 in the password.</span></span> | `true` |
| [<span data-ttu-id="e5cf9-171">RequiredLength</span><span class="sxs-lookup"><span data-stu-id="e5cf9-171">RequiredLength</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredlength) | <span data-ttu-id="e5cf9-172">암호의 최소 길이입니다.</span><span class="sxs-lookup"><span data-stu-id="e5cf9-172">The minimum length of the password.</span></span> | <span data-ttu-id="e5cf9-173">6</span><span class="sxs-lookup"><span data-stu-id="e5cf9-173">6</span></span> |
| [<span data-ttu-id="e5cf9-174">RequireLowercase</span><span class="sxs-lookup"><span data-stu-id="e5cf9-174">RequireLowercase</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirelowercase) | <span data-ttu-id="e5cf9-175">암호에 소문자가 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="e5cf9-175">Requires a lowercase character in the password.</span></span> | `true` |
| [<span data-ttu-id="e5cf9-176">RequireNonAlphanumeric</span><span class="sxs-lookup"><span data-stu-id="e5cf9-176">RequireNonAlphanumeric</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirenonalphanumeric) | <span data-ttu-id="e5cf9-177">암호의 영숫자가 아닌 문자가 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="e5cf9-177">Requires a non-alphanumeric character in the password.</span></span> | `true` |
| [<span data-ttu-id="e5cf9-178">RequireUppercase</span><span class="sxs-lookup"><span data-stu-id="e5cf9-178">RequireUppercase</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireuppercase) | <span data-ttu-id="e5cf9-179">암호에 대문자가 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="e5cf9-179">Requires an uppercase character in the password.</span></span> | `true` |

::: moniker-end

### <a name="sign-in"></a><span data-ttu-id="e5cf9-180">로그인</span><span class="sxs-lookup"><span data-stu-id="e5cf9-180">Sign-in</span></span>

<span data-ttu-id="e5cf9-181">다음 코드에서는 `SignIn` 설정 (기본값):</span><span class="sxs-lookup"><span data-stu-id="e5cf9-181">The following code sets `SignIn` settings (to default values):</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_si)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,44-46,50-52)] 

::: moniker-end

<span data-ttu-id="e5cf9-182">[IdentityOptions.SignIn](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.signin) 지정 된 [SignInOptions](/dotnet/api/microsoft.aspnetcore.identity.signinoptions) 표에 표시 된 속성을 사용 하 여 합니다.</span><span class="sxs-lookup"><span data-stu-id="e5cf9-182">[IdentityOptions.SignIn](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.signin) specifies the [SignInOptions](/dotnet/api/microsoft.aspnetcore.identity.signinoptions) with the properties shown in the table.</span></span>

| <span data-ttu-id="e5cf9-183">속성</span><span class="sxs-lookup"><span data-stu-id="e5cf9-183">Property</span></span> | <span data-ttu-id="e5cf9-184">설명</span><span class="sxs-lookup"><span data-stu-id="e5cf9-184">Description</span></span> | <span data-ttu-id="e5cf9-185">기본</span><span class="sxs-lookup"><span data-stu-id="e5cf9-185">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="e5cf9-186">RequireConfirmedEmail</span><span class="sxs-lookup"><span data-stu-id="e5cf9-186">RequireConfirmedEmail</span></span>](/dotnet/api/microsoft.aspnetcore.identity.signinoptions.requireconfirmedemail) | <span data-ttu-id="e5cf9-187">로그인 전자 메일을 확인된 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="e5cf9-187">Requires a confirmed email to sign in.</span></span> | `false` |
| [<span data-ttu-id="e5cf9-188">RequireConfirmedPhoneNumber</span><span class="sxs-lookup"><span data-stu-id="e5cf9-188">RequireConfirmedPhoneNumber</span></span>](/dotnet/api/microsoft.aspnetcore.identity.signinoptions.requireconfirmedphonenumber) | <span data-ttu-id="e5cf9-189">확인 된 전화번호에 로그인 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="e5cf9-189">Requires a confirmed phone number to sign in.</span></span> | `false` |

### <a name="tokens"></a><span data-ttu-id="e5cf9-190">토큰</span><span class="sxs-lookup"><span data-stu-id="e5cf9-190">Tokens</span></span>

<span data-ttu-id="e5cf9-191">[IdentityOptions.Tokens](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.tokens) 지정 된 [TokenOptions](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions) 표에 표시 된 속성을 사용 하 여 합니다.</span><span class="sxs-lookup"><span data-stu-id="e5cf9-191">[IdentityOptions.Tokens](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.tokens) specifies the [TokenOptions](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions) with the properties shown in the table.</span></span>


|                                                        <span data-ttu-id="e5cf9-192">속성</span><span class="sxs-lookup"><span data-stu-id="e5cf9-192">Property</span></span>                                                         |                                                                                      <span data-ttu-id="e5cf9-193">설명</span><span class="sxs-lookup"><span data-stu-id="e5cf9-193">Description</span></span>                                                                                      |
|-------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     [<span data-ttu-id="e5cf9-194">AuthenticatorTokenProvider</span><span class="sxs-lookup"><span data-stu-id="e5cf9-194">AuthenticatorTokenProvider</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.authenticatortokenprovider)     |                                       <span data-ttu-id="e5cf9-195">가져오거나 설정 합니다 `AuthenticatorTokenProvider` 인증자를 사용 하 여 2 단계 로그인의 유효성을 검사 하는 데 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="e5cf9-195">Gets or sets the `AuthenticatorTokenProvider` used to validate two-factor sign-ins with an authenticator.</span></span>                                       |
|       [<span data-ttu-id="e5cf9-196">ChangeEmailTokenProvider</span><span class="sxs-lookup"><span data-stu-id="e5cf9-196">ChangeEmailTokenProvider</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.changeemailtokenprovider)       |                                     <span data-ttu-id="e5cf9-197">가져오거나 설정 합니다 `ChangeEmailTokenProvider` 전자 메일 변경 확인 전자 메일에 사용 되는 토큰을 생성 하는 데 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="e5cf9-197">Gets or sets the `ChangeEmailTokenProvider` used to generate tokens used in email change confirmation emails.</span></span>                                     |
| [<span data-ttu-id="e5cf9-198">ChangePhoneNumberTokenProvider</span><span class="sxs-lookup"><span data-stu-id="e5cf9-198">ChangePhoneNumberTokenProvider</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.changephonenumbertokenprovider) |                                      <span data-ttu-id="e5cf9-199">가져오거나 설정 합니다 `ChangePhoneNumberTokenProvider` 전화 번호를 변경 하는 경우 사용 되는 토큰을 생성 하는 데 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="e5cf9-199">Gets or sets the `ChangePhoneNumberTokenProvider` used to generate tokens used when changing phone numbers.</span></span>                                      |
| [<span data-ttu-id="e5cf9-200">EmailConfirmationTokenProvider</span><span class="sxs-lookup"><span data-stu-id="e5cf9-200">EmailConfirmationTokenProvider</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.emailconfirmationtokenprovider) |                                             <span data-ttu-id="e5cf9-201">계정 확인 전자 메일에 사용 되는 토큰을 생성 하는 데 토큰 공급자를 가져오거나 설정 합니다.</span><span class="sxs-lookup"><span data-stu-id="e5cf9-201">Gets or sets the token provider used to generate tokens used in account confirmation emails.</span></span>                                              |
|     [<span data-ttu-id="e5cf9-202">PasswordResetTokenProvider</span><span class="sxs-lookup"><span data-stu-id="e5cf9-202">PasswordResetTokenProvider</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.passwordresettokenprovider)     | <span data-ttu-id="e5cf9-203">가져오거나 설정 합니다 [IUserTwoFactorTokenProvider<TUser> ](/dotnet/api/microsoft.aspnetcore.identity.iusertwofactortokenprovider-1) 암호 재설정 전자 메일에 사용 되는 토큰을 생성 하는 데 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="e5cf9-203">Gets or sets the [IUserTwoFactorTokenProvider<TUser>](/dotnet/api/microsoft.aspnetcore.identity.iusertwofactortokenprovider-1) used to generate tokens used in password reset emails.</span></span> |
|                    [<span data-ttu-id="e5cf9-204">ProviderMap</span><span class="sxs-lookup"><span data-stu-id="e5cf9-204">ProviderMap</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.providermap)                    |                <span data-ttu-id="e5cf9-205">생성 하는 데 사용 된 [사용자 토큰 공급자](/dotnet/api/microsoft.aspnetcore.identity.tokenproviderdescriptor) 공급자의 이름으로 사용 된 키를 사용 하 여 합니다.</span><span class="sxs-lookup"><span data-stu-id="e5cf9-205">Used to construct a [User Token Provider](/dotnet/api/microsoft.aspnetcore.identity.tokenproviderdescriptor) with the key used as the provider's name.</span></span>                 |

### <a name="user"></a><span data-ttu-id="e5cf9-206">사용자</span><span class="sxs-lookup"><span data-stu-id="e5cf9-206">User</span></span>

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_user)]

<span data-ttu-id="e5cf9-207">[IdentityOptions.User](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.user) 지정 된 [UserOptions](/dotnet/api/microsoft.aspnetcore.identity.useroptions) 표에 표시 된 속성을 사용 하 여 합니다.</span><span class="sxs-lookup"><span data-stu-id="e5cf9-207">[IdentityOptions.User](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.user) specifies the [UserOptions](/dotnet/api/microsoft.aspnetcore.identity.useroptions) with the properties shown in the table.</span></span>

| <span data-ttu-id="e5cf9-208">속성</span><span class="sxs-lookup"><span data-stu-id="e5cf9-208">Property</span></span> | <span data-ttu-id="e5cf9-209">설명</span><span class="sxs-lookup"><span data-stu-id="e5cf9-209">Description</span></span> | <span data-ttu-id="e5cf9-210">기본</span><span class="sxs-lookup"><span data-stu-id="e5cf9-210">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="e5cf9-211">AllowedUserNameCharacters</span><span class="sxs-lookup"><span data-stu-id="e5cf9-211">AllowedUserNameCharacters</span></span>](/dotnet/api/microsoft.aspnetcore.identity.useroptions.allowedusernamecharacters) | <span data-ttu-id="e5cf9-212">사용자 이름에 허용 되는 문자입니다.</span><span class="sxs-lookup"><span data-stu-id="e5cf9-212">Allowed characters in the username.</span></span> | <span data-ttu-id="e5cf9-213">abcdefghijklmnopqrstuvwxyz</span><span class="sxs-lookup"><span data-stu-id="e5cf9-213">abcdefghijklmnopqrstuvwxyz</span></span><br><span data-ttu-id="e5cf9-214">ABCDEFGHIJKLMNOPQRSTUVWXYZ</span><span class="sxs-lookup"><span data-stu-id="e5cf9-214">ABCDEFGHIJKLMNOPQRSTUVWXYZ</span></span><br><span data-ttu-id="e5cf9-215">0123456789</span><span class="sxs-lookup"><span data-stu-id="e5cf9-215">0123456789</span></span><br><span data-ttu-id="e5cf9-216">-.\_@+</span><span class="sxs-lookup"><span data-stu-id="e5cf9-216">-.\_@+</span></span> |
| [<span data-ttu-id="e5cf9-217">RequireUniqueEmail</span><span class="sxs-lookup"><span data-stu-id="e5cf9-217">RequireUniqueEmail</span></span>](/dotnet/api/microsoft.aspnetcore.identity.useroptions.requireuniqueemail) | <span data-ttu-id="e5cf9-218">각 사용자에 게 고유한 전자 메일에 필요 합니다.</span><span class="sxs-lookup"><span data-stu-id="e5cf9-218">Requires each user to have a unique email.</span></span> | `false` |

### <a name="cookie-settings"></a><span data-ttu-id="e5cf9-219">쿠키 설정</span><span class="sxs-lookup"><span data-stu-id="e5cf9-219">Cookie settings</span></span>

<span data-ttu-id="e5cf9-220">구성에서 앱의 쿠키 `Startup.ConfigureServices`합니다.</span><span class="sxs-lookup"><span data-stu-id="e5cf9-220">Configure the app's cookie in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="e5cf9-221">[ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionExtensions_ConfigureApplicationCookie_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_Cookies_CookieAuthenticationOptions__) 를 호출 해야 합니다 **한 후** 호출 `AddIdentity` 또는 `AddDefaultIdentity`합니다.</span><span class="sxs-lookup"><span data-stu-id="e5cf9-221">[ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionExtensions_ConfigureApplicationCookie_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_Cookies_CookieAuthenticationOptions__) must be called **after** calling `AddIdentity` or `AddDefaultIdentity`.</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_cookie)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?name=snippet_configurecookie)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-59,72-80,84)]

::: moniker-end

<span data-ttu-id="e5cf9-222">자세한 내용은 [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions)합니다.</span><span class="sxs-lookup"><span data-stu-id="e5cf9-222">For more information, see [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions).</span></span>
