---
title: ASP.NET Core에서 HTTP.sys 웹 서버 구현
author: guardrex
description: Windows의 ASP.NET Core에 대한 웹 서버인 HTTP.sys에 대해 알아봅니다. HTTP.sys 커널 모드 드라이버를 기반으로 한 HTTP.sys는 IIS 없이 인터넷에 대한 직접 연결에 사용될 수 있는 Kestrel에 대한 대안입니다.
monikerRange: '>= aspnetcore-2.0'
ms.author: tdykstra
ms.custom: mvc
ms.date: 01/03/2019
uid: fundamentals/servers/httpsys
ms.openlocfilehash: 46538d256ae2c5f3b7e6c725fa8f29092759f69f
ms.sourcegitcommit: 97d7a00bd39c83a8f6bccb9daa44130a509f75ce
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/08/2019
ms.locfileid: "54098856"
---
# <a name="httpsys-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="347d3-104">ASP.NET Core에서 HTTP.sys 웹 서버 구현</span><span class="sxs-lookup"><span data-stu-id="347d3-104">HTTP.sys web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="347d3-105">작성자: [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher), [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="347d3-105">By [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="347d3-106">[HTTP.sys](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture#hypertext-transfer-protocol-stack-httpsys)는 Windows에서만 실행되는 [ASP.NET Core에 대한 웹 서버](xref:fundamentals/servers/index)입니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-106">[HTTP.sys](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture#hypertext-transfer-protocol-stack-httpsys) is a [web server for ASP.NET Core](xref:fundamentals/servers/index) that only runs on Windows.</span></span> <span data-ttu-id="347d3-107">HTTP.sys는 [Kestrel](xref:fundamentals/servers/kestrel) 서버에 대한 대안이며 Kestel이 제공하지 않는 일부 기능을 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-107">HTTP.sys is an alternative to [Kestrel](xref:fundamentals/servers/kestrel) server and offers some features that Kestrel doesn't provide.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="347d3-108">HTTP.sys는 [ASP.NET Core 모듈](xref:host-and-deploy/aspnet-core-module)과 호환되지 않으므로 IIS 또는 IIS Express와 함께 사용될 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-108">HTTP.sys isn't compatible with the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) and can't be used with IIS or IIS Express.</span></span>

<span data-ttu-id="347d3-109">HTTP.sys는 다음과 같은 기능을 지원합니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-109">HTTP.sys supports the following features:</span></span>

* [<span data-ttu-id="347d3-110">Windows 인증</span><span class="sxs-lookup"><span data-stu-id="347d3-110">Windows Authentication</span></span>](xref:security/authentication/windowsauth)
* <span data-ttu-id="347d3-111">포트 공유</span><span class="sxs-lookup"><span data-stu-id="347d3-111">Port sharing</span></span>
* <span data-ttu-id="347d3-112">SNI를 사용하는 HTTPS</span><span class="sxs-lookup"><span data-stu-id="347d3-112">HTTPS with SNI</span></span>
* <span data-ttu-id="347d3-113">TLS를 통한 HTTP/2(Windows 10 이상)</span><span class="sxs-lookup"><span data-stu-id="347d3-113">HTTP/2 over TLS (Windows 10 or later)</span></span>
* <span data-ttu-id="347d3-114">직접 파일 전송</span><span class="sxs-lookup"><span data-stu-id="347d3-114">Direct file transmission</span></span>
* <span data-ttu-id="347d3-115">응답 캐싱</span><span class="sxs-lookup"><span data-stu-id="347d3-115">Response caching</span></span>
* <span data-ttu-id="347d3-116">WebSockets(Windows 8 이상)</span><span class="sxs-lookup"><span data-stu-id="347d3-116">WebSockets (Windows 8 or later)</span></span>

<span data-ttu-id="347d3-117">지원되는 Windows 버전:</span><span class="sxs-lookup"><span data-stu-id="347d3-117">Supported Windows versions:</span></span>

* <span data-ttu-id="347d3-118">Windows 7 이상</span><span class="sxs-lookup"><span data-stu-id="347d3-118">Windows 7 or later</span></span>
* <span data-ttu-id="347d3-119">Windows Server 2008 R2 이상</span><span class="sxs-lookup"><span data-stu-id="347d3-119">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="347d3-120">[예제 코드 살펴보기 및 다운로드](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample) ([다운로드 방법](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="347d3-120">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-httpsys"></a><span data-ttu-id="347d3-121">HTTP.sys를 사용하는 경우</span><span class="sxs-lookup"><span data-stu-id="347d3-121">When to use HTTP.sys</span></span>

<span data-ttu-id="347d3-122">HTTP.sys는 다음과 같은 배포에 유용합니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-122">HTTP.sys is useful for deployments where:</span></span>

* <span data-ttu-id="347d3-123">IIS를 사용하지 않고 인터넷에 서버를 직접 노출해야 하는 경우</span><span class="sxs-lookup"><span data-stu-id="347d3-123">There's a need to expose the server directly to the Internet without using IIS.</span></span>

  ![HTTP.sys는 인터넷과 직접 통신합니다.](httpsys/_static/httpsys-to-internet.png)

* <span data-ttu-id="347d3-125">내부 배포에는 [Windows 인증](xref:security/authentication/windowsauth)처럼 Kestrel에서 사용할 수 없는 기능이 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-125">An internal deployment requires a feature not available in Kestrel, such as [Windows Authentication](xref:security/authentication/windowsauth).</span></span>

  ![HTTP.sys는 내부 네트워크와 직접 통신합니다.](httpsys/_static/httpsys-to-internal.png)

<span data-ttu-id="347d3-127">HTTP.sys는 많은 유형의 공격으로부터 보호하고 모든 기능을 갖춘 웹 서버의 견고성, 보안 및 확장성을 제공하는 완성도 높은 기술입니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-127">HTTP.sys is mature technology that protects against many types of attacks and provides the robustness, security, and scalability of a full-featured web server.</span></span> <span data-ttu-id="347d3-128">IIS 자체는 HTTP.sys 위에 HTTP 수신기로 실행됩니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-128">IIS itself runs as an HTTP listener on top of HTTP.sys.</span></span>

## <a name="http2-support"></a><span data-ttu-id="347d3-129">HTTP/2 지원</span><span class="sxs-lookup"><span data-stu-id="347d3-129">HTTP/2 support</span></span>

<span data-ttu-id="347d3-130">다음 기본 요구 사항이 충족되는 경우 ASP.NET Core 앱에 대해[HTTP/2](https://httpwg.org/specs/rfc7540.html)가 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-130">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is enabled for ASP.NET Core apps if the following base requirements are met:</span></span>

* <span data-ttu-id="347d3-131">Windows Server 2016/Windows 10 이상</span><span class="sxs-lookup"><span data-stu-id="347d3-131">Windows Server 2016/Windows 10 or later</span></span>
* <span data-ttu-id="347d3-132">[ALPN(Application-Layer Protocol Negotiation)](https://tools.ietf.org/html/rfc7301#section-3) 연결</span><span class="sxs-lookup"><span data-stu-id="347d3-132">[Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) connection</span></span>
* <span data-ttu-id="347d3-133">TLS 1.2 이상 연결</span><span class="sxs-lookup"><span data-stu-id="347d3-133">TLS 1.2 or later connection</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="347d3-134">HTTP/2 연결이 설정된 경우 [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*)에서 `HTTP/2`를 보고합니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-134">If an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/2`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="347d3-135">HTTP/2 연결이 설정된 경우 [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*)에서 `HTTP/1.1`을 보고합니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-135">If an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/1.1`.</span></span>

::: moniker-end

<span data-ttu-id="347d3-136">HTTP/2는 기본적으로 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-136">HTTP/2 is enabled by default.</span></span> <span data-ttu-id="347d3-137">HTTP/2 연결이 설정되지 않는 경우 연결이 HTTP/1.1로 대체됩니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-137">If an HTTP/2 connection isn't established, the connection falls back to HTTP/1.1.</span></span> <span data-ttu-id="347d3-138">Windows의 이후 릴리스에서는 HTTP.sys를 사용하여 HTTP/2를 사용하지 않도록 하는 기능을 포함하여 HTTP/2 구성 플래그를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-138">In a future release of Windows, HTTP/2 configuration flags will be available, including the ability to disable HTTP/2 with HTTP.sys.</span></span>

## <a name="kernel-mode-authentication-with-kerberos"></a><span data-ttu-id="347d3-139">Kerberos를 사용하여 커널 모드 인증</span><span class="sxs-lookup"><span data-stu-id="347d3-139">Kernel mode authentication with Kerberos</span></span>

<span data-ttu-id="347d3-140">HTTP.sys는 Kerberos 인증 프로토콜을 사용하여 커널 모드 인증에 위임합니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-140">HTTP.sys delegates to kernel mode authentication with the Kerberos authentication protocol.</span></span> <span data-ttu-id="347d3-141">사용자 모드 인증은 Kerberos 및 HTTP.sys로 지원되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-141">User mode authentication isn't supported with Kerberos and HTTP.sys.</span></span> <span data-ttu-id="347d3-142">머신 계정은 Active Directory에서 가져온 Kerberos 토큰/티켓의 암호를 해독하는 데 사용되고 사용자를 인증하는 서버에 클라이언트에 의해 전달되어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-142">The machine account must be used to decrypt the Kerberos token/ticket that's obtained from Active Directory and forwarded by the client to the server to authenticate the user.</span></span> <span data-ttu-id="347d3-143">앱의 사용자가 아닌 호스트에 대해 SPN(서비스 사용자 이름)을 등록합니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-143">Register the Service Principal Name (SPN) for the host, not the user of the app.</span></span>

## <a name="how-to-use-httpsys"></a><span data-ttu-id="347d3-144">HTTP.sys 사용 방법</span><span class="sxs-lookup"><span data-stu-id="347d3-144">How to use HTTP.sys</span></span>

### <a name="configure-the-aspnet-core-app-to-use-httpsys"></a><span data-ttu-id="347d3-145">HTTP.sys를 사용하도록 ASP.NET Core 앱 구성</span><span class="sxs-lookup"><span data-stu-id="347d3-145">Configure the ASP.NET Core app to use HTTP.sys</span></span>

1. <span data-ttu-id="347d3-146">[Microsoft.AspNetCore.App 메타패키지](xref:fundamentals/metapackage-app)([nuget.org](https://www.nuget.org/packages/Microsoft.AspNetCore.App/))(ASP.NET Core 2.1이상)를 사용할 경우 프로젝트 파일의 패키지 참조가 필요하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-146">A package reference in the project file isn't required when using the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) ([nuget.org](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)) (ASP.NET Core 2.1 or later).</span></span> <span data-ttu-id="347d3-147">`Microsoft.AspNetCore.App` 메타패키지를 사용하지 않는 경우 [Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/)에 패키지 참조를 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-147">When not using the `Microsoft.AspNetCore.App` metapackage, add a package reference to [Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/).</span></span>

2. <span data-ttu-id="347d3-148">웹 호스트를 빌드할 때 [UseHttpSys](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderhttpsysextensions.usehttpsys) 확장 메서드를 호출하여 필요한 [HTTP.sys 옵션](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions)을 지정합니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-148">Call the [UseHttpSys](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderhttpsysextensions.usehttpsys) extension method when building the web host, specifying any required [HTTP.sys options](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions):</span></span>

   [!code-csharp[](httpsys/sample/Program.cs?name=snippet1&highlight=4-12)]

   <span data-ttu-id="347d3-149">추가 HTTP.sys 구성은 [레지스트리 설정](https://support.microsoft.com/help/820129/http-sys-registry-settings-for-windows)을 통해 처리됩니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-149">Additional HTTP.sys configuration is handled through [registry settings](https://support.microsoft.com/help/820129/http-sys-registry-settings-for-windows).</span></span>

   <span data-ttu-id="347d3-150">**HTTP.sys 옵션**</span><span class="sxs-lookup"><span data-stu-id="347d3-150">**HTTP.sys options**</span></span>

   | <span data-ttu-id="347d3-151">속성</span><span class="sxs-lookup"><span data-stu-id="347d3-151">Property</span></span> | <span data-ttu-id="347d3-152">설명</span><span class="sxs-lookup"><span data-stu-id="347d3-152">Description</span></span> | <span data-ttu-id="347d3-153">기본</span><span class="sxs-lookup"><span data-stu-id="347d3-153">Default</span></span> |
   | -------- | ----------- | :-----: |
   | [<span data-ttu-id="347d3-154">AllowSynchronousIO</span><span class="sxs-lookup"><span data-stu-id="347d3-154">AllowSynchronousIO</span></span>](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.allowsynchronousio) | <span data-ttu-id="347d3-155">`HttpContext.Request.Body` 및 `HttpContext.Response.Body`에 대해 동기 입력/출력이 허용되는지 여부를 제어합니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-155">Control whether synchronous input/output is allowed for the `HttpContext.Request.Body` and `HttpContext.Response.Body`.</span></span> | `true` |
   | [<span data-ttu-id="347d3-156">Authentication.AllowAnonymous</span><span class="sxs-lookup"><span data-stu-id="347d3-156">Authentication.AllowAnonymous</span></span>](/dotnet/api/microsoft.aspnetcore.server.httpsys.authenticationmanager.allowanonymous) | <span data-ttu-id="347d3-157">익명 요청을 허용합니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-157">Allow anonymous requests.</span></span> | `true` |
   | [<span data-ttu-id="347d3-158">Authentication.Schemes</span><span class="sxs-lookup"><span data-stu-id="347d3-158">Authentication.Schemes</span></span>](/dotnet/api/microsoft.aspnetcore.server.httpsys.authenticationmanager.schemes) | <span data-ttu-id="347d3-159">허용되는 인증 체계를 지정합니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-159">Specify the allowed authentication schemes.</span></span> <span data-ttu-id="347d3-160">수신기를 삭제하기 전에 언제든지 수정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-160">May be modified at any time prior to disposing the listener.</span></span> <span data-ttu-id="347d3-161">값은 [AuthenticationSchemes 열거형](/dotnet/api/microsoft.aspnetcore.server.httpsys.authenticationschemes): `Basic`, `Kerberos`, `Negotiate`, `None`, `NTLM`에서 제공됩니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-161">Values are provided by the [AuthenticationSchemes enum](/dotnet/api/microsoft.aspnetcore.server.httpsys.authenticationschemes): `Basic`, `Kerberos`, `Negotiate`, `None`, and `NTLM`.</span></span> | `None` |
   | [<span data-ttu-id="347d3-162">EnableResponseCaching</span><span class="sxs-lookup"><span data-stu-id="347d3-162">EnableResponseCaching</span></span>](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.enableresponsecaching) | <span data-ttu-id="347d3-163">적합한 헤더가 있는 응답에 대해 [커널 모드](/windows-hardware/drivers/gettingstarted/user-mode-and-kernel-mode) 캐싱을 시도합니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-163">Attempt [kernel-mode](/windows-hardware/drivers/gettingstarted/user-mode-and-kernel-mode) caching for responses with eligible headers.</span></span> <span data-ttu-id="347d3-164">응답에 `Set-Cookie`, `Vary` 또는 `Pragma` 헤더가 포함될 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-164">The response may not include `Set-Cookie`, `Vary`, or `Pragma` headers.</span></span> <span data-ttu-id="347d3-165">`public` 및 `shared-max-age` 또는 `max-age` 값인 `Cache-Control` 헤더나 `Expires` 헤더가 포함되어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-165">It must include a `Cache-Control` header that's `public` and either a `shared-max-age` or `max-age` value, or an `Expires` header.</span></span> | `true` |
   | [<span data-ttu-id="347d3-166">MaxAccepts</span><span class="sxs-lookup"><span data-stu-id="347d3-166">MaxAccepts</span></span>](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.maxaccepts) | <span data-ttu-id="347d3-167">최대 동시 승인 수입니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-167">The maximum number of concurrent accepts.</span></span> | <span data-ttu-id="347d3-168">5 &times; [Environment.<br>ProcessorCount](/dotnet/api/system.environment.processorcount)</span><span class="sxs-lookup"><span data-stu-id="347d3-168">5 &times; [Environment.<br>ProcessorCount](/dotnet/api/system.environment.processorcount)</span></span> |
   | [<span data-ttu-id="347d3-169">MaxConnections</span><span class="sxs-lookup"><span data-stu-id="347d3-169">MaxConnections</span></span>](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.maxconnections) | <span data-ttu-id="347d3-170">허용되는 최대 동시 연결 수입니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-170">The maximum number of concurrent connections to accept.</span></span> <span data-ttu-id="347d3-171">무한의 경우 `-1`을 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-171">Use `-1` for infinite.</span></span> <span data-ttu-id="347d3-172">레지스트리의 시스템 수준 설정을 사용하려면 `null`을 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-172">Use `null` to use the registry's machine-wide setting.</span></span> | `null`<br><span data-ttu-id="347d3-173">(제한 없음)</span><span class="sxs-lookup"><span data-stu-id="347d3-173">(unlimited)</span></span> |
   | [<span data-ttu-id="347d3-174">MaxRequestBodySize</span><span class="sxs-lookup"><span data-stu-id="347d3-174">MaxRequestBodySize</span></span>](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.maxrequestbodysize) | <span data-ttu-id="347d3-175"><a href="#maxrequestbodysize">MaxRequestBodySize</a> 섹션을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="347d3-175">See the <a href="#maxrequestbodysize">MaxRequestBodySize</a> section.</span></span> | <span data-ttu-id="347d3-176">30000000바이트</span><span class="sxs-lookup"><span data-stu-id="347d3-176">30000000 bytes</span></span><br><span data-ttu-id="347d3-177">(~28.6MB)</span><span class="sxs-lookup"><span data-stu-id="347d3-177">(~28.6 MB)</span></span> |
   | [<span data-ttu-id="347d3-178">RequestQueueLimit</span><span class="sxs-lookup"><span data-stu-id="347d3-178">RequestQueueLimit</span></span>](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.requestqueuelimit) | <span data-ttu-id="347d3-179">큐에 대기할 수 있는 최대 요청 수입니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-179">The maximum number of requests that can be queued.</span></span> | <span data-ttu-id="347d3-180">1000</span><span class="sxs-lookup"><span data-stu-id="347d3-180">1000</span></span> |
   | [<span data-ttu-id="347d3-181">ThrowWriteExceptions</span><span class="sxs-lookup"><span data-stu-id="347d3-181">ThrowWriteExceptions</span></span>](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.throwwriteexceptions) | <span data-ttu-id="347d3-182">클라이언트 연결 해제로 인해 실패한 응답 본문 쓰기가 예외를 throw하거나 정상적으로 완료되어야 하는지 여부를 나타납니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-182">Indicate if response body writes that fail due to client disconnects should throw exceptions or complete normally.</span></span> | `false`<br><span data-ttu-id="347d3-183">(정상적으로 완료)</span><span class="sxs-lookup"><span data-stu-id="347d3-183">(complete normally)</span></span> |
   | [<span data-ttu-id="347d3-184">Timeouts</span><span class="sxs-lookup"><span data-stu-id="347d3-184">Timeouts</span></span>](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.timeouts) | <span data-ttu-id="347d3-185">레지스트리에 구성될 수도 있는 HTTP.sys [TimeoutManager](/dotnet/api/microsoft.aspnetcore.server.httpsys.timeoutmanager) 구성을 표시합니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-185">Expose the HTTP.sys [TimeoutManager](/dotnet/api/microsoft.aspnetcore.server.httpsys.timeoutmanager) configuration, which may also be configured in the registry.</span></span> <span data-ttu-id="347d3-186">API 링크를 따라 기본값을 포함하여 각 설정에 대해 자세히 알아보세요.</span><span class="sxs-lookup"><span data-stu-id="347d3-186">Follow the API links to learn more about each setting, including default values:</span></span><ul><li><span data-ttu-id="347d3-187">[TimeoutManager.DrainEntityBody](/dotnet/api/microsoft.aspnetcore.server.httpsys.timeoutmanager.drainentitybody) &ndash; HTTP 서버 API가 지속적 연결에서 엔터티 본문을 비우는 데 허용되는 시간입니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-187">[TimeoutManager.DrainEntityBody](/dotnet/api/microsoft.aspnetcore.server.httpsys.timeoutmanager.drainentitybody) &ndash; Time allowed for the HTTP Server API to drain the entity body on a Keep-Alive connection.</span></span></li><li><span data-ttu-id="347d3-188">[TimeoutManager.EntityBody](/dotnet/api/microsoft.aspnetcore.server.httpsys.timeoutmanager.entitybody) &ndash; 요청 엔터티가 도착하는 데 허용되는 시간입니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-188">[TimeoutManager.EntityBody](/dotnet/api/microsoft.aspnetcore.server.httpsys.timeoutmanager.entitybody) &ndash; Time allowed for the request entity body to arrive.</span></span></li><li><span data-ttu-id="347d3-189">[TimeoutManager.HeaderWait](/dotnet/api/microsoft.aspnetcore.server.httpsys.timeoutmanager.headerwait) &ndash; HTTP 서버 API가 요청 헤더를 구문 분석하는 데 허용되는 시간입니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-189">[TimeoutManager.HeaderWait](/dotnet/api/microsoft.aspnetcore.server.httpsys.timeoutmanager.headerwait) &ndash; Time allowed for the HTTP Server API to parse the request header.</span></span></li><li><span data-ttu-id="347d3-190">[TimeoutManager.IdleConnection](/dotnet/api/microsoft.aspnetcore.server.httpsys.timeoutmanager.idleconnection) &ndash; 유휴 연결에 허용되는 시간입니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-190">[TimeoutManager.IdleConnection](/dotnet/api/microsoft.aspnetcore.server.httpsys.timeoutmanager.idleconnection) &ndash; Time allowed for an idle connection.</span></span></li><li><span data-ttu-id="347d3-191">[TimeoutManager.MinSendBytesPerSecond](/dotnet/api/microsoft.aspnetcore.server.httpsys.timeoutmanager.minsendbytespersecond) &ndash; 응답에 대한 최소 전송 속도입니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-191">[TimeoutManager.MinSendBytesPerSecond](/dotnet/api/microsoft.aspnetcore.server.httpsys.timeoutmanager.minsendbytespersecond) &ndash; The minimum send rate for the response.</span></span></li><li><span data-ttu-id="347d3-192">[TimeoutManager.RequestQueue](/dotnet/api/microsoft.aspnetcore.server.httpsys.timeoutmanager.requestqueue) &ndash; 앱이 선택하기 전에 요청이 요청 큐에 남아 있는 데 허용되는 시간입니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-192">[TimeoutManager.RequestQueue](/dotnet/api/microsoft.aspnetcore.server.httpsys.timeoutmanager.requestqueue) &ndash; Time allowed for the request to remain in the request queue before the app picks it up.</span></span></li></ul> |  |
   | [<span data-ttu-id="347d3-193">UrlPrefixes</span><span class="sxs-lookup"><span data-stu-id="347d3-193">UrlPrefixes</span></span>](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.urlprefixes) | <span data-ttu-id="347d3-194">[UrlPrefixCollection](/dotnet/api/microsoft.aspnetcore.server.httpsys.urlprefixcollection)을 지정하여 HTTP.sys를 등록합니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-194">Specify the [UrlPrefixCollection](/dotnet/api/microsoft.aspnetcore.server.httpsys.urlprefixcollection) to register with HTTP.sys.</span></span> <span data-ttu-id="347d3-195">컬렉션에 접두사를 추가하는 데 사용되는 [UrlPrefixCollection.Add](/dotnet/api/microsoft.aspnetcore.server.httpsys.urlprefixcollection.add)가 가장 유용합니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-195">The most useful is [UrlPrefixCollection.Add](/dotnet/api/microsoft.aspnetcore.server.httpsys.urlprefixcollection.add), which is used to add a prefix to the collection.</span></span> <span data-ttu-id="347d3-196">이러한 API는 수신기를 삭제하기 전에 언제든지 수정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-196">These may be modified at any time prior to disposing the listener.</span></span> |  |

   <a name="maxrequestbodysize"></a>
   <span data-ttu-id="347d3-197">**MaxRequestBodySize**</span><span class="sxs-lookup"><span data-stu-id="347d3-197">**MaxRequestBodySize**</span></span>

   <span data-ttu-id="347d3-198">요청 본문에 대해 허용되는 최대 크기(바이트)입니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-198">The maximum allowed size of any request body in bytes.</span></span> <span data-ttu-id="347d3-199">`null`로 설정하면 최대 요청 본문 크기는 무제한입니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-199">When set to `null`, the maximum request body size is unlimited.</span></span> <span data-ttu-id="347d3-200">항상 무제한인 업그레이드된 연결에는 이 제한이 영향을 미치지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-200">This limit has no effect on upgraded connections, which are always unlimited.</span></span>

   <span data-ttu-id="347d3-201">단일 `IActionResult`에 대해 ASP.NET Core MVC 앱에서 제한을 재정의할 때는 작업 메서드에서 [RequestSizeLimitAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requestsizelimitattribute) 특성을 사용하는 방법이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-201">The recommended method to override the limit in an ASP.NET Core MVC app for a single `IActionResult` is to use the [RequestSizeLimitAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requestsizelimitattribute) attribute on an action method:</span></span>

   ```csharp
   [RequestSizeLimit(100000000)]
   public IActionResult MyActionMethod()
   ```

   <span data-ttu-id="347d3-202">앱에서 요청을 읽기 시작한 후 요청에 대한 제한을 구성하려고 하면 예외가 throw됩니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-202">An exception is thrown if the app attempts to configure the limit on a request after the app has started reading the request.</span></span> <span data-ttu-id="347d3-203">`IsReadOnly` 속성을 사용하여 `MaxRequestBodySize` 속성이 제한을 구성하기에 너무 늦은, 읽기 전용 상태인지를 나타낼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-203">An `IsReadOnly` property can be used to indicate if the `MaxRequestBodySize` property is in a read-only state, meaning it's too late to configure the limit.</span></span>

   <span data-ttu-id="347d3-204">앱에서 요청별로 [MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.maxrequestbodysize)를 재정의해야 하는 경우 [IHttpMaxRequestBodySizeFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttpmaxrequestbodysizefeature)를 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-204">If the app should override [MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.maxrequestbodysize) per-request, use the [IHttpMaxRequestBodySizeFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttpmaxrequestbodysizefeature):</span></span>

   [!code-csharp[](httpsys/sample/Startup.cs?name=snippet1&highlight=6-7)]

3. <span data-ttu-id="347d3-205">Visual Studio를 사용하는 경우 앱이 IIS 또는 IIS Express를 실행하도록 구성되지 않았는지 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-205">If using Visual Studio, make sure the app isn't configured to run IIS or IIS Express.</span></span>

   <span data-ttu-id="347d3-206">Visual Studio에서 기본 실행 프로필은 IIS Express용입니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-206">In Visual Studio, the default launch profile is for IIS Express.</span></span> <span data-ttu-id="347d3-207">프로젝트를 콘솔 앱으로 실행하려면 다음 스크린샷에 표시된 것처럼 선택한 프로필을 수동으로 변경합니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-207">To run the project as a console app, manually change the selected profile, as shown in the following screen shot:</span></span>

   ![콘솔 앱 프로필 선택](httpsys/_static/vs-choose-profile.png)

### <a name="configure-windows-server"></a><span data-ttu-id="347d3-209">Windows Server 구성</span><span class="sxs-lookup"><span data-stu-id="347d3-209">Configure Windows Server</span></span>

1. <span data-ttu-id="347d3-210">앱용으로 열 포트를 결정하고 Windows 방화벽 또는 [PowerShell cmdlet](https://technet.microsoft.com/library/jj554906)을 통해 방화벽 포트를 열어 HTTP.sys에 도달하는 트래픽을 허용합니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-210">Determine the ports to open for the app and use Windows Firewall or [PowerShell cmdlets](https://technet.microsoft.com/library/jj554906) to open firewall ports to allow traffic to reach HTTP.sys.</span></span> <span data-ttu-id="347d3-211">Azure VM에 배포할 경우 [네트워크 보안 그룹](/azure/virtual-network/security-overview)에서 포트를 엽니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-211">When deploying to an Azure VM, open the ports in the [Network Security Group](/azure/virtual-network/security-overview).</span></span> <span data-ttu-id="347d3-212">다음 명령 및 앱 구성에서는 포트 443이 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-212">In the following commands and app configuration, port 443 is used.</span></span>

1. <span data-ttu-id="347d3-213">필요한 경우 X.509 인증서를 구하여 설치합니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-213">Obtain and install X.509 certificates, if required.</span></span>

   <span data-ttu-id="347d3-214">Windows에서 [New-SelfSignedCertificate PowerShell cmdlet](/powershell/module/pkiclient/new-selfsignedcertificate)을 사용하여 자체 서명된 인증서를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-214">On Windows, create self-signed certificates using the [New-SelfSignedCertificate PowerShell cmdlet](/powershell/module/pkiclient/new-selfsignedcertificate).</span></span> <span data-ttu-id="347d3-215">지원되지 않는 예는 [UpdateIISExpressSSLForChrome.ps1](https://github.com/aspnet/Docs/tree/master/aspnetcore/includes/make-x509-cert/UpdateIISExpressSSLForChrome.ps1)을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="347d3-215">For an unsupported example, see [UpdateIISExpressSSLForChrome.ps1](https://github.com/aspnet/Docs/tree/master/aspnetcore/includes/make-x509-cert/UpdateIISExpressSSLForChrome.ps1).</span></span>

   <span data-ttu-id="347d3-216">서버의 **로컬 머신** > **개인** 저장소에 자체 서명 또는 CA 서명 인증서를 설치합니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-216">Install either self-signed or CA-signed certificates in the server's **Local Machine** > **Personal** store.</span></span>

1. <span data-ttu-id="347d3-217">앱이 [프레임워크 종속 배포](/dotnet/core/deploying/#framework-dependent-deployments-fdd)인 경우 .NET Core, .NET Framework 또는 둘 다(앱이 NET Framework를 대상으로 하는 .NET Core 앱인 경우)를 설치합니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-217">If the app is a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd), install .NET Core, .NET Framework, or both (if the app is a .NET Core app targeting the .NET Framework).</span></span>

   * <span data-ttu-id="347d3-218">**.NET Core** &ndash; 앱에 .NET Core가 필요한 경우 [.NET Core 다운로드](https://dotnet.microsoft.com/download)에서 **.NET Core 런타임** 설치 관리자를 가져와 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-218">**.NET Core** &ndash; If the app requires .NET Core, obtain and run the **.NET Core Runtime** installer from [.NET Core Downloads](https://dotnet.microsoft.com/download).</span></span> <span data-ttu-id="347d3-219">서버에 전체 SDK를 설치하지 마세요.</span><span class="sxs-lookup"><span data-stu-id="347d3-219">Don't install the full SDK on the server.</span></span>
   * <span data-ttu-id="347d3-220">**.NET Framework** &ndash; 앱에 .NET Framework가 필요한 경우 [.NET Framework: 설치 가이드](/dotnet/framework/install/)를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="347d3-220">**.NET Framework** &ndash; If the app requires .NET Framework, see the [.NET Framework installation guide](/dotnet/framework/install/).</span></span> <span data-ttu-id="347d3-221">필수 .NET Framework를 설치합니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-221">Install the required .NET Framework.</span></span> <span data-ttu-id="347d3-222">최신 .NET Framework의 설치 관리자는 [.NET Core 다운로드](https://dotnet.microsoft.com/download) 페이지에서 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-222">The installer for the latest .NET Framework is available from from the [.NET Core Downloads](https://dotnet.microsoft.com/download) page.</span></span>

   <span data-ttu-id="347d3-223">앱이 [자체 포함 배포](/dotnet/core/deploying/#framework-dependent-deployments-scd)인 경우 앱의 배포에 런타임이 포함됩니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-223">If the app is a [self-contained deployment](/dotnet/core/deploying/#framework-dependent-deployments-scd), the app includes the runtime in its deployment.</span></span> <span data-ttu-id="347d3-224">서버에 프레임워크를 설치할 필요가 없습니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-224">No framework installation is required on the server.</span></span>

1. <span data-ttu-id="347d3-225">앱에서 URL 및 포트를 구성합니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-225">Configure URLs and ports in the app.</span></span>

   <span data-ttu-id="347d3-226">기본적으로 ASP.NET Core는 `http://localhost:5000`으로 바인딩합니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-226">By default, ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="347d3-227">URL 접두사 및 포트를 구성하려면 다음 옵션을 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-227">To configure URL prefixes and ports, options include:</span></span>

   * [<span data-ttu-id="347d3-228">UseUrls</span><span class="sxs-lookup"><span data-stu-id="347d3-228">UseUrls</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useurls)
   * <span data-ttu-id="347d3-229">`urls` 명령줄 인수</span><span class="sxs-lookup"><span data-stu-id="347d3-229">`urls` command-line argument</span></span>
   * <span data-ttu-id="347d3-230">`ASPNETCORE_URLS`환경 변수</span><span class="sxs-lookup"><span data-stu-id="347d3-230">`ASPNETCORE_URLS` environment variable</span></span>
   * [<span data-ttu-id="347d3-231">UrlPrefixes</span><span class="sxs-lookup"><span data-stu-id="347d3-231">UrlPrefixes</span></span>](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.urlprefixes)

   <span data-ttu-id="347d3-232">다음 코드 예제는 포트 443에서 서버의 로컬 IP 주소 `10.0.0.4`와 함께 [UrlPrefixes](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.urlprefixes)를 사용하는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-232">The following code example shows how to use [UrlPrefixes](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.urlprefixes) with the server's local IP address `10.0.0.4` on port 443:</span></span>

   [!code-csharp[](httpsys/sample_snapshot/Program.cs?name=snippet1&highlight=11)]

   <span data-ttu-id="347d3-233">`UrlPrefixes`의 장점은 형식이 잘못된 접두사에 대해 오류 메시지가 즉시 생성된다는 점입니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-233">An advantage of `UrlPrefixes` is that an error message is generated immediately for improperly formatted prefixes.</span></span>

   <span data-ttu-id="347d3-234">`UrlPrefixes`의 설정은 `UseUrls`/`urls`/`ASPNETCORE_URLS` 설정을 재정의합니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-234">The settings in `UrlPrefixes` override `UseUrls`/`urls`/`ASPNETCORE_URLS` settings.</span></span> <span data-ttu-id="347d3-235">따라서 `UseUrls`, `urls` 및 `ASPNETCORE_URLS` 환경 변수의 장점은 Kestrel과 HTTP.sys 간을 쉽게 전환할 수 있다는 점입니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-235">Therefore, an advantage of `UseUrls`, `urls`, and the `ASPNETCORE_URLS` environment variable is that it's easier to switch between Kestrel and HTTP.sys.</span></span> <span data-ttu-id="347d3-236">자세한 내용은 <xref:fundamentals/host/web-host>을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="347d3-236">For more information, see <xref:fundamentals/host/web-host>.</span></span>

   <span data-ttu-id="347d3-237">HTTP.sys는 [HTTP Server API UrlPrefix 문자열 형식](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)을 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-237">HTTP.sys uses the [HTTP Server API UrlPrefix string formats](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span></span>

   > [!WARNING]
   > <span data-ttu-id="347d3-238">최상위 와일드카드 바인딩(`http://*:80/` 및 `http://+:80`)을 사용하지 **않아야** 합니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-238">Top-level wildcard bindings (`http://*:80/` and `http://+:80`) should **not** be used.</span></span> <span data-ttu-id="347d3-239">최상위 와일드카드 바인딩으로 인해 앱 보안 취약성이 생길 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-239">Top-level wildcard bindings create app security vulnerabilities.</span></span> <span data-ttu-id="347d3-240">강력한 와일드카드와 약한 와일드카드 모두에 적용됩니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-240">This applies to both strong and weak wildcards.</span></span> <span data-ttu-id="347d3-241">와일드카드보다는 명시적 호스트 이름 또는 IP 주소를 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-241">Use explicit host names or IP addresses rather than wildcards.</span></span> <span data-ttu-id="347d3-242">전체 부모 도메인을 제어하는 경우 하위 도메인 와일드카드 바인딩(예: `*.mysub.com`)은 보안 위험이 아닙니다(취약한 `*.com`과 반대임).</span><span class="sxs-lookup"><span data-stu-id="347d3-242">Subdomain wildcard binding (for example, `*.mysub.com`) isn't a security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="347d3-243">자세한 내용은 [RFC 7230: Section 5.4: Host](https://tools.ietf.org/html/rfc7230#section-5.4)(RFC 7230: 섹션 5.4: 호스트)를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="347d3-243">For more information, see [RFC 7230: Section 5.4: Host](https://tools.ietf.org/html/rfc7230#section-5.4).</span></span>

1. <span data-ttu-id="347d3-244">서버에 URL 접두사를 미리 등록합니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-244">Preregister URL prefixes on the server.</span></span>

   <span data-ttu-id="347d3-245">HTTP.sys 구성에 대한 기본 제공 도구는 *netsh.exe*입니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-245">The built-in tool for configuring HTTP.sys is *netsh.exe*.</span></span> <span data-ttu-id="347d3-246">*netsh.exe*는 URL 접두사를 예약하고 X.509 인증서를 할당하는 데 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-246">*netsh.exe* is used to reserve URL prefixes and assign X.509 certificates.</span></span> <span data-ttu-id="347d3-247">도구를 사용하려면 관리자 권한이 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-247">The tool requires administrator privileges.</span></span>

   <span data-ttu-id="347d3-248">*netsh.exe* 도구를 사용하여 앱의 URL을 등록합니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-248">Use the *netsh.exe* tool to register URLs for the app:</span></span>

   ```console
   netsh http add urlacl url=<URL> user=<USER>
   ```

   * <span data-ttu-id="347d3-249">`<URL>` &ndash; 정규화된 URL(Uniform Resource Locator)입니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-249">`<URL>` &ndash; The fully qualified Uniform Resource Locator (URL).</span></span> <span data-ttu-id="347d3-250">와일드카드 바인딩을 사용하지 마세요.</span><span class="sxs-lookup"><span data-stu-id="347d3-250">Don't use a wildcard binding.</span></span> <span data-ttu-id="347d3-251">유효한 호스트 이름 또는 로컬 IP 주소를 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-251">Use a valid hostname or local IP address.</span></span> <span data-ttu-id="347d3-252">‘URL에는 후행 슬래시가 포함되어야 합니다.’</span><span class="sxs-lookup"><span data-stu-id="347d3-252">*The URL must include a trailing slash.*</span></span>
   * <span data-ttu-id="347d3-253">`<USER>` &ndash; 사용자 또는 사용자-그룹 이름을 지정합니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-253">`<USER>` &ndash; Specifies the user or user-group name.</span></span>

   <span data-ttu-id="347d3-254">다음 예제에서 서버의 로컬 IP 주소는 `10.0.0.4`입니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-254">In the following example, the local IP address of the server is `10.0.0.4`:</span></span>

   ```console
   netsh http add urlacl url=https://10.0.0.4:443/ user=Users
   ```

   <span data-ttu-id="347d3-255">URL이 등록되면 도구가 `URL reservation successfully added`로 응답합니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-255">When a URL is registered, the tool responds with `URL reservation successfully added`.</span></span>

   <span data-ttu-id="347d3-256">등록된 URL을 삭제하려면 `delete urlacl` 명령을 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-256">To delete a registered URL, use the `delete urlacl` command:</span></span>

   ```console
   netsh http delete urlacl url=<URL>
   ```

1. <span data-ttu-id="347d3-257">서버에 X.509 인증서를 등록합니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-257">Register X.509 certificates on the server.</span></span>

   <span data-ttu-id="347d3-258">*netsh.exe* 도구를 사용하여 앱의 인증서를 등록합니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-258">Use the *netsh.exe* tool to register certificates for the app:</span></span>

   ```console
   netsh http add sslcert ipport=<IP>:<PORT> certhash=<THUMBPRINT> appid="{<GUID>}"
   ```

   * <span data-ttu-id="347d3-259">`<IP>` &ndash; 바인딩의 로컬 IP 주소를 지정합니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-259">`<IP>` &ndash; Specifies the local IP address for the binding.</span></span> <span data-ttu-id="347d3-260">와일드카드 바인딩을 사용하지 마세요.</span><span class="sxs-lookup"><span data-stu-id="347d3-260">Don't use a wildcard binding.</span></span> <span data-ttu-id="347d3-261">유효한 IP 주소를 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-261">Use a valid IP address.</span></span>
   * <span data-ttu-id="347d3-262">`<PORT>` &ndash; 바인딩의 포트를 지정합니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-262">`<PORT>` &ndash; Specifies the port for the binding.</span></span>
   * <span data-ttu-id="347d3-263">`<THUMBPRINT>` &ndash; X.509 인증서 지문입니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-263">`<THUMBPRINT>` &ndash; The X.509 certificate thumbprint.</span></span>
   * <span data-ttu-id="347d3-264">`<GUID>` &ndash; 정보 제공용 앱을 나타내는 개발자가 생성한 GUID입니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-264">`<GUID>` &ndash; A developer-generated GUID to represent the app for informational purposes.</span></span>

   <span data-ttu-id="347d3-265">참조용으로 GUID를 패키지 태그로 앱에 저장합니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-265">For reference purposes, store the GUID in the app as a package tag:</span></span>

   * <span data-ttu-id="347d3-266">Visual Studio에서 다음을 수행합니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-266">In Visual Studio:</span></span>
     * <span data-ttu-id="347d3-267">**솔루션 탐색기**에서 앱을 마우스 오른쪽 단추로 클릭하고 **속성**을 선택하여 앱의 프로젝트 속성을 엽니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-267">Open the app's project properties by right-clicking on the app in **Solution Explorer** and selecting **Properties**.</span></span>
     * <span data-ttu-id="347d3-268">**패키지** 탭을 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-268">Select the **Package** tab.</span></span>
     * <span data-ttu-id="347d3-269">**태그** 필드에 직접 만든 GUID를 입력합니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-269">Enter the GUID that you created in the **Tags** field.</span></span>
   * <span data-ttu-id="347d3-270">Visual Studio를 사용하지 않는 경우:</span><span class="sxs-lookup"><span data-stu-id="347d3-270">When not using Visual Studio:</span></span>
     * <span data-ttu-id="347d3-271">앱의 프로젝트 파일을 엽니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-271">Open the app's project file.</span></span>
     * <span data-ttu-id="347d3-272">직접 만든 GUID를 사용하여 `<PackageTags>` 속성을 새로운 또는 기존 `<PropertyGroup>`에 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-272">Add a `<PackageTags>` property to a new or existing `<PropertyGroup>` with the GUID that you created:</span></span>

       ```xml
       <PropertyGroup>
         <PackageTags>9412ee86-c21b-4eb8-bd89-f650fbf44931</PackageTags>
       </PropertyGroup>
       ```

   <span data-ttu-id="347d3-273">다음 예제에서는</span><span class="sxs-lookup"><span data-stu-id="347d3-273">In the following example:</span></span>

   * <span data-ttu-id="347d3-274">서버의 로컬 IP 주소는 `10.0.0.4`입니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-274">The local IP address of the server is `10.0.0.4`.</span></span>
   * <span data-ttu-id="347d3-275">온라인 임의 GUID 생성기는 `appid` 값을 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-275">An online random GUID generator provides the `appid` value.</span></span>

   ```console
   netsh http add sslcert 
       ipport=10.0.0.4:443 
       certhash=b66ee04419d4ee37464ab8785ff02449980eae10 
       appid="{9412ee86-c21b-4eb8-bd89-f650fbf44931}"
   ```

   <span data-ttu-id="347d3-276">인증서가 등록되면 도구가 `SSL Certificate successfully added`로 응답합니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-276">When a certificate is registered, the tool responds with `SSL Certificate successfully added`.</span></span>

   <span data-ttu-id="347d3-277">인증서 등록을 삭제하려면 `delete sslcert` 명령을 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-277">To delete a certificate registration, use the `delete sslcert` command:</span></span>

   ```console
   netsh http delete sslcert ipport=<IP>:<PORT>
   ```

   <span data-ttu-id="347d3-278">*netsh.exe*에 대한 참조 문서입니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-278">Reference documentation for *netsh.exe*:</span></span>

   * [<span data-ttu-id="347d3-279">HTTP(Hypertext Transfer Protocol)에 대한 Netsh 명령</span><span class="sxs-lookup"><span data-stu-id="347d3-279">Netsh Commands for Hypertext Transfer Protocol (HTTP)</span></span>](https://technet.microsoft.com/library/cc725882.aspx)
   * [<span data-ttu-id="347d3-280">UrlPrefix 문자열</span><span class="sxs-lookup"><span data-stu-id="347d3-280">UrlPrefix Strings</span></span>](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

1. <span data-ttu-id="347d3-281">앱을 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-281">Run the app.</span></span>

   <span data-ttu-id="347d3-282">1024보다 큰 포트 번호로 HTTP(HTTPS 아님)를 사용하여 localhost에 바인딩하는 경우에는 앱을 실행하는 데 관리자 권한이 필요하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-282">Administrator privileges aren't required to run the app when binding to localhost using HTTP (not HTTPS) with a port number greater than 1024.</span></span> <span data-ttu-id="347d3-283">다른 구성(예: 로컬 IP 주소 사용 또는 포트 443에 바인딩)의 경우 관리자 권한으로 앱을 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-283">For other configurations (for example, using a local IP address or binding to port 443), run the app with administrator privileges.</span></span>

   <span data-ttu-id="347d3-284">이 앱은 서버의 공용 IP 주소에서 응답합니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-284">The app responds at the server's public IP address.</span></span> <span data-ttu-id="347d3-285">이 예제에서는 서버가 `104.214.79.47`의 공용 IP 주소에 있는 인터넷에서 연결됩니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-285">In this example, the server is reached from the Internet at its public IP address of `104.214.79.47`.</span></span>

   <span data-ttu-id="347d3-286">이 예제에는 개발 인증서가 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-286">A development certificate is used in this example.</span></span> <span data-ttu-id="347d3-287">브라우저의 신뢰할 수 없는 인증서 경고를 무시한 후 페이지가 안전하게 로드됩니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-287">The page loads securely after bypassing the browser's untrusted certificate warning.</span></span>

   ![로드된 앱의 인덱스 페이지를 보여 주는 브라우저 창](httpsys/_static/browser.png)

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="347d3-289">프록시 서버 및 부하 분산 장치 시나리오</span><span class="sxs-lookup"><span data-stu-id="347d3-289">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="347d3-290">인터넷 또는 회사 네트워크의 요청과 상호 작용하는 HTTP.sys에서 호스팅하는 앱의 경우, 프록시 서버 및 부하 분산 장치 뒤에서 호스팅할 때 추가 구성이 필요할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="347d3-290">For apps hosted by HTTP.sys that interact with requests from the Internet or a corporate network, additional configuration might be required when hosting behind proxy servers and load balancers.</span></span> <span data-ttu-id="347d3-291">자세한 내용은 [프록시 서버 및 부하 분산 장치를 사용하도록 ASP.NET Core 구성](xref:host-and-deploy/proxy-load-balancer)을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="347d3-291">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="347d3-292">추가 자료</span><span class="sxs-lookup"><span data-stu-id="347d3-292">Additional resources</span></span>

* [<span data-ttu-id="347d3-293">HTTP.sys에서 Windows 인증 사용</span><span class="sxs-lookup"><span data-stu-id="347d3-293">Enable Windows Authentication with HTTP.sys</span></span>](xref:security/authentication/windowsauth#enable-windows-authentication-with-httpsys)
* <span data-ttu-id="347d3-294">[HTTP Server API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx)(HTTP 서버 API)</span><span class="sxs-lookup"><span data-stu-id="347d3-294">[HTTP Server API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx)</span></span>
* [<span data-ttu-id="347d3-295">aspnet/HttpSysServer GitHub 리포지토리(소스 코드)</span><span class="sxs-lookup"><span data-stu-id="347d3-295">aspnet/HttpSysServer GitHub repository (source code)</span></span>](https://github.com/aspnet/HttpSysServer/)
* <xref:fundamentals/host/index>
* <xref:test/troubleshoot>
