---
title: ASP.NET Core Blazor WebAssembly에서 웹 API 호출
author: guardrex
description: CORS(원본 간 리소스 공유) 요청 만들기를 포함하여 JSON 도우미를 사용하여 Blazor WebAssembly 앱에서 웹 API를 호출하는 방법을 알아봅니다.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/28/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: blazor/call-web-api
ms.openlocfilehash: db1f6a357f63b405bf2f3b98e51c9aeffda97d66
ms.sourcegitcommit: 066d66ea150f8aab63f9e0e0668b06c9426296fd
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85242526"
---
# <a name="call-a-web-api-from-aspnet-core-blazor"></a>ASP.NET Core Blazor에서 웹 API 호출

작성자: [Luke Latham](https://github.com/guardrex), [Daniel Roth](https://github.com/danroth27) 및 [Juan De la Cruz](https://github.com/juandelacruz23)

> [!NOTE]
> 이 항목은 Blazor WebAssembly에 적용됩니다. [Blazor 서버](xref:blazor/hosting-models#blazor-server) 앱은 일반적으로 <xref:System.Net.Http.IHttpClientFactory>를 사용하여 만드는 <xref:System.Net.Http.HttpClient> 인스턴스를 사용하여 웹 API를 호출합니다. Blazor 서버에 적용되는 지침은 <xref:fundamentals/http-requests>를 참조하세요.

[Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) 앱은 미리 구성된 <xref:System.Net.Http.HttpClient> 서비스를 사용하여 웹 API를 호출합니다. Blazor JSON 도우미 또는 <xref:System.Net.Http.HttpRequestMessage>를 사용하여 JavaScript [Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API) 옵션을 포함할 수 있는 요청을 작성합니다. Blazor WebAssembly 앱의 <xref:System.Net.Http.HttpClient> 서비스는 원본 서버에 대해 다시 요청을 수행하는 데 중점을 둡니다. 이 항목의 지침은 Blazor WebAssembly 앱과만 관련이 있습니다.

[샘플 코드 보기 및 다운로드](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/)([다운로드 방법](xref:index#how-to-download-a-sample)): `BlazorWebAssemblySample` 앱을 선택합니다.

`BlazorWebAssemblySample` 샘플 앱에서 다음 구성 요소를 참조하세요.

* 웹 API 호출(`Pages/CallWebAPI.razor`)
* HTTP 요청 테스터(`Components/HTTPRequestTester.razor`)

## <a name="packages"></a>패키지

프로젝트 파일에서 [`System.Net.Http.Json`](https://www.nuget.org/packages/System.Net.Http.Json/) NuGet 패키지를 참조합니다.

## <a name="add-the-httpclient-service"></a>HttpClient 서비스 추가

`Program.Main`에서 아직 없는 경우 <xref:System.Net.Http.HttpClient> 서비스를 추가합니다.

```csharp
builder.Services.AddTransient(sp => 
    new HttpClient
    {
        BaseAddress = new Uri(builder.HostEnvironment.BaseAddress)
    });
```

## <a name="httpclient-and-json-helpers"></a>HttpClient 및 JSON 도우미

Blazor WebAssembly 앱에서 [`HttpClient`](xref:fundamentals/http-requests)는 원본 서버에 대해 다시 요청을 수행하기 위해 미리 구성된 서비스로 사용할 수 있습니다.

Blazor 서버 앱은 기본적으로 <xref:System.Net.Http.HttpClient> 서비스를 포함하지 않습니다. [`HttpClient` 팩터리 인프라](xref:fundamentals/http-requests)를 사용하여 앱에 <xref:System.Net.Http.HttpClient>를 제공합니다.

<xref:System.Net.Http.HttpClient> 및 JSON 도우미는 타사 웹 API 엔드포인트를 호출하는 데에도 사용됩니다. <xref:System.Net.Http.HttpClient>는 브라우저 [Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API)를 사용하여 구현되며 동일한 원본 정책 적용을 비롯한 제한 사항이 적용됩니다.

클라이언트의 기본 주소가 원래 서버의 주소로 설정됩니다. [`@inject`](xref:mvc/views/razor#inject) 지시문을 사용하여 <xref:System.Net.Http.HttpClient> 인스턴스를 주입합니다.

```razor
@using System.Net.Http
@inject HttpClient Http
```

다음 예제에서 Todo 웹 API는 CRUD(만들기, 읽기, 업데이트 및 삭제) 작업을 처리합니다. 예제는 다음을 저장하는 `TodoItem` 클래스를 기준으로 합니다.

* ID(`Id`, `long`): 항목의 고유한 ID입니다.
* 이름(`Name`, `string`): 항목의 이름입니다.
* 상태(`IsComplete`, `bool`): 할 일 항목이 완료되었는지 여부를 나타냅니다.

```csharp
private class TodoItem
{
    public long Id { get; set; }
    public string Name { get; set; }
    public bool IsComplete { get; set; }
}
```

JSON 도우미 메서드는 URI(다음 예제의 웹 API)에 요청을 보내고 응답을 처리합니다.

* <xref:System.Net.Http.Json.HttpClientJsonExtensions.GetFromJsonAsync%2A>: HTTP GET 요청을 보내고 JSON 응답 본문을 구문 분석하여 개체를 만듭니다.

  다음 코드에서 `todoItems`는 구성 요소에 의해 표시됩니다. `GetTodoItems` 메서드는 구성 요소가 렌더링을 완료할 때 트리거됩니다([`OnInitializedAsync`](xref:blazor/components/lifecycle#component-initialization-methods)). 전체 예제는 샘플 앱을 참조하세요.

  ```razor
  @using System.Net.Http
  @inject HttpClient Http

  @code {
      private TodoItem[] todoItems;

      protected override async Task OnInitializedAsync() => 
          todoItems = await Http.GetFromJsonAsync<TodoItem[]>("api/TodoItems");
  }
  ```

* <xref:System.Net.Http.Json.HttpClientJsonExtensions.PostAsJsonAsync%2A>: JSON 인코딩 콘텐츠를 포함하여 HTTP POST 요청을 보내고 JSON 응답 본문을 구문 분석하여 개체를 만듭니다.

  다음 코드에서 구성 요소의 바인딩된 요소는 `newItemName`을 제공합니다. `AddItem` 메서드는 `<button>` 요소를 선택하여 트리거됩니다. 전체 예제는 샘플 앱을 참조하세요.

  ```razor
  @using System.Net.Http
  @inject HttpClient Http

  <input @bind="newItemName" placeholder="New Todo Item" />
  <button @onclick="@AddItem">Add</button>

  @code {
      private string newItemName;

      private async Task AddItem()
      {
          var addItem = new TodoItem { Name = newItemName, IsComplete = false };
          await Http.PostAsJsonAsync("api/TodoItems", addItem);
      }
  }
  ```
  
  <xref:System.Net.Http.Json.HttpClientJsonExtensions.PostAsJsonAsync%2A>를 호출하면 <xref:System.Net.Http.HttpResponseMessage>가 반환됩니다. 응답 메시지에서 JSON 콘텐츠를 역직렬화하려면 `ReadFromJsonAsync<T>` 확장 메서드를 사용합니다.
  
  ```csharp
  var content = response.Content.ReadFromJsonAsync<WeatherForecast>();
  ```

* <xref:System.Net.Http.Json.HttpClientJsonExtensions.PutAsJsonAsync%2A>: JSON 인코딩 콘텐츠를 포함하여 HTTP PUT 요청을 보냅니다.

  다음 코드에서 구성 요소의 바인딩된 요소는 `Name` 및 `IsCompleted`에 대한 `editItem` 값을 제공합니다. 항목의 `Id`는 UI의 다른 부분에서 해당 항목을 선택하고 `EditItem`을 호출하면 설정됩니다. `SaveItem` 메서드는 Save `<button>` 요소를 선택하여 트리거됩니다. 전체 예제는 샘플 앱을 참조하세요.

  ```razor
  @using System.Net.Http
  @inject HttpClient Http

  <input type="checkbox" @bind="editItem.IsComplete" />
  <input @bind="editItem.Name" />
  <button @onclick="@SaveItem">Save</button>

  @code {
      private TodoItem editItem = new TodoItem();

      private void EditItem(long id)
      {
          editItem = todoItems.Single(i => i.Id == id);
      }

      private async Task SaveItem() =>
          await Http.PutAsJsonAsync($"api/TodoItems/{editItem.Id}, editItem);
  }
  ```
  
  <xref:System.Net.Http.Json.HttpClientJsonExtensions.PutAsJsonAsync%2A>를 호출하면 <xref:System.Net.Http.HttpResponseMessage>가 반환됩니다. 응답 메시지에서 JSON 콘텐츠를 역직렬화하려면 <xref:System.Net.Http.Json.HttpContentJsonExtensions.ReadFromJsonAsync%2A> 확장 메서드를 사용합니다.
  
  ```csharp
  var content = response.content.ReadFromJsonAsync<WeatherForecast>();
  ```

<xref:System.Net.Http>는 HTTP 요청을 보내고 HTTP 응답을 받기 위한 추가 확장 메서드를 포함합니다. <xref:System.Net.Http.HttpClient.DeleteAsync%2A?displayProperty=nameWithType>는 웹 API에 HTTP DELETE 요청을 보내는 데 사용됩니다.

다음 코드에서 Delete `<button>` 요소는 `DeleteItem` 메서드를 호출합니다. 바인딩된 `<input>` 요소는 삭제할 항목의 `id`를 제공합니다. 전체 예제는 샘플 앱을 참조하세요.

```razor
@using System.Net.Http
@inject HttpClient Http

<input @bind="id" />
<button @onclick="@DeleteItem">Delete</button>

@code {
    private long id;

    private async Task DeleteItem() =>
        await Http.DeleteAsync($"api/TodoItems/{id}");
}
```

## <a name="named-httpclient-with-ihttpclientfactory"></a>IHttpClientFactory를 사용하여 명명된 HttpClient

<xref:System.Net.Http.IHttpClientFactory> 서비스 및 명명된 <xref:System.Net.Http.HttpClient>의 구성이 지원됩니다.

프로젝트 파일에서 [`Microsoft.Extensions.Http`](https://www.nuget.org/packages/Microsoft.Extensions.Http/) NuGet 패키지를 참조합니다.

`Program.Main`(`Program.cs`):

```csharp
builder.Services.AddHttpClient("ServerAPI", client => 
    client.BaseAddress = new Uri(builder.HostEnvironment.BaseAddress));
```

`FetchData` 구성 요소(`Pages/FetchData.razor`):

```razor
@inject IHttpClientFactory ClientFactory

...

@code {
    private WeatherForecast[] forecasts;

    protected override async Task OnInitializedAsync()
    {
        var client = ClientFactory.CreateClient("ServerAPI");

        forecasts = await client.GetFromJsonAsync<WeatherForecast[]>(
            "WeatherForecast");
    }
}
```

## <a name="typed-httpclient"></a>형식화된 HttpClient

형식화된 <xref:System.Net.Http.HttpClient>는 앱의 기본값 또는 명명된 <xref:System.Net.Http.HttpClient> 인스턴스를 사용하여 하나 이상의 웹 API 엔드포인트에서 데이터를 반환합니다.

`WeatherForecastClient.cs`:

```csharp
using System.Net.Http;
using System.Net.Http.Json;
using System.Threading.Tasks;

public class WeatherForecastClient
{
    private readonly HttpClient client;

    public WeatherForecastClient(HttpClient client)
    {
        this.client = client;
    }

    public async Task<WeatherForecast[]> GetForecastAsync()
    {
        var forecasts = new WeatherForecast[0];
    
        try
        {
            forecasts = await client.GetFromJsonAsync<WeatherForecast[]>(
                "WeatherForecast");
        }
        catch
        {
            ...
        }
    
        return forecasts;
    }
}
```

`Program.Main`(`Program.cs`):

```csharp
builder.Services.AddHttpClient<WeatherForecastClient>(client => 
    client.BaseAddress = new Uri(builder.HostEnvironment.BaseAddress));
```

구성 요소는 형식화된 <xref:System.Net.Http.HttpClient>를 삽입하여 웹 API를 호출합니다.

`FetchData` 구성 요소(`Pages/FetchData.razor`):

```razor
@inject WeatherForecastClient Client

...

@code {
    private WeatherForecast[] forecasts;

    protected override async Task OnInitializedAsync()
    {
        forecasts = await Client.GetForecastAsync();
    }
}
```

## <a name="handle-errors"></a>오류 처리

웹 API와 상호 작용하는 동안 오류가 발생하는 경우 개발자 코드로 처리할 수 있습니다. 예를 들어 <xref:System.Net.Http.Json.HttpClientJsonExtensions.GetFromJsonAsync%2A>에는 `application/json`의 `Content-Type`을 가진 서버 API의 JSON 응답이 필요합니다. 응답이 JSON 형식이 아닌 경우 콘텐츠 유효성 검사는 <xref:System.NotSupportedException>을 throw합니다.

다음 예제에서는 날씨 예측 데이터 요청에 대한 URI 엔드포인트의 철자가 잘못되었습니다. URI는 `WeatherForecast`가 되어야 하지만 호출에는 `WeatherForcast`(“e” 누락)로 표시됩니다.

<xref:System.Net.Http.Json.HttpClientJsonExtensions.GetFromJsonAsync%2A> 호출에서는 JSON이 반환되어야 하지만, 서버는 `text/html`의 `Content-Type`을 사용하여 서버에서 처리되지 않은 예외에 대한 HTML을 반환합니다. 경로를 찾을 수 없고 미들웨어에서 요청에 대한 페이지 또는 보기를 제공할 수 없기 때문에 처리되지 않은 예외가 서버에서 발생합니다.

클라이언트의 <xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitializedAsync%2A>에서 응답 콘텐츠가 JSON이 아닌 것으로 확인되면 <xref:System.NotSupportedException>이 throw됩니다. `catch` 블록에서 예외가 catch됩니다. 여기서 사용자 지정 논리는 오류를 기록하거나 사용자에게 친숙한 오류 메시지를 표시할 수 있습니다.

```csharp
protected override async Task OnInitializedAsync()
{
    try
    {
        forecasts = await Http.GetFromJsonAsync<WeatherForecast[]>(
            "WeatherForcast");
    }
    catch (NotSupportedException exception)
    {
        ...
    }
}
```

> [!NOTE]
> 앞의 예제는 데모용입니다. 엔드포인트가 없거나 서버에서 처리되지 않은 예외가 발생한 경우에도 JSON을 반환하도록 웹 API 서버 앱을 구성할 수 있습니다.

자세한 내용은 <xref:blazor/fundamentals/handle-errors>를 참조하세요.

## <a name="cross-origin-resource-sharing-cors"></a>CORS(원본 간 리소스 공유)

브라우저 보안 때문에 웹 페이지에서 해당 웹 페이지를 제공한 도메인이 아닌 다른 도메인에 요청을 수행할 수는 없습니다. 이러한 제한 사항을 *동일 원본 정책*이라고 합니다. 동일 원본 정책은 악성 사이트에서 다른 사이트의 중요한 데이터를 읽지 못하도록 차단합니다. 브라우저에서 원본이 다른 엔드포인트로의 요청을 만들려면 *엔드포인트*가 [CORS(원본 간 리소스 공유)](https://www.w3.org/TR/cors/)를 사용하도록 설정해야 합니다.

[Blazor WebAssembly 샘플 앱(BlazorWebAssemblySample)](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/)은 웹 API 호출 구성 요소(`Pages/CallWebAPI.razor`)에서 CORS를 사용하는 방법을 보여 줍니다.

다른 사이트에서 앱에 대해 CORS(원본 간 리소스 공유) 요청을 수행하도록 허용하려면 <xref:security/cors>를 참조하세요.

## <a name="additional-resources"></a>추가 자료

* <xref:blazor/security/webassembly/additional-scenarios>: 보안 웹 API 요청을 만들기 위해 <xref:System.Net.Http.HttpClient> 사용에 대한 검사를 포함합니다.
* <xref:fundamentals/http-requests>
* <xref:security/enforcing-ssl>
* [Kestrel HTTPS 엔드포인트 구성](xref:fundamentals/servers/kestrel#endpoint-configuration)
* [W3C의 CORS(원본 간 리소스 공유)](https://www.w3.org/TR/cors/)
