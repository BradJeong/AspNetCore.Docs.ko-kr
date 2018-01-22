---
title: "앵커 태그 도우미 | Microsoft Docs"
author: pkellner
description: "앵커 태그 도우미를 사용 하는 방법을 보여 줍니다."
ms.author: riande
manager: wpickett
ms.date: 12/20/2017
ms.topic: article
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/anchor-tag-helper
ms.openlocfilehash: 7923876c792544ac4d559eb8de29475d8a4b37e0
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/19/2018
---
# <a name="anchor-tag-helper"></a><span data-ttu-id="a7a1e-103">앵커 태그 도우미</span><span class="sxs-lookup"><span data-stu-id="a7a1e-103">Anchor Tag Helper</span></span>

<span data-ttu-id="a7a1e-104">작성자: [Peter Kellner](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="a7a1e-104">By [Peter Kellner](http://peterkellner.net)</span></span> 

<span data-ttu-id="a7a1e-105">앵커 태그 도우미 향상 HTML 앵커 (`<a ... ></a>`) 새 특성을 추가 하 여는 태그입니다.</span><span class="sxs-lookup"><span data-stu-id="a7a1e-105">The Anchor Tag Helper enhances the HTML anchor (`<a ... ></a>`) tag by adding new attributes.</span></span> <span data-ttu-id="a7a1e-106">생성 된 링크 (에 `href` 태그) 새 특성을 사용 하 여 만들어집니다.</span><span class="sxs-lookup"><span data-stu-id="a7a1e-106">The link generated (on the `href` tag) is created using the new attributes.</span></span> <span data-ttu-id="a7a1e-107">해당 URL에 https와 같은 선택적 프로토콜은 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a7a1e-107">That URL can include an optional protocol such as https.</span></span>

<span data-ttu-id="a7a1e-108">다음 스피커 컨트롤러는이 문서에는 샘플에서 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="a7a1e-108">The speaker controller below is used in samples in this document.</span></span>

<span data-ttu-id="a7a1e-109">**SpeakerController.cs**</span><span class="sxs-lookup"><span data-stu-id="a7a1e-109">**SpeakerController.cs**</span></span> 

[!code-csharp[SpeakerController](sample/TagHelpersBuiltInAspNetCore/src/TagHelpersBuiltInAspNetCore/Controllers/SpeakerController.cs)]


## <a name="anchor-tag-helper-attributes"></a><span data-ttu-id="a7a1e-110">앵커 태그 도우미 특성</span><span class="sxs-lookup"><span data-stu-id="a7a1e-110">Anchor Tag Helper Attributes</span></span>

### <a name="asp-controller"></a><span data-ttu-id="a7a1e-111">asp-controller</span><span class="sxs-lookup"><span data-stu-id="a7a1e-111">asp-controller</span></span>

<span data-ttu-id="a7a1e-112">`asp-controller`URL을 생성 하는 컨트롤러를 연결 하는 데 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="a7a1e-112">`asp-controller` is used to associate which controller will be used to generate the URL.</span></span> <span data-ttu-id="a7a1e-113">지정 된 컨트롤러는 현재 프로젝트에 존재 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="a7a1e-113">The controllers specified must exist in the current project.</span></span> <span data-ttu-id="a7a1e-114">다음 코드는 모든 스피커를 나열합니다.</span><span class="sxs-lookup"><span data-stu-id="a7a1e-114">The following code lists all speakers:</span></span> 

```cshtml
<a asp-controller="Speaker" asp-action="Index">All Speakers</a>
```

<span data-ttu-id="a7a1e-115">생성된 된 태그가 됩니다.</span><span class="sxs-lookup"><span data-stu-id="a7a1e-115">The generated markup will be:</span></span>

```html
<a href="/Speaker">All Speakers</a>
```

<span data-ttu-id="a7a1e-116">경우는 `asp-controller` 지정 및 `asp-action` 않습니다 기본 `asp-action` 현재 실행 중인 뷰의 기본 컨트롤러 메서드가 됩니다.</span><span class="sxs-lookup"><span data-stu-id="a7a1e-116">If the `asp-controller` is specified and `asp-action` is not, the default `asp-action` will be the default controller method of the currently executing view.</span></span> <span data-ttu-id="a7a1e-117">위 예에서 경우 `asp-action` 생략이 앵커 태그 도우미에서 생성 되 고 *HomeController*의 `Index` 보기 (**/홈**), 생성된 된 태그 됩니다:</span><span class="sxs-lookup"><span data-stu-id="a7a1e-117">That is, in the above example, if `asp-action` is left out, and this Anchor Tag Helper is generated from *HomeController*'s `Index` view (**/Home**), the generated markup will be:</span></span>

```html
<a href="/Home">All Speakers</a>
```

### <a name="asp-action"></a><span data-ttu-id="a7a1e-118">asp-action</span><span class="sxs-lookup"><span data-stu-id="a7a1e-118">asp-action</span></span>

<span data-ttu-id="a7a1e-119">`asp-action`포함할 컨트롤러의 동작 메서드의 이름으로 생성 된 `href`합니다.</span><span class="sxs-lookup"><span data-stu-id="a7a1e-119">`asp-action` is the name of the action method in the controller that will be included in the generated `href`.</span></span> <span data-ttu-id="a7a1e-120">예를 들어 다음 코드는 생성 된 설정 `href` 스피커 세부 정보 페이지를 가리키도록 합니다.</span><span class="sxs-lookup"><span data-stu-id="a7a1e-120">For example, the following code set the generated `href` to point to the speaker detail page:</span></span>

```html
<a asp-controller="Speaker" asp-action="Detail">Speaker Detail</a>
```

<span data-ttu-id="a7a1e-121">생성된 된 태그가 됩니다.</span><span class="sxs-lookup"><span data-stu-id="a7a1e-121">The generated markup will be:</span></span>

```html
<a href="/Speaker/Detail">Speaker Detail</a>
```

<span data-ttu-id="a7a1e-122">되지 않은 경우 `asp-controller` 특성 지정, 호출 현재 뷰를 실행 하는 보기 기본 컨트롤러가 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="a7a1e-122">If no `asp-controller` attribute is specified, the default controller calling the view executing the current view will be used.</span></span>  
 
<span data-ttu-id="a7a1e-123">경우 특성 `asp-action` 은 `Index`, 아무 작업도 앞에 기본 URL에 추가 됩니다 `Index` 메서드를 호출 합니다.</span><span class="sxs-lookup"><span data-stu-id="a7a1e-123">If the attribute `asp-action` is `Index`, then no action is appended to the URL, leading to the default `Index` method being called.</span></span> <span data-ttu-id="a7a1e-124">작업이 지정 된 (또는 기본 설정)에서 참조 되는 컨트롤러에 있어야 `asp-controller`합니다.</span><span class="sxs-lookup"><span data-stu-id="a7a1e-124">The action specified (or defaulted), must exist in the controller referenced in `asp-controller`.</span></span>

### <a name="asp-page"></a><span data-ttu-id="a7a1e-125">asp-page</span><span class="sxs-lookup"><span data-stu-id="a7a1e-125">asp-page</span></span>

<span data-ttu-id="a7a1e-126">사용 하 여는 `asp-page` 특정 페이지를 가리키도록 해당 URL을 설정 하는 앵커 태그의 특성입니다.</span><span class="sxs-lookup"><span data-stu-id="a7a1e-126">Use the `asp-page` attribute in an anchor tag to set its URL to point to a specific page.</span></span> <span data-ttu-id="a7a1e-127">슬래시를 사용 하 여 페이지 이름을 접두사로 사용 "/" URL을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="a7a1e-127">Prefixing the page name with a forward slash "/" creates the URL.</span></span> <span data-ttu-id="a7a1e-128">현재 디렉터리에 "스피커" 페이지를 가리키고 아래의 예제에 대 한 URL입니다.</span><span class="sxs-lookup"><span data-stu-id="a7a1e-128">The URL in the sample below points to the "Speaker" page in the current directory.</span></span>

```cshtml
<a asp-page="/Speakers">All Speakers</a>
```

<span data-ttu-id="a7a1e-129">`asp-page` 특성 앞의 코드 예제에 다음 코드 조각에서와 비슷한 뷰에서 HTML 출력을 렌더링 합니다.</span><span class="sxs-lookup"><span data-stu-id="a7a1e-129">The `asp-page` attribute in the previous code sample renders HTML output in the view that is similar to the following snippet:</span></span>

```html
<a href="/items?page=%2FSpeakers">Speakers</a>
```

<span data-ttu-id="a7a1e-130">`asp-page` 특성은 함께 사용할 수 없습니다는 `asp-route`, `asp-controller`, 및 `asp-action` 특성입니다.</span><span class="sxs-lookup"><span data-stu-id="a7a1e-130">The `asp-page` attribute is mutually exclusive with the `asp-route`, `asp-controller`, and `asp-action` attributes.</span></span> <span data-ttu-id="a7a1e-131">그러나 `asp-page` 와 함께 사용할 수 `asp-route-id` 다음 코드 샘플에서 보여 주듯이 라우팅을 제어할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a7a1e-131">However, `asp-page` can be used with `asp-route-id` to control routing, as the following code sample demonstrates:</span></span>

```cshtml
<a asp-page="/Speaker" asp-route-id="@speaker.Id">View Speaker</a>
```

<span data-ttu-id="a7a1e-132">`asp-route-id` 는 다음과 같은 출력을 생성 합니다.</span><span class="sxs-lookup"><span data-stu-id="a7a1e-132">The `asp-route-id` produces the following output:</span></span>

```html
https://localhost:44399/Speakers/Index/2?page=%2FSpeaker
```

> [!NOTE]
> <span data-ttu-id="a7a1e-133">사용 하는 `asp-page` 의 특성 Razor 페이지, Url 해야 상대 경로 예를 들어 `"./Speaker"`합니다.</span><span class="sxs-lookup"><span data-stu-id="a7a1e-133">To use the `asp-page` attribute in Razor Pages, the URLs must be a relative path, for example `"./Speaker"`.</span></span> <span data-ttu-id="a7a1e-134">상대 경로 `asp-page` 특성에서에서 사용할 수 없는 MVC 뷰.</span><span class="sxs-lookup"><span data-stu-id="a7a1e-134">Relative paths in the `asp-page` attribute are not available in MVC views.</span></span> <span data-ttu-id="a7a1e-135">대신 MVC 뷰에 대 한 "/" 구문을 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="a7a1e-135">Use the "/" syntax for MVC views instead.</span></span>

### <a name="asp-route-value"></a><span data-ttu-id="a7a1e-136">asp-route-{value}</span><span class="sxs-lookup"><span data-stu-id="a7a1e-136">asp-route-{value}</span></span>

<span data-ttu-id="a7a1e-137">`asp-route-`와일드 카드 경로 접두사입니다.</span><span class="sxs-lookup"><span data-stu-id="a7a1e-137">`asp-route-` is a wild card route prefix.</span></span> <span data-ttu-id="a7a1e-138">모든 값을 추가한 후 뒤에 오는 대시 잠재적 경로 매개 변수로 해석 됩니다.</span><span class="sxs-lookup"><span data-stu-id="a7a1e-138">Any value you put after the trailing dash will be interpreted as a potential route parameter.</span></span> <span data-ttu-id="a7a1e-139">기본 경로가 없는 경우이 경로 접두사 요청 매개 변수 및 값으로 생성 된 href에 추가 됩니다.</span><span class="sxs-lookup"><span data-stu-id="a7a1e-139">If a default route is not found, this route prefix will be appended to the generated href as a request parameter and value.</span></span> <span data-ttu-id="a7a1e-140">그렇지 않은 경우에 경로 템플릿을 대체 됩니다.</span><span class="sxs-lookup"><span data-stu-id="a7a1e-140">Otherwise it will be substituted in the route template.</span></span>

<span data-ttu-id="a7a1e-141">가정 하 고 있는 컨트롤러 메서드가 다음과 같이 정의.</span><span class="sxs-lookup"><span data-stu-id="a7a1e-141">Assuming you have a controller method defined as follows:</span></span>

```csharp
public IActionResult AnchorTagHelper(string id)
{
    var speaker = new SpeakerData()
    {
        SpeakerId = 12
    };
    return View(viewName, speaker);
}
```

<span data-ttu-id="a7a1e-142">에 정의 된 기본 경로 템플릿을 있고 프로그램 *Startup.cs* 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="a7a1e-142">And have the default route template defined in your *Startup.cs* as follows:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
});

```

<span data-ttu-id="a7a1e-143">**cshtml** 앵커 태그 도우미를 사용 하는 데 필요한 포함 된 파일의 **스피커** 보기에는 컨트롤러에서 전달 하는 모델 매개 변수는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="a7a1e-143">The **cshtml** file that contains the Anchor Tag Helper necessary to use the **speaker** model parameter passed in from the controller to the view is as follows:</span></span>

```cshtml
@model SpeakerData
<!DOCTYPE html>
<html><body>
<a asp-controller='Speaker' asp-action='Detail' asp-route-id=@Model.SpeakerId>SpeakerId: @Model.SpeakerId</a>
<body></html>
```

<span data-ttu-id="a7a1e-144">생성 된 HTML 때문에 다음과 같이 높아집니다 다음 **id** 기본 경로에서 찾았습니다.</span><span class="sxs-lookup"><span data-stu-id="a7a1e-144">The generated HTML will then be as follows because **id** was found in the default route.</span></span>

```html
<a href='/Speaker/Detail/12'>SpeakerId: 12</a>
```

<span data-ttu-id="a7a1e-145">경로 접두사 발견 라우팅 서식 파일의 일부가 아닌 경우, 즉 다음과 같은 **cshtml** 파일:</span><span class="sxs-lookup"><span data-stu-id="a7a1e-145">If the route prefix is not part of the routing template found, which is the case with the following **cshtml** file:</span></span>

```cshtml
@model SpeakerData
<!DOCTYPE html>
<html><body>
<a asp-controller='Speaker' asp-action='Detail' asp-route-speakerid=@Model.SpeakerId>SpeakerId: @Model.SpeakerId</a>
<body></html>
```

<span data-ttu-id="a7a1e-146">생성 된 HTML 때문에 다음과 같이 높아집니다 다음 **speakerid** 일치 경로에서 찾을 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="a7a1e-146">The generated HTML will then be as follows because **speakerid** was not found in the route matched:</span></span>

```html
<a href='/Speaker/Detail?speakerid=12'>SpeakerId: 12</a>
```

<span data-ttu-id="a7a1e-147">어느 경우 `asp-controller` 또는 `asp-action` 는 지정 하지 않으면 되 고 뒤에 동일한 기본 처리는 `asp-route` 특성입니다.</span><span class="sxs-lookup"><span data-stu-id="a7a1e-147">If either `asp-controller` or `asp-action` are not specified, then the same default processing is followed as is in the `asp-route` attribute.</span></span>

### <a name="asp-route"></a><span data-ttu-id="a7a1e-148">asp-route</span><span class="sxs-lookup"><span data-stu-id="a7a1e-148">asp-route</span></span>

<span data-ttu-id="a7a1e-149">`asp-route`명명 된 경로에 직접 연결 되는 URL을 만들 방법을 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="a7a1e-149">`asp-route` provides a way to create a URL that links directly to a named route.</span></span> <span data-ttu-id="a7a1e-150">라우팅 특성을 사용 하는 경로 이름을 지정할 수 있습니다에 표시 된 대로 `SpeakerController` 에서 사용 하 고 해당 `Evaluations` 메서드.</span><span class="sxs-lookup"><span data-stu-id="a7a1e-150">Using routing attributes, a route can be named as shown in the `SpeakerController` and used in its `Evaluations` method.</span></span>

<span data-ttu-id="a7a1e-151">`Name = "speakerevals"`URL을 사용 하 여 해당 컨트롤러 메서드에 대 한 직접 경로 생성 하는 앵커 태그 도우미 지시 `/Speaker/Evaluations`합니다.</span><span class="sxs-lookup"><span data-stu-id="a7a1e-151">`Name = "speakerevals"` tells the Anchor Tag Helper to generate a route directly to that controller method using the URL `/Speaker/Evaluations`.</span></span> <span data-ttu-id="a7a1e-152">경우 `asp-controller` 또는 `asp-action` 외에 지정 `asp-route`, 예상 대로 경로가 생성 되지 않을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a7a1e-152">If `asp-controller` or `asp-action` is specified in addition to `asp-route`, the route generated may not be what you expect.</span></span> <span data-ttu-id="a7a1e-153">`asp-route`특성 중 하나를 사용할 수 없습니다 `asp-controller` 또는 `asp-action` 경로 충돌 하지 않도록 합니다.</span><span class="sxs-lookup"><span data-stu-id="a7a1e-153">`asp-route` should not be used with either of the attributes `asp-controller` or `asp-action` to avoid a route conflict.</span></span>

### <a name="asp-all-route-data"></a><span data-ttu-id="a7a1e-154">asp-all-route-data</span><span class="sxs-lookup"><span data-stu-id="a7a1e-154">asp-all-route-data</span></span>

<span data-ttu-id="a7a1e-155">`asp-all-route-data`키/값 쌍에 키가 매개 변수 이름 및 값은 해당 키와 연결 된 값의 사전을 만들 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a7a1e-155">`asp-all-route-data` allows creating a dictionary of key value pairs where the key is the parameter name and the value is the value associated with that key.</span></span>

<span data-ttu-id="a7a1e-156">다음 예제와 인라인 사전이 만들어지고 razor 보기에 데이터가 전달 됩니다.</span><span class="sxs-lookup"><span data-stu-id="a7a1e-156">As the example below shows, an inline dictionary is created and the data is passed to the razor view.</span></span> <span data-ttu-id="a7a1e-157">대신 데이터 모델을 사용 하 여 전달할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a7a1e-157">As an alternative, the data could also be passed in with your model.</span></span>

```cshtml
@{
    var dict =
        new Dictionary<string, string>
        {
            {"speakerId", "11"},
            {"currentYear", "true"}
        };
}
<a asp-route="speakerevalscurrent"
asp-all-route-data="dict">SpeakerEvals</a>
```

<span data-ttu-id="a7a1e-158">위의 코드에서는 다음 URL을 생성 합니다: http://localhost/Speaker/EvaluationsCurrent?speakerId=11&currentYear=true</span><span class="sxs-lookup"><span data-stu-id="a7a1e-158">The code above generates the following URL: http://localhost/Speaker/EvaluationsCurrent?speakerId=11&currentYear=true</span></span>

<span data-ttu-id="a7a1e-159">링크를 클릭할 때, 컨트롤러 메서드에 `EvaluationsCurrent` 호출 됩니다.</span><span class="sxs-lookup"><span data-stu-id="a7a1e-159">When the link is clicked, the controller method `EvaluationsCurrent` is called.</span></span> <span data-ttu-id="a7a1e-160">해당 컨트롤러에서 생성 된 어떤 일치 하는 두 개의 문자열 매개 변수를에 있기 때문에 호출 되는 `asp-all-route-data` 사전입니다.</span><span class="sxs-lookup"><span data-stu-id="a7a1e-160">It is called because that controller has two string parameters that match what has been created from the `asp-all-route-data` dictionary.</span></span>

<span data-ttu-id="a7a1e-161">사전 일치 항목의 키는 매개 변수를 라우팅할 적절 하 게 경로에 해당 값이 대체 됩니다 및 요청 매개 변수도 생성 됩니다. 다른 일치 하지 않는 값입니다.</span><span class="sxs-lookup"><span data-stu-id="a7a1e-161">If any keys in the dictionary match route parameters, those values will be substituted in the route as appropriate and the other non-matching values will be generated as request parameters.</span></span>

### <a name="asp-fragment"></a><span data-ttu-id="a7a1e-162">asp-fragment</span><span class="sxs-lookup"><span data-stu-id="a7a1e-162">asp-fragment</span></span>

<span data-ttu-id="a7a1e-163">`asp-fragment`URL에 추가할 URL 조각을 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="a7a1e-163">`asp-fragment` defines a URL fragment to append to the URL.</span></span> <span data-ttu-id="a7a1e-164">앵커 태그 도우미 해시 문자를 추가 합니다 (#).</span><span class="sxs-lookup"><span data-stu-id="a7a1e-164">The Anchor Tag Helper will add the hash character (#).</span></span> <span data-ttu-id="a7a1e-165">태그를 만들면:</span><span class="sxs-lookup"><span data-stu-id="a7a1e-165">If you create a tag:</span></span>

```cshtml
<a asp-action="Evaluations" asp-controller="Speaker"  
   asp-fragment="SpeakerEvaluations">About Speaker Evals</a>
```

<span data-ttu-id="a7a1e-166">생성 된 URL 됩니다: http://localhost/Speaker/Evaluations#SpeakerEvaluations</span><span class="sxs-lookup"><span data-stu-id="a7a1e-166">The generated URL will be: http://localhost/Speaker/Evaluations#SpeakerEvaluations</span></span>

<span data-ttu-id="a7a1e-167">해시 태그는 클라이언트 쪽 응용 프로그램을 작성할 때 유용 합니다.</span><span class="sxs-lookup"><span data-stu-id="a7a1e-167">Hash tags are useful when building client-side applications.</span></span> <span data-ttu-id="a7a1e-168">쉽게 표시 하 고 예를 들어 JavaScript에서 검색에 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a7a1e-168">They can be used for easy marking and searching in JavaScript, for example.</span></span>

### <a name="asp-area"></a><span data-ttu-id="a7a1e-169">asp 영역</span><span class="sxs-lookup"><span data-stu-id="a7a1e-169">asp-area</span></span>

<span data-ttu-id="a7a1e-170">`asp-area`ASP.NET Core 사용 하 여 적절 한 경로 설정 하는 영역 이름을 설정 합니다.</span><span class="sxs-lookup"><span data-stu-id="a7a1e-170">`asp-area` sets the area name that ASP.NET Core uses to set the appropriate route.</span></span> <span data-ttu-id="a7a1e-171">어떻게 영역 특성으로 인해 경로 다시 매핑하여의 예는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="a7a1e-171">Below are examples of how the area attribute causes a remapping of routes.</span></span> <span data-ttu-id="a7a1e-172">설정 `asp-area` 블로그에 디렉터리 접두사 `Areas/Blogs` 관련된 컨트롤러와이 앵커 태그에 대 한 뷰 경로에 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a7a1e-172">Setting `asp-area` to Blogs prefixes the directory `Areas/Blogs` to the routes of the associated controllers and views for this anchor tag.</span></span>

* <span data-ttu-id="a7a1e-173">프로젝트 이름</span><span class="sxs-lookup"><span data-stu-id="a7a1e-173">Project name</span></span>
  * <span data-ttu-id="a7a1e-174">wwwroot</span><span class="sxs-lookup"><span data-stu-id="a7a1e-174">wwwroot</span></span>
  * <span data-ttu-id="a7a1e-175">영역</span><span class="sxs-lookup"><span data-stu-id="a7a1e-175">Areas</span></span>
    * <span data-ttu-id="a7a1e-176">블로그</span><span class="sxs-lookup"><span data-stu-id="a7a1e-176">Blogs</span></span>
      * <span data-ttu-id="a7a1e-177">컨트롤러</span><span class="sxs-lookup"><span data-stu-id="a7a1e-177">Controllers</span></span>
        * <span data-ttu-id="a7a1e-178">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="a7a1e-178">HomeController.cs</span></span>
      * <span data-ttu-id="a7a1e-179">보기</span><span class="sxs-lookup"><span data-stu-id="a7a1e-179">Views</span></span>
        * <span data-ttu-id="a7a1e-180">홈</span><span class="sxs-lookup"><span data-stu-id="a7a1e-180">Home</span></span>
          * <span data-ttu-id="a7a1e-181">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="a7a1e-181">Index.cshtml</span></span>
          * <span data-ttu-id="a7a1e-182">AboutBlog.cshtml</span><span class="sxs-lookup"><span data-stu-id="a7a1e-182">AboutBlog.cshtml</span></span>
  * <span data-ttu-id="a7a1e-183">컨트롤러</span><span class="sxs-lookup"><span data-stu-id="a7a1e-183">Controllers</span></span>

<span data-ttu-id="a7a1e-184">와 같은 유효한 영역 태그 지정 ```area="Blogs"``` 참조 하는 경우는 ```AboutBlog.cshtml``` 파일은 다음과 같습니다 앵커 태그 도우미를 사용 하 여 합니다.</span><span class="sxs-lookup"><span data-stu-id="a7a1e-184">Specifying an area tag that is valid, such as ```area="Blogs"``` when referencing the ```AboutBlog.cshtml``` file will look like the following using the Anchor Tag Helper.</span></span>

```cshtml
<a asp-action="AboutBlog" asp-controller="Home" asp-area="Blogs">Blogs About</a>
```

<span data-ttu-id="a7a1e-185">생성 된 HTML 영역 세그먼트를 포함 됩니다 및 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="a7a1e-185">The generated HTML will include the areas segment and will be as follows:</span></span>

```html
<a href="/Blogs/Home/AboutBlog">Blogs About</a>
```

> [!TIP]
> <span data-ttu-id="a7a1e-186">웹 응용 프로그램에서 작동 하도록 MVC 영역 경로 템플릿이 있는 경우 영역에 대 한 참조가 포함 되어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="a7a1e-186">For MVC areas to work in a web application, the route template must include a reference to the area if it exists.</span></span> <span data-ttu-id="a7a1e-187">두 번째 매개 변수는 해당 서식 파일의는 `routes.MapRoute` 메서드 호출으로 표시 됩니다.`template: '"{area:exists}/{controller=Home}/{action=Index}"'`</span><span class="sxs-lookup"><span data-stu-id="a7a1e-187">That template, which is the second parameter of the `routes.MapRoute` method call, will appear as: `template: '"{area:exists}/{controller=Home}/{action=Index}"'`</span></span>

### <a name="asp-protocol"></a><span data-ttu-id="a7a1e-188">asp-protocol</span><span class="sxs-lookup"><span data-stu-id="a7a1e-188">asp-protocol</span></span>

<span data-ttu-id="a7a1e-189">`asp-protocol` 프로토콜을 지정 하는 (예: `https`) URL에서 합니다.</span><span class="sxs-lookup"><span data-stu-id="a7a1e-189">The `asp-protocol` is for specifying a protocol (such as `https`) in your URL.</span></span> <span data-ttu-id="a7a1e-190">앵커 태그 도우미 프로토콜을 포함 하는 예제는 다음과 같이 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="a7a1e-190">An example Anchor Tag Helper that includes the protocol will look as follows:</span></span>

```<a asp-protocol="https" asp-action="About" asp-controller="Home">About</a>```

<span data-ttu-id="a7a1e-191">및 HTML을 다음과 같이 생성 됩니다.</span><span class="sxs-lookup"><span data-stu-id="a7a1e-191">and will generate HTML as follows:</span></span>

```<a href="https://localhost/Home/About">About</a>```

<span data-ttu-id="a7a1e-192">예제에서 도메인 localhost, 하지만 앵커 태그 도우미의 URL을 생성 하는 경우 웹 사이트의 공용 도메인 사용 하 여 합니다.</span><span class="sxs-lookup"><span data-stu-id="a7a1e-192">The domain in the example is localhost, but the Anchor Tag Helper uses the website's public domain when generating the URL.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a7a1e-193">추가 리소스</span><span class="sxs-lookup"><span data-stu-id="a7a1e-193">Additional resources</span></span>

* [<span data-ttu-id="a7a1e-194">영역</span><span class="sxs-lookup"><span data-stu-id="a7a1e-194">Areas</span></span>](xref:mvc/controllers/areas)
