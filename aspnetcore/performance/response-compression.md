---
title: ASP.NET Core에서 응답 압축
author: guardrex
description: ASP.NET Core 앱에서 응답 압축 미들웨어를 사용하는 방법 및 응답 압축에 대해 알아봅니다.
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/18/2018
uid: performance/response-compression
ms.openlocfilehash: 51ab51652a7b3f9b4ef97b3abbffe2e398c0bfb5
ms.sourcegitcommit: 816f39e852a8f453e8682081871a31bc66db153a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/19/2018
ms.locfileid: "53637757"
---
# <a name="response-compression-in-aspnet-core"></a><span data-ttu-id="baee1-103">ASP.NET Core에서 응답 압축</span><span class="sxs-lookup"><span data-stu-id="baee1-103">Response compression in ASP.NET Core</span></span>

<span data-ttu-id="baee1-104">[Luke Latham](https://github.com/guardrex)으로</span><span class="sxs-lookup"><span data-stu-id="baee1-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="baee1-105">[예제 코드 살펴보기 및 다운로드](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples) ([다운로드 방법](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="baee1-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="baee1-106">네트워크 대역폭은 제한 된 리소스입니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-106">Network bandwidth is a limited resource.</span></span> <span data-ttu-id="baee1-107">일반적으로 응답의 크기를 줄이면 앱의 응답성 데이터는 종종 크게 증가 합니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-107">Reducing the size of the response usually increases the responsiveness of an app, often dramatically.</span></span> <span data-ttu-id="baee1-108">페이로드 크기를 줄이는 한 가지 방법은 응용 프로그램의 응답을 압축 하는 경우</span><span class="sxs-lookup"><span data-stu-id="baee1-108">One way to reduce payload sizes is to compress an app's responses.</span></span>

## <a name="when-to-use-response-compression-middleware"></a><span data-ttu-id="baee1-109">응답 압축 미들웨어를 사용 하는 경우</span><span class="sxs-lookup"><span data-stu-id="baee1-109">When to use Response Compression Middleware</span></span>

<span data-ttu-id="baee1-110">IIS, Apache 또는 Nginx에서 서버 기반 응답 압축 기술을 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-110">Use server-based response compression technologies in IIS, Apache, or Nginx.</span></span> <span data-ttu-id="baee1-111">미들웨어의 성능이 아마도 일치 하지 않기 때문 서버 모듈입니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-111">The performance of the middleware probably won't match that of the server modules.</span></span> <span data-ttu-id="baee1-112">[HTTP.sys 서버](xref:fundamentals/servers/httpsys) 서버와 [Kestrel](xref:fundamentals/servers/kestrel) server 기본 제공 압축 지원 현재 제공 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-112">[HTTP.sys server](xref:fundamentals/servers/httpsys) server and [Kestrel](xref:fundamentals/servers/kestrel) server don't currently offer built-in compression support.</span></span>

<span data-ttu-id="baee1-113">경우에 응답 압축 미들웨어를 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-113">Use Response Compression Middleware when you're:</span></span>

* <span data-ttu-id="baee1-114">다음 서버 기반 압축 기술을 사용할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-114">Unable to use the following server-based compression technologies:</span></span>
  * [<span data-ttu-id="baee1-115">IIS 동적 압축이 모듈</span><span class="sxs-lookup"><span data-stu-id="baee1-115">IIS Dynamic Compression module</span></span>](https://www.iis.net/overview/reliability/dynamiccachingandcompression)
  * [<span data-ttu-id="baee1-116">Apache mod_deflate 모듈</span><span class="sxs-lookup"><span data-stu-id="baee1-116">Apache mod_deflate module</span></span>](http://httpd.apache.org/docs/current/mod/mod_deflate.html)
  * [<span data-ttu-id="baee1-117">Nginx 압축 및 압축 풀기</span><span class="sxs-lookup"><span data-stu-id="baee1-117">Nginx Compression and Decompression</span></span>](https://www.nginx.com/resources/admin-guide/compression-and-decompression/)
* <span data-ttu-id="baee1-118">직접 호스트:</span><span class="sxs-lookup"><span data-stu-id="baee1-118">Hosting directly on:</span></span>
  * <span data-ttu-id="baee1-119">[HTTP.sys 서버](xref:fundamentals/servers/httpsys) (이전의 WebListener)</span><span class="sxs-lookup"><span data-stu-id="baee1-119">[HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called WebListener)</span></span>
  * [<span data-ttu-id="baee1-120">Kestrel 서버</span><span class="sxs-lookup"><span data-stu-id="baee1-120">Kestrel server</span></span>](xref:fundamentals/servers/kestrel)

## <a name="response-compression"></a><span data-ttu-id="baee1-121">응답 압축</span><span class="sxs-lookup"><span data-stu-id="baee1-121">Response compression</span></span>

<span data-ttu-id="baee1-122">일반적으로 압축 되지 않음 응답에서 응답 압축 이점을 얻을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-122">Usually, any response not natively compressed can benefit from response compression.</span></span> <span data-ttu-id="baee1-123">일반적으로 압축 되지 않음 응답은 다음과 같습니다. CSS, JavaScript, HTML, XML 및 JSON입니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-123">Responses not natively compressed typically include: CSS, JavaScript, HTML, XML, and JSON.</span></span> <span data-ttu-id="baee1-124">PNG 파일 등 기본적으로 압축 된 자산을 압축 하지 않아야 합니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-124">You shouldn't compress natively compressed assets, such as PNG files.</span></span> <span data-ttu-id="baee1-125">기본적으로 압축 된 응답을 압축 하려는 경우 크기와 전송 시간에 적은 추가 감소는 압축을 처리 하는 데 걸린 시간을 기준으로 늘린다 가능성이 높습니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-125">If you attempt to further compress a natively compressed response, any small additional reduction in size and transmission time will likely be overshadowed by the time it took to process the compression.</span></span> <span data-ttu-id="baee1-126">(파일의 콘텐츠 및 압축의 효율성)에 따라 약 150 1000 바이트 보다 작은 파일을 압축 하지 마십시오.</span><span class="sxs-lookup"><span data-stu-id="baee1-126">Don't compress files smaller than about 150-1000 bytes (depending on the file's content and the efficiency of compression).</span></span> <span data-ttu-id="baee1-127">작은 파일을 압축 하는 오버 헤드는 압축 되지 않은 파일 보다 큰 압축 된 파일을 생성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-127">The overhead of compressing small files may produce a compressed file larger than the uncompressed file.</span></span>

<span data-ttu-id="baee1-128">클라이언트 전송 하 여 해당 기능의 서버를 알려야 클라이언트 압축 된 콘텐츠를 처리 하는 경우는 `Accept-Encoding` 요청 헤더입니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-128">When a client can process compressed content, the client must inform the server of its capabilities by sending the `Accept-Encoding` header with the request.</span></span> <span data-ttu-id="baee1-129">서버에서 압축 된 콘텐츠를 보내는 경우에서 정보를 포함 해야 하는 `Content-Encoding` 압축 된 응답 인코딩 되는 방법에 대 한 헤더입니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-129">When a server sends compressed content, it must include information in the `Content-Encoding` header on how the compressed response is encoded.</span></span> <span data-ttu-id="baee1-130">미들웨어에서 지 원하는 콘텐츠 인코딩 지정은 다음 표에 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-130">Content encoding designations supported by the middleware are shown in the following table.</span></span>

::: moniker range=">= aspnetcore-2.2"

| <span data-ttu-id="baee1-131">`Accept-Encoding` 헤더 값</span><span class="sxs-lookup"><span data-stu-id="baee1-131">`Accept-Encoding` header values</span></span> | <span data-ttu-id="baee1-132">지원 되는 미들웨어</span><span class="sxs-lookup"><span data-stu-id="baee1-132">Middleware Supported</span></span> | <span data-ttu-id="baee1-133">설명</span><span class="sxs-lookup"><span data-stu-id="baee1-133">Description</span></span> |
| ------------------------------- | :------------------: | ----------- |
| `br`                            | <span data-ttu-id="baee1-134">예 (기본값)</span><span class="sxs-lookup"><span data-stu-id="baee1-134">Yes (default)</span></span>        | [<span data-ttu-id="baee1-135">Brotli 압축 된 데이터 형식</span><span class="sxs-lookup"><span data-stu-id="baee1-135">Brotli compressed data format</span></span>](https://tools.ietf.org/html/rfc7932) |
| `deflate`                       | <span data-ttu-id="baee1-136">아니요</span><span class="sxs-lookup"><span data-stu-id="baee1-136">No</span></span>                   | [<span data-ttu-id="baee1-137">DEFLATE 압축 된 데이터 형식</span><span class="sxs-lookup"><span data-stu-id="baee1-137">DEFLATE compressed data format</span></span>](https://tools.ietf.org/html/rfc1951) |
| `exi`                           | <span data-ttu-id="baee1-138">아니요</span><span class="sxs-lookup"><span data-stu-id="baee1-138">No</span></span>                   | [<span data-ttu-id="baee1-139">W3C 효율적인 XML 교환</span><span class="sxs-lookup"><span data-stu-id="baee1-139">W3C Efficient XML Interchange</span></span>](https://tools.ietf.org/id/draft-varga-netconf-exi-capability-00.html) |
| `gzip`                          | <span data-ttu-id="baee1-140">예</span><span class="sxs-lookup"><span data-stu-id="baee1-140">Yes</span></span>                  | [<span data-ttu-id="baee1-141">Gzip 파일 형식</span><span class="sxs-lookup"><span data-stu-id="baee1-141">Gzip file format</span></span>](https://tools.ietf.org/html/rfc1952) |
| `identity`                      | <span data-ttu-id="baee1-142">예</span><span class="sxs-lookup"><span data-stu-id="baee1-142">Yes</span></span>                  | <span data-ttu-id="baee1-143">"인코딩 없이" 식별자: 응답을 인코딩할 수 있어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-143">"No encoding" identifier: The response must not be encoded.</span></span> |
| `pack200-gzip`                  | <span data-ttu-id="baee1-144">아니요</span><span class="sxs-lookup"><span data-stu-id="baee1-144">No</span></span>                   | [<span data-ttu-id="baee1-145">네트워크 전송 형식에 Java 아카이브</span><span class="sxs-lookup"><span data-stu-id="baee1-145">Network Transfer Format for Java Archives</span></span>](https://jcp.org/aboutJava/communityprocess/review/jsr200/index.html) |
| `*`                             | <span data-ttu-id="baee1-146">예</span><span class="sxs-lookup"><span data-stu-id="baee1-146">Yes</span></span>                  | <span data-ttu-id="baee1-147">모든 사용 가능한 콘텐츠 인코딩을 명시적으로 요청 하지</span><span class="sxs-lookup"><span data-stu-id="baee1-147">Any available content encoding not explicitly requested</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-2.2"

| <span data-ttu-id="baee1-148">`Accept-Encoding` 헤더 값</span><span class="sxs-lookup"><span data-stu-id="baee1-148">`Accept-Encoding` header values</span></span> | <span data-ttu-id="baee1-149">지원 되는 미들웨어</span><span class="sxs-lookup"><span data-stu-id="baee1-149">Middleware Supported</span></span> | <span data-ttu-id="baee1-150">설명</span><span class="sxs-lookup"><span data-stu-id="baee1-150">Description</span></span> |
| ------------------------------- | :------------------: | ----------- |
| `br`                            | <span data-ttu-id="baee1-151">아니요</span><span class="sxs-lookup"><span data-stu-id="baee1-151">No</span></span>                   | [<span data-ttu-id="baee1-152">Brotli 압축 된 데이터 형식</span><span class="sxs-lookup"><span data-stu-id="baee1-152">Brotli compressed data format</span></span>](https://tools.ietf.org/html/rfc7932) |
| `deflate`                       | <span data-ttu-id="baee1-153">아니요</span><span class="sxs-lookup"><span data-stu-id="baee1-153">No</span></span>                   | [<span data-ttu-id="baee1-154">DEFLATE 압축 된 데이터 형식</span><span class="sxs-lookup"><span data-stu-id="baee1-154">DEFLATE compressed data format</span></span>](https://tools.ietf.org/html/rfc1951) |
| `exi`                           | <span data-ttu-id="baee1-155">아니요</span><span class="sxs-lookup"><span data-stu-id="baee1-155">No</span></span>                   | [<span data-ttu-id="baee1-156">W3C 효율적인 XML 교환</span><span class="sxs-lookup"><span data-stu-id="baee1-156">W3C Efficient XML Interchange</span></span>](https://tools.ietf.org/id/draft-varga-netconf-exi-capability-00.html) |
| `gzip`                          | <span data-ttu-id="baee1-157">예 (기본값)</span><span class="sxs-lookup"><span data-stu-id="baee1-157">Yes (default)</span></span>        | [<span data-ttu-id="baee1-158">Gzip 파일 형식</span><span class="sxs-lookup"><span data-stu-id="baee1-158">Gzip file format</span></span>](https://tools.ietf.org/html/rfc1952) |
| `identity`                      | <span data-ttu-id="baee1-159">예</span><span class="sxs-lookup"><span data-stu-id="baee1-159">Yes</span></span>                  | <span data-ttu-id="baee1-160">"인코딩 없이" 식별자: 응답을 인코딩할 수 있어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-160">"No encoding" identifier: The response must not be encoded.</span></span> |
| `pack200-gzip`                  | <span data-ttu-id="baee1-161">아니요</span><span class="sxs-lookup"><span data-stu-id="baee1-161">No</span></span>                   | [<span data-ttu-id="baee1-162">네트워크 전송 형식에 Java 아카이브</span><span class="sxs-lookup"><span data-stu-id="baee1-162">Network Transfer Format for Java Archives</span></span>](https://jcp.org/aboutJava/communityprocess/review/jsr200/index.html) |
| `*`                             | <span data-ttu-id="baee1-163">예</span><span class="sxs-lookup"><span data-stu-id="baee1-163">Yes</span></span>                  | <span data-ttu-id="baee1-164">모든 사용 가능한 콘텐츠 인코딩을 명시적으로 요청 하지</span><span class="sxs-lookup"><span data-stu-id="baee1-164">Any available content encoding not explicitly requested</span></span> |

::: moniker-end

<span data-ttu-id="baee1-165">자세한 내용은 참조는 [IANA 공식 콘텐츠 코딩 목록](http://www.iana.org/assignments/http-parameters/http-parameters.xml#http-content-coding-registry)합니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-165">For more information, see the [IANA Official Content Coding List](http://www.iana.org/assignments/http-parameters/http-parameters.xml#http-content-coding-registry).</span></span>

<span data-ttu-id="baee1-166">미들웨어를 사용 하면 사용자 지정에 대 한 추가 압축 공급자를 추가 하려면 `Accept-Encoding` 헤더 값입니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-166">The middleware allows you to add additional compression providers for custom `Accept-Encoding` header values.</span></span> <span data-ttu-id="baee1-167">자세한 내용은 [사용자 지정 공급자](#custom-providers) 아래.</span><span class="sxs-lookup"><span data-stu-id="baee1-167">For more information, see [Custom Providers](#custom-providers) below.</span></span>

<span data-ttu-id="baee1-168">미들웨어 품질 값에 응답할 수 (qvalue, `q`) 가중치를 보내면 클라이언트에서 압축 체계를 우선 순위를 지정 합니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-168">The middleware is capable of reacting to quality value (qvalue, `q`) weighting when sent by the client to prioritize compression schemes.</span></span> <span data-ttu-id="baee1-169">자세한 내용은 참조 하세요. [RFC 7231: 허용 인코딩](https://tools.ietf.org/html/rfc7231#section-5.3.4)합니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-169">For more information, see [RFC 7231: Accept-Encoding](https://tools.ietf.org/html/rfc7231#section-5.3.4).</span></span>

<span data-ttu-id="baee1-170">압축 알고리즘은 압축 속도 압축의 효율성 간의 균형을 유지 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-170">Compression algorithms are subject to a tradeoff between compression speed and the effectiveness of the compression.</span></span> <span data-ttu-id="baee1-171">*효율성* 이 컨텍스트에서 참조 출력의 크기를 압축 합니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-171">*Effectiveness* in this context refers to the size of the output after compression.</span></span> <span data-ttu-id="baee1-172">최소 크기를 가장 하 여 이루어집니다 *최적의* 압축 합니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-172">The smallest size is achieved by the most *optimal* compression.</span></span>

<span data-ttu-id="baee1-173">요청에 관련 된 헤더, 보내기, 캐싱 및 압축 된 내용을 받기는 아래 표에 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-173">The headers involved in requesting, sending, caching, and receiving compressed content are described in the table below.</span></span>

| <span data-ttu-id="baee1-174">헤더</span><span class="sxs-lookup"><span data-stu-id="baee1-174">Header</span></span>             | <span data-ttu-id="baee1-175">역할</span><span class="sxs-lookup"><span data-stu-id="baee1-175">Role</span></span> |
| ------------------ | ---- |
| `Accept-Encoding`  | <span data-ttu-id="baee1-176">보낼 클라이언트에서 서버로 인코딩 체계를 클라이언트에 허용 되는 콘텐츠를 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-176">Sent from the client to the server to indicate the content encoding schemes acceptable to the client.</span></span> |
| `Content-Encoding` | <span data-ttu-id="baee1-177">서버에서 클라이언트로 보낼 페이로드에 콘텐츠 인코딩을 나타낼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-177">Sent from the server to the client to indicate the encoding of the content in the payload.</span></span> |
| `Content-Length`   | <span data-ttu-id="baee1-178">압축이 발생할 때를 `Content-Length` 응답 압축 하는 경우는 본문 콘텐츠가 변경 이후 헤더를 제거 합니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-178">When compression occurs, the `Content-Length` header is removed, since the body content changes when the response is compressed.</span></span> |
| `Content-MD5`      | <span data-ttu-id="baee1-179">압축이 발생할 때를 `Content-MD5` 본문 콘텐츠가 변경 된 후 해시 더 이상 유효 헤더를 제거 합니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-179">When compression occurs, the `Content-MD5` header is removed, since the body content has changed and the hash is no longer valid.</span></span> |
| `Content-Type`     | <span data-ttu-id="baee1-180">콘텐츠의 MIME 형식을 지정합니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-180">Specifies the MIME type of the content.</span></span> <span data-ttu-id="baee1-181">모든 응답을 지정 해야 해당 `Content-Type`합니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-181">Every response should specify its `Content-Type`.</span></span> <span data-ttu-id="baee1-182">미들웨어는 응답을 압축 해야 하는 경우를 확인 하려면이 값을 확인 합니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-182">The middleware checks this value to determine if the response should be compressed.</span></span> <span data-ttu-id="baee1-183">미들웨어 집합을 지정 [MIME 형식 기본](#mime-types) 인코딩할 수 있지만 바꾸거나 MIME 형식을 추가할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-183">The middleware specifies a set of [default MIME types](#mime-types) that it can encode, but you can replace or add MIME types.</span></span> |
| `Vary`             | <span data-ttu-id="baee1-184">값을 사용 하 여 서버에서 전송 하는 경우 `Accept-Encoding` 클라이언트와 프록시에는 `Vary` 헤더는 클라이언트에 캐시 해야 하는 프록시를 나타냅니다 (다)의 값을 기반으로 하는 응답을 `Accept-Encoding` 요청의 헤더입니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-184">When sent by the server with a value of `Accept-Encoding` to clients and proxies, the `Vary` header indicates to the client or proxy that it should cache (vary) responses based on the value of the `Accept-Encoding` header of the request.</span></span> <span data-ttu-id="baee1-185">사용 하 여 콘텐츠를 반환 하는 결과 `Vary: Accept-Encoding` 헤더는 모두 압축 하 고 압축 되지 않은 응답이 별도로 캐시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-185">The result of returning content with the `Vary: Accept-Encoding` header is that both compressed and uncompressed responses are cached separately.</span></span> |

<span data-ttu-id="baee1-186">사용 하 여 응답 압축 미들웨어의 기능을 탐색 합니다 [샘플 앱](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples)합니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-186">Explore the features of the Response Compression Middleware with the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples).</span></span> <span data-ttu-id="baee1-187">이 샘플에서는 다음을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-187">The sample illustrates:</span></span>

* <span data-ttu-id="baee1-188">Gzip 및 사용자 지정 압축 공급자를 사용 하 여 앱 응답을 압축 합니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-188">The compression of app responses using Gzip and custom compression providers.</span></span>
* <span data-ttu-id="baee1-189">압축에 대 한 MIME 형식 목록을 기본 MIME 형식을 추가 하는 방법.</span><span class="sxs-lookup"><span data-stu-id="baee1-189">How to add a MIME type to the default list of MIME types for compression.</span></span>

## <a name="package"></a><span data-ttu-id="baee1-190">Package</span><span class="sxs-lookup"><span data-stu-id="baee1-190">Package</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="baee1-191">미들웨어를 프로젝트에 포함 하려면에 대 한 참조를 추가 합니다 [Microsoft.AspNetCore.App 메타 패키지](xref:fundamentals/metapackage-app)를 포함 하는 합니다 [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) 패키지.</span><span class="sxs-lookup"><span data-stu-id="baee1-191">To include the middleware in a project, add a reference to the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), which includes the [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) package.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="baee1-192">미들웨어를 프로젝트에 포함 하려면에 대 한 참조를 추가 합니다 [Microsoft.AspNetCore.All 메타 패키지](xref:fundamentals/metapackage)를 포함 하는 합니다 [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) 패키지.</span><span class="sxs-lookup"><span data-stu-id="baee1-192">To include the middleware in a project, add a reference to the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage), which includes the [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) package.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="baee1-193">미들웨어를 프로젝트에 포함 하려면에 대 한 참조를 추가 합니다 [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) 패키지 있습니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-193">To include the middleware in a project, add a reference to the [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) package.</span></span>

::: moniker-end

## <a name="configuration"></a><span data-ttu-id="baee1-194">구성하기</span><span class="sxs-lookup"><span data-stu-id="baee1-194">Configuration</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="baee1-195">다음 코드에 기본 MIME 형식 및 압축 공급자에 대 한 응답 압축 미들웨어를 사용 하도록 설정 하는 방법을 보여 줍니다 ([Brotli](#brotli-compression-provider) 하 고 [Gzip](#gzip-compression-provider)):</span><span class="sxs-lookup"><span data-stu-id="baee1-195">The following code shows how to enable the Response Compression Middleware for default MIME types and compression providers ([Brotli](#brotli-compression-provider) and [Gzip](#gzip-compression-provider)):</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="baee1-196">다음 코드에서는 기본 MIME 형식에 대 한 응답 압축 미들웨어를 사용 하도록 설정 하 고 [Gzip 압축 공급자](#gzip-compression-provider):</span><span class="sxs-lookup"><span data-stu-id="baee1-196">The following code shows how to enable the Response Compression Middleware for default MIME types and the [Gzip Compression Provider](#gzip-compression-provider):</span></span>

::: moniker-end

```csharp
public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddResponseCompression();
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
        app.UseResponseCompression();
    }
}
```

> [!NOTE]
> <span data-ttu-id="baee1-197">와 같은 도구를 사용 하 여 [Fiddler](http://www.telerik.com/fiddler)를 [Firebug](http://getfirebug.com/), 또는 [Postman](https://www.getpostman.com/) 설정 하는 `Accept-Encoding` 요청 헤더 및 응답 헤더, 크기 및 본문을 연구 합니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-197">Use a tool like [Fiddler](http://www.telerik.com/fiddler), [Firebug](http://getfirebug.com/), or [Postman](https://www.getpostman.com/) to set the `Accept-Encoding` request header and study the response headers, size, and body.</span></span>

<span data-ttu-id="baee1-198">요청 하지 않고 샘플 앱을 제출 합니다 `Accept-Encoding` 헤더 응답 압축 되지를 확인 합니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-198">Submit a request to the sample app without the `Accept-Encoding` header and observe that the response is uncompressed.</span></span> <span data-ttu-id="baee1-199">`Content-Encoding` 및 `Vary` 헤더 응답에 없습니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-199">The `Content-Encoding` and `Vary` headers aren't present on the response.</span></span>

![Accept-encoding 헤더 없이 요청의 결과 보여 주는 fiddler 창입니다.](response-compression/_static/request-uncompressed.png)

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="baee1-202">샘플 응용 프로그램에 요청을 제출 합니다 `Accept-Encoding: br` 헤더 (Brotli 압축) 응답 압축 되어 있는지 확인 합니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-202">Submit a request to the sample app with the `Accept-Encoding: br` header (Brotli compression) and observe that the response is compressed.</span></span> <span data-ttu-id="baee1-203">합니다 `Content-Encoding` 고 `Vary` 헤더가 응답에 있는 합니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-203">The `Content-Encoding` and `Vary` headers are present on the response.</span></span>

![Accept-encoding 헤더를 사용 하 여 요청의 결과 및 브라질의 값을 보여 주는 fiddler 창입니다.](response-compression/_static/request-compressed-br.png)

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="baee1-207">샘플 응용 프로그램에 요청을 제출 합니다 `Accept-Encoding: gzip` 헤더 응답 압축 되어 있는지 확인 합니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-207">Submit a request to the sample app with the `Accept-Encoding: gzip` header and observe that the response is compressed.</span></span> <span data-ttu-id="baee1-208">합니다 `Content-Encoding` 고 `Vary` 헤더가 응답에 있는 합니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-208">The `Content-Encoding` and `Vary` headers are present on the response.</span></span>

![Accept-encoding 헤더를 사용 하 여 요청의 결과 및 gzip 값을 보여 주는 fiddler 창입니다.](response-compression/_static/request-compressed.png)

::: moniker-end

## <a name="providers"></a><span data-ttu-id="baee1-212">공급자</span><span class="sxs-lookup"><span data-stu-id="baee1-212">Providers</span></span>

::: moniker range=">= aspnetcore-2.2"

### <a name="brotli-compression-provider"></a><span data-ttu-id="baee1-213">Brotli 압축 공급자</span><span class="sxs-lookup"><span data-stu-id="baee1-213">Brotli Compression Provider</span></span>

<span data-ttu-id="baee1-214">사용 합니다 `BrotliCompressionProvider` 사용 하 여 응답을 압축 하는 [Brotli 압축 된 데이터 형식](https://tools.ietf.org/html/rfc7932)합니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-214">Use the `BrotliCompressionProvider` to compress responses with the [Brotli compressed data format](https://tools.ietf.org/html/rfc7932).</span></span>

<span data-ttu-id="baee1-215">압축 공급자가 없습니다에 명시적으로 추가 되는 <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:</span><span class="sxs-lookup"><span data-stu-id="baee1-215">If no compression providers are explicitly added to the <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:</span></span>

* <span data-ttu-id="baee1-216">Brotli 압축 공급자가 함께 압축 공급자의 배열에 기본적으로 추가 합니다 [Gzip 압축 공급자](#gzip-compression-provider)합니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-216">The Brotli Compression Provider is added by default to the array of compression providers along with the [Gzip compression provider](#gzip-compression-provider).</span></span>
* <span data-ttu-id="baee1-217">Brotli 압축 Brotli 압축 된 데이터 형식을 클라이언트에서 지원 되는 압축 기본값은입니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-217">Compression defaults to Brotli compression when the Brotli compressed data format is supported by the client.</span></span> <span data-ttu-id="baee1-218">클라이언트에서 지원 되지 않으면 Brotli 압축 기본값은 Gzip 클라이언트에서 Gzip 압축을 지 원하는 경우입니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-218">If Brotli isn't supported by the client, compression defaults to Gzip when the client supports Gzip compression.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();
}
```

<span data-ttu-id="baee1-219">모든 압축 공급자 명시적으로 추가 되 면 Brotoli 압축 공급자를 추가 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-219">The Brotoli Compression Provider must be added when any compression providers are explicitly added:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression(options =>
    {
        options.Providers.Add<BrotliCompressionProvider>();
        options.Providers.Add<GzipCompressionProvider>();
        options.Providers.Add<CustomCompressionProvider>();
        options.MimeTypes = 
            ResponseCompressionDefaults.MimeTypes.Concat(
                new[] { "image/svg+xml" });
    });
}
```

<span data-ttu-id="baee1-220">압축을 사용 하 여 수준 설정 `BrotliCompressionProviderOptions`합니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-220">Set the compression level with `BrotliCompressionProviderOptions`.</span></span> <span data-ttu-id="baee1-221">가장 빠른 압축 수준을 Brotli 압축 공급자 기본값 ([CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel))에 가장 효율적인 압축을 생성 하지 않을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-221">The Brotli Compression Provider defaults to the fastest compression level ([CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel)), which might not produce the most efficient compression.</span></span> <span data-ttu-id="baee1-222">가장 효율적인 압축을 원하는 경우 최적의 압축에 대 한 미들웨어를 구성 합니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-222">If the most efficient compression is desired, configure the middleware for optimal compression.</span></span>

| <span data-ttu-id="baee1-223">압축 수준</span><span class="sxs-lookup"><span data-stu-id="baee1-223">Compression Level</span></span> | <span data-ttu-id="baee1-224">설명</span><span class="sxs-lookup"><span data-stu-id="baee1-224">Description</span></span> |
| ----------------- | ----------- |
| [<span data-ttu-id="baee1-225">CompressionLevel.Fastest</span><span class="sxs-lookup"><span data-stu-id="baee1-225">CompressionLevel.Fastest</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="baee1-226">압축 결과 출력 최적으로 압축 되지 않으며 경우에 가능한 한 빨리 완료 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-226">Compression should complete as quickly as possible, even if the resulting output isn't optimally compressed.</span></span> |
| [<span data-ttu-id="baee1-227">CompressionLevel.NoCompression</span><span class="sxs-lookup"><span data-stu-id="baee1-227">CompressionLevel.NoCompression</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="baee1-228">압축 하지 않고 수행 되어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-228">No compression should be performed.</span></span> |
| [<span data-ttu-id="baee1-229">CompressionLevel.Optimal</span><span class="sxs-lookup"><span data-stu-id="baee1-229">CompressionLevel.Optimal</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="baee1-230">응답은 최적으로 압축 되어야 압축을 완료 하는 데 시간이 많이 걸린다는 하는 경우에 합니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-230">Responses should be optimally compressed, even if the compression takes more time to complete.</span></span> |

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();

    services.Configure<BrotliCompressionProviderOptions>(options => 
    {
        options.Level = CompressionLevel.Fastest;
    });
}
```

::: moniker-end

### <a name="gzip-compression-provider"></a><span data-ttu-id="baee1-231">Gzip 압축 공급자</span><span class="sxs-lookup"><span data-stu-id="baee1-231">Gzip Compression Provider</span></span>

<span data-ttu-id="baee1-232">사용 합니다 <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProvider> 사용 하 여 응답을 압축 하는 [Gzip 파일 형식](https://tools.ietf.org/html/rfc1952)합니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-232">Use the <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProvider> to compress responses with the [Gzip file format](https://tools.ietf.org/html/rfc1952).</span></span>

<span data-ttu-id="baee1-233">압축 공급자가 없습니다에 명시적으로 추가 되는 <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:</span><span class="sxs-lookup"><span data-stu-id="baee1-233">If no compression providers are explicitly added to the <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="baee1-234">Gzip 압축 공급자가 함께 압축 공급자의 배열에 기본적으로 추가 합니다 [Brotli 압축 공급자](#brotli-compression-provider)합니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-234">The Gzip Compression Provider is added by default to the array of compression providers along with the [Brotli Compression Provider](#brotli-compression-provider).</span></span>
* <span data-ttu-id="baee1-235">Brotli 압축 Brotli 압축 된 데이터 형식을 클라이언트에서 지원 되는 압축 기본값은입니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-235">Compression defaults to Brotli compression when the Brotli compressed data format is supported by the client.</span></span> <span data-ttu-id="baee1-236">클라이언트에서 지원 되지 않으면 Brotli 압축 기본값은 Gzip 클라이언트에서 Gzip 압축을 지 원하는 경우입니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-236">If Brotli isn't supported by the client, compression defaults to Gzip when the client supports Gzip compression.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="baee1-237">Gzip 압축 공급자는 기본적으로 압축 공급자의 배열에 추가 됩니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-237">The Gzip Compression Provider is added by default to the array of compression providers.</span></span>
* <span data-ttu-id="baee1-238">클라이언트에서 Gzip 압축을 지 원하는 경우 Gzip 압축 기본값은입니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-238">Compression defaults to Gzip when the client supports Gzip compression.</span></span>

::: moniker-end

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();
}
```

<span data-ttu-id="baee1-239">Gzip 압축 공급자는 모든 압축 공급자 명시적으로 추가 되 면 추가 되어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-239">The Gzip Compression Provider must be added when any compression providers are explicitly added:</span></span>

::: moniker range=">= aspnetcore-2.2"

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression(options =>
    {
        options.Providers.Add<BrotliCompressionProvider>();
        options.Providers.Add<GzipCompressionProvider>();
        options.Providers.Add<CustomCompressionProvider>();
        options.MimeTypes = 
            ResponseCompressionDefaults.MimeTypes.Concat(
                new[] { "image/svg+xml" });
    });
}
```

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-2.1"

[!code-csharp[](response-compression/samples/2.x/Startup.cs?name=snippet1&highlight=5)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet2&highlight=5)]

::: moniker-end

<span data-ttu-id="baee1-240">압축을 사용 하 여 수준 설정 <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProviderOptions>합니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-240">Set the compression level with <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProviderOptions>.</span></span> <span data-ttu-id="baee1-241">가장 빠른 압축 수준을 기본값은 Gzip 압축 공급자 ([CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel))에 가장 효율적인 압축을 생성 하지 않을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-241">The Gzip Compression Provider defaults to the fastest compression level ([CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel)), which might not produce the most efficient compression.</span></span> <span data-ttu-id="baee1-242">가장 효율적인 압축을 원하는 경우 최적의 압축에 대 한 미들웨어를 구성 합니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-242">If the most efficient compression is desired, configure the middleware for optimal compression.</span></span>

| <span data-ttu-id="baee1-243">압축 수준</span><span class="sxs-lookup"><span data-stu-id="baee1-243">Compression Level</span></span> | <span data-ttu-id="baee1-244">설명</span><span class="sxs-lookup"><span data-stu-id="baee1-244">Description</span></span> |
| ----------------- | ----------- |
| [<span data-ttu-id="baee1-245">CompressionLevel.Fastest</span><span class="sxs-lookup"><span data-stu-id="baee1-245">CompressionLevel.Fastest</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="baee1-246">압축 결과 출력 최적으로 압축 되지 않으며 경우에 가능한 한 빨리 완료 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-246">Compression should complete as quickly as possible, even if the resulting output isn't optimally compressed.</span></span> |
| [<span data-ttu-id="baee1-247">CompressionLevel.NoCompression</span><span class="sxs-lookup"><span data-stu-id="baee1-247">CompressionLevel.NoCompression</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="baee1-248">압축 하지 않고 수행 되어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-248">No compression should be performed.</span></span> |
| [<span data-ttu-id="baee1-249">CompressionLevel.Optimal</span><span class="sxs-lookup"><span data-stu-id="baee1-249">CompressionLevel.Optimal</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="baee1-250">응답은 최적으로 압축 되어야 압축을 완료 하는 데 시간이 많이 걸린다는 하는 경우에 합니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-250">Responses should be optimally compressed, even if the compression takes more time to complete.</span></span> |

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();

    services.Configure<GzipCompressionProviderOptions>(options => 
    {
        options.Level = CompressionLevel.Fastest;
    });
}
```

### <a name="custom-providers"></a><span data-ttu-id="baee1-251">사용자 지정 공급자</span><span class="sxs-lookup"><span data-stu-id="baee1-251">Custom providers</span></span>

<span data-ttu-id="baee1-252">사용 하 여 사용자 지정 압축 구현을 만들 <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider>합니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-252">Create custom compression implementations with <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider>.</span></span> <span data-ttu-id="baee1-253">합니다 <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider.EncodingName*> 이 인코딩 콘텐츠를 나타내는 `ICompressionProvider` 생성 합니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-253">The <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider.EncodingName*> represents the content encoding that this `ICompressionProvider` produces.</span></span> <span data-ttu-id="baee1-254">에 지정 된 목록에 따라 공급자를 선택 하 여이 정보를 사용 하는 미들웨어를 `Accept-Encoding` 요청의 헤더입니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-254">The middleware uses this information to choose the provider based on the list specified in the `Accept-Encoding` header of the request.</span></span>

<span data-ttu-id="baee1-255">클라이언트를 사용 하 여 요청을 제출 샘플 앱을 사용 하 여 `Accept-Encoding: mycustomcompression` 헤더입니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-255">Using the sample app, the client submits a request with the `Accept-Encoding: mycustomcompression` header.</span></span> <span data-ttu-id="baee1-256">미들웨어는 사용자 지정 압축 구현을 사용 및 사용 하 여 응답을 반환 합니다를 `Content-Encoding: mycustomcompression` 헤더입니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-256">The middleware uses the custom compression implementation and returns the response with a `Content-Encoding: mycustomcompression` header.</span></span> <span data-ttu-id="baee1-257">클라이언트는 작동 하기 위한 사용자 지정 압축 구현에 대 한 순서에서 사용자 지정 인코딩 압축을 풀 수 있어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-257">The client must be able to decompress the custom encoding in order for a custom compression implementation to work.</span></span>

::: moniker range=">= aspnetcore-2.2"

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression(options =>
    {
        options.Providers.Add<BrotliCompressionProvider>();
        options.Providers.Add<GzipCompressionProvider>();
        options.Providers.Add<CustomCompressionProvider>();
        options.MimeTypes = 
            ResponseCompressionDefaults.MimeTypes.Concat(
                new[] { "image/svg+xml" });
    });
}
```

```csharp
public class CustomCompressionProvider : ICompressionProvider
{
    public string EncodingName => "mycustomcompression";
    public bool SupportsFlush => true;

    public Stream CreateStream(Stream outputStream)
    {
        // Create a custom compression stream wrapper here
        return outputStream;
    }
}
```

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-2.1"

[!code-csharp[](response-compression/samples/2.x/Startup.cs?name=snippet1&highlight=6,12-15)]

[!code-csharp[](response-compression/samples/2.x/CustomCompressionProvider.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet2&highlight=6,12-15)]

[!code-csharp[](response-compression/samples/1.x/CustomCompressionProvider.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="baee1-258">샘플 응용 프로그램에 요청을 제출 합니다 `Accept-Encoding: mycustomcompression` 헤더 응답 헤더를 확인 합니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-258">Submit a request to the sample app with the `Accept-Encoding: mycustomcompression` header and observe the response headers.</span></span> <span data-ttu-id="baee1-259">합니다 `Vary` 고 `Content-Encoding` 헤더가 응답에 있는 합니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-259">The `Vary` and `Content-Encoding` headers are present on the response.</span></span> <span data-ttu-id="baee1-260">응답 본문 (표시 되지 않음) 샘플에서 압축 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-260">The response body (not shown) isn't compressed by the sample.</span></span> <span data-ttu-id="baee1-261">에 압축 구현 하지 않습니다는 `CustomCompressionProvider` 샘플의 클래스입니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-261">There isn't a compression implementation in the `CustomCompressionProvider` class of the sample.</span></span> <span data-ttu-id="baee1-262">그러나 샘플 이러한 압축 알고리즘을 구현 하는 위치를 보여줍니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-262">However, the sample shows where you would implement such a compression algorithm.</span></span>

![Accept-encoding 헤더를 사용 하 여 요청의 결과 및 mycustomcompression 값을 보여 주는 fiddler 창입니다.](response-compression/_static/request-custom-compression.png)

## <a name="mime-types"></a><span data-ttu-id="baee1-265">MIME 형식</span><span class="sxs-lookup"><span data-stu-id="baee1-265">MIME types</span></span>

<span data-ttu-id="baee1-266">미들웨어 압축에 대 한 MIME 형식의 기본 집합을 지정합니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-266">The middleware specifies a default set of MIME types for compression:</span></span>

* `application/javascript`
* `application/json`
* `application/xml`
* `text/css`
* `text/html`
* `text/json`
* `text/plain`
* `text/xml`

<span data-ttu-id="baee1-267">바꾸거나 응답 압축 미들웨어 옵션을 사용 하 여 MIME 형식을 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-267">Replace or append MIME types with the Response Compression Middleware options.</span></span> <span data-ttu-id="baee1-268">참고 해당 와일드 카드 MIME 형식을 `text/*` 지원 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-268">Note that wildcard MIME types, such as `text/*` aren't supported.</span></span> <span data-ttu-id="baee1-269">샘플 앱에 대 한 MIME 형식 추가 `image/svg+xml` 압축 및 ASP.NET Core 배너 이미지는 (*banner.svg*).</span><span class="sxs-lookup"><span data-stu-id="baee1-269">The sample app adds a MIME type for `image/svg+xml` and compresses and serves the ASP.NET Core banner image (*banner.svg*).</span></span>

::: moniker range=">= aspnetcore-2.2"

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression(options =>
    {
        options.Providers.Add<BrotliCompressionProvider>();
        options.Providers.Add<GzipCompressionProvider>();
        options.Providers.Add<CustomCompressionProvider>();
        options.MimeTypes = 
            ResponseCompressionDefaults.MimeTypes.Concat(
                new[] { "image/svg+xml" });
    });
}
```

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-2.1"

[!code-csharp[](response-compression/samples/2.x/Startup.cs?name=snippet1&highlight=7-9)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet2&highlight=7-9)]

::: moniker-end

## <a name="compression-with-secure-protocol"></a><span data-ttu-id="baee1-270">보안 프로토콜을 사용 하 여 압축</span><span class="sxs-lookup"><span data-stu-id="baee1-270">Compression with secure protocol</span></span>

<span data-ttu-id="baee1-271">보안 연결을 통해 압축 된 응답으로 제어할 수 있습니다는 `EnableForHttps` 옵션을 기본적으로 비활성화 됩니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-271">Compressed responses over secure connections can be controlled with the `EnableForHttps` option, which is disabled by default.</span></span> <span data-ttu-id="baee1-272">와 같은 보안 문제가 발생할 수 있습니다 동적으로 생성 된 페이지를 사용 하 여 압축을 사용 하는 [범죄](https://wikipedia.org/wiki/CRIME_(security_exploit)) 하 고 [위반](https://wikipedia.org/wiki/BREACH_(security_exploit)) 공격입니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-272">Using compression with dynamically generated pages can lead to security problems such as the [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) and [BREACH](https://wikipedia.org/wiki/BREACH_(security_exploit)) attacks.</span></span>

## <a name="adding-the-vary-header"></a><span data-ttu-id="baee1-273">Vary 헤더를 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-273">Adding the Vary header</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="baee1-274">경우에 따라 응답을 압축 된 `Accept-Encoding` 헤더 가지 잠재적으로 여러 개의 압축 된 버전의 응답 및 압축 되지 않은 버전을 합니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-274">When compressing responses based on the `Accept-Encoding` header, there are potentially multiple compressed versions of the response and an uncompressed version.</span></span> <span data-ttu-id="baee1-275">여러 버전 있으며 저장 하는 클라이언트와 프록시 캐시 지시 하려면 합니다 `Vary` 함께 머리글이 추가 됩니다는 `Accept-Encoding` 값입니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-275">In order to instruct client and proxy caches that multiple versions exist and should be stored, the `Vary` header is added with an `Accept-Encoding` value.</span></span> <span data-ttu-id="baee1-276">미들웨어를 추가 하는 ASP.NET Core 2.0 이상에서는 `Vary` 응답 압축 되어 있을 때 자동으로 헤더입니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-276">In ASP.NET Core 2.0 or later, the middleware adds the `Vary` header automatically when the response is compressed.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="baee1-277">경우에 따라 응답을 압축 된 `Accept-Encoding` 헤더 가지 잠재적으로 여러 개의 압축 된 버전의 응답 및 압축 되지 않은 버전을 합니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-277">When compressing responses based on the `Accept-Encoding` header, there are potentially multiple compressed versions of the response and an uncompressed version.</span></span> <span data-ttu-id="baee1-278">여러 버전 있으며 저장 하는 클라이언트와 프록시 캐시 지시 하려면 합니다 `Vary` 함께 머리글이 추가 됩니다는 `Accept-Encoding` 값입니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-278">In order to instruct client and proxy caches that multiple versions exist and should be stored, the `Vary` header is added with an `Accept-Encoding` value.</span></span> <span data-ttu-id="baee1-279">ASP.NET core에서 1.x에서 추가 `Vary` 헤더를 응답은 수동으로 수행:</span><span class="sxs-lookup"><span data-stu-id="baee1-279">In ASP.NET Core 1.x, adding the `Vary` header to the response is accomplished manually:</span></span>

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet1)]

::: moniker-end

## <a name="middleware-issue-when-behind-an-nginx-reverse-proxy"></a><span data-ttu-id="baee1-280">Nginx 역방향 프록시 뒤에 있을 때는 미들웨어 문제</span><span class="sxs-lookup"><span data-stu-id="baee1-280">Middleware issue when behind an Nginx reverse proxy</span></span>

<span data-ttu-id="baee1-281">Nginx에서 프록시를 요청 하는 경우는 `Accept-Encoding` 헤더를 제거 합니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-281">When a request is proxied by Nginx, the `Accept-Encoding` header is removed.</span></span> <span data-ttu-id="baee1-282">제거 된 `Accept-Encoding` 헤더에서 응답 압축 미들웨어를 방지 합니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-282">Removal of the `Accept-Encoding` header prevents the middleware from compressing the response.</span></span> <span data-ttu-id="baee1-283">자세한 내용은 참조 하세요. [NGINX: 압축 및 압축 풀기](https://www.nginx.com/resources/admin-guide/compression-and-decompression/)합니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-283">For more information, see [NGINX: Compression and Decompression](https://www.nginx.com/resources/admin-guide/compression-and-decompression/).</span></span> <span data-ttu-id="baee1-284">이 문제를 추적 하 여 [Nginx에 대 한 통과 압축 파악 (aspnet/BasicMiddleware \#123)](https://github.com/aspnet/BasicMiddleware/issues/123)합니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-284">This issue is tracked by [Figure out pass-through compression for Nginx (aspnet/BasicMiddleware \#123)](https://github.com/aspnet/BasicMiddleware/issues/123).</span></span>

## <a name="working-with-iis-dynamic-compression"></a><span data-ttu-id="baee1-285">IIS 동적 압축이 사용</span><span class="sxs-lookup"><span data-stu-id="baee1-285">Working with IIS dynamic compression</span></span>

<span data-ttu-id="baee1-286">활성 IIS 동적 압축이 모듈 앱에 대해 사용 하지 않도록 설정 하려는 서버 수준에서 구성에 있는 경우에 대 한 추가 사용 하 여 모듈을 사용 하지 않도록 설정 합니다 *web.config* 파일입니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-286">If you have an active IIS Dynamic Compression Module configured at the server level that you would like to disable for an app, disable the module with an addition to the *web.config* file.</span></span> <span data-ttu-id="baee1-287">보다 자세한 내용은 [IIS 모듈 비활성화](xref:host-and-deploy/iis/modules#disabling-iis-modules)를 참고하시기 바랍니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-287">For more information, see [Disabling IIS modules](xref:host-and-deploy/iis/modules#disabling-iis-modules).</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="baee1-288">문제 해결</span><span class="sxs-lookup"><span data-stu-id="baee1-288">Troubleshooting</span></span>

<span data-ttu-id="baee1-289">와 같은 도구를 사용 하 여 [Fiddler](https://www.telerik.com/fiddler)를 [Firebug](https://getfirebug.com/), 또는 [Postman](https://www.getpostman.com/)를 설정할 수 있도록는 `Accept-Encoding` 요청 헤더 및 응답 헤더, 크기 및 본문을 연구 합니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-289">Use a tool like [Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/), or [Postman](https://www.getpostman.com/), which allow you to set the `Accept-Encoding` request header and study the response headers, size, and body.</span></span> <span data-ttu-id="baee1-290">기본적으로 응답 압축 미들웨어는 다음 조건을 충족 하는 응답을 압축 합니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-290">By default, Response Compression Middleware compresses responses that meet the following conditions:</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="baee1-291">합니다 `Accept-Encoding` 헤더 값이 있는지 `br`, `gzip`, `*`를 설정 하는 사용자 지정 압축 공급자와 일치 하는 사용자 지정 인코딩 또는 합니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-291">The `Accept-Encoding` header is present with a value of `br`, `gzip`, `*`, or custom encoding that matches a custom compression provider that you've established.</span></span> <span data-ttu-id="baee1-292">값이 아니어야 `identity` 품질 값 또는 (qvalue, `q`) 0 (영)을 설정 합니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-292">The value must not be `identity` or have a quality value (qvalue, `q`) setting of 0 (zero).</span></span>
* <span data-ttu-id="baee1-293">MIME 형식 (`Content-Type`) 설정 해야 하며에 구성 된 MIME 형식과 일치 해야 합니다 <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>합니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-293">The MIME type (`Content-Type`) must be set and must match a MIME type configured on the <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.</span></span>
* <span data-ttu-id="baee1-294">요청을 포함 하지 않아야 합니다 `Content-Range` 헤더입니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-294">The request must not include the `Content-Range` header.</span></span>
* <span data-ttu-id="baee1-295">요청 응답 압축 미들웨어 옵션에서 보안 프로토콜 (https)를 구성 하지 않으면 안전 하지 않은 프로토콜 (http)를 사용 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-295">The request must use insecure protocol (http), unless secure protocol (https) is configured in the Response Compression Middleware options.</span></span> <span data-ttu-id="baee1-296">*위험 유의 [위에서 설명한](#compression-with-secure-protocol) 보안 콘텐츠 압축을 사용 하도록 설정할 때.*</span><span class="sxs-lookup"><span data-stu-id="baee1-296">*Note the danger [described above](#compression-with-secure-protocol) when enabling secure content compression.*</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="baee1-297">`Accept-Encoding` 헤더의 값을 사용 하 여 있는지 `gzip`, `*`를 설정 하는 사용자 지정 압축 공급자와 일치 하는 사용자 지정 인코딩 또는 합니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-297">The `Accept-Encoding` header is present with a value of `gzip`, `*`, or custom encoding that matches a custom compression provider that you've established.</span></span> <span data-ttu-id="baee1-298">값이 아니어야 `identity` 품질 값 또는 (qvalue, `q`) 0 (영)을 설정 합니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-298">The value must not be `identity` or have a quality value (qvalue, `q`) setting of 0 (zero).</span></span>
* <span data-ttu-id="baee1-299">MIME 형식 (`Content-Type`) 설정 해야 하며에 구성 된 MIME 형식과 일치 해야 합니다 <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>합니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-299">The MIME type (`Content-Type`) must be set and must match a MIME type configured on the <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.</span></span>
* <span data-ttu-id="baee1-300">요청을 포함 하지 않아야 합니다 `Content-Range` 헤더입니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-300">The request must not include the `Content-Range` header.</span></span>
* <span data-ttu-id="baee1-301">요청 응답 압축 미들웨어 옵션에서 보안 프로토콜 (https)를 구성 하지 않으면 안전 하지 않은 프로토콜 (http)를 사용 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-301">The request must use insecure protocol (http), unless secure protocol (https) is configured in the Response Compression Middleware options.</span></span> <span data-ttu-id="baee1-302">*위험 유의 [위에서 설명한](#compression-with-secure-protocol) 보안 콘텐츠 압축을 사용 하도록 설정할 때.*</span><span class="sxs-lookup"><span data-stu-id="baee1-302">*Note the danger [described above](#compression-with-secure-protocol) when enabling secure content compression.*</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="baee1-303">추가 자료</span><span class="sxs-lookup"><span data-stu-id="baee1-303">Additional resources</span></span>

* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/index>
* [<span data-ttu-id="baee1-304">Mozilla Developer Network: 허용 인코딩</span><span class="sxs-lookup"><span data-stu-id="baee1-304">Mozilla Developer Network: Accept-Encoding</span></span>](https://developer.mozilla.org/docs/Web/HTTP/Headers/Accept-Encoding)
* [<span data-ttu-id="baee1-305">RFC 7231 섹션 3.1.2.1: 콘텐츠 구분을 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="baee1-305">RFC 7231 Section 3.1.2.1: Content Codings</span></span>](https://tools.ietf.org/html/rfc7231#section-3.1.2.1)
* [<span data-ttu-id="baee1-306">RFC 7230 섹션 4.2.3: Gzip 코딩</span><span class="sxs-lookup"><span data-stu-id="baee1-306">RFC 7230 Section 4.2.3: Gzip Coding</span></span>](https://tools.ietf.org/html/rfc7230#section-4.2.3)
* [<span data-ttu-id="baee1-307">GZIP 파일 형식 사양 버전 4.3</span><span class="sxs-lookup"><span data-stu-id="baee1-307">GZIP file format specification version 4.3</span></span>](http://www.ietf.org/rfc/rfc1952.txt)
