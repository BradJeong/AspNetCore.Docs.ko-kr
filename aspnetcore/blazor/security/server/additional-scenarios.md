---
title: ASP.NET Core Blazor 서버 추가 보안 시나리오
author: guardrex
description: 추가 보안 시나리오에 대해 Blazor 서버를 구성하는 방법을 알아봅니다.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 06/04/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: blazor/security/server/additional-scenarios
ms.openlocfilehash: 7f92f82d6e78771494b9f2358ee19153502f8dcb
ms.sourcegitcommit: 066d66ea150f8aab63f9e0e0668b06c9426296fd
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85243046"
---
# <a name="aspnet-core-blazor-server-additional-security-scenarios"></a>ASP.NET Core Blazor 서버 추가 보안 시나리오

작성자: [Javier Calvarro Nelson](https://github.com/javiercn)

## <a name="pass-tokens-to-a-blazor-server-app"></a>Blazor 서버 앱으로 토큰 전달

Blazor 서버 앱의 Razor 구성 요소 외부에서 사용할 수 있는 토큰은 이 섹션에서 설명하는 방법을 통해 구성 요소로 전달할 수 있습니다. 완전한 `Startup.ConfigureServices` 예제와 샘플 코드를 보려면 [Passing tokens to a server-side Blazor application](https://github.com/javiercn/blazor-server-aad-sample)(서버 쪽 Blazor 애플리케이션으로 토큰 전달)을 참조하세요.

일반 Razor Pages나 MVC 앱을 인증하는 것처럼 Blazor 서버 앱을 인증합니다. 토큰을 프로비저닝하고 인증 쿠키에 저장합니다. 예를 들어:

```csharp
using Microsoft.AspNetCore.Authentication.OpenIdConnect;

...

services.Configure<OpenIdConnectOptions>(AzureADDefaults.OpenIdScheme, options =>
{
    options.ResponseType = "code";
    options.SaveTokens = true;

    options.Scope.Add("offline_access");
    options.Scope.Add("{SCOPE}");
    options.Resource = "{RESOURCE}";
});
```

액세스 토큰 및 새로 고침 토큰과 함께 초기 앱 상태를 전달하는 클래스를 정의합니다.

```csharp
public class InitialApplicationState
{
    public string AccessToken { get; set; }
    public string RefreshToken { get; set; }
}
```

Blazor 앱 내에서 [DI(종속성 주입)](xref:blazor/fundamentals/dependency-injection)로부터 토큰을 확인하는 데 사용할 수 있는 **범위가 지정된** 토큰 공급자 서비스를 정의합니다.

```csharp
public class TokenProvider
{
    public string AccessToken { get; set; }
    public string RefreshToken { get; set; }
}
```

`Startup.ConfigureServices`에서 다음 서비스를 추가합니다.

* `IHttpClientFactory`
* `TokenProvider`

```csharp
services.AddHttpClient();
services.AddScoped<TokenProvider>();
```

`_Host.cshtml` 파일에서 `InitialApplicationState`의 인스턴스를 만들어 앱에 매개 변수로 전달합니다.

```cshtml
@using Microsoft.AspNetCore.Authentication

...

@{
    var tokens = new InitialApplicationState
    {
        AccessToken = await HttpContext.GetTokenAsync("access_token"),
        RefreshToken = await HttpContext.GetTokenAsync("refresh_token")
    };
}

<app>
    <component type="typeof(App)" param-InitialState="tokens" 
        render-mode="ServerPrerendered" />
</app>
```

`App` 구성 요소(`App.razor`)에서 서비스를 확인하고 매개 변수로 받은 데이터를 사용하여 초기화합니다.

```razor
@inject TokenProvider TokenProvider

...

@code {
    [Parameter]
    public InitialApplicationState InitialState { get; set; }

    protected override Task OnInitializedAsync()
    {
        TokenProvider.AccessToken = InitialState.AccessToken;
        TokenProvider.RefreshToken = InitialState.RefreshToken;

        return base.OnInitializedAsync();
    }
}
```

보안 API 요청을 만드는 서비스에서 토큰 공급자를 주입하고 토큰을 가져와서 API를 호출합니다.

```csharp
public class WeatherForecastService
{
    private readonly TokenProvider _store;

    public WeatherForecastService(IHttpClientFactory clientFactory, 
        TokenProvider tokenProvider)
    {
        Client = clientFactory.CreateClient();
        _store = tokenProvider;
    }

    public HttpClient Client { get; }

    public async Task<WeatherForecast[]> GetForecastAsync(DateTime startDate)
    {
        var token = _store.AccessToken;
        var request = new HttpRequestMessage(HttpMethod.Get, 
            "https://localhost:5003/WeatherForecast");
        request.Headers.Add("Authorization", $"Bearer {token}");
        var response = await Client.SendAsync(request);
        response.EnsureSuccessStatusCode();

        return await response.Content.ReadAsAsync<WeatherForecast[]>();
    }
}
```

## <a name="set-the-authentication-scheme"></a>인증 체계 설정

여러 인증 미들웨어를 사용하여 인증 체계가 두 개 이상인 앱의 경우 Blazor가 사용하는 체계를 `Startup.Configure`의 엔드포인트 구성에서 명시적으로 설정할 수 있습니다. 다음 예제에서는 Azure Active Directory 체계를 설정합니다.

```csharp
endpoints.MapBlazorHub().RequireAuthorization(
    new AuthorizeAttribute 
    {
        AuthenticationSchemes = AzureADDefaults.AuthenticationScheme
    });
```

## <a name="use-open-id-connect-oidc-v20-endpoints"></a>OIDC(Open ID Connect) v2.0 엔드포인트 사용

인증 라이브러리 및 Blazor 템플릿은 OIDC(Open ID Connect) v1.0 엔드포인트를 사용합니다. v2.0 엔드포인트를 사용하려면 <xref:Microsoft.AspNetCore.Builder.OpenIdConnectOptions>에서 <xref:Microsoft.AspNetCore.Builder.OpenIdConnectOptions.Authority?displayProperty=nameWithType> 옵션을 구성해야 합니다.

```csharp
services.Configure<OpenIdConnectOptions>(AzureADDefaults.OpenIdScheme, 
    options =>
    {
        options.Authority += "/v2.0";
    }
```

또는 앱 설정(`appsettings.json`) 파일에서 설정을 수행할 수 있습니다.

```json
{
  "AzureAd": {
    "Authority": "https://login.microsoftonline.com/common/oauth2/v2.0/",
    ...
  }
}
```

인증 기관에 대한 세그먼트의 추적이 앱의 OIDC 공급자에 적합하지 않은 경우(예: 비 AAD 공급자) <xref:Microsoft.AspNetCore.Builder.OpenIdConnectOptions.Authority> 속성을 직접 설정합니다. <xref:Microsoft.AspNetCore.Builder.OpenIdConnectOptions> 또는 앱 설정 파일에서 <xref:Microsoft.AspNetCore.Builder.OpenIdConnectOptions.Authority> 키를 사용하여 속성을 설정합니다.

### <a name="code-changes"></a>코드 변경 내용

* ID 토큰의 클레임 목록은 v2.0 엔드포인트의 경우 변경됩니다. 자세한 내용은 Azure 설명서에서 [Microsoft ID 플랫폼(v2.0)으로 업데이트하는 이유](/azure/active-directory/azuread-dev/azure-ad-endpoint-comparison)를 참조하세요.
* v2.0 엔드포인트의 경우 범위 URI에 리소스가 지정되어 있으므로 <xref:Microsoft.AspNetCore.Builder.OpenIdConnectOptions>에서 <xref:Microsoft.AspNetCore.Builder.OpenIdConnectOptions.Resource?displayProperty=nameWithType> 속성 설정을 제거합니다.

  ```csharp
  services.Configure<OpenIdConnectOptions>(AzureADDefaults.OpenIdScheme, options => 
      {
          ...
          options.Resource = "...";    // REMOVE THIS LINE
          ...
      }
      ```

  For more information, see [Scopes, not resources](/azure/active-directory/azuread-dev/azure-ad-endpoint-comparison#scopes-not-resources) in the Azure documentation.

### App ID URI

* When using v2.0 endpoints, APIs define an *`App ID URI`*, which is meant to represent a unique identifier for the API.
* All scopes include the App ID URI as a prefix, and v2.0 endpoints emit access tokens with the App ID URI as the audience.
* When using V2.0 endpoints, the client ID configured in the Server API changes from the API Application ID (Client ID) to the App ID URI.

`appsettings.json`:

```json
{
  "AzureAd": {
    ...
    "ClientId": "https://{TENANT}.onmicrosoft.com/{APP NAME}"
    ...
  }
}
```

OIDC 공급자 앱 등록 설명에서 사용해야 하는 앱 ID URI를 확인할 수 있습니다.
