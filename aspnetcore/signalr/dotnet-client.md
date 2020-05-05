---
title: ASP.NET Core SignalR .net 클라이언트
author: bradygaster
description: ASP.NET Core SignalR .net 클라이언트에 대 한 정보
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 01/14/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: signalr/dotnet-client
ms.openlocfilehash: 77d7eb81abc4ec7a6f4f15bbe5d96cedc64cb330
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/04/2020
ms.locfileid: "82767215"
---
# <a name="aspnet-core-signalr-net-client"></a>ASP.NET Core SignalR .NET 클라이언트

ASP.NET Core SignalR .NET 클라이언트 라이브러리를 사용 하면 .NET 앱에서 SignalR 허브와 통신할 수 있습니다.

[예제 코드 살펴보기 및 다운로드](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/dotnet-client/sample) ([다운로드 방법](xref:index#how-to-download-a-sample))

이 문서의 코드 샘플은 ASP.NET Core SignalR .NET 클라이언트를 사용 하는 WPF 앱입니다.

## <a name="install-the-signalr-net-client-package"></a>SignalR .NET 클라이언트 패키지를 설치 합니다.

[AspNetCore SignalR](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR.Client) 패키지는 .Net 클라이언트가 SignalR hubs에 연결 하는 데 필요 합니다.

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

클라이언트 라이브러리를 설치 하려면 **패키지 관리자 콘솔** 창에서 다음 명령을 실행 합니다.

```powershell
Install-Package Microsoft.AspNetCore.SignalR.Client
```

# <a name="net-core-cli"></a>[.NET Core CLI](#tab/netcore-cli)

클라이언트 라이브러리를 설치 하려면 명령 셸에서 다음 명령을 실행 합니다.

```dotnetcli
dotnet add package Microsoft.AspNetCore.SignalR.Client
```

---

## <a name="connect-to-a-hub"></a>허브에 연결

연결을 설정 하려면를 `HubConnectionBuilder` 만들고를 호출 `Build`합니다. 연결을 빌드하는 동안 허브 URL, 프로토콜, 전송 유형, 로그 수준, 헤더 및 기타 옵션을 구성할 수 있습니다. 모든 `HubConnectionBuilder` 메서드를에 `Build`삽입 하 여 필요한 옵션을 구성 합니다. 와 `StartAsync`의 연결을 시작 합니다.

[!code-csharp[Build hub connection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_MainWindowClass&highlight=15-17,39)]

## <a name="handle-lost-connection"></a>손실 된 연결 처리

::: moniker range=">= aspnetcore-3.0"

### <a name="automatically-reconnect"></a>자동으로 다시 연결

는 <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection> 의 메서드를 `WithAutomaticReconnect` 사용 하 여 자동으로 다시 연결 되도록 <xref:Microsoft.AspNetCore.SignalR.Client.HubConnectionBuilder>구성할 수 있습니다. 기본적으로 자동으로 다시 연결 되지 않습니다.

```csharp
HubConnection connection= new HubConnectionBuilder()
    .WithUrl(new Uri("http://127.0.0.1:5000/chatHub"))
    .WithAutomaticReconnect()
    .Build();
```

매개 변수를 `WithAutomaticReconnect()` 사용 하지 않으면는 각 다시 연결 시도를 시도 하기 전에 0, 2, 10 및 30 초 동안 대기 하도록 클라이언트를 구성 하 고, 실패 한 네 번 시도 하면 중지 합니다.

다시 연결 시도를 시작 하기 전에 `HubConnection` 는 `HubConnectionState.Reconnecting` 상태로 전환 되 고 이벤트를 `Reconnecting` 발생 시킵니다.  이렇게 하면 연결이 손실 되었음을 사용자에 게 경고 하 고 UI 요소를 사용 하지 않도록 설정할 수 있습니다. 비 대화형 앱은 메시지 큐를 시작 하거나 삭제할 수 있습니다.

```csharp
connection.Reconnecting += error =>
{
    Debug.Assert(connection.State == HubConnectionState.Reconnecting);

    // Notify users the connection was lost and the client is reconnecting.
    // Start queuing or dropping messages.

    return Task.CompletedTask;
};
```

클라이언트가 처음 네 번의 시도 내에서 다시 연결 하는 `HubConnection` 경우는 `Connected` 상태로 다시 전환 되 고 이벤트를 `Reconnected` 발생 시킵니다. 이렇게 하면 연결을 다시 설정 하 고 대기 중인 모든 메시지를 큐에서 제거 하는 사용자에 게 알릴 수 있습니다.

연결이 서버에서 완전히 새로운 형태 이므로 `ConnectionId` `Reconnected` 이벤트 처리기에 새가 제공 됩니다.

> [!WARNING]
> 가 `Reconnected` [협상을 건너뛰도록](xref:signalr/configuration#configure-client-options)구성 된 경우 `connectionId` `HubConnection` 이벤트 처리기의 매개 변수는 null이 됩니다.

```csharp
connection.Reconnected += connectionId =>
{
    Debug.Assert(connection.State == HubConnectionState.Connected);

    // Notify users the connection was reestablished.
    // Start dequeuing messages queued while reconnecting if any.

    return Task.CompletedTask;
};
```

`WithAutomaticReconnect()`초기 시작 오류 `HubConnection` 를 다시 시도 하도록를 구성 하지 않으므로 시작 실패를 수동으로 처리 해야 합니다.

```csharp
public static async Task<bool> ConnectWithRetryAsync(HubConnection connection, CancellationToken token)
{
    // Keep trying to until we can start or the token is canceled.
    while (true)
    {
        try
        {
            await connection.StartAsync(token);
            Debug.Assert(connection.State == HubConnectionState.Connected);
            return true;
        }
        catch when (token.IsCancellationRequested)
        {
            return false;
        }
        catch
        {
            // Failed to connect, trying again in 5000 ms.
            Debug.Assert(connection.State == HubConnectionState.Disconnected);
            await Task.Delay(5000);
        }
    }
}
```

클라이언트가 처음 네 번의 시도 내에서 성공적으로 다시 연결 되지 `HubConnection` 않으면이 `Disconnected` 상태로 전환 되 고 이벤트를 <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed> 발생 시킵니다. 이를 통해 연결을 수동으로 다시 시작 하거나 연결이 영구적으로 손실 되었음을 사용자에 게 알릴 수 있습니다.

```csharp
connection.Closed += error =>
{
    Debug.Assert(connection.State == HubConnectionState.Disconnected);

    // Notify users the connection has been closed or manually try to restart the connection.

    return Task.CompletedTask;
};
```

연결을 끊거나 다시 연결 하는 시간을 변경 하기 전에 사용자가 다시 연결 시도 횟수 `WithAutomaticReconnect` 를 구성 하기 위해는 각 다시 연결 시도를 시작 하기 전에 대기할 지연 시간 (밀리초)을 나타내는 숫자 배열을 허용 합니다.

```csharp
HubConnection connection= new HubConnectionBuilder()
    .WithUrl(new Uri("http://127.0.0.1:5000/chatHub"))
    .WithAutomaticReconnect(new[] { TimeSpan.Zero, TimeSpan.Zero, TimeSpan.FromSeconds(10) })
    .Build();

    // .WithAutomaticReconnect(new[] { TimeSpan.Zero, TimeSpan.FromSeconds(2), TimeSpan.FromSeconds(10), TimeSpan.FromSeconds(30) }) yields the default behavior.
```

앞의 예제에서는 연결이 `HubConnection` 끊긴 후 즉시 다시 연결 시도를 시작 하도록를 구성 합니다. 기본 구성의 경우에도 마찬가지입니다.

첫 번째 다시 연결 시도가 실패 하면 두 번째 다시 연결 시도도 기본 구성에서와 같이 2 초 동안 대기 하는 대신 즉시 시작 됩니다.

두 번째 다시 연결 시도가 실패 하면 세 번째 다시 연결 시도가 10 초 후에 다시 시작 됩니다 .이는 기본 구성 처럼 다시 시작 됩니다.

그런 다음 사용자 지정 동작은 세 번째 다시 연결 시도가 실패 한 후 중지 하 여 기본 동작에서 다시 달라 지므로 합니다. 기본 구성에서는 30 초 내에 다시 한 번 더 다시 연결 하려고 시도 합니다.

자동 다시 연결 시도의 타이밍과 수를 더 많이 제어 하려는 경우는 라는 `WithAutomaticReconnect` `IRetryPolicy` `NextRetryDelay`단일 메서드가 있는 인터페이스를 구현 하는 개체를 허용 합니다.

`NextRetryDelay`는 형식의 `RetryContext`단일 인수를 사용 합니다. `RetryContext` `PreviousRetryCount`에는, `ElapsedTime` 및 `RetryReason`의 세 가지 속성이 있습니다 .이 `long`속성은 `TimeSpan` `Exception` 각각, 및입니다. 첫 번째 다시 연결을 시도 하기 `PreviousRetryCount` 전에 `ElapsedTime` 와는 모두 0이 되며 `RetryReason` 은 연결이 끊어지는 원인이 되는 예외입니다. 실패 한 각 재시도 후에 `PreviousRetryCount` 는 1 `ElapsedTime` 씩 증가 하 고, 지금까지 다시 연결 하는 데 걸린 시간을 반영 하도록 업데이트 되며, `RetryReason` 는 마지막 다시 연결 시도가 실패 한 원인이 됩니다.

`NextRetryDelay`는 `HubConnection` 다음 다시 연결 시도 전에 대기 시간을 나타내는 TimeSpan을 반환 해야 합니다. `null` 그렇지 않으면이 다시 연결을 중지 해야 합니다.

```csharp
public class RandomRetryPolicy : IRetryPolicy
{
    private readonly Random _random = new Random();

    public TimeSpan? NextRetryDelay(RetryContext retryContext)
    {
        // If we've been reconnecting for less than 60 seconds so far,
        // wait between 0 and 10 seconds before the next reconnect attempt.
        if (retryContext.ElapsedTime < TimeSpan.FromSeconds(60))
        {
            return TimeSpan.FromSeconds(_random.NextDouble() * 10);
        }
        else
        {
            // If we've been reconnecting for more than 60 seconds so far, stop reconnecting.
            return null;
        }
    }
}
```

```csharp
HubConnection connection = new HubConnectionBuilder()
    .WithUrl(new Uri("http://127.0.0.1:5000/chatHub"))
    .WithAutomaticReconnect(new RandomRetryPolicy())
    .Build();
```

또는 [수동으로 다시 연결](#manually-reconnect)에 설명 된 대로 클라이언트를 수동으로 다시 연결 하는 코드를 작성할 수 있습니다.

::: moniker-end

### <a name="manually-reconnect"></a>수동으로 다시 연결

::: moniker range="< aspnetcore-3.0"

> [!WARNING]
> 3.0 이전 버전의에 대 한 SignalR .net 클라이언트는 자동으로 다시 연결 되지 않습니다. 클라이언트를 수동으로 다시 연결 하는 코드를 작성 해야 합니다.

::: moniker-end

<xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed> 이벤트를 사용 하 여 손실 된 연결에 응답 합니다. 예를 들어 다시 연결을 자동화할 수 있습니다.

이벤트 `Closed` 에는를 `Task`반환 하는 대리자가 필요 합니다 .이 메서드를 사용 `async void`하지 않고 비동기 코드를 실행할 수 있습니다. 동기적으로 실행 되는 `Closed` 이벤트 처리기에서 대리자 시그니처를 만족 시키려면 다음 `Task.CompletedTask`을 반환 합니다.

```csharp
connection.Closed += (error) => {
    // Do your close logic.
    return Task.CompletedTask;
};
```

비동기 지원을 위한 주요 이유는 연결을 다시 시작할 수 있도록 하는 것입니다. 연결 시작은 비동기 작업입니다.

연결을 `Closed` 다시 시작 하는 처리기에서 다음 예제와 같이 서버 오버 로드를 방지 하기 위해 임의 지연이 발생할 때까지 대기 하는 것이 좋습니다.

[!code-csharp[Use Closed event handler to automate reconnection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ClosedRestart)]

## <a name="call-hub-methods-from-client"></a>클라이언트에서 허브 메서드 호출

`InvokeAsync`허브에서 메서드를 호출 합니다. 허브 메서드 이름 및 허브 메서드에 정의 된 모든 인수를에 `InvokeAsync`전달 합니다. SignalR는 비동기 이므로를 호출 `async` 하 `await` 는 경우 및를 사용 합니다.

[!code-csharp[InvokeAsync method](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_InvokeAsync)]

메서드 `InvokeAsync` 는 서버 메서드가 `Task` 반환 될 때 완료 되는를 반환 합니다. 반환 값 (있는 경우)은의 결과로 제공 됩니다 `Task`. 서버에서 메서드에 의해 throw 되는 모든 예외에 오류가 `Task`발생 합니다. 구문을 `await` 사용 하 여 서버 메서드가 완료 될 때까지 기다리거나 `try...catch` 구문을 사용 하 여 오류를 처리 합니다.

메서드 `SendAsync` 는 메시지가 서버로 `Task` 전송 될 때 완료 되는를 반환 합니다. 서버 메서드가 완료 될 때까지 기다리지 `Task` 않으므로 반환 값이 제공 되지 않습니다. 메시지를 보내는 동안 클라이언트에서 throw 되는 모든 예외에 오류가 `Task`발생 합니다. 및 `await` `try...catch` 구문을 사용 하 여 보내기 오류를 처리 합니다.

> [!NOTE]
> *서버 리스 모드*에서 Azure SignalR 서비스를 사용 하는 경우 클라이언트에서 허브 메서드를 호출할 수 없습니다. 자세한 내용은 [ SignalR 서비스 설명서](/azure/azure-signalr/signalr-concept-serverless-development-config)를 참조 하세요.

## <a name="call-client-methods-from-hub"></a>허브에서 클라이언트 메서드 호출

빌드 후에를 사용 하 `connection.On` 여 허브에서 호출 하는 메서드를 정의 합니다.

[!code-csharp[Define client methods](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ConnectionOn)]

에서 `connection.On` 위의 코드는 서버 쪽 코드가 메서드를 `SendAsync` 사용 하 여 호출 하는 경우 실행 됩니다.

[!code-csharp[Call client method](dotnet-client/sample/signalrchat/hubs/chathub.cs?name=snippet_SendMessage)]

## <a name="error-handling-and-logging"></a>오류 처리 및 로깅

Try-catch 문으로 오류를 처리 합니다. 개체를 `Exception` 검사 하 여 오류가 발생 한 후 수행할 적절 한 작업을 결정 합니다.

[!code-csharp[Logging](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ErrorHandling)]

## <a name="additional-resources"></a>추가 자료

* [허브](xref:signalr/hubs)
* [JavaScript 클라이언트](xref:signalr/javascript-client)
* [Azure에 게시](xref:signalr/publish-to-azure-web-app)
* [Azure SignalR 서비스 서버 리스 설명서](/azure/azure-signalr/signalr-concept-serverless-development-config)
