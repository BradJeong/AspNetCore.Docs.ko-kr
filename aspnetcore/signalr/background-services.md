---
title: 백그라운드 서비스 SignalR 의 호스트 ASP.NET Core
author: bradygaster
description: .NET Core BackgroundService 클래스에서 클라이언트 SignalR 에 메시지를 보내는 방법에 대해 알아봅니다.
monikerRange: '>= aspnetcore-2.2'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: signalr/background-services
ms.openlocfilehash: d5f1668d601f520939956985e46c62f3a5bdfcfa
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/04/2020
ms.locfileid: "82777295"
---
# <a name="host-aspnet-core-signalr-in-background-services"></a><span data-ttu-id="4d940-103">백그라운드 서비스 SignalR 의 호스트 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4d940-103">Host ASP.NET Core SignalR in background services</span></span>

<span data-ttu-id="4d940-104">[Brady Gaster](https://twitter.com/bradygaster)</span><span class="sxs-lookup"><span data-stu-id="4d940-104">By [Brady Gaster](https://twitter.com/bradygaster)</span></span>

<span data-ttu-id="4d940-105">이 문서에서는 다음에 대 한 지침을 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="4d940-105">This article provides guidance for:</span></span>

* <span data-ttu-id="4d940-106">ASP.NET Core SignalR 로 호스트 된 백그라운드 작업자 프로세스를 사용 하 여 허브를 호스팅합니다.</span><span class="sxs-lookup"><span data-stu-id="4d940-106">Hosting SignalR Hubs using a background worker process hosted with ASP.NET Core.</span></span>
* <span data-ttu-id="4d940-107">.NET Core [BackgroundService](xref:Microsoft.Extensions.Hosting.BackgroundService)내에서 연결 된 클라이언트에 메시지를 보냅니다.</span><span class="sxs-lookup"><span data-stu-id="4d940-107">Sending messages to connected clients from within a .NET Core [BackgroundService](xref:Microsoft.Extensions.Hosting.BackgroundService).</span></span>

<span data-ttu-id="4d940-108">[샘플 코드 보기 또는 다운로드](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/background-service/sample/) [(다운로드 방법)](xref:index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="4d940-108">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/background-service/sample/) [(how to download)](xref:index#how-to-download-a-sample)</span></span>

## <a name="enable-signalr-in-startup"></a><span data-ttu-id="4d940-109">시작 SignalR 시 사용</span><span class="sxs-lookup"><span data-stu-id="4d940-109">Enable SignalR in startup</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="4d940-110">백그라운드 작업자 SignalR 프로세스의 컨텍스트에서 ASP.NET Core 허브를 호스트 하는 것은 ASP.NET Core 웹 앱에서 허브를 호스트 하는 것과 동일 합니다.</span><span class="sxs-lookup"><span data-stu-id="4d940-110">Hosting ASP.NET Core SignalR Hubs in the context of a background worker process is identical to hosting a Hub in an ASP.NET Core web app.</span></span> <span data-ttu-id="4d940-111">`Startup.ConfigureServices` 메서드에서는를 호출 `services.AddSignalR` 하 여 지원 SignalR되는 DI (ASP.NET Core 종속성 주입) 계층에 필요한 서비스를 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="4d940-111">In the `Startup.ConfigureServices` method, calling `services.AddSignalR` adds the required services to the ASP.NET Core Dependency Injection (DI) layer to support SignalR.</span></span> <span data-ttu-id="4d940-112">에서 `Startup.Configure`메서드는 `MapHub` ASP.NET Core 요청 파이프라인의 허브 `UseEndpoints` 끝점을 연결 하기 위해 콜백에서 호출 됩니다.</span><span class="sxs-lookup"><span data-stu-id="4d940-112">In `Startup.Configure`, the `MapHub` method is called in the `UseEndpoints` callback to connect the Hub endpoints in the ASP.NET Core request pipeline.</span></span>

```csharp
public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddSignalR();
        services.AddHostedService<Worker>();
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
        if (env.IsDevelopment())
        {
            app.UseDeveloperExceptionPage();
        }

        app.UseRouting();
        app.UseEndpoints(endpoints =>
        {
            endpoints.MapHub<ClockHub>("/hubs/clock");
        });
    }
}
```

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

<span data-ttu-id="4d940-113">백그라운드 작업자 SignalR 프로세스의 컨텍스트에서 ASP.NET Core 허브를 호스트 하는 것은 ASP.NET Core 웹 앱에서 허브를 호스트 하는 것과 동일 합니다.</span><span class="sxs-lookup"><span data-stu-id="4d940-113">Hosting ASP.NET Core SignalR Hubs in the context of a background worker process is identical to hosting a Hub in an ASP.NET Core web app.</span></span> <span data-ttu-id="4d940-114">`Startup.ConfigureServices` 메서드에서는를 호출 `services.AddSignalR` 하 여 지원 SignalR되는 DI (ASP.NET Core 종속성 주입) 계층에 필요한 서비스를 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="4d940-114">In the `Startup.ConfigureServices` method, calling `services.AddSignalR` adds the required services to the ASP.NET Core Dependency Injection (DI) layer to support SignalR.</span></span> <span data-ttu-id="4d940-115">에서 `Startup.Configure` `UseSignalR` 메서드를 호출 하 여 ASP.NET Core 요청 파이프라인의 허브 끝점을 연결 합니다.</span><span class="sxs-lookup"><span data-stu-id="4d940-115">In `Startup.Configure`, the `UseSignalR` method is called to connect the Hub endpoint(s) in the ASP.NET Core request pipeline.</span></span>

[!code-csharp[Startup](background-service/sample/Server/Startup.cs?name=Startup)]

::: moniker-end

<span data-ttu-id="4d940-116">앞의 예제에서 `ClockHub` 클래스는 `Hub<T>` 클래스를 구현 하 여 강력한 형식의 허브를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="4d940-116">In the preceding example, the `ClockHub` class implements the `Hub<T>` class to create a strongly typed Hub.</span></span> <span data-ttu-id="4d940-117">는 `ClockHub` 끝점 `/hubs/clock`에서 요청에 응답 `Startup` 하도록 클래스에서 구성 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="4d940-117">The `ClockHub` has been configured in the `Startup` class to respond to requests at the endpoint `/hubs/clock`.</span></span>

<span data-ttu-id="4d940-118">강력한 형식의 허브에 대 한 자세한 내용은 [ASP.NET Core SignalR 에 대해 허브 사용](xref:signalr/hubs#strongly-typed-hubs)을 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="4d940-118">For more information on strongly typed Hubs, see [Use hubs in SignalR for ASP.NET Core](xref:signalr/hubs#strongly-typed-hubs).</span></span>

> [!NOTE]
> <span data-ttu-id="4d940-119">이 기능은 [\<허브 t>](xref:Microsoft.AspNetCore.SignalR.Hub`1) 클래스로 제한 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="4d940-119">This functionality isn't limited to the [Hub\<T>](xref:Microsoft.AspNetCore.SignalR.Hub`1) class.</span></span> <span data-ttu-id="4d940-120">[Dynamichub](xref:Microsoft.AspNetCore.SignalR.DynamicHub)와 같은 [허브](xref:Microsoft.AspNetCore.SignalR.Hub)에서 상속 되는 모든 클래스도 작동 합니다.</span><span class="sxs-lookup"><span data-stu-id="4d940-120">Any class that inherits from [Hub](xref:Microsoft.AspNetCore.SignalR.Hub), such as [DynamicHub](xref:Microsoft.AspNetCore.SignalR.DynamicHub), will also work.</span></span>

[!code-csharp[Startup](background-service/sample/Server/ClockHub.cs?name=ClockHub)]

<span data-ttu-id="4d940-121">강력 하 게 형식화 `ClockHub` 된에서 사용 하는 `IClock` 인터페이스는 인터페이스입니다.</span><span class="sxs-lookup"><span data-stu-id="4d940-121">The interface used by the strongly typed `ClockHub` is the `IClock` interface.</span></span>

[!code-csharp[Startup](background-service/sample/HubServiceInterfaces/IClock.cs?name=IClock)]

## <a name="call-a-signalr-hub-from-a-background-service"></a><span data-ttu-id="4d940-122">백그라운드 서비스 SignalR 에서 허브 호출</span><span class="sxs-lookup"><span data-stu-id="4d940-122">Call a SignalR Hub from a background service</span></span>

<span data-ttu-id="4d940-123">시작 중에는 `Worker` `BackgroundService`를 사용 하 여 `AddHostedService`클래스를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4d940-123">During startup, the `Worker` class, a `BackgroundService`, is enabled using `AddHostedService`.</span></span>

```csharp
services.AddHostedService<Worker>();
```

<span data-ttu-id="4d940-124">는 SignalR `Startup` 단계 중에도 활성화 되므로 각 허브는 ASP.NET Core의 HTTP 요청 파이프라인에서 개별 끝점에 연결 됩니다. 각 허브는 서버 `IHubContext<T>` 에서로 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="4d940-124">Since SignalR is also enabled up during the `Startup` phase, in which each Hub is attached to an individual endpoint in ASP.NET Core's HTTP request pipeline, each Hub is represented by an `IHubContext<T>` on the server.</span></span> <span data-ttu-id="4d940-125">ASP.NET Core의 DI 기능을 사용 하 여 클래스, MVC 컨트롤러 클래스 또는 `BackgroundService` Razor 페이지 모델과 같이 호스팅 계층에 의해 인스턴스화된 다른 클래스는 생성 중의 `IHubContext<ClockHub, IClock>` 인스턴스를 허용 하 여 서버 쪽 허브에 대 한 참조를 가져올 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4d940-125">Using ASP.NET Core's DI features, other classes instantiated by the hosting layer, like `BackgroundService` classes, MVC Controller classes, or Razor page models, can get references to server-side Hubs by accepting instances of `IHubContext<ClockHub, IClock>` during construction.</span></span>

[!code-csharp[Startup](background-service/sample/Server/Worker.cs?name=Worker)]

<span data-ttu-id="4d940-126">`ExecuteAsync` 메서드가 백그라운드 서비스에서 반복적으로 호출 되 면 서버의 현재 날짜 및 시간이를 `ClockHub`사용 하 여 연결 된 클라이언트에 전송 됩니다.</span><span class="sxs-lookup"><span data-stu-id="4d940-126">As the `ExecuteAsync` method is called iteratively in the background service, the server's current date and time are sent to the connected clients using the `ClockHub`.</span></span>

## <a name="react-to-signalr-events-with-background-services"></a><span data-ttu-id="4d940-127">백그라운드 서비스 SignalR 를 사용 하 여 이벤트에 대응</span><span class="sxs-lookup"><span data-stu-id="4d940-127">React to SignalR events with background services</span></span>

<span data-ttu-id="4d940-128">SignalR 또는 .net 데스크톱 응용 프로그램에서 JavaScript 클라이언트를 사용 하는 단일 페이지 앱과 마찬가지로를 사용 하 여 <xref:signalr/dotnet-client>를 사용할 `BackgroundService` 수 `IHostedService` 있습니다. 또는 구현을 사용 하 여 허브 SignalR 에 연결 하 고 이벤트에 응답할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4d940-128">Like a Single Page App using the JavaScript client for SignalR or a .NET desktop app can do using the using the <xref:signalr/dotnet-client>, a `BackgroundService` or `IHostedService` implementation can also be used to connect to SignalR Hubs and respond to events.</span></span>

<span data-ttu-id="4d940-129">클래스 `ClockHubClient` 는 `IClock` 인터페이스와 `IHostedService` 인터페이스를 둘 다 구현 합니다.</span><span class="sxs-lookup"><span data-stu-id="4d940-129">The `ClockHubClient` class implements both the `IClock` interface and the `IHostedService` interface.</span></span> <span data-ttu-id="4d940-130">이러한 방식으로에서 계속 실행 하 `Startup` 고 서버에서 허브 이벤트에 응답 하는 데 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4d940-130">This way it can be enabled during `Startup` to run continuously and respond to Hub events from the server.</span></span>

```csharp
public partial class ClockHubClient : IClock, IHostedService
{
}
```

<span data-ttu-id="4d940-131">초기화 중에는 `ClockHubClient` 의 `HubConnection` 인스턴스를 만들고 메서드를 `IClock.ShowTime` 허브 `ShowTime` 이벤트에 대 한 처리기로 사용 하도록 설정 합니다.</span><span class="sxs-lookup"><span data-stu-id="4d940-131">During initialization, the `ClockHubClient` creates an instance of a `HubConnection` and enables the `IClock.ShowTime` method as the handler for the Hub's `ShowTime` event.</span></span>

[!code-csharp[The ClockHubClient constructor](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=ClockHubClientCtor)]

<span data-ttu-id="4d940-132">`IHostedService.StartAsync` 구현에서 `HubConnection` 가 비동기적으로 시작 됩니다.</span><span class="sxs-lookup"><span data-stu-id="4d940-132">In the `IHostedService.StartAsync` implementation, the `HubConnection` is started asynchronously.</span></span>

[!code-csharp[StartAsync method](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=StartAsync)]

<span data-ttu-id="4d940-133">`IHostedService.StopAsync` 메서드를 실행 하는 `HubConnection` 동안은 비동기적으로 삭제 됩니다.</span><span class="sxs-lookup"><span data-stu-id="4d940-133">During the `IHostedService.StopAsync` method, the `HubConnection` is disposed of asynchronously.</span></span>

[!code-csharp[StopAsync method](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=StopAsync)]

## <a name="additional-resources"></a><span data-ttu-id="4d940-134">추가 자료</span><span class="sxs-lookup"><span data-stu-id="4d940-134">Additional resources</span></span>

* [<span data-ttu-id="4d940-135">시작</span><span class="sxs-lookup"><span data-stu-id="4d940-135">Get started</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="4d940-136">허브</span><span class="sxs-lookup"><span data-stu-id="4d940-136">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="4d940-137">Azure에 게시</span><span class="sxs-lookup"><span data-stu-id="4d940-137">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
* [<span data-ttu-id="4d940-138">강력한 형식의 허브</span><span class="sxs-lookup"><span data-stu-id="4d940-138">Strongly typed Hubs</span></span>](xref:signalr/hubs#strongly-typed-hubs)
