---
title: 브라우저 앱에서 gRPC 사용
author: jamesnk
description: gRPC-Web을 사용하여 브라우저 앱에서 호출할 수 있도록 gRPC 서비스를 ASP.NET Core에서 구성하는 방법을 알아봅니다.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 05/26/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: grpc/browser
ms.openlocfilehash: 6f66a94b41e6e13550396e2e19fdf48f9dc63d46
ms.sourcegitcommit: 6a71b560d897e13ad5b61d07afe4fcb57f8ef6dc
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84106600"
---
# <a name="use-grpc-in-browser-apps"></a>브라우저 앱에서 gRPC 사용

작성자: [James Newton-King](https://twitter.com/jamesnk)

브라우저 기반 앱에서 HTTP/2 gRPC 서비스를 호출할 수는 없습니다. [gRPC-Web](https://github.com/grpc/grpc/blob/master/doc/PROTOCOL-WEB.md)은 브라우저 JavaScript 및 Blazor 앱에서 gRPC 서비스를 호출할 수 있게 해주는 프로토콜입니다. 이 문서에서는 .NET Core에서 gRPC-Web을 사용하는 방법을 설명합니다.

## <a name="grpc-web-in-aspnet-core-vs-envoy"></a>ASP.NET Core의 gRPC-Web 및 Envoy

ASP.NET Core 앱에 gRPC-Web을 추가하는 방법은 두 가지입니다.

* ASP.NET Core에서 gRPC HTTP/2와 함께 gRPC-Web을 지원합니다. 이 옵션은 `Grpc.AspNetCore.Web` 패키지에서 제공하는 미들웨어를 사용합니다.
* [Envoy 프록시의](https://www.envoyproxy.io/) gRPC-Web 지원을 사용하여 gRPC-Web을 gRPC HTTP/2로 변환합니다. 그러면 변환된 호출이 ASP.NET Core 앱으로 전달됩니다.

각 접근 방식에는 장/단점이 있습니다. 앱 환경에서 Envoy를 프록시로 이미 사용하고 있는 경우 이를 사용하여 gRPC-Web 지원을 제공하는 것이 적합할 수 있습니다. ASP.NET Core만 필요한 gRPC-Web을 위한 단순한 솔루션을 원하는 경우 `Grpc.AspNetCore.Web`을 선택하는 것이 좋습니다.

## <a name="configure-grpc-web-in-aspnet-core"></a>ASP.NET Core에서 gRPC-Web 구성

HTTP/2 gRPC와 함께 gRPC-Web을 지원하도록 ASP.NET Core에서 호스트된 gRPC 서비스를 구성할 수 있습니다. gRPC-Web을 사용하기 위해 서비스를 변경할 필요는 없습니다. 시작 구성만 수정하면 됩니다.

ASP.NET Core gRPC 서비스에서 gRPC-Web을 사용하도록 설정하려면 다음을 수행합니다.

* [Grpc.AspNetCore.Web](https://www.nuget.org/packages/Grpc.AspNetCore.Web) 패키지의 참조를 추가합니다.
* *Startup.cs*에 `UseGrpcWeb` 및 `EnableGrpcWeb`을 추가하여 gRPC-Web을 사용하도록 앱을 구성합니다.

[!code-csharp[](~/grpc/browser/sample/Startup.cs?name=snippet_1&highlight=10,14)]

위의 코드는

* 라우팅 이후, 엔드포인트 이전에 gRPC-Web 미들웨어인 `UseGrpcWeb`을 추가합니다.
* `EnableGrpcWeb`을 사용하여 `endpoints.MapGrpcService<GreeterService>()` 메서드에서 gRPC-Web을 지원하도록 지정합니다. 

또는 모든 서비스가 기본적으로 gRPC-Web을 지원하고 `EnableGrpcWeb`이 필요하지 않도록 gRPC-Web 미들웨어를 구성할 수 있습니다. 미들웨어를 추가할 때 `new GrpcWebOptions { DefaultEnabled = true }`를 지정합니다.

[!code-csharp[](~/grpc/browser/sample/AllServicesSupportExample_Startup.cs?name=snippet_1&highlight=12)]

> [!NOTE]
> .NET Core 3.x에서 [Http.sys로 호스트](xref:fundamentals/servers/httpsys)될 때 gRPC-Web이 실패하는 알려진 문제가 있습니다.
>
> gRPC-Web이 Http.sys에서 작동하도록 하는 해결 방법은 [여기](https://github.com/grpc/grpc-dotnet/issues/853#issuecomment-610078202)에서 확인할 수 있습니다.

### <a name="grpc-web-and-cors"></a>gRPC-Web 및 CORS

브라우저 보안 때문에 웹 페이지에서 해당 웹 페이지를 제공한 도메인이 아닌 다른 도메인에 요청을 수행할 수는 없습니다. 이 제한은 브라우저 앱에서 gRPC-Web 호출을 수행하는 데에도 적용됩니다. 예를 들어 `https://www.contoso.com`에서 제공하는 브라우저 앱은 `https://services.contoso.com`에서 호스트된 gRPC-Web 서비스를 호출할 수 없습니다. CORS(원본 간 리소스 공유)를 사용하여 이 제한을 완화할 수 있습니다.

브라우저 앱에서 원본 간 gRPC-Web 호출을 수행할 수 있게 하려면 [ASP.NET Core에서 CORS](xref:security/cors)를 설정합니다. 기본 제공 CORS 지원을 사용하고, <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>를 통해 gRPC 특정 헤더를 공개합니다.

[!code-csharp[](~/grpc/browser/sample/CORS_Startup.cs?name=snippet_1&highlight=5-11,19,24)]

위의 코드는

* `AddCors`를 호출하여 CORS 서비스를 추가하고 gRPC 특정 헤더를 공개하는 CORS 정책을 구성합니다.
* `UseCors`를 호출하여 라우팅 이후, 엔드포인트 이전에 CORS 미들웨어를 추가합니다.
* `RequiresCors`를 사용하여 `endpoints.MapGrpcService<GreeterService>()` 메서드에서 CORS를 지원하도록 지정합니다.

## <a name="call-grpc-web-from-the-browser"></a>브라우저에서 gRPC-Web 호출

브라우저 앱은 gRPC-Web을 사용하여 gRPC 서비스를 호출할 수 있습니다. 브라우저에서 gRPC-Web을 사용하여 gRPC 서비스를 호출하는 경우 다음과 같은 몇 가지 요구 사항과 제한 사항이 있습니다.

* 서버가 gRPC-Web을 지원하도록 구성되어 있어야 합니다.
* 클라이언트 스트리밍 및 양방향 스트리밍 호출이 지원되지 않습니다. 서버 스트리밍이 지원됩니다.
* 다른 도메인에 있는 gRPC 서비스를 호출하려면 서버에서 [CORS](xref:security/cors)를 구성해야 합니다.

### <a name="javascript-grpc-web-client"></a>JavaScript gRPC-Web 클라이언트

JavaScript gRPC-Web 클라이언트가 있습니다. JavaScript에서 gRPC-Web을 사용하는 방법에 대한 자세한 내용은 [gRPC-Web을 사용하여 JavaScript 클라이언트 코드 작성](https://github.com/grpc/grpc-web/tree/master/net/grpc/gateway/examples/helloworld#write-client-code)을 참조하세요.

### <a name="configure-grpc-web-with-the-net-grpc-client"></a>.NET gRPC 클라이언트를 사용하여 gRPC-Web 구성

gRPC-Web 호출을 수행하도록 .NET gRPC 클라이언트를 구성할 수 있습니다. 이 기능은 브라우저에서 호스트되고 JavaScript 코드와 동일한 HTTP 제한 사항이 있는 [Blazor WebAssembly](xref:blazor/index#blazor-webassembly) 앱에 유용합니다. .NET 클라이언트를 사용하여 gRPC-Web을 호출하는 것은 [HTTP/2 gRPC와 동일](xref:grpc/client)합니다. 채널을 만드는 방법만 수정하면 됩니다.

gRPC-Web을 사용하려면 다음을 수행합니다.

* [Grpc.Net.Client.Web](https://www.nuget.org/packages/Grpc.Net.Client.Web) 패키지의 참조를 추가합니다.
* [Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client) 패키지의 참조가 2.29.0 이상인지 확인합니다.
* `GrpcWebHandler`를 사용하도록 채널을 구성합니다.

[!code-csharp[](~/grpc/browser/sample/Handler.cs?name=snippet_1)]

위의 코드는

* gRPC-Web을 사용하도록 채널을 구성합니다.
* 클라이언트를 만들고 채널을 사용하여 호출합니다.

`GrpcWebHandler`에는 다음과 같은 구성 옵션이 있습니다.

* **InnerHandler**: gRPC HTTP 요청을 수행하는 기본 <xref:System.Net.Http.HttpMessageHandler>입니다(예: `HttpClientHandler`).
* **GrpcWebMode**: gRPC HTTP 요청 `Content-Type`이 `application/grpc-web` 또는 `application/grpc-web-text`인지를 지정하는 열거형 형식입니다.
    * `GrpcWebMode.GrpcWeb`은 인코딩 없이 전송되도록 콘텐츠를 구성합니다. 기본값.
    * `GrpcWebMode.GrpcWebText`는 base64로 인코딩되도록 콘텐츠를 구성합니다. 브라우저에서 서버 스트리밍 호출을 수행하는 데 필요합니다.
* **HttpVersion**: 기본 gRPC HTTP 요청에 [HttpRequestMessage.Version](xref:System.Net.Http.HttpRequestMessage.Version)을 설정하는 데 사용되는 HTTP 프로토콜 `Version`입니다. gRPC-Web은 특정 버전이 필요하지 않으며, 지정하지 않을 경우 기본값을 재정의하지 않습니다.

> [!IMPORTANT]
> 생성된 gRPC 클라이언트에는 단항 메서드를 호출하기 위한 동기 및 비동기 메서드가 있습니다. 예를 들어 `SayHello`는 동기이고, `SayHelloAsync`는 비동기입니다. Blazor WebAssembly 앱에서 동기 메서드를 호출하면 앱이 응답하지 않게 됩니다. 비동기 메서드는 항상 Blazor WebAssembly에서 사용해야 합니다.

## <a name="additional-resources"></a>추가 자료

* [웹 클라이언트용 gRPC GitHub 프로젝트](https://github.com/grpc/grpc-web)
* <xref:security/cors>
