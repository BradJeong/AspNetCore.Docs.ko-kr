---
title: ASP.NET Core 2.1 MVC SameSite cookie 샘플
author: rick-anderson
description: ASP.NET Core 2.1 MVC SameSite cookie 샘플
monikerRange: '>= aspnetcore-2.1 < aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 12/03/2019
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: security/samesite/mvc21
ms.openlocfilehash: 4239321531f3a7696a15b1dea164450ea0860c2b
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/26/2020
ms.locfileid: "85409061"
---
# <a name="aspnet-core-21-mvc-samesite-cookie-sample"></a><span data-ttu-id="c1b55-103">ASP.NET Core 2.1 MVC SameSite cookie 샘플</span><span class="sxs-lookup"><span data-stu-id="c1b55-103">ASP.NET Core 2.1 MVC SameSite cookie sample</span></span>

<span data-ttu-id="c1b55-104">ASP.NET Core 2.1는 [SameSite](https://www.owasp.org/index.php/SameSite) 특성에 대 한 기본 제공 지원을 제공 하지만 원래 표준에 기록 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="c1b55-104">ASP.NET Core 2.1 has built-in support for the [SameSite](https://www.owasp.org/index.php/SameSite) attribute, but it was written to the original standard.</span></span> <span data-ttu-id="c1b55-105">[패치 된 동작이](https://github.com/dotnet/aspnetcore/issues/8212) 의 의미를 변경 `SameSite.None` 하 여 값을 `None` 전혀 내보내지 않고 sameSite 특성을 값으로 내보냅니다.</span><span class="sxs-lookup"><span data-stu-id="c1b55-105">The [patched behavior](https://github.com/dotnet/aspnetcore/issues/8212) changed the meaning of `SameSite.None` to emit the sameSite attribute with a value of `None`, rather than not emit the value at all.</span></span> <span data-ttu-id="c1b55-106">값을 내보내지 않으려면 `SameSite` 쿠키에 대 한 속성을-1로 설정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c1b55-106">If you want to not emit the value you can set the `SameSite` property on a cookie to -1.</span></span>

## <a name="writing-the-samesite-attribute"></a><a name="sampleCode"></a><span data-ttu-id="c1b55-107">SameSite 특성 작성</span><span class="sxs-lookup"><span data-stu-id="c1b55-107">Writing the SameSite attribute</span></span>

<span data-ttu-id="c1b55-108">다음은 쿠키에 SameSite 특성을 작성 하는 방법의 예입니다.</span><span class="sxs-lookup"><span data-stu-id="c1b55-108">Following is an example of how to write a SameSite attribute on a cookie:</span></span>

```c#
var cookieOptions = new CookieOptions
{
    // Set the secure flag, which Chrome's changes will require for SameSite none.
    // Note this will also require you to be running on HTTPS
    Secure = true,

    // Set the cookie to HTTP only which is good practice unless you really do need
    // to access it client side in scripts.
    HttpOnly = true,

    // Add the SameSite attribute, this will emit the attribute with a value of none.
    // To not emit the attribute at all set the SameSite property to (SameSiteMode)(-1).
    SameSite = SameSiteMode.None
};

// Add the cookie to the response cookie collection
Response.Cookies.Append(CookieName, "cookieValue", cookieOptions);
```

## <a name="setting-cookie-authentication-and-session-state-cookies"></a><span data-ttu-id="c1b55-109">쿠키 인증 및 세션 상태 쿠키 설정</span><span class="sxs-lookup"><span data-stu-id="c1b55-109">Setting Cookie Authentication and Session State cookies</span></span>

<span data-ttu-id="c1b55-110">쿠키 인증, 세션 상태 및 [기타 다양 한 구성 요소](https://docs.microsoft.com/aspnet/core/security/samesite?view=aspnetcore-2.1) 는 쿠키 옵션을 통해 sameSite 옵션을 설정 합니다. 예를 들면</span><span class="sxs-lookup"><span data-stu-id="c1b55-110">Cookie authentication, session state and [various other components](https://docs.microsoft.com/aspnet/core/security/samesite?view=aspnetcore-2.1) set their sameSite options via Cookie options, for example</span></span>

```c#
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        options.Cookie.SameSite = SameSiteMode.None;
        options.Cookie.SecurePolicy = CookieSecurePolicy.Always;
        options.Cookie.IsEssential = true;
    });

services.AddSession(options =>
{
    options.Cookie.SameSite = SameSiteMode.None;
    options.Cookie.SecurePolicy = CookieSecurePolicy.Always;
    options.Cookie.IsEssential = true;
});
```

<span data-ttu-id="c1b55-111">위의 코드에서 쿠키 인증과 세션 상태는 모두 sameSite 특성을로 설정 하 `None` 고, 특성을 값으로 내보내고, `None` 보안 특성을 true로 설정 합니다.</span><span class="sxs-lookup"><span data-stu-id="c1b55-111">In the preceding code, both cookie authentication and session state set their sameSite attribute to `None`, emitting the attribute with a `None` value, and also set the Secure attribute to true.</span></span>

### <a name="run-the-sample"></a><span data-ttu-id="c1b55-112">샘플 실행</span><span class="sxs-lookup"><span data-stu-id="c1b55-112">Run the sample</span></span>

<span data-ttu-id="c1b55-113">[샘플 프로젝트](https://github.com/blowdart/AspNetSameSiteSamples/tree/master/AspNetCore21MVC)를 실행 하는 경우에는 초기 페이지에서 브라우저 디버거를 로드 하 고 사이트에 대 한 쿠키 컬렉션을 보는 데 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="c1b55-113">If you run the [sample project](https://github.com/blowdart/AspNetSameSiteSamples/tree/master/AspNetCore21MVC), load your browser debugger on the initial page and use it to view the cookie collection for the site.</span></span> <span data-ttu-id="c1b55-114">Edge 및 Chrome에서이 작업을 수행 하려면 `F12` 탭을 선택 하 `Application` 고 섹션의 옵션 아래에서 사이트 URL을 클릭 `Cookies` `Storage` 합니다.</span><span class="sxs-lookup"><span data-stu-id="c1b55-114">To do so in Edge and Chrome press `F12` then select the `Application` tab and click the site URL under the `Cookies` option in the `Storage` section.</span></span>

![브라우저 디버거 쿠키 목록](BrowserDebugger.png)

<span data-ttu-id="c1b55-116">위의 이미지에서 볼 수 있습니다. 예를 들어 "SameSite 쿠키 만들기" 단추를 클릭 하면 샘플 `Lax` [코드](#sampleCode)에 설정 된 값과 일치 하는 SameSite 특성 값이 포함 됩니다.</span><span class="sxs-lookup"><span data-stu-id="c1b55-116">You can see from the image above that the cookie created by the sample when you click the "Create SameSite Cookie" button has a SameSite attribute value of `Lax`, matching the value set in the [sample code](#sampleCode).</span></span>

## <a name="intercepting-cookies"></a><a name="interception"></a><span data-ttu-id="c1b55-117">쿠키 가로채기</span><span class="sxs-lookup"><span data-stu-id="c1b55-117">Intercepting cookies</span></span>

<span data-ttu-id="c1b55-118">쿠키를 가로채 고 사용자의 브라우저 에이전트에서 지원에 따라 없음 값을 조정 하려면 미들웨어를 사용 해야 합니다 `CookiePolicy` .</span><span class="sxs-lookup"><span data-stu-id="c1b55-118">In order to intercept cookies, to adjust the none value according to its support in the user's browser agent you must use the `CookiePolicy` middleware.</span></span> <span data-ttu-id="c1b55-119">쿠키를 작성 하 고 내에서 구성 된 구성 요소 **보다 먼저** http 요청 파이프라인에 배치 해야 합니다 `ConfigureServices()` .</span><span class="sxs-lookup"><span data-stu-id="c1b55-119">This must be placed into the http request pipeline **before** any components that write cookies and configured within `ConfigureServices()`.</span></span>

<span data-ttu-id="c1b55-120">`app.UseCookiePolicy()` `Configure(IApplicationBuilder, IHostingEnvironment)` [Startup.cs](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNetCore21MVC/Startup.cs)의 메서드에서 파이프라인 사용에 삽입 합니다.</span><span class="sxs-lookup"><span data-stu-id="c1b55-120">To insert it into the pipeline use `app.UseCookiePolicy()` in the `Configure(IApplicationBuilder, IHostingEnvironment)` method in [Startup.cs](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNetCore21MVC/Startup.cs).</span></span> <span data-ttu-id="c1b55-121">예를 들면 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="c1b55-121">For example:</span></span>

```c#
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
       app.UseDeveloperExceptionPage();
    }
    else
    {
        app.UseExceptionHandler("/Home/Error");
        app.UseHsts();
    }

    app.UseHttpsRedirection();
    app.UseStaticFiles();
    app.UseCookiePolicy();
    app.UseAuthentication();
    app.UseSession();

    app.UseMvc(routes =>
    {
        routes.MapRoute(
            name: "default",
            template: "{controller=Home}/{action=Index}/{id?}");
    });
}
```

<span data-ttu-id="c1b55-122">그런 다음 쿠키가 `ConfigureServices(IServiceCollection services)` 추가 되거나 삭제 될 때 도우미 클래스를 호출 하도록 쿠키 정책을 구성 합니다.</span><span class="sxs-lookup"><span data-stu-id="c1b55-122">Then in the `ConfigureServices(IServiceCollection services)` configure the cookie policy to call out to a helper class when cookies are appended or deleted.</span></span> <span data-ttu-id="c1b55-123">예를 들면 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="c1b55-123">For example:</span></span>

```c#
public void ConfigureServices(IServiceCollection services)
{
    services.Configure<CookiePolicyOptions>(options =>
    {
        options.CheckConsentNeeded = context => true;
        options.MinimumSameSitePolicy = SameSiteMode.None;
        options.OnAppendCookie = cookieContext =>
            CheckSameSite(cookieContext.Context, cookieContext.CookieOptions);
        options.OnDeleteCookie = cookieContext =>
            CheckSameSite(cookieContext.Context, cookieContext.CookieOptions);
    });
}

private void CheckSameSite(HttpContext httpContext, CookieOptions options)
{
    if (options.SameSite == SameSiteMode.None)
    {
        var userAgent = httpContext.Request.Headers["User-Agent"].ToString();
        if (SameSite.BrowserDetection.DisallowsSameSiteNone(userAgent))
        {
            options.SameSite = (SameSiteMode)(-1);
        }
    }
}
```

<span data-ttu-id="c1b55-124">도우미 함수 `CheckSameSite(HttpContext, CookieOptions)` :</span><span class="sxs-lookup"><span data-stu-id="c1b55-124">The helper function `CheckSameSite(HttpContext, CookieOptions)`:</span></span>

* <span data-ttu-id="c1b55-125">쿠키가 요청에 추가 되거나 요청에서 삭제 될 때 호출 됩니다.</span><span class="sxs-lookup"><span data-stu-id="c1b55-125">Is called when cookies are appended to the request or deleted from the request.</span></span>
* <span data-ttu-id="c1b55-126">`SameSite`속성이로 설정 되어 있는지 확인 `None` 합니다.</span><span class="sxs-lookup"><span data-stu-id="c1b55-126">Checks to see if the `SameSite` property is set to `None`.</span></span>
* <span data-ttu-id="c1b55-127">`SameSite`가로 설정 되어 `None` 있고 현재 사용자 에이전트가 none 특성 값을 지원 하지 않는 것으로 알려진 경우</span><span class="sxs-lookup"><span data-stu-id="c1b55-127">If `SameSite` is set to `None` and the current user agent is known to not support the none attribute value.</span></span> <span data-ttu-id="c1b55-128">확인은 [SameSiteSupport](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/samesite/sample/snippets/SameSiteSupport.cs) 클래스를 사용 하 여 수행 됩니다.</span><span class="sxs-lookup"><span data-stu-id="c1b55-128">The check is done using the [SameSiteSupport](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/samesite/sample/snippets/SameSiteSupport.cs) class:</span></span>
  * <span data-ttu-id="c1b55-129">`SameSite`속성을로 설정 하 여 값을 내보내지 않도록 설정 합니다.`(SameSiteMode)(-1)`</span><span class="sxs-lookup"><span data-stu-id="c1b55-129">Sets `SameSite` to not emit the value by setting the property to `(SameSiteMode)(-1)`</span></span>

## <a name="targeting-net-framework"></a><span data-ttu-id="c1b55-130">대상 지정 .NET Framework</span><span class="sxs-lookup"><span data-stu-id="c1b55-130">Targeting .NET Framework</span></span>

<span data-ttu-id="c1b55-131">ASP.NET Core 및 System.web (ASP.NET 클래식)에는 SameSite의 독립적인 구현이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c1b55-131">ASP.NET Core and System.Web (ASP.NET Classic) have independent implementations of SameSite.</span></span> <span data-ttu-id="c1b55-132">.NET Framework에 대 한 SameSite KB 패치는 ASP.NET Core를 사용 하는 경우에는 필요 하지 않으며 SameSite에 대 한 최소 프레임 워크 버전 요구 사항 (.NET 4.7.2)이 ASP.NET Core에 적용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="c1b55-132">The SameSite KB patches for .NET Framework are not required if using ASP.NET Core nor does the System.Web SameSite minimum framework version requirement (.NET 4.7.2) apply to ASP.NET Core.</span></span>

<span data-ttu-id="c1b55-133">.NET의 ASP.NET Core 적절 한 수정을 얻으려면 nuget 패키지 종속성을 업데이트 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="c1b55-133">ASP.NET Core on .NET requires updating nuget package dependencies to get the appropriate fixes.</span></span>

<span data-ttu-id="c1b55-134">.NET Framework에 대 한 ASP.NET Core 변경 내용을 얻으려면 패치 된 패키지 및 버전 (2.1.14 이상 2.1 버전)에 대 한 직접 참조를 확인 합니다.</span><span class="sxs-lookup"><span data-stu-id="c1b55-134">To get the ASP.NET Core changes for .NET Framework ensure that you have a direct reference to the patched packages and versions (2.1.14 or later 2.1 versions).</span></span>

```xml
<PackageReference Include="Microsoft.Net.Http.Headers" Version="2.1.14" />
<PackageReference Include="Microsoft.AspNetCore.CookiePolicy" Version="2.1.14" />
```

### <a name="more-information"></a><span data-ttu-id="c1b55-135">추가 정보</span><span class="sxs-lookup"><span data-stu-id="c1b55-135">More Information</span></span>
 
<span data-ttu-id="c1b55-136">[Chrome 업데이트](https://www.chromium.org/updates/same-site) 
 [ASP.NET Core SameSite 설명서](https://docs.microsoft.com/aspnet/core/security/samesite?view=aspnetcore-2.1) 
 [ASP.NET Core 2.1 SameSite 변경 공지](https://github.com/dotnet/aspnetcore/issues/8212)</span><span class="sxs-lookup"><span data-stu-id="c1b55-136">[Chrome Updates](https://www.chromium.org/updates/same-site)
[ASP.NET Core SameSite Documentation](https://docs.microsoft.com/aspnet/core/security/samesite?view=aspnetcore-2.1)
[ASP.NET Core 2.1 SameSite Change Announcement](https://github.com/dotnet/aspnetcore/issues/8212)</span></span>