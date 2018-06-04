<span data-ttu-id="4cfc0-101">*Views/HelloWorld/Index.cshtml* Razor 보기 파일의 콘텐츠를 다음으로 대체합니다.</span><span class="sxs-lookup"><span data-stu-id="4cfc0-101">Replace the contents of the *Views/HelloWorld/Index.cshtml* Razor view file with the following:</span></span>

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/HelloWorld/Index.cshtml)]

<span data-ttu-id="4cfc0-102">`http://localhost:xxxx/HelloWorld`로 이동합니다.</span><span class="sxs-lookup"><span data-stu-id="4cfc0-102">Navigate to `http://localhost:xxxx/HelloWorld`.</span></span> <span data-ttu-id="4cfc0-103">`HelloWorldController`에서 `Index` 메서드는 많은 작업을 수행하지 않았습니다. `return View();` 문을 실행했습니다. 해당 문은 메서드가 브라우저에 대한 응답을 렌더링하기 위해 보기 템플릿 파일을 사용해야 함을 지정했습니다.</span><span class="sxs-lookup"><span data-stu-id="4cfc0-103">The `Index` method in the `HelloWorldController` didn't do much; it ran the statement `return View();`, which specified that the method should use a view template file to render a response to the browser.</span></span> <span data-ttu-id="4cfc0-104">보기 템플릿 파일의 이름을 명시적으로 지정하지 않았기 때문에 MVC는 */Views/HelloWorld* 폴더에서 *Index.cshtml* 보기 파일을 사용하도록 기본 설정되었습니다.</span><span class="sxs-lookup"><span data-stu-id="4cfc0-104">Because you didn't explicitly specify the name of the view template file, MVC defaulted to using the *Index.cshtml* view file in the */Views/HelloWorld* folder.</span></span> <span data-ttu-id="4cfc0-105">아래 이미지는 보기에서 하드 코드된 “Hello from our View Template!”</span><span class="sxs-lookup"><span data-stu-id="4cfc0-105">The image below shows the string "Hello from our View Template!"</span></span> <span data-ttu-id="4cfc0-106">문자열을 보여줍니다.</span><span class="sxs-lookup"><span data-stu-id="4cfc0-106">hard-coded in the view.</span></span>

![브라우저 창](~/tutorials/first-mvc-app/adding-view/_static/hell_template.png)

<span data-ttu-id="4cfc0-108">브라우저 창이 작은 경우(예: 모바일 장치) **홈**, **정보** 및 **연락처** 링크를 보기 위해 오른쪽 위에 있는 [부트스트랩 탐색 단추](http://getbootstrap.com/components/#navbar)를 설정/해제(탭)해야 할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4cfc0-108">If your browser window is small (for example on a mobile device), you might need to toggle (tap) the [Bootstrap navigation button](http://getbootstrap.com/components/#navbar) in the upper right to see the **Home**, **About**, and **Contact** links.</span></span>

![부트스트랩 탐색 단추가 강조 표시된 브라우저 창](~/tutorials/first-mvc-app/adding-view/_static/1.png)

## <a name="changing-views-and-layout-pages"></a><span data-ttu-id="4cfc0-110">보기 및 레이아웃 페이지 변경</span><span class="sxs-lookup"><span data-stu-id="4cfc0-110">Changing views and layout pages</span></span>

<span data-ttu-id="4cfc0-111">메뉴 링크를 누릅니다(**MvcMovie**, **홈**, **정보**).</span><span class="sxs-lookup"><span data-stu-id="4cfc0-111">Tap the menu links (**MvcMovie**, **Home**, **About**).</span></span> <span data-ttu-id="4cfc0-112">각 페이지는 동일한 메뉴 레이아웃을 표시합니다.</span><span class="sxs-lookup"><span data-stu-id="4cfc0-112">Each page shows the same menu layout.</span></span> <span data-ttu-id="4cfc0-113">메뉴 레이아웃은 *Views/Shared/_Layout.cshtml* 파일에서 구현됩니다.</span><span class="sxs-lookup"><span data-stu-id="4cfc0-113">The menu layout is implemented in the *Views/Shared/_Layout.cshtml* file.</span></span> <span data-ttu-id="4cfc0-114">*Views/Shared/_Layout.cshtml* 파일을 엽니다.</span><span class="sxs-lookup"><span data-stu-id="4cfc0-114">Open the *Views/Shared/_Layout.cshtml* file.</span></span>

<span data-ttu-id="4cfc0-115">[레이아웃](xref:mvc/views/layout) 템플릿을 사용하면 한 곳에서 사이트의 HTML 컨테이너 레이아웃을 지정한 다음 사이트에서 여러 페이지에 걸쳐 적용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4cfc0-115">[Layout](xref:mvc/views/layout) templates allow you to specify the HTML container layout of your site in one place and then apply it across multiple pages in your site.</span></span> <span data-ttu-id="4cfc0-116">`@RenderBody()` 줄을 찾습니다.</span><span class="sxs-lookup"><span data-stu-id="4cfc0-116">Find the `@RenderBody()` line.</span></span> <span data-ttu-id="4cfc0-117">`RenderBody`는 사용자가 만드는 모든 보기 전용 페이지가 표시되는 자리 표시자이며 레이아웃 페이지에서 *래핑됩니다*.</span><span class="sxs-lookup"><span data-stu-id="4cfc0-117">`RenderBody` is a placeholder where all the view-specific pages you create show up, *wrapped* in the layout page.</span></span> <span data-ttu-id="4cfc0-118">예를 들어 **정보** 링크를 선택하는 경우 **Views/Home/About.cshtml** 보기는 `RenderBody` 메서드 내에서 렌더링됩니다.</span><span class="sxs-lookup"><span data-stu-id="4cfc0-118">For example, if you select the **About** link, the **Views/Home/About.cshtml** view is rendered inside the `RenderBody` method.</span></span>

## <a name="change-the-title-and-menu-link-in-the-layout-file"></a><span data-ttu-id="4cfc0-119">레이아웃 파일의 제목 및 메뉴 링크 변경</span><span class="sxs-lookup"><span data-stu-id="4cfc0-119">Change the title and menu link in the layout file</span></span>

<span data-ttu-id="4cfc0-120">제목 요소에서 `MvcMovie`를 `Movie App`으로 변경합니다.</span><span class="sxs-lookup"><span data-stu-id="4cfc0-120">In the title element, change `MvcMovie` to `Movie App`.</span></span> <span data-ttu-id="4cfc0-121">레이아웃 템플릿에서 앵커 텍스트를 `MvcMovie`에서 `Movie App`로, 컨트롤러를 `Home`에서 아래 강조 표시된 `Movies`로 변경합니다.</span><span class="sxs-lookup"><span data-stu-id="4cfc0-121">Change the anchor text in the layout template from `MvcMovie` to `Movie App` and the controller from `Home` to `Movies` as highlighted below:</span></span>

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="4cfc0-122">[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/_Layout.cshtml?highlight=7,31)]</span><span class="sxs-lookup"><span data-stu-id="4cfc0-122">[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/_Layout.cshtml?highlight=7,31)]</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="4cfc0-123">[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/_Layout21.cshtml?highlight=6,29)]</span><span class="sxs-lookup"><span data-stu-id="4cfc0-123">[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/_Layout21.cshtml?highlight=6,29)]</span></span>
::: moniker-end

>[!WARNING]
> <span data-ttu-id="4cfc0-124">`Movies` 컨트롤러를 아직 구현하지 않았으므로 해당 링크를 클릭하면 404(찾을 수 없음) 오류가 나타납니다.</span><span class="sxs-lookup"><span data-stu-id="4cfc0-124">We haven't implemented the `Movies` controller yet, so if you click on that link, you'll get a 404 (Not found) error.</span></span>

<span data-ttu-id="4cfc0-125">변경 내용을 저장하고 **정보** 링크를 누릅니다.</span><span class="sxs-lookup"><span data-stu-id="4cfc0-125">Save your changes and tap the **About** link.</span></span> <span data-ttu-id="4cfc0-126">브라우저 탭의 제목에서 **정보 - Mvc 동영상** 대신 **정보 - 동영상 앱**을 표시하는 방법을 확인합니다:</span><span class="sxs-lookup"><span data-stu-id="4cfc0-126">Notice how the title on the browser tab now displays **About - Movie App** instead of **About - Mvc Movie**:</span></span> 

![정보 탭](~/tutorials/first-mvc-app/adding-view/_static/about2.png)

<span data-ttu-id="4cfc0-128">**연락처** 링크를 누르고 제목 및 앵커 텍스트가 **동영상 앱**을 표시하는지 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="4cfc0-128">Tap the **Contact** link and notice that the title and anchor text also display **Movie App**.</span></span> <span data-ttu-id="4cfc0-129">레이아웃 템플릿에 변경 사항을 한 번 적용할 수 있었으며 사이트의 모든 페이지에 새 링크 텍스트 및 새 제목을 반영했습니다.</span><span class="sxs-lookup"><span data-stu-id="4cfc0-129">We were able to make the change once in the layout template and have all pages on the site reflect the new link text and new title.</span></span>

<span data-ttu-id="4cfc0-130">*Views/_ViewStart.cshtml* 파일을 검사합니다.</span><span class="sxs-lookup"><span data-stu-id="4cfc0-130">Examine the *Views/_ViewStart.cshtml* file:</span></span>


```HTML
@{
    Layout = "_Layout";
}
```

<span data-ttu-id="4cfc0-131">*Views/_ViewStart.cshtml* 파일에서 각 보기에 대한 *Views/Shared/_Layout.cshtml* 파일을 가져 옵니다.</span><span class="sxs-lookup"><span data-stu-id="4cfc0-131">The *Views/_ViewStart.cshtml* file brings in the *Views/Shared/_Layout.cshtml* file to each view.</span></span> <span data-ttu-id="4cfc0-132">`Layout` 속성을 사용하여 다른 레이아웃 보기를 설정하거나 레이아웃 파일을 사용하지 않도록 `null`로 설정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4cfc0-132">You can use the `Layout` property to set a different layout view, or set it to `null` so no layout file will be used.</span></span>

<span data-ttu-id="4cfc0-133">`Index` 보기의 제목을 변경합니다.</span><span class="sxs-lookup"><span data-stu-id="4cfc0-133">Change the title of the `Index` view.</span></span>

<span data-ttu-id="4cfc0-134">*Views/HelloWorld/Index.cshtml*을 엽니다.</span><span class="sxs-lookup"><span data-stu-id="4cfc0-134">Open *Views/HelloWorld/Index.cshtml*.</span></span> <span data-ttu-id="4cfc0-135">변경을 적용하는 두 군데의 위치가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4cfc0-135">There are two places to make a change:</span></span>

   * <span data-ttu-id="4cfc0-136">브라우저의 제목에 표시되는 텍스트</span><span class="sxs-lookup"><span data-stu-id="4cfc0-136">The text that appears in the title of the browser.</span></span>
   * <span data-ttu-id="4cfc0-137">보조 헤더(`<h2>` 요소)</span><span class="sxs-lookup"><span data-stu-id="4cfc0-137">The secondary header (`<h2>` element).</span></span>

<span data-ttu-id="4cfc0-138">어떤 코드에서 어떤 앱의 부분을 변경하는지 볼 수 있도록 약간 다르게 할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4cfc0-138">You'll make them slightly different so you can see which bit of code changes which part of the app.</span></span>


```HTML
@{
    ViewData["Title"] = "Movie List";
}

<h2>My Movie List</h2>

<p>Hello from our View Template!</p>
```

<span data-ttu-id="4cfc0-139">위의 코드에서 `ViewData["Title"] = "Movie List";`는 `ViewData` 사전의 `Title` 속성을 "Movie List"로 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="4cfc0-139">`ViewData["Title"] = "Movie List";` in the code above sets the `Title` property of the `ViewData` dictionary to "Movie List".</span></span> <span data-ttu-id="4cfc0-140">`Title` 속성은 레이아웃 페이지의 `<title>` HTML 요소에서 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="4cfc0-140">The `Title` property is used in the `<title>` HTML element in the layout page:</span></span>


```HTML
<title>@ViewData["Title"] - Movie App</title>
   ```

<span data-ttu-id="4cfc0-141">변경 내용을 저장하고 `http://localhost:xxxx/HelloWorld`로 이동합니다.</span><span class="sxs-lookup"><span data-stu-id="4cfc0-141">Save your change and navigate to `http://localhost:xxxx/HelloWorld`.</span></span> <span data-ttu-id="4cfc0-142">브라우저 제목, 기본 제목 및 작은 제목이 변경된 것을 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="4cfc0-142">Notice that the browser title, the primary heading, and the secondary headings have changed.</span></span> <span data-ttu-id="4cfc0-143">(브라우저에서 변경 내용을 확인할 수 없는 경우 캐시된 콘텐츠를 보고 있을 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4cfc0-143">(If you don't see changes in the browser, you might be viewing cached content.</span></span> <span data-ttu-id="4cfc0-144">브라우저에서 Ctrl+F5 키를 눌러 로드될 서버에서 응답을 강제로 적용합니다.) 브라우저 제목은 *Index.cshtml* 보기 템플릿에서 설정한 `ViewData["Title"]` 및 레이아웃 파일에 추가된 추가 "- Movie App"으로 만들어집니다.</span><span class="sxs-lookup"><span data-stu-id="4cfc0-144">Press Ctrl+F5 in your browser to force the response from the server to be loaded.) The browser title is created with `ViewData["Title"]` we set in the *Index.cshtml* view template and the additional "- Movie App" added in the layout file.</span></span>

<span data-ttu-id="4cfc0-145">또한 *Index.cshtml* 보기 템플릿에서 콘텐츠가 *Views/Shared/_Layout.cshtml* 보기 템플릿으로 병합되고 단일 HTML 응답이 브라우저로 전송된 방식을 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="4cfc0-145">Also notice how the content in the *Index.cshtml* view template was merged with the *Views/Shared/_Layout.cshtml* view template and a single HTML response was sent to the browser.</span></span> <span data-ttu-id="4cfc0-146">레이아웃 템플릿을 사용하면 응용 프로그램의 모든 페이지에 걸쳐 적용되는 변경 내용을 쉽게 만들 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4cfc0-146">Layout templates make it really easy to make changes that apply across all of the pages in your application.</span></span> <span data-ttu-id="4cfc0-147">자세한 내용은 [레이아웃](xref:mvc/views/layout)을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="4cfc0-147">To learn more see [Layout](xref:mvc/views/layout).</span></span>

![동영상 목록 보기](~/tutorials/first-mvc-app/adding-view/_static/hell3.png)

<span data-ttu-id="4cfc0-149">그렇지만 일부 “데이터”(이 경우 “Hello from our View Template!”</span><span class="sxs-lookup"><span data-stu-id="4cfc0-149">Our little bit of "data" (in this case the "Hello from our View Template!"</span></span> <span data-ttu-id="4cfc0-150">메시지)는 하드 코드되었습니다.</span><span class="sxs-lookup"><span data-stu-id="4cfc0-150">message) is hard-coded, though.</span></span> <span data-ttu-id="4cfc0-151">MVC 응용 프로그램에는 "V"(보기)가 있으며 "C"(컨트롤러)가 있지만 "M"(모델)은 아직 없습니다.</span><span class="sxs-lookup"><span data-stu-id="4cfc0-151">The MVC application has a "V" (view) and you've got a "C" (controller), but no "M" (model) yet.</span></span>

## <a name="passing-data-from-the-controller-to-the-view"></a><span data-ttu-id="4cfc0-152">컨트롤러에서 보기로 데이터 전달</span><span class="sxs-lookup"><span data-stu-id="4cfc0-152">Passing Data from the Controller to the View</span></span>

<span data-ttu-id="4cfc0-153">컨트롤러 작업은 들어오는 URL 요청에 대한 응답으로 호출됩니다.</span><span class="sxs-lookup"><span data-stu-id="4cfc0-153">Controller actions are invoked in response to an incoming URL request.</span></span> <span data-ttu-id="4cfc0-154">컨트롤러 클래스는 들어오는 브라우저 요청을 처리하는 코드를 작성하는 위치입니다.</span><span class="sxs-lookup"><span data-stu-id="4cfc0-154">A controller class is where you write the code that handles the incoming browser requests.</span></span> <span data-ttu-id="4cfc0-155">컨트롤러는 데이터 원본에서 데이터를 검색하고 브라우저에 다시 전송할 응답의 유형을 결정합니다.</span><span class="sxs-lookup"><span data-stu-id="4cfc0-155">The controller retrieves data from a data source and decides what type of response to send back to the browser.</span></span> <span data-ttu-id="4cfc0-156">보기 템플릿은 브라우저에 대한 HTML 응답을 만들고 서식 지정하도록 컨트롤러에서 사용될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4cfc0-156">View templates can be used from a controller to generate and format an HTML response to the browser.</span></span>

<span data-ttu-id="4cfc0-157">컨트롤러는 보기 템플릿에서 응답을 렌더링하기 위해 필요한 데이터를 제공할 책임이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4cfc0-157">Controllers are responsible for providing the data required in order for a view template to render a response.</span></span> <span data-ttu-id="4cfc0-158">모범 사례: 보기 템플릿은 비즈니스 논리를 수행하거나 데이터베이스와 직접 상호 작용하지 **않아야** 합니다.</span><span class="sxs-lookup"><span data-stu-id="4cfc0-158">A best practice: View templates should **not** perform business logic or interact with a database directly.</span></span> <span data-ttu-id="4cfc0-159">대신 보기 템플릿은 컨트롤러에 의해 제공되는 데이터와만 작동해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="4cfc0-159">Rather, a view template should work only with the data that's provided to it by the controller.</span></span> <span data-ttu-id="4cfc0-160">이 "문제의 분리"를 유지 관리하면 코드를 깨끗하고, 테스트 가능하고 유지 관리 가능하게 유지할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4cfc0-160">Maintaining this "separation of concerns" helps keep your code clean, testable, and maintainable.</span></span>

<span data-ttu-id="4cfc0-161">현재 `HelloWorldController` 클래스에서 `Welcome` 메서드는 `name` 및 `ID` 매개 변수를 가져온 다음 값을 브라우저로 직접 출력합니다.</span><span class="sxs-lookup"><span data-stu-id="4cfc0-161">Currently, the `Welcome` method in the `HelloWorldController` class takes a `name` and a `ID` parameter and then outputs the values directly to the browser.</span></span> <span data-ttu-id="4cfc0-162">컨트롤러가 이 응답을 문자열로 렌더링하게 하는 대신, 보기 템플릿을 사용하도록 컨트롤러를 변경합니다.</span><span class="sxs-lookup"><span data-stu-id="4cfc0-162">Rather than have the controller render this response as a string, change the controller to use a view template instead.</span></span> <span data-ttu-id="4cfc0-163">보기 템플릿은 응답을 생성합니다. 즉, 응답을 생성하기 위해 컨트롤러에서 보기로 일부 적절한 데이터를 전달해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="4cfc0-163">The view template generates a dynamic response, which means that appropriate bits of data must be passed from the controller to the view in order to generate the response.</span></span> <span data-ttu-id="4cfc0-164">보기 템플릿에서 액세스할 수 있도록 보기 템플릿이 `ViewData` 사전에서 필요한 동적 데이터(매개 변수)를 컨트롤러에서 배치하도록 하여 이를 수행합니다.</span><span class="sxs-lookup"><span data-stu-id="4cfc0-164">Do this by having the controller put the dynamic data (parameters) that the view template needs in a `ViewData` dictionary that the view template can then access.</span></span>

<span data-ttu-id="4cfc0-165">*HelloWorldController.cs* 파일로 돌아가서 `Message` 및 `NumTimes` 값을 `ViewData` 사전에 추가하도록 `Welcome` 메서드를 변경합니다.</span><span class="sxs-lookup"><span data-stu-id="4cfc0-165">Return to the *HelloWorldController.cs* file and change the `Welcome` method to add a `Message` and `NumTimes` value to the `ViewData` dictionary.</span></span> <span data-ttu-id="4cfc0-166">`ViewData` 사전은 동적 개체입니다. 즉, 원하는 것을 넣을 수 있습니다. 사용자가 어떤 것을 넣을 때까지 `ViewData` 개체에는 정의된 속성이 없습니다.</span><span class="sxs-lookup"><span data-stu-id="4cfc0-166">The `ViewData` dictionary is a dynamic object, which means you can put whatever you want in to it; the `ViewData` object has no defined properties until you put something inside it.</span></span> <span data-ttu-id="4cfc0-167">[MVC 모델 바인딩 시스템](xref:mvc/models/model-binding)은 주소 표시줄의 쿼리 문자열에서 메서드의 매개 변수로 명명된 매개 변수(`name` 및 `numTimes`)를 자동으로 매핑합니다.</span><span class="sxs-lookup"><span data-stu-id="4cfc0-167">The [MVC model binding system](xref:mvc/models/model-binding) automatically maps the named parameters (`name` and `numTimes`) from the query string in the address bar to parameters in your method.</span></span> <span data-ttu-id="4cfc0-168">전체 *HelloWorldController.cs* 파일은 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="4cfc0-168">The complete *HelloWorldController.cs* file looks like this:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_5)]

<span data-ttu-id="4cfc0-169">`ViewData` 사전 개체는 보기에 전달되는 데이터를 포함합니다.</span><span class="sxs-lookup"><span data-stu-id="4cfc0-169">The `ViewData` dictionary object contains data that will be passed to the view.</span></span> 

<span data-ttu-id="4cfc0-170">*Views/HelloWorld/Welcome.cshtml*이라는 시작 보기 템플릿을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="4cfc0-170">Create a Welcome view template named *Views/HelloWorld/Welcome.cshtml*.</span></span>

<span data-ttu-id="4cfc0-171">"Hello" `NumTimes`를 표시하는 *Welcome.cshtml* 보기 템플릿에 반복을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="4cfc0-171">You'll create a loop in the *Welcome.cshtml* view template that displays "Hello" `NumTimes`.</span></span> <span data-ttu-id="4cfc0-172">*Views/HelloWorld/Welcome.cshtml*의 콘텐츠를 다음으로 대체합니다.</span><span class="sxs-lookup"><span data-stu-id="4cfc0-172">Replace the contents of *Views/HelloWorld/Welcome.cshtml* with the following:</span></span>

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/HelloWorld/Welcome.cshtml)]

<span data-ttu-id="4cfc0-173">변경 내용을 저장하고 다음 URL로 이동합니다.</span><span class="sxs-lookup"><span data-stu-id="4cfc0-173">Save your changes and browse to the following URL:</span></span>

`http://localhost:xxxx/HelloWorld/Welcome?name=Rick&numtimes=4`

<span data-ttu-id="4cfc0-174">URL에서 데이터를 가져오고 [MVC 모델 바인더](xref:mvc/models/model-binding)를 사용하여 컨트롤러에 전달됩니다.</span><span class="sxs-lookup"><span data-stu-id="4cfc0-174">Data is taken from the URL and passed to the controller using the [MVC model binder](xref:mvc/models/model-binding) .</span></span> <span data-ttu-id="4cfc0-175">컨트롤러는 데이터를 `ViewData` 사전으로 패키징하고 해당 개체를 보기에 전달합니다.</span><span class="sxs-lookup"><span data-stu-id="4cfc0-175">The controller packages the data into a `ViewData` dictionary and passes that object to the view.</span></span> <span data-ttu-id="4cfc0-176">그런 다음 보기는 데이터를 HTML로 브라우저에 렌더링합니다.</span><span class="sxs-lookup"><span data-stu-id="4cfc0-176">The view then renders the data as HTML to the browser.</span></span>

![시작 레이블 및 네 번 표시되는 구 Hello Rick을 보여 주는 정보 보기](~/tutorials/first-mvc-app/adding-view/_static/rick2.png)

<span data-ttu-id="4cfc0-178">위의 샘플에서 `ViewData` 사전을 사용하여 컨트롤러에서 보기로 데이터를 전달했습니다.</span><span class="sxs-lookup"><span data-stu-id="4cfc0-178">In the sample above, we used the `ViewData` dictionary to pass data from the controller to a view.</span></span> <span data-ttu-id="4cfc0-179">자습서의 뒷부분에서 보기 모델을 사용하여 컨트롤러에서 보기로 데이터를 전달합니다.</span><span class="sxs-lookup"><span data-stu-id="4cfc0-179">Later in the tutorial, we will use a view model to pass data from a controller to a view.</span></span> <span data-ttu-id="4cfc0-180">데이터를 전달하는 보기 모델 방법은 일반적으로 `ViewData` 사전 접근 방법보다 훨씬 많이 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="4cfc0-180">The view model approach to passing data is generally much preferred over the `ViewData` dictionary approach.</span></span> <span data-ttu-id="4cfc0-181">자세한 내용은 [MVC에서 ViewModel vs ViewData vs ViewBag vs TempData vs Session](http://www.mytecbits.com/microsoft/dot-net/viewmodel-viewdata-viewbag-tempdata-mvc)을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="4cfc0-181">See [ViewModel vs ViewData vs ViewBag vs TempData vs Session in MVC](http://www.mytecbits.com/microsoft/dot-net/viewmodel-viewdata-viewbag-tempdata-mvc) for more information.</span></span>

<span data-ttu-id="4cfc0-182">이는 모델에 대한 일종의 "M"이었지만 데이터베이스 종류는 아니었습니다.</span><span class="sxs-lookup"><span data-stu-id="4cfc0-182">Well, that was a kind of an "M" for model, but not the database kind.</span></span> <span data-ttu-id="4cfc0-183">지금까지 학습한 것을 살펴보고 동영상의 데이터베이스를 만들어 보겠습니다.</span><span class="sxs-lookup"><span data-stu-id="4cfc0-183">Let's take what we've learned and create a database of movies.</span></span>
