---
title: .NET 클라이언트를 사용하여 gRPC 서비스 호출
author: jamesnk
description: .NET gRPC 클라이언트를 사용하여 gRPC 서비스를 호출하는 방법을 알아봅니다.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 04/21/2020
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: grpc/client
ms.openlocfilehash: 9ebe36cdb17e858fd82216b090e3e89169197101
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/26/2020
ms.locfileid: "85406188"
---
# <a name="call-grpc-services-with-the-net-client"></a><span data-ttu-id="cc036-103">.NET 클라이언트를 사용하여 gRPC 서비스 호출</span><span class="sxs-lookup"><span data-stu-id="cc036-103">Call gRPC services with the .NET client</span></span>

<span data-ttu-id="cc036-104">.NET gRPC 클라이언트 라이브러리는 [Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client) NuGet 패키지에서 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="cc036-104">A .NET gRPC client library is available in the [Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client) NuGet package.</span></span> <span data-ttu-id="cc036-105">이 문서에서는 다음을 수행하는 방법을 설명합니다.</span><span class="sxs-lookup"><span data-stu-id="cc036-105">This document explains how to:</span></span>

* <span data-ttu-id="cc036-106">gRPC 서비스를 호출하도록 gRPC 클라이언트를 구성합니다.</span><span class="sxs-lookup"><span data-stu-id="cc036-106">Configure a gRPC client to call gRPC services.</span></span>
* <span data-ttu-id="cc036-107">단항, 서버 스트리밍, 클라이언트 스트리밍 및 양방향 스트리밍 메서드에 대한 gRPC 호출을 수행합니다.</span><span class="sxs-lookup"><span data-stu-id="cc036-107">Make gRPC calls to unary, server streaming, client streaming, and bi-directional streaming methods.</span></span>

## <a name="configure-grpc-client"></a><span data-ttu-id="cc036-108">gRPC 클라이언트 구성</span><span class="sxs-lookup"><span data-stu-id="cc036-108">Configure gRPC client</span></span>

<span data-ttu-id="cc036-109">gRPC 클라이언트는 [ *\*.proto* 파일에서 생성](xref:grpc/basics#generated-c-assets)되는 구체적인 클라이언트 형식입니다.</span><span class="sxs-lookup"><span data-stu-id="cc036-109">gRPC clients are concrete client types that are [generated from *\*.proto* files](xref:grpc/basics#generated-c-assets).</span></span> <span data-ttu-id="cc036-110">구체적인 gRPC 클라이언트에는 *\*.proto* 파일에서 gRPC 서비스로 변환되는 메서드가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="cc036-110">The concrete gRPC client has methods that translate to the gRPC service in the *\*.proto* file.</span></span>

<span data-ttu-id="cc036-111">채널에서 gRPC 클라이언트를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="cc036-111">A gRPC client is created from a channel.</span></span> <span data-ttu-id="cc036-112">먼저 `GrpcChannel.ForAddress`를 사용하여 채널을 만든 다음, 채널을 사용하여 gRPC 클라이언트를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="cc036-112">Start by using `GrpcChannel.ForAddress` to create a channel, and then use the channel to create a gRPC client:</span></span>

```csharp
var channel = GrpcChannel.ForAddress("https://localhost:5001");
var client = new Greet.GreeterClient(channel);
```

<span data-ttu-id="cc036-113">채널은 gRPC 서비스에 대한 장기 연결을 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="cc036-113">A channel represents a long-lived connection to a gRPC service.</span></span> <span data-ttu-id="cc036-114">채널이 생성되면 서비스 호출과 관련된 옵션을 사용하여 구성합니다.</span><span class="sxs-lookup"><span data-stu-id="cc036-114">When a channel is created, it is configured with options related to calling a service.</span></span> <span data-ttu-id="cc036-115">예를 들어 호출, 최대 전송 및 수신 메시지 크기, 로깅을 수행하는 데 사용되는 `HttpClient`를 `GrpcChannelOptions`에서 지정하고 `GrpcChannel.ForAddress`와 함께 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="cc036-115">For example, the `HttpClient` used to make calls, the maximum send and receive message size, and logging can be specified on `GrpcChannelOptions` and used with `GrpcChannel.ForAddress`.</span></span> <span data-ttu-id="cc036-116">전체 옵션 목록은 [클라이언트 구성 옵션](xref:grpc/configuration#configure-client-options)을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="cc036-116">For a complete list of options, see [client configuration options](xref:grpc/configuration#configure-client-options).</span></span>

```csharp
var channel = GrpcChannel.ForAddress("https://localhost:5001");

var greeterClient = new Greet.GreeterClient(channel);
var counterClient = new Count.CounterClient(channel);

// Use clients to call gRPC services
```

<span data-ttu-id="cc036-117">채널 및 클라이언트 성능과 사용법:</span><span class="sxs-lookup"><span data-stu-id="cc036-117">Channel and client performance and usage:</span></span>

* <span data-ttu-id="cc036-118">채널을 만드는 작업은 비용이 많이 들 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="cc036-118">Creating a channel can be an expensive operation.</span></span> <span data-ttu-id="cc036-119">gRPC 호출에 채널을 재사용하면 성능 혜택이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="cc036-119">Reusing a channel for gRPC calls provides performance benefits.</span></span>
* <span data-ttu-id="cc036-120">gRPC 클라이언트는 채널을 사용하여 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="cc036-120">gRPC clients are created with channels.</span></span> <span data-ttu-id="cc036-121">gRPC 클라이언트는 경량 개체이며 캐시하거나 재사용할 필요가 없습니다.</span><span class="sxs-lookup"><span data-stu-id="cc036-121">gRPC clients are lightweight objects and don't need to be cached or reused.</span></span>
* <span data-ttu-id="cc036-122">다양한 형식의 클라이언트를 포함하여 여러 gRPC 클라이언트를 한 채널에서 만들 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="cc036-122">Multiple gRPC clients can be created from a channel, including different types of clients.</span></span>
* <span data-ttu-id="cc036-123">채널 및 채널에서 만든 클라이언트를 여러 스레드에서 안전하게 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="cc036-123">A channel and clients created from the channel can safely be used by multiple threads.</span></span>
* <span data-ttu-id="cc036-124">채널에서 만든 클라이언트는 여러 개의 동시 호출을 수행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="cc036-124">Clients created from the channel can make multiple simultaneous calls.</span></span>

<span data-ttu-id="cc036-125">`GrpcChannel.ForAddress`가 gRPC 클라이언트를 만들기 위한 유일한 옵션은 아닙니다.</span><span class="sxs-lookup"><span data-stu-id="cc036-125">`GrpcChannel.ForAddress` isn't the only option for creating a gRPC client.</span></span> <span data-ttu-id="cc036-126">ASP.NET Core 앱에서 gRPC 서비스를 호출하는 경우 [gRPC 클라이언트 팩터리 통합](xref:grpc/clientfactory)을 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="cc036-126">If calling gRPC services from an ASP.NET Core app, consider [gRPC client factory integration](xref:grpc/clientfactory).</span></span> <span data-ttu-id="cc036-127">`HttpClientFactory`와 gRPC 통합은 중앙에서 gRPC 클라이언트를 만드는 대체 방법을 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="cc036-127">gRPC integration with `HttpClientFactory` offers a centralized alternative to creating gRPC clients.</span></span>

> [!NOTE]
> <span data-ttu-id="cc036-128">[.NET 클라이언트를 사용하여 안전하지 않은 gRPC 서비스를 호출](xref:grpc/troubleshoot#call-insecure-grpc-services-with-net-core-client)하려면 추가 구성이 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="cc036-128">Additional configuration is required to [call insecure gRPC services with the .NET client](xref:grpc/troubleshoot#call-insecure-grpc-services-with-net-core-client).</span></span>

> [!NOTE]
> <span data-ttu-id="cc036-129">`Grpc.Net.Client`를 사용하여 HTTP/2를 통해 gRPC를 호출하는 기능은 현재 Xamarin에서 지원되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="cc036-129">Calling gRPC over HTTP/2 with `Grpc.Net.Client` is currently not supported on Xamarin.</span></span> <span data-ttu-id="cc036-130">향후 Xamarin 릴리스에서 HTTP/2 지원을 개선하기 위해 노력하고 있습니다.</span><span class="sxs-lookup"><span data-stu-id="cc036-130">We are working to improve HTTP/2 support in a future Xamarin release.</span></span> <span data-ttu-id="cc036-131">현재 실행 가능한 대체 방법은 [Grpc.Core](https://www.nuget.org/packages/Grpc.Core) 및 [gRPC-Web](xref:grpc/browser)입니다.</span><span class="sxs-lookup"><span data-stu-id="cc036-131">[Grpc.Core](https://www.nuget.org/packages/Grpc.Core) and [gRPC-Web](xref:grpc/browser) are viable alternatives that work today.</span></span>

## <a name="make-grpc-calls"></a><span data-ttu-id="cc036-132">gRPC 호출 수행</span><span class="sxs-lookup"><span data-stu-id="cc036-132">Make gRPC calls</span></span>

<span data-ttu-id="cc036-133">클라이언트에서 메서드를 호출하여 gRPC 호출을 시작합니다.</span><span class="sxs-lookup"><span data-stu-id="cc036-133">A gRPC call is initiated by calling a method on the client.</span></span> <span data-ttu-id="cc036-134">gRPC 클라이언트는 메시지 직렬화 및 gRPC 호출에 올바른 서비스 주소를 지정하는 작업을 처리합니다.</span><span class="sxs-lookup"><span data-stu-id="cc036-134">The gRPC client will handle message serialization and addressing the gRPC call to the correct service.</span></span>

<span data-ttu-id="cc036-135">gRPC에는 다양한 형식의 메서드가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="cc036-135">gRPC has different types of methods.</span></span> <span data-ttu-id="cc036-136">클라이언트를 사용하여 gRPC 호출을 수행하는 방법은 호출되는 메서드 형식에 따라 다릅니다.</span><span class="sxs-lookup"><span data-stu-id="cc036-136">How the client is used to make a gRPC call depends on the type of method called.</span></span> <span data-ttu-id="cc036-137">gRPC 메서드 형식은 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="cc036-137">The gRPC method types are:</span></span>

* <span data-ttu-id="cc036-138">단항</span><span class="sxs-lookup"><span data-stu-id="cc036-138">Unary</span></span>
* <span data-ttu-id="cc036-139">서버 스트리밍</span><span class="sxs-lookup"><span data-stu-id="cc036-139">Server streaming</span></span>
* <span data-ttu-id="cc036-140">클라이언트 스트리밍</span><span class="sxs-lookup"><span data-stu-id="cc036-140">Client streaming</span></span>
* <span data-ttu-id="cc036-141">양방향 스트리밍</span><span class="sxs-lookup"><span data-stu-id="cc036-141">Bi-directional streaming</span></span>

### <a name="unary-call"></a><span data-ttu-id="cc036-142">단항 호출</span><span class="sxs-lookup"><span data-stu-id="cc036-142">Unary call</span></span>

<span data-ttu-id="cc036-143">단항 호출은 클라이언트에서 요청 메시지를 보내는 것으로 시작합니다.</span><span class="sxs-lookup"><span data-stu-id="cc036-143">A unary call starts with the client sending a request message.</span></span> <span data-ttu-id="cc036-144">서비스가 완료되면 응답 메시지가 반환됩니다.</span><span class="sxs-lookup"><span data-stu-id="cc036-144">A response message is returned when the service finishes.</span></span>

```csharp
var client = new Greet.GreeterClient(channel);
var response = await client.SayHelloAsync(new HelloRequest { Name = "World" });

Console.WriteLine("Greeting: " + response.Message);
// Greeting: Hello World
```

<span data-ttu-id="cc036-145">\* *.proto* 파일에 있는 각 단항 서비스 메서드는 구체적인 gRPC 클라이언트 형식에 해당 메서드를 호출하기 위한 두 가지 .NET 메서드인 비동기 메서드와 차단 메서드를 생성합니다.</span><span class="sxs-lookup"><span data-stu-id="cc036-145">Each unary service method in the *\*.proto* file will result in two .NET methods on the concrete gRPC client type for calling the method: an asynchronous method and a blocking method.</span></span> <span data-ttu-id="cc036-146">예를 들어 `GreeterClient`에서 `SayHello`를 호출하는 방법에는 다음 두 가지가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="cc036-146">For example, on `GreeterClient` there are two ways of calling `SayHello`:</span></span>

* <span data-ttu-id="cc036-147">`GreeterClient.SayHelloAsync` - `Greeter.SayHello` 서비스를 비동기적으로 호출합니다.</span><span class="sxs-lookup"><span data-stu-id="cc036-147">`GreeterClient.SayHelloAsync` - calls `Greeter.SayHello` service asynchronously.</span></span> <span data-ttu-id="cc036-148">대기할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="cc036-148">Can be awaited.</span></span>
* <span data-ttu-id="cc036-149">`GreeterClient.SayHello` - `Greeter.SayHello` 서비스를 호출하고 완료될 때까지 차단합니다.</span><span class="sxs-lookup"><span data-stu-id="cc036-149">`GreeterClient.SayHello` - calls `Greeter.SayHello` service and blocks until complete.</span></span> <span data-ttu-id="cc036-150">비동기 코드에서는 사용하면 안 됩니다.</span><span class="sxs-lookup"><span data-stu-id="cc036-150">Don't use in asynchronous code.</span></span>

### <a name="server-streaming-call"></a><span data-ttu-id="cc036-151">서버 스트리밍 호출</span><span class="sxs-lookup"><span data-stu-id="cc036-151">Server streaming call</span></span>

<span data-ttu-id="cc036-152">서버 스트리밍 호출은 클라이언트에서 요청 메시지를 보내는 것으로 시작됩니다.</span><span class="sxs-lookup"><span data-stu-id="cc036-152">A server streaming call starts with the client sending a request message.</span></span> <span data-ttu-id="cc036-153">`ResponseStream.MoveNext()`는 서비스에서 스트리밍된 메시지를 읽습니다.</span><span class="sxs-lookup"><span data-stu-id="cc036-153">`ResponseStream.MoveNext()` reads messages streamed from the service.</span></span> <span data-ttu-id="cc036-154">`ResponseStream.MoveNext()`에서 `false`를 반환하면 서버 스트리밍 호출이 완료됩니다.</span><span class="sxs-lookup"><span data-stu-id="cc036-154">The server streaming call is complete when `ResponseStream.MoveNext()` returns `false`.</span></span>

```csharp
var client = new Greet.GreeterClient(channel);
using var call = client.SayHellos(new HelloRequest { Name = "World" });

while (await call.ResponseStream.MoveNext())
{
    Console.WriteLine("Greeting: " + call.ResponseStream.Current.Message);
    // "Greeting: Hello World" is written multiple times
}
```

<span data-ttu-id="cc036-155">C# 8 이상을 사용하는 경우 `await foreach` 구문을 사용하여 메시지를 읽을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="cc036-155">When using C# 8 or later, the `await foreach` syntax can be used to read messages.</span></span> <span data-ttu-id="cc036-156">`IAsyncStreamReader<T>.ReadAllAsync()` 확장 메서드는 응답 스트림에서 모든 메시지를 읽습니다.</span><span class="sxs-lookup"><span data-stu-id="cc036-156">The `IAsyncStreamReader<T>.ReadAllAsync()` extension method reads all messages from the response stream:</span></span>

```csharp
var client = new Greet.GreeterClient(channel);
using var call = client.SayHellos(new HelloRequest { Name = "World" });

await foreach (var response in call.ResponseStream.ReadAllAsync())
{
    Console.WriteLine("Greeting: " + response.Message);
    // "Greeting: Hello World" is written multiple times
}
```

### <a name="client-streaming-call"></a><span data-ttu-id="cc036-157">클라이언트 스트리밍 호출</span><span class="sxs-lookup"><span data-stu-id="cc036-157">Client streaming call</span></span>

<span data-ttu-id="cc036-158">클라이언트 스트리밍 호출은 클라이언트에서 메시지를 보내지 ‘않고’ 시작됩니다. </span><span class="sxs-lookup"><span data-stu-id="cc036-158">A client streaming call starts *without* the client sending a message.</span></span> <span data-ttu-id="cc036-159">클라이언트는 `RequestStream.WriteAsync`를 사용하여 메시지를 보내도록 선택할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="cc036-159">The client can choose to send messages with `RequestStream.WriteAsync`.</span></span> <span data-ttu-id="cc036-160">클라이언트가 메시지 전송을 완료하면 서비스에 알리기 위해 `RequestStream.CompleteAsync`를 호출해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="cc036-160">When the client has finished sending messages, `RequestStream.CompleteAsync` should be called to notify the service.</span></span> <span data-ttu-id="cc036-161">서비스에서 응답 메시지를 반환하면 호출이 완료됩니다.</span><span class="sxs-lookup"><span data-stu-id="cc036-161">The call is finished when the service returns a response message.</span></span>

```csharp
var client = new Counter.CounterClient(channel);
using var call = client.AccumulateCount();

for (var i = 0; i < 3; i++)
{
    await call.RequestStream.WriteAsync(new CounterRequest { Count = 1 });
}
await call.RequestStream.CompleteAsync();

var response = await call;
Console.WriteLine($"Count: {response.Count}");
// Count: 3
```

### <a name="bi-directional-streaming-call"></a><span data-ttu-id="cc036-162">양방향 스트리밍 호출</span><span class="sxs-lookup"><span data-stu-id="cc036-162">Bi-directional streaming call</span></span>

<span data-ttu-id="cc036-163">양방향 스트리밍 호출은 클라이언트에서 메시지를 보내지 ‘않고’ 시작됩니다. </span><span class="sxs-lookup"><span data-stu-id="cc036-163">A bi-directional streaming call starts *without* the client sending a message.</span></span> <span data-ttu-id="cc036-164">클라이언트는 `RequestStream.WriteAsync`를 사용하여 메시지를 보내도록 선택할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="cc036-164">The client can choose to send messages with `RequestStream.WriteAsync`.</span></span> <span data-ttu-id="cc036-165">서비스에서 스트리밍된 메시지는 `ResponseStream.MoveNext()` 또는 `ResponseStream.ReadAllAsync()`를 사용하여 액세스할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="cc036-165">Messages streamed from the service are accessible with `ResponseStream.MoveNext()` or `ResponseStream.ReadAllAsync()`.</span></span> <span data-ttu-id="cc036-166">`ResponseStream`에 더는 메시지가 없으면 양방향 스트리밍 호출이 완료됩니다.</span><span class="sxs-lookup"><span data-stu-id="cc036-166">The bi-directional streaming call is complete when the `ResponseStream` has no more messages.</span></span>

```csharp
var client = new Echo.EchoClient(channel);
using var call = client.Echo();

Console.WriteLine("Starting background task to receive messages");
var readTask = Task.Run(async () =>
{
    await foreach (var response in call.ResponseStream.ReadAllAsync())
    {
        Console.WriteLine(response.Message);
        // Echo messages sent to the service
    }
});

Console.WriteLine("Starting to send messages");
Console.WriteLine("Type a message to echo then press enter.");
while (true)
{
    var result = Console.ReadLine();
    if (string.IsNullOrEmpty(result))
    {
        break;
    }

    await call.RequestStream.WriteAsync(new EchoMessage { Message = result });
}

Console.WriteLine("Disconnecting");
await call.RequestStream.CompleteAsync();
await readTask;
```

<span data-ttu-id="cc036-167">양방향 스트리밍 호출 중에 클라이언트와 서비스는 언제든지 서로에게 메시지를 보낼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="cc036-167">During a bi-directional streaming call, the client and service can send messages to each other at any time.</span></span> <span data-ttu-id="cc036-168">양방향 호출과 상호 작용하는 데 가장 적합한 클라이언트 논리는 서비스 논리에 따라 다릅니다.</span><span class="sxs-lookup"><span data-stu-id="cc036-168">The best client logic for interacting with a bi-directional call varies depending upon the service logic.</span></span>

## <a name="access-grpc-trailers"></a><span data-ttu-id="cc036-169">gRPC 트레일러 액세스</span><span class="sxs-lookup"><span data-stu-id="cc036-169">Access gRPC trailers</span></span>

<span data-ttu-id="cc036-170">gRPC 호출은 gRPC 트레일러를 반환할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="cc036-170">gRPC calls may return gRPC trailers.</span></span> <span data-ttu-id="cc036-171">gRPC 트레일러는 호출에 대한 이름/값 메타데이터를 제공하는 데 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="cc036-171">gRPC trailers are used to provide name/value metadata about a call.</span></span> <span data-ttu-id="cc036-172">트레일러는 HTTP 헤더와 유사한 기능을 제공하지만 호출이 끝날 때 수신됩니다.</span><span class="sxs-lookup"><span data-stu-id="cc036-172">Trailers provide similar functionality to HTTP headers, but are received at the end of the call.</span></span>

<span data-ttu-id="cc036-173">gRPC 트레일러는 메타데이터의 컬렉션을 반환하는 `GetTrailers()`를 사용하여 액세스할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="cc036-173">gRPC trailers are accessible using `GetTrailers()`, which returns a collection of metadata.</span></span> <span data-ttu-id="cc036-174">응답이 완료된 후 트레일러가 반환되므로 모든 응답 메시지를 대기한 이후에 트레일러에 액세스해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="cc036-174">Trailers are returned after the response is complete, therefore, you must await all response messages before accessing the trailers.</span></span>

<span data-ttu-id="cc036-175">단항 및 클라이언트 스트리밍 호출은 `GetTrailers()`를 호출하기 전에 `ResponseAsync`를 대기해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="cc036-175">Unary and client streaming calls must await `ResponseAsync` before calling `GetTrailers()`:</span></span>

```csharp
var client = new Greet.GreeterClient(channel);
using var call = client.SayHelloAsync(new HelloRequest { Name = "World" });
var response = await call.ResponseAsync;

Console.WriteLine("Greeting: " + response.Message);
// Greeting: Hello World

var trailers = call.GetTrailers();
var myValue = trailers.First(e => e.Key == "my-trailer-name");
```

<span data-ttu-id="cc036-176">서버 및 양방향 스트리밍 호출은 `GetTrailers()`를 호출하기 전에 응답 스트림 대기를 완료해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="cc036-176">Server and bidirectional streaming calls must finish awaiting the response stream before calling `GetTrailers()`:</span></span>

```csharp
var client = new Greet.GreeterClient(channel);
using var call = client.SayHellos(new HelloRequest { Name = "World" });

await foreach (var response in call.ResponseStream.ReadAllAsync())
{
    Console.WriteLine("Greeting: " + response.Message);
    // "Greeting: Hello World" is written multiple times
}

var trailers = call.GetTrailers();
var myValue = trailers.First(e => e.Key == "my-trailer-name");
```

<span data-ttu-id="cc036-177">`RpcException`에서도 gRPC 트레일러에 액세스할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="cc036-177">gRPC trailers are also accessible from `RpcException`.</span></span> <span data-ttu-id="cc036-178">서비스에서 비정상 gRPC 상태와 함께 트레일러를 반환할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="cc036-178">A service may return trailers together with a non-OK gRPC status.</span></span> <span data-ttu-id="cc036-179">이 경우에는 gRPC 클라이언트에서 throw된 예외에서 트레일러가 검색됩니다.</span><span class="sxs-lookup"><span data-stu-id="cc036-179">In this situation the trailers are retrieved from the exception thrown by the gRPC client:</span></span>

```csharp
var client = new Greet.GreeterClient(channel);
string myValue = null;

try
{
    using var call = client.SayHelloAsync(new HelloRequest { Name = "World" });
    var response = await call.ResponseAsync;

    Console.WriteLine("Greeting: " + response.Message);
    // Greeting: Hello World

    var trailers = call.GetTrailers();
    myValue = trailers.First(e => e.Key == "my-trailer-name");
}
catch (RpcException ex)
{
    var trailers = ex.Trailers;
    myValue = trailers.First(e => e.Key == "my-trailer-name");
}
```

## <a name="additional-resources"></a><span data-ttu-id="cc036-180">추가 자료</span><span class="sxs-lookup"><span data-stu-id="cc036-180">Additional resources</span></span>

* <xref:grpc/clientfactory>
* <xref:grpc/basics>
