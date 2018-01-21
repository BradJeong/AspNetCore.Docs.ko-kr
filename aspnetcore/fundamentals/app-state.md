---
title: "ASP.NET Core에서 세션 및 응용 프로그램 상태"
author: rick-anderson
description: "요청 간에 유지 응용 프로그램 및 사용자 (세션) 상태에 접근 합니다."
ms.author: riande
manager: wpickett
ms.date: 11/27/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/app-state
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 13b4d759ae574cdf9899ca148f0ffd3d9df6f9ae
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/19/2018
---
# <a name="introduction-to-session-and-application-state-in-aspnet-core"></a>ASP.NET Core에서 세션 및 응용 프로그램 상태 소개

여 [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), 및 [Diana LaRose](https://github.com/DianaLaRose)

HTTP는 상태 비저장 프로토콜입니다. 웹 서버 독립적인 요청으로 각 HTTP 요청을 처리 하 고 이전 요청에서 사용자 값을 유지 하지 않습니다. 이 문서에서는 응용 프로그램 및 요청 간에 세션 상태를 유지 하기 위해 다양 한 방법 설명 합니다. 

## <a name="session-state"></a>세션 상태

세션 상태는 사용자가 웹앱을 탐색하는 동안 사용자 데이터를 저장하는 데 사용할 수 있는 ASP.NET Core의 기능입니다. 세션 상태 서버에 사전 또는 해시 테이블, 구성 된 브라우저에서 요청을 통해 데이터를 유지 합니다. 세션 데이터는 캐시에 의해 지원 됩니다.

ASP.NET Core 클라이언트 각 요청과 함께 서버에 전송 된 세션 ID를 포함 하는 쿠키를 제공 하 여 세션 상태를 유지 합니다. 서버 세션 ID를 사용 하 여 세션 데이터를 가져옵니다. 세션 쿠키의 브라우저 관련 이기 때문에 브라우저 세션을 공유할 수 없습니다. 세션 쿠키는 브라우저 세션이 끝나면 삭제 됩니다. 쿠키가 만료 된 세션에 대 한 수신 되 면 같은 세션 쿠키를 사용 하는 새 세션 생성 됩니다. 

서버는 마지막 요청 이후 제한 된 시간에 대 한 세션을 유지합니다. 세션 제한 시간을 설정 하거나 기본값인 20 분을 사용할 수 있습니다. 세션 상태는 영구적으로 유지 될 필요가 없습니다 있지만 특정 세션에 고유한 사용자 데이터를 저장 하는 데 이상적입니다. 데이터에서에서 삭제 됩니다 백업 저장소 중 하나를 호출할 때 `Session.Clear` 세션 데이터 저장소에서 만료 될 때 또는 합니다. 브라우저를 닫을 때 세션 쿠키를 삭제 하는 경우 또는 서버를 알지 못합니다.

> [!WARNING]
> 세션에 중요 한 데이터를 저장 하지 마십시오. 클라이언트 수 하지 브라우저를 닫고 세션 쿠키의 선택을 취소 (및 일부 브라우저 세션 쿠키 활성 상태를 유지할 전체 windows). 또한 세션; 단일 사용자로 제한 될 수 있습니다. 다음 사용자는 동일한 세션 계속 될 수 있습니다.

메모리 내 세션 공급자는 로컬 서버에 세션 데이터를 저장합니다. 서버 팜에서 웹 앱을 실행 하려는 경우 특정 서버에 각 세션에 연결할 고정 세션을 사용 해야 합니다. Windows Azure 웹 사이트 플랫폼 고정 세션 (응용 프로그램 요청 라우팅 또는 ARR) 기본값으로 사용 됩니다. 그러나는 고정 세션 확장성에 영향을 줄 수 있으며 웹 응용 프로그램 업데이트 복잡 하 게 됩니다. Redis를 사용 하는 편이 더 나은지 되었거나 SQL Server 분산 캐시는 고정 세션이 필요 하지 않습니다. 자세한 내용은 참조 [분산 캐시 작업](xref:performance/caching/distributed)합니다. 서비스 공급자를 설정에 대 한 자세한 내용은 참조 하십시오. [세션 구성](#configuring-session) 이 문서의 뒷부분에 나오는 합니다.

<a name="temp"></a>
## <a name="tempdata"></a>TempData

ASP.NET Core MVC 노출 된 [TempData](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) 속성에는 [컨트롤러](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.controller?view=aspnetcore-2.0)합니다. 이 속성은 판독될 때까지 데이터를 저장합니다. `Keep` 및 `Peek` 메서드를 사용하여 삭제 없이 데이터를 검사할 수 있습니다. `TempData`단일 요청 보다 많이 필요한 데이터의 경우 리디렉션, 데 특히 유용 합니다. `TempData`이 공급자에 의해 구현 TempData 예를 들어 세션 상태 또는 쿠키를 사용 하 여 합니다.

<a name="tempdata-providers"></a>
### <a name="tempdata-providers"></a>TempData 공급자

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

ASP.NET Core 2.0 이상에서는 TempData 쿠키에 저장 하려면 기본적으로는 쿠키 기반 TempData 공급자를 사용 합니다.

쿠키 데이터가 인코딩된는 [Base64UrlTextEncoder](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.webutilities.base64urltextencoder?view=aspnetcore-2.0)합니다. 단일 쿠키 크기가 쿠키 암호화 되 고 청크를 있으므로 제한에 ASP.NET Core 1.x 적용 되지 않습니다. 쿠키 데이터가 같은 보안 문제를 일으킬 수 암호화 된 데이터를 압축 하기 때문에 압축 되지 않은 [범죄](https://wikipedia.org/wiki/CRIME_(security_exploit)) 및 [위반](https://wikipedia.org/wiki/BREACH_(security_exploit)) 공격입니다. 쿠키 기반 TempData 공급자에 대 한 자세한 내용은 참조 하십시오. [CookieTempDataProvider](https://github.com/aspnet/Mvc/blob/dev/src/Microsoft.AspNetCore.Mvc.ViewFeatures/ViewFeatures/CookieTempDataProvider.cs)합니다.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

ASP.NET Core 1.0 및 1.1에서는 세션 상태 TempData 공급자가 기본값입니다.

--------------

<a name="choose-temp"></a>
### <a name="choosing-a-tempdata-provider"></a>TempData 공급자 선택

TempData 공급자를 선택 하려면와 같은 몇 가지 고려 사항:

1. 응용 프로그램이 이미 다른 용도에 세션 상태를 사용 합니까? 이 경우 응용 프로그램 데이터의 크기) (외에 추가 비용 없이 세션 상태 TempData 공급자를 사용 하 여 있습니다.
2. 응용 프로그램 사용 TempData만 제한적으로 상대적으로 적은 양의 데이터 (최대 500 바이트)에 대 한 합니까? 따라서 쿠키 TempData 공급자 TempData를 전달 하는 각 요청에 작은 비용 추가 됩니다. 그렇지 않은 경우 세션 상태 TempData 공급자 TempData 사용 될 때까지 왕복 많은 양의 각 요청에 데이터를 방지 하는 데 특히 도움이 될 수 있습니다.
3. 응용 프로그램 웹 팜 (다중 서버)에서 실행 됩니까? 이 경우 쿠키 TempData 공급자를 사용 하는 데 필요한 추가 구성 없이 있습니다.

> [!NOTE]
> 대부분의 웹 클라이언트 (예: 웹 브라우저)에 각 쿠키, 쿠키의 총 수 또는 둘 다의 최대 크기에 대 한 제한을 적용합니다. 따라서 쿠키 TempData 공급자를 사용 하는 경우 응용 프로그램에는 이러한 제한을 초과 하지 않습니다 확인 합니다. 암호화의 오버 헤드에 대 한 계정 및 청크는 데이터의 전체 크기를 고려 합니다.

<a name="config-temp"></a>
### <a name="configure-the-tempdata-provider"></a>TempData 공급자 구성

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

쿠키 기반 TempData 공급자는 기본적으로 사용 됩니다. 다음 `Startup` 클래스 코드 TempData 세션 기반 공급자를 구성 합니다.

[!code-csharp[](app-state/sample/src/WebAppSessionDotNetCore2.0App/StartupTempDataSession.cs?name=snippet_TempDataSession&highlight=4,6,11)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

다음 `Startup` 클래스 코드 TempData 세션 기반 공급자를 구성 합니다.

[!code-csharp[](app-state/sample/src/WebAppSession/StartupTempDataSession.cs?name=snippet_TempDataSession&highlight=4,9)]

---

순서 지정 하는 것은 미들웨어 구성 요소에 대 한 중요 합니다. 위의 예에서 형식의 예외가 `InvalidOperationException` 발생 때 `UseSession` 후 호출 `UseMvcWithDefaultRoute`합니다. 참조 [미들웨어 순서](xref:fundamentals/middleware#ordering) 자세한 세부 정보에 대 한 합니다.

> [!IMPORTANT]
> .NET Framework 대상 지정 및 세션 기반 공급자를 사용 하 여 추가 하는 경우는 [Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session) NuGet 패키지를 프로젝트입니다.

## <a name="query-strings"></a>쿼리 문자열

새 요청 쿼리 문자열에 추가 하 여 다른 한 요청에서 제한 된 양의 데이터를 전달할 수 있습니다. 이에 포함 된 상태를 전자 메일 또는 소셜 네트워크를 통해 공유할 수 있는 링크를 허용 하는 영구적인 방식으로 상태를 캡처하는 데 유용 합니다. 그러나 이러한 이유로 해야 사용 하지 쿼리 문자열을 중요 한 데이터에 대 한 합니다. 뿐 아니라 쉽게 공유 되 고 쿼리 문자열에 데이터를 포함 하 여 만들 수에 대 한 기회 [교차 사이트 요청 위조 (CSRF)](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) 공격으로, 사용자가 인증 하는 동안 악성 사이트를 방문 하도록 유도할 수 있습니다. 공격자가 다음 앱에서 사용자 데이터를 도용 하거나 사용자를 대신 하 여 악의적인 작업을 수행할 수 있습니다. 유지 된 모든 응용 프로그램이 나 세션 상태는 CSRF 공격 으로부터 보호 해야 합니다. CSRF 공격에 대 한 자세한 내용은 참조 하십시오. [방지 교차 사이트 요청 위조 (XSRF/CSRF) 공격에 ASP.NET Core](../security/anti-request-forgery.md)합니다.

## <a name="post-data-and-hidden-fields"></a>게시 데이터 및 숨김된 필드

데이터 숨겨진된 양식 필드에 저장 되며 다음 요청에 다시 게시 될 수 있습니다. 다중 페이지 폼에서 일반적입니다. 그러나 클라이언트 잠재적으로 데이터를 변조할 수 있습니다, 때문에 서버 해야 항상 유효성 합니다. 

## <a name="cookies"></a>쿠키

쿠키는 웹 응용 프로그램에서 사용자 관련 데이터를 저장 하는 방법을 제공 합니다. 쿠키는 모든 요청과 함께 전송 되므로, 해당 크기를 최소로 유지 되어야 합니다. 이상적으로 서버에 저장 된 실제 데이터와 식별자만 쿠키에 저장 되어야 합니다. 대부분의 브라우저는 4096 바이트에 쿠키를 제한합니다. 또한 제한 된 개수의 쿠키는 각 도메인에 대해 사용할 수 있습니다.  

쿠키는 변조 될 이기 때문에 서버에서 이러한 유효성을 검사 해야 합니다. 클라이언트에 쿠키의 지 속성 사용자 작업 및 만료 될 수 있는 경우에 클라이언트에서 데이터 지 속성 형식의 가장 영 속은 일반적으로.

쿠키는 자주 사용 개인 설정에 대 한 콘텐츠 알려진된 사용자에 대 한 사용자 지정한 합니다. 사용자만 파악 하 고 대부분의 경우에서 인증 되지 않은, 때문에 사용자 이름, 계정 이름 또는 고유한 사용자 ID (예: GUID)를 쿠키에 저장 하 여 쿠키를 일반적으로 보호할 수 있습니다. 그런 다음 사이트의 사용자 개인 설정 인프라에 액세스할 쿠키를 사용할 수 있습니다.

## <a name="httpcontextitems"></a>HttpContext.Items

`Items` 컬렉션은 데이터를 저장 하는 좋은 위치 하나의 특정 요청을 처리 동안에 필요 합니다. 컬렉션의 각 요청 후 삭제 됩니다. `Items` 컬렉션은 가장 요청 하는 중에 다른 시점에서 작동 하 고 매개 변수를 전달할 방법이 없습니다 직접 때 통신 하도록 구성 요소 또는 미들웨어에 대 한 방법으로 사용 됩니다. 자세한 내용은 참조 [HttpContext.Items 작업](#working-with-httpcontextitems)이 문서의 뒷부분에 나오는 합니다.

## <a name="cache"></a>캐시

캐싱은 데이터 저장 및 검색 하는 효율적인 방법입니다. 시간 및 기타 고려 사항에 따라 캐시 된 항목의 수명을 제어할 수 있습니다. 에 대 한 자세한 내용은 [캐싱](../performance/caching/index.md)합니다.

<a name="session"></a>
## <a name="working-with-session-state"></a>세션 상태 사용

### <a name="configuring-session"></a>세션 구성

`Microsoft.AspNetCore.Session` 패키지 세션 상태를 관리 하기 위한 미들웨어를 제공 합니다. 세션 미들웨어를 사용 하도록 설정 하려면 `Startup` 포함 해야 합니다.

- 중 하나는 [IDistributedCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.distributed.idistributedcache) 메모리 캐시 합니다. `IDistributedCache` 구현이 세션에 대 한 백업 저장소로 사용 됩니다.
- [AddSession](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.dependencyinjection.sessionservicecollectionextensions#Microsoft_Extensions_DependencyInjection_SessionServiceCollectionExtensions_AddSession_Microsoft_Extensions_DependencyInjection_IServiceCollection_) 필요 "Microsoft.AspNetCore.Session" NuGet 패키지를 호출 합니다.
- [UseSession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.sessionmiddlewareextensions#methods_) 호출 합니다.

다음 코드에서는 메모리 내 세션 공급자를 설정 하는 방법을 보여 줍니다.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](app-state/sample/src/WebAppSessionDotNetCore2.0App/Startup.cs?highlight=11-19,24)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](app-state/sample/src/WebAppSession/Startup.cs?highlight=11-19,24)]

---

세션을 참조할 수 있습니다 `HttpContext` 일단 설치 되 고 구성 합니다.

액세스 하려는 경우 `Session` 전에 `UseSession` 예외 호출 된 `InvalidOperationException: Session has not been configured for this application or request` throw 됩니다.

새 하려고 하면 `Session` (즉, 세션 쿠키가 없는 만든)에 쓰기를 이미 시작 된 후의 `Response` 스트림, 예외 `InvalidOperationException: The session cannot be established after the response has started` throw 됩니다. 웹 서버 로그에서 예외를 확인할 수 있습니다. 브라우저에 표시 되지 않습니다.

### <a name="loading-session-asynchronously"></a>세션을 비동기적으로 로드 

기본 세션 레코드를 로드 하는 기본 세션 공급자에서 ASP.NET Core [IDistributedCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.distributed.idistributedcache) 경우에만 비동기적으로 저장소는 [ISession.LoadAsync](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.isession#Microsoft_AspNetCore_Http_ISession_LoadAsync) 명시적으로 메서드는 하기 전에  `TryGetValue`, `Set`, 또는 `Remove` 메서드. 경우 `LoadAsync` 내부 먼저 호출 되지 않습니다는 영향을 줄 수 크기를 조정 하는 앱의 기능 세션 레코드를 동기적으로 로드 합니다.

이 패턴을 적용 하는 응용 프로그램을 래핑하는 [DistributedSessionStore](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.session.distributedsessionstore) 및 [DistributedSession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.session.distributedsession) 경우 예외를 throw 하는 버전와 구현 된 `LoadAsync` 메서드는 없습니다 이전에 호출 `TryGetValue`, `Set`, 또는 `Remove`합니다. 서비스 컨테이너에 래핑된 버전을 등록 합니다.

### <a name="implementation-details"></a>구현 세부 정보

세션 쿠키를 사용 하 여 추적 하 고 단일 브라우저에서 요청을 식별 합니다. 기본적으로이 쿠키 이름은 "입니다. 경로 사용 하 여 AspNet.Session"하며"/"입니다. 쿠키 기본 도메인을 지정 하지 않으므로, 것 사용할 수 없어 클라이언트 쪽 스크립트를 페이지에서 (때문에 `CookieHttpOnly` 기본값으로 `true`).

사용 하 여 세션 기본값을 재정의 하려면 `SessionOptions`:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](app-state/sample/src/WebAppSessionDotNetCore2.0App/StartupCopy.cs?name=snippet1&highlight=8-12)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](app-state/sample/src/WebAppSession/StartupCopy.cs?name=snippet1&highlight=8-12)]

---

서버에서 사용 하는 `IdleTimeout` 기간 세션 유휴 상태일 수 있는 내용이 중단 하기 전에 확인 하는 속성입니다. 이 속성은 쿠키 만료 기한 무관 합니다. (에서 읽기 또는 쓰기) 세션 미들웨어를 통해 전달 되는 각 요청은 제한 시간이 다시 설정 합니다.

때문에 `Session` 은 *잠기지 않은*, 마지막 세션의 내용을 수정 하려고 하는 둘 다 두 요청 첫 번째 재정의 합니다. `Session`로 구현 됩니다는 *일관 된 세션*, 즉, 모든 내용을 함께 저장 됩니다. 세션 (서로 다른 키)의 다른 부분을 수정 하는 두 요청 서로 저하 계속 될 수 있습니다.

### <a name="setting-and-getting-session-values"></a>설정 및 세션 값 가져오기

세션을 통해 액세스는 `Session` 속성 `HttpContext`합니다. 이 속성은 한 [ISession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.isession) 구현 합니다.

다음 예제에서는 설정 및 가져오기는 int와 문자열을 보여 줍니다.

[!code-csharp[Main](app-state/sample/src/WebAppSession/Controllers/HomeController.cs?range=8-27,49)]

다음 확장 메서드를 추가 하는 경우 설정할 수 있으며 세션에 직렬화 가능 개체를 가져옵니다.

[!code-csharp[Main](app-state/sample/src/WebAppSession/Extensions/SessionExtensions.cs)]

다음 샘플에는 설정 하 고 직렬화 가능 개체를 가져오는 방법을 보여 줍니다.

[!code-csharp[Main](app-state/sample/src/WebAppSession/Controllers/HomeController.cs?name=snippet2)]


## <a name="working-with-httpcontextitems"></a>HttpContext.Items 작업

`HttpContext` 추상화 형식의 사전 컬렉션에 대 한 지원을 제공 `IDictionary<object, object>`라는 `Items`합니다. 이 컬렉션은의 시작 부분부터 사용할 수는 *HttpRequest* 은 각 요청의 끝에 삭제 됩니다. 키가 지정 된 항목에 값을 지정 하거나 특정 키에 대 한 값을 요청 하 여 액세스할 수 있습니다.

아래 샘플에서 [미들웨어](middleware.md) 추가 `isVerified` 에 `Items` 컬렉션입니다.

```csharp
app.Use(async (context, next) =>
{
    // perform some verification
    context.Items["isVerified"] = true;
    await next.Invoke();
});
```

파이프라인의 뒷부분에 나오는 다른 미들웨어를 액세스 하기:

```csharp
app.Run(async (context) =>
{
    await context.Response.WriteAsync("Verified request? " + 
        context.Items["isVerified"]);
});
```

단일 앱에서 사용할만 미들웨어에 대 한 `string` 키 허용 됩니다. 그러나 응용 프로그램 간에 공유 되는 미들웨어 키가 충돌 될 가능성을 방지 하기 위해 고유한 개체 키를 사용 해야 합니다. 여러 응용 프로그램에서 사용 해야 하는 미들웨어를 개발 하는 경우 아래와 같이 미들웨어 클래스에 정의 된 개체를 고유 키를 사용 합니다.

```csharp
public class SampleMiddleware
{
    public static readonly object SampleKey = new Object();

    public async Task Invoke(HttpContext httpContext)
    {
        httpContext.Items[SampleKey] = "some value";
        // additional code omitted
    }
}
```

다른 코드에 저장 된 값에 액세스할 수 `HttpContext.Items` 미들웨어 클래스에 의해 노출 되는 키를 사용 하 여:

```csharp
public class HomeController : Controller
{
    public IActionResult Index()
    {
        string value = HttpContext.Items[SampleMiddleware.SampleKey];
    }
}
```

이 방법은 또한 반복 되는 코드에서 여러 위치에서 "매직 문자열"를 제거 하 여 이점이 있습니다.

<a name="appstate-errors"></a>

## <a name="application-state-data"></a>응용 프로그램 상태 데이터

사용 하 여 [종속성 주입](xref:fundamentals/dependency-injection) 모든 사용자에 게 데이터를 사용할 수 있도록 하려면:

1. 데이터를 포함 하는 서비스 정의 (예를 들어 라는 클래스 `MyAppData`).

```csharp
public class MyAppData
{
    // Declare properties/methods/etc.
} 
```
2. 에 대 한 서비스 클래스를 추가 `ConfigureServices` (예를 들어 `services.AddSingleton<MyAppData>();`).
3. 각 컨트롤러에서 데이터 서비스 클래스를 사용 합니다.

```csharp
public class MyController : Controller
{
    public MyController(MyAppData myService)
    {
        // Do something with the service (read some data from it, 
        // store it in a private field/property, etc.)
    }
} 
```

## <a name="common-errors-when-working-with-session"></a>세션을 작업할 때 일반적인 오류

* "'Microsoft.AspNetCore.Session.DistributedSessionStore' 활성화 하는 동안 'Microsoft.Extensions.Caching.Distributed.IDistributedCache' 형식에 대 한 서비스를 확인할 수 없습니다."

  일반적으로는 하나 이상의 구성에 실패 `IDistributedCache` 구현 합니다. 자세한 내용은 참조 [분산 캐시 작업](xref:performance/caching/distributed) 및 [메모리 캐싱에](xref:performance/caching/memory)합니다.

* 유지 하는 미들웨어가 세션 세션 이벤트 (예: 데이터베이스를 사용할 수 없는 경우), 예외를 기록 하 고이 swallow 합니다. 요청 매우 예기치 않은 동작이에 일반적으로 계속 다음 됩니다.

일반적인 예:

사용자 세션에 장바구니를 저장합니다. 사용자 항목을 추가 하지만 커밋이 실패 합니다. 응용 프로그램 있는지 알 수 없으므로 실패에 대 한 "는 항목이 추가 되었습니다.", true 아닌 메시지를 보고 합니다.

이러한 오류를 확인 하는 권장된 방법은 호출 하는 것 `await feature.Session.CommitAsync();` 완료 되 면 응용 프로그램 코드에서 세션에 기록 합니다. 그런 다음 어떤 점이 좋은지 오류가 발생 하 여 수행할 수 있습니다. 호출할 때 동일한 방식으로 작동 `LoadAsync`합니다.


### <a name="additional-resources"></a>추가 리소스


* [ASP.NET Core 1.x:이 문서에 사용 되는 코드 샘플](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/app-state/sample/src/WebAppSession)
* [ASP.NET Core 2.x:이 문서에 사용 되는 코드 샘플](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/app-state/sample/src/WebAppSessionDotNetCore2.0App)
