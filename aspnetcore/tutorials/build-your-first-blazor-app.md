---
title: '첫 번째 Blazor 앱 빌드' author: description: 'Blazor 앱을 단계별로 빌드합니다.'
monikerRange: ms.author: ms.custom: ms.date: no-loc:
- 'Blazor'
- 'Identity'
- 'Let's Encrypt'
- 'Razor'
- ‘SignalR’ uid: 

---
# <a name="build-your-first-blazor-app"></a>첫 번째 Blazor 앱 빌드

작성자: [Daniel Roth](https://github.com/danroth27) 및 [Luke Latham](https://github.com/guardrex)

이 자습서에서는 Blazor 앱을 만들고 수정하는 방법을 보여줍니다. 다음과 같은 작업을 수행하는 방법을 살펴봅니다.

> [!div class="checklist"]
> * 할 일 목록 Blazor 앱 프로젝트 만들기
> * Razor 구성 요소 수정
> * 구성 요소에서 이벤트 처리 및 데이터 바인딩 사용
> * Blazor 앱에서 DI(종속성 주입) 및 라우팅 사용

이 모든 과정을 마치면 채팅 앱을 실행할 수 있습니다.

## <a name="build-components"></a>구성 요소 빌드

1. <xref:blazor/get-started> 문서의 지침에 따라 이 자습서에 사용할 Blazor 프로젝트를 만듭니다. 프로젝트 이름을 *ToDoList*로 지정합니다.

1. *Pages* 폴더에 있는 앱의 3개 페이지로 각각 이동합니다. 홈, 카운터 및 Fetch 데이터. 이 페이지는 Razor 구성 요소 파일 *Index.razor*, *Counter.razor* 및 *FetchData.razor*로 구현됩니다.

1. Counter 페이지에서 **Click me** 단추를 선택하여 페이지 새로 고침 없이 카운터를 증가시킵니다. 웹 페이지에서 카운터를 증가시키려면 일반적으로 JavaScript를 작성해야 하지만, Blazor를 사용하여 C#를 대신 작성할 수 있습니다.

1. *Counter.razor* 파일에서 `Counter` 구성 요소의 구현에 대해 살펴봅니다.

   *Pages/Counter.razor*:

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/Counter1.razor)]

   `Counter` 구성 요소의 UI는 HTML을 사용하여 정의됩니다. 동적 렌더링 논리(예: 루프, 조건, 식)는 [Razor](xref:mvc/views/razor)라고 하는 포함된 C# 구문을 사용하여 추가됩니다. HTML 태그 및 C# 렌더링 논리는 빌드 시 구성 요소 클래스로 변환됩니다. 생성되는 .NET 클래스의 이름은 파일 이름과 일치합니다.

   구성 요소 클래스의 멤버는 `@code` 블록에서 정의됩니다. `@code` 블록에서 구성 요소 상태(속성, 필드) 및 메서드가 이벤트 처리 또는 다른 구성 요소 논리 정의를 위해 지정됩니다. 그런 다음, 이러한 멤버를 구성 요소 렌더링 논리의 일부 및 이벤트 처리에 사용합니다.

   **Click me** 단추를 선택하면:

   * `Counter` 구성 요소의 등록된 `onclick` 처리기가 호출됩니다(`IncrementCount` 메서드).
   * `Counter` 구성 요소가 렌더 트리를 다시 생성합니다.
   * 새 렌더 트리를 이전 렌더 트리와 비교합니다.
   * DOM(문서 개체 모델)에 대한 수정 내용만 적용됩니다. 표시된 개수가 업데이트됩니다.

1. `Counter` 구성 요소의 C# 논리를 수정하여 카운트를 하나 대신 둘씩 증가하도록 합니다.

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/Counter2.razor?highlight=14)]

1. 앱을 다시 빌드하고 실행하여 변경 내용을 확인합니다. **Click me** 단추를 선택합니다. 카운터가 2씩 증가됩니다.

## <a name="use-components"></a>구성 요소 사용

HTML 구문을 사용하여 구성 요소를 다른 구성 요소에 포함합니다.

1. `Index` 구성 요소(*Index.razor*)에 `<Counter />` 요소를 추가하여 `Counter` 구성 요소를 앱의 `Index` 구성 요소에 추가합니다.

   이 자습서의 환경으로 Blazor WebAssembly를 사용하고 있다면 `Index` 구성 요소에 `SurveyPrompt` 구성 요소가 사용되고 있을 것입니다. 해당 `<SurveyPrompt>` 요소를 `<Counter />` 요소로 대체합니다. 이 자습서의 환경으로 Blazor 서버 앱을 사용하고 있다면 `Index` 구성 요소에 `<Counter />` 요소를 추가합니다.

   *Pages/Index.razor*:

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/Index1.razor?highlight=7)]

1. 앱을 다시 빌드하고 실행합니다. `Index` 구성 요소에 자체 카운터가 있습니다.

## <a name="component-parameters"></a>구성 요소 매개 변수

구성 요소는 매개 변수를 포함할 수도 있습니다. 구성 요소 매개 변수는 `[Parameter]` 특성이 지정된 구성 요소 클래스의 공용 속성을 사용하여 정의됩니다. 특성을 사용하여 태그에서 구성 요소의 인수를 지정합니다.

1. 구성 요소의 `@code` C# 코드를 다음과 같이 업데이트합니다.

   * `[Parameter]` 특성이 지정된 공용 `IncrementAmount` 속성을 추가합니다.
   * `currentCount` 값을 증가시킬 때 `IncrementAmount` 속성을 사용하도록 `IncrementCount` 메서드를 변경합니다.

   *Pages/Counter.razor*:

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/Counter.razor?highlight=13,17)]

   <!-- Add back when supported.
       > [!NOTE]
       > From Visual Studio, you can quickly add a component parameter by using the `para` snippet. Type `para` and press the `Tab` key twice.
   -->

1. 특성을 사용하여 `Index` 구성 요소의 `<Counter>` 요소에 `IncrementAmount` 매개 변수를 지정합니다. 카운터가 10씩 증분하도록 값을 설정합니다.

   *Pages/Index.razor*:

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/Index2.razor?highlight=7)]

1. `Index` 구성 요소를 다시 로드합니다. **Click me** 단추가 선택될 때마다 카운터가 10씩 증가합니다. `Counter` 구성 요소의 카운터는 계속 1씩 증가합니다.

## <a name="route-to-components"></a>구성 요소 경로

*Counter.razor* 파일 맨 위의 `@page` 지시문은 `Counter` 구성 요소가 라우팅 엔드포인트임을 지정합니다. `Counter` 구성 요소는 `/counter`로 전송된 요청을 처리합니다. `@page` 지시문이 없으면 구성 요소는 라우팅된 요청을 처리하지 않지만, 해당 구성 요소는 여전히 다른 구성 요소에서 사용될 수 있습니다.

## <a name="dependency-injection"></a>종속성 주입

### <a name="blazor-server-experience"></a>Blazor 서버 환경

Blazor 서버 앱을 사용할 경우 `WeatherForecastService` 서비스가 `Startup.ConfigureServices`에서 [싱글톤](xref:fundamentals/dependency-injection#service-lifetimes)으로 등록됩니다. 해당 서비스의 인스턴스는 [DI(종속성 주입)](xref:fundamentals/dependency-injection)을 통해서 앱 전체에서 사용할 수 있습니다.

[!code-csharp[](build-your-first-blazor-app/samples_snapshot/3.x/Startup.cs?highlight=5)]

`@inject` 지시문은 `WeatherForecastService` 서비스의 인스턴스를 `FetchData` 구성 요소에 주입하기 위해서 사용됩니다.

*Pages/FetchData.razor*:

[!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData1.razor?highlight=3)]

`FetchData` 구성 요소는 주입된 서비스를 `ForecastService`로 사용하여 `WeatherForecast` 개체의 배열을 검색합니다.

[!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData2.razor?highlight=6)]

### <a name="blazor-webassembly-experience"></a>Blazor WebAssembly 환경

Blazor WebAssembly 앱을 사용할 경우 *wwwroot/sample-data* 폴더의 *weather.json* 파일에서 일기 예보 데이터를 가져오기 위해서 `HttpClient`가 주입됩니다.

*Pages/FetchData.razor*:

[!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData1_client.razor?highlight=7-9)]

[`@foreach`](/dotnet/csharp/language-reference/keywords/foreach-in) 루프는 각 예측 인스턴스를 날씨 데이터 테이블의 행으로 렌더링하는 데 사용됩니다.

[!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData3.razor?highlight=11-19)]

## <a name="build-a-todo-list"></a>할 일 목록 빌드

앱에 간단한 할 일 목록을 구현하는 새 구성 요소를 추가합니다.

1. *Pages* 폴더에서 새 `Todo` Razor 구성 요소를 앱에 추가합니다. Visual Studio에서 **Pages** 폴더를 마우스 오른쪽 단추로 클릭하고 **추가** > **새 항목** >  **Razor 구성 요소**를 선택합니다. 구성 요소의 파일 이름을 *Todo.razor*로 지정합니다. 다른 개발 환경에서는 *Todo.razor*라는 **Pages** 폴더에 빈 파일을 추가합니다.

1. 구성 요소에 대한 초기 태그를 제공합니다.

   ```razor
   @page "/todo"

   <h3>Todo</h3>
   ```

1. `Todo` 구성 요소를 탐색 모음에 추가합니다.

   `NavMenu` 구성 요소(*Shared/NavMenu.razor*)는 앱의 레이아웃에서 사용됩니다. 레이아웃은 앱의 콘텐츠 중복을 방지할 수 있는 구성 요소입니다.

   *Shared/NavMenu.razor* 파일의 기존 목록 항목 아래에 다음 목록 항목 태그를 추가하여 `Todo` 구성 요소에 대한 `<NavLink>` 요소를 추가합니다.

   ```razor
   <li class="nav-item px-3">
       <NavLink class="nav-link" href="todo">
           <span class="oi oi-list-rich" aria-hidden="true"></span> Todo
       </NavLink>
   </li>
   ```

1. 앱을 다시 빌드하고 실행합니다. 새 Todo 페이지를 방문하여 `Todo` 구성 요소에 대한 링크가 작동하는지 확인합니다.

1. 프로젝트 루트에 *TodoItem.cs* 파일을 추가하여 Todo 항목을 나타내는 클래스를 저장합니다. `TodoItem` 클래스에 대해 다음 C# 코드를 사용합니다.

   [!code-csharp[](build-your-first-blazor-app/samples_snapshot/3.x/TodoItem.cs)]

1. `Todo` 구성 요소(*Pages/Todo.razor*)로 돌아갑니다.

   * `@code` 블록에 있는 할 일 항목에 대한 필드를 추가합니다. `Todo` 구성 요소는 이 필드를 사용하여 할 일 목록의 상태를 유지 관리합니다.
   * 순서가 지정되지 않은 목록 태그 및 `foreach` 루프를 추가하여 각 할 일 항목을 목록 항목(`<li>`)으로 렌더링합니다.

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo4.razor?highlight=5-10,12-14)]

1. 목록에 할 일 항목을 추가하려면 앱에 UI 요소가 필요합니다. 순서가 지정되지 않은 목록(`<ul>...</ul>`) 아래에 텍스트 입력(`<input>`)과 단추(`<button>`)를 추가합니다.

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo5.razor?highlight=12-13)]

1. 앱을 다시 빌드하고 실행합니다. 단추에 이벤트 처리기가 연결되어 있지 않으므로 **Add todo** 단추를 선택해도 아무 일도 발생하지 않습니다.

1. `Todo` 구성 요소에 `AddTodo` 메서드를 추가하고 `@onclick` 특성을 사용하여 이를 단추 선택에 등록합니다. 단추가 선택되면 `AddTodo` C# 메서드가 호출됩니다.

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo6.razor?highlight=2,7-10)]

1. 새 할 일 항목의 제목을 가져오기 위해 `@code` 블록의 상단에 `newTodo` 문자열 필드를 추가하고 `<input>` 요소에서 `bind` 특성을 사용하여 이를 텍스트 입력 값에 바인딩합니다.

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo7.razor?highlight=2)]

   ```razor
   <input placeholder="Something todo" @bind="newTodo" />
   ```

1. `AddTodo` 메서드를 수정하여 지정한 제목으로 `TodoItem`을 목록에 추가합니다. `newTodo`를 빈 문자열로 설정하여 텍스트 입력 값을 지웁니다.

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo8.razor?highlight=19-26)]

1. 앱을 다시 빌드하고 실행합니다. 할 일 목록에 몇 개의 할 일 항목을 추가하여 새 코드를 테스트합니다.

1. 각 할 일 항목의 제목 텍스트를 편집 가능하게 설정하고 확인란을 통해 사용자가 완료된 항목을 추적하도록 도울 수 있습니다. 각 할 일 항목의 확인란 입력을 추가하고 해당 값을 `IsDone` 속성에 바인딩합니다. `@todo.Title`을 `@todo.Title`에 바인딩된 `<input>` 요소로 변경합니다.

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo9.razor?highlight=5-6)]

1. 해당 값이 바인딩되었는지 확인하기 위해 `<h3>` 헤더를 수정하여 완료되지 않은(`IsDone`이 `false`) 할 일 항목 수를 표시합니다.

   ```razor
   <h3>Todo (@todos.Count(todo => !todo.IsDone))</h3>
   ```

1. 완료된 `Todo` 구성 요소(*Pages/Todo.razor*)는 다음과 같습니다.

   [!code-razor[](build-your-first-blazor-app/samples_snapshot/3.x/Todo.razor)]

1. 앱을 다시 빌드하고 실행합니다. 할 일 항목을 추가하여 새 코드를 테스트합니다.

## <a name="next-steps"></a>다음 단계

본 자습서에서는 다음 작업에 관한 방법을 학습했습니다.

> [!div class="checklist"]
> * 할 일 목록 Blazor 앱 프로젝트 만들기
> * Razor 구성 요소 수정
> * 구성 요소에서 이벤트 처리 및 데이터 바인딩 사용
> * Blazor 앱에서 DI(종속성 주입) 및 라우팅 사용

구성 요소를 빌드하고 사용하는 방법을 알아봅니다.

> [!div class="nextstepaction"]
> <xref:blazor/components>
