---
title: ASP.NET Core의 앵커 태그 도우미
author: pkellner
description: ASP.NET Core 앵커 태그 도우미 특성 및 HTML 앵커 태그의 동작을 확장할 때 각 특성이 담당하는 역할을 확인합니다.
ms.author: scaddie
ms.custom: mvc
ms.date: 10/10/2018
uid: mvc/views/tag-helpers/builtin-th/anchor-tag-helper
ms.openlocfilehash: 13508729c1e3b64a8b0e6965da57880738ab85c3
ms.sourcegitcommit: 4bdf7703aed86ebd56b9b4bae9ad5700002af32d
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/15/2018
ms.locfileid: "49325551"
---
# <a name="anchor-tag-helper-in-aspnet-core"></a><span data-ttu-id="10923-103">ASP.NET Core의 앵커 태그 도우미</span><span class="sxs-lookup"><span data-stu-id="10923-103">Anchor Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="10923-104">작성자: [Peter Kellner](http://peterkellner.net) 및 [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="10923-104">By [Peter Kellner](http://peterkellner.net) and [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="10923-105">[앵커 태그 도우미](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper)는 새 특성을 추가하여 표준 HTML 앵커(`<a ... ></a>`) 태그를 향상시킵니다.</span><span class="sxs-lookup"><span data-stu-id="10923-105">The [Anchor Tag Helper](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper) enhances the standard HTML anchor (`<a ... ></a>`) tag by adding new attributes.</span></span> <span data-ttu-id="10923-106">규칙에 따라 해당 특성들의 이름은 `asp-` 접두사로 시작됩니다.</span><span class="sxs-lookup"><span data-stu-id="10923-106">By convention, the attribute names are prefixed with `asp-`.</span></span> <span data-ttu-id="10923-107">렌더링 된 앵커 요소의 `href` 특성값은 `asp-` 특성값에 따라 결정됩니다.</span><span class="sxs-lookup"><span data-stu-id="10923-107">The rendered anchor element's `href` attribute value is determined by the values of the `asp-` attributes.</span></span>

<span data-ttu-id="10923-108">태그 도우미에 대한 개요는 <xref:mvc/views/tag-helpers/intro>를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="10923-108">For an overview of Tag Helpers, see <xref:mvc/views/tag-helpers/intro>.</span></span>

<span data-ttu-id="10923-109">[예제 코드 살펴보기 및 다운로드](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/built-in/samples)([다운로드 방법](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="10923-109">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/built-in/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="10923-110">이 문서의 예제 전반에서는 다음의 *SpeakerController*가 사용됩니다. </span><span class="sxs-lookup"><span data-stu-id="10923-110">*SpeakerController* is used in samples throughout this document:</span></span>

[!code-csharp[](samples/TagHelpersBuiltIn/Controllers/SpeakerController.cs?name=snippet_SpeakerController)]

<span data-ttu-id="10923-111">`asp-` 특성의 목록은 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="10923-111">An inventory of the `asp-` attributes follows.</span></span>

## <a name="asp-controller"></a><span data-ttu-id="10923-112">asp-controller</span><span class="sxs-lookup"><span data-stu-id="10923-112">asp-controller</span></span>

<span data-ttu-id="10923-113">[asp-controller](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.controller) 특성은 URL 생성에 사용되는 컨트롤러를 지정합니다.</span><span class="sxs-lookup"><span data-stu-id="10923-113">The [asp-controller](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.controller) attribute assigns the controller used for generating the URL.</span></span> <span data-ttu-id="10923-114">다음 태그는 모든 스피커를 나열합니다.</span><span class="sxs-lookup"><span data-stu-id="10923-114">The following markup lists all speakers:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspController)]

<span data-ttu-id="10923-115">생성된 HTML:</span><span class="sxs-lookup"><span data-stu-id="10923-115">The generated HTML:</span></span>

```html
<a href="/Speaker">All Speakers</a>
```

<span data-ttu-id="10923-116">`asp-controller` 특성이 지정되고 `asp-action`이 지정되지 않았을 경우, 기본 `asp-action` 값은 현재 실행 중인 뷰와 연결된 컨트롤러 액션입니다.</span><span class="sxs-lookup"><span data-stu-id="10923-116">If the `asp-controller` attribute is specified and `asp-action` isn't, the default `asp-action` value is the controller action associated with the currently executing view.</span></span> <span data-ttu-id="10923-117">*HomeController*의 *Index* 뷰(*/Home*)에서 위의 태그에서 `asp-action`을 생략한 앵커 태그 도우미를 사용할 경우, 생성되는 HTML은 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="10923-117">If `asp-action` is omitted from the preceding markup, and the Anchor Tag Helper is used in *HomeController*'s *Index* view (*/Home*), the generated HTML is:</span></span>

```html
<a href="/Home">All Speakers</a>
```

## <a name="asp-action"></a><span data-ttu-id="10923-118">asp-action</span><span class="sxs-lookup"><span data-stu-id="10923-118">asp-action</span></span>

<span data-ttu-id="10923-119">[asp-action](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.action) 특성값은 생성되는 `href` 특성에 포함될 컨트롤러 액션의 이름을 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="10923-119">The [asp-action](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.action) attribute value represents the controller action name included in the generated `href` attribute.</span></span> <span data-ttu-id="10923-120">다음 태그는 생성된 `href` 특성값을 스피커 평가 페이지로 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="10923-120">The following markup sets the generated `href` attribute value to the speaker evaluations page:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspAction)]

<span data-ttu-id="10923-121">생성된 HTML:</span><span class="sxs-lookup"><span data-stu-id="10923-121">The generated HTML:</span></span>

```html
<a href="/Speaker/Evaluations">Speaker Evaluations</a>
```

<span data-ttu-id="10923-122">`asp-controller` 특성을 지정하지 않으면 현재 뷰를 실행 중인 뷰를 호출하는 기본 컨트롤러가 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="10923-122">If no `asp-controller` attribute is specified, the default controller calling the view executing the current view is used.</span></span>

<span data-ttu-id="10923-123">`asp-action` 특성값이 `Index`면 URL에 아무런 액션도 추가되지 않으며 기본 `Index` 액션이 호출됩니다.</span><span class="sxs-lookup"><span data-stu-id="10923-123">If the `asp-action` attribute value is `Index`, then no action is appended to the URL, leading to the invocation of the default `Index` action.</span></span> <span data-ttu-id="10923-124">지정된 (또는 기본값으로 설정된) 액션은 `asp-controller`에서 참조되는 컨트롤러에 존재해야 합니다. </span><span class="sxs-lookup"><span data-stu-id="10923-124">The action specified (or defaulted), must exist in the controller referenced in `asp-controller`.</span></span>

## <a name="asp-route-value"></a><span data-ttu-id="10923-125">asp-route-{value}</span><span class="sxs-lookup"><span data-stu-id="10923-125">asp-route-{value}</span></span>

<span data-ttu-id="10923-126">[asp-route-{value}](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.routevalues) 특성을 사용하면 와일드카드 경로 접두사를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="10923-126">The [asp-route-{value}](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.routevalues) attribute enables a wildcard route prefix.</span></span> <span data-ttu-id="10923-127">`{value}` 자리 표시자에 위치하는 모든 값은 잠재적인 경로 매개 변수로 해석됩니다. </span><span class="sxs-lookup"><span data-stu-id="10923-127">Any value occupying the `{value}` placeholder is interpreted as a potential route parameter.</span></span> <span data-ttu-id="10923-128">기본 경로가 발견되지 않으면 이 경로 접두사는 생성되는 `href` 특성에 요청 매개 변수 및 값으로 추가됩니다.</span><span class="sxs-lookup"><span data-stu-id="10923-128">If a default route isn't found, this route prefix is appended to the generated `href` attribute as a request parameter and value.</span></span> <span data-ttu-id="10923-129">그렇지 않으면 경로 템플릿에서 이 경로 접두사가 대체됩니다.</span><span class="sxs-lookup"><span data-stu-id="10923-129">Otherwise, it's substituted in the route template.</span></span>

<span data-ttu-id="10923-130">다음 컨트롤러 액션을 살펴보시기 바랍니다.</span><span class="sxs-lookup"><span data-stu-id="10923-130">Consider the following controller action:</span></span>

[!code-csharp[](samples/TagHelpersBuiltIn/Controllers/BuiltInTagController.cs?name=snippet_AnchorTagHelperAction)]

<span data-ttu-id="10923-131">*Startup.Configure*에 정의된 기본 경로 템플릿을 사용할 경우: </span><span class="sxs-lookup"><span data-stu-id="10923-131">With a default route template defined in *Startup.Configure*:</span></span>

[!code-csharp[](samples/TagHelpersBuiltIn/Startup.cs?name=snippet_UseMvc&highlight=8-10)]

<span data-ttu-id="10923-132">다음 MVC 뷰는 액션을 통해서 제공되는 모델을 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="10923-132">The MVC view uses the model, provided by the action, as follows:</span></span>

```cshtml
@model Speaker
<!DOCTYPE html>
<html>
<body>
    <a asp-controller="Speaker"
       asp-action="Detail" 
       asp-route-id="@Model.SpeakerId">SpeakerId: @Model.SpeakerId</a>
</body>
</html>
```

<span data-ttu-id="10923-133">이 경우 기본 경로의 `{id?}` 자리 표시자가 일치합니다.</span><span class="sxs-lookup"><span data-stu-id="10923-133">The default route's `{id?}` placeholder was matched.</span></span> <span data-ttu-id="10923-134">생성된 HTML:</span><span class="sxs-lookup"><span data-stu-id="10923-134">The generated HTML:</span></span>

```html
<a href="/Speaker/Detail/12">SpeakerId: 12</a>
```

<span data-ttu-id="10923-135">이번에는 다음 MVC 뷰와 같이 경로 접두사가 라우팅 템플릿의 일부와 일치하지 않는다고 가정해보겠습니다.</span><span class="sxs-lookup"><span data-stu-id="10923-135">Assume the route prefix isn't part of the matching routing template, as with the following MVC view:</span></span>

```cshtml
@model Speaker
<!DOCTYPE html>
<html>
<body>
    <a asp-controller="Speaker" 
       asp-action="Detail" 
       asp-route-speakerid="@Model.SpeakerId">SpeakerId: @Model.SpeakerId</a>
<body>
</html>
```

<span data-ttu-id="10923-136">일치하는 경로에서 `speakerid`를 찾을 수 없기 때문에 생성된 HTML은 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="10923-136">The following HTML is generated because `speakerid` wasn't found in the matching route:</span></span>

```html
<a href="/Speaker/Detail?speakerid=12">SpeakerId: 12</a>
```

<span data-ttu-id="10923-137">`asp-controller` 또는 `asp-action`이 지정되지 않으면 `asp-route` 특성과 동일한 기본 처리가 그대로 수행됩니다.</span><span class="sxs-lookup"><span data-stu-id="10923-137">If either `asp-controller` or `asp-action` aren't specified, then the same default processing is followed as is in the `asp-route` attribute.</span></span>

## <a name="asp-route"></a><span data-ttu-id="10923-138">asp-route</span><span class="sxs-lookup"><span data-stu-id="10923-138">asp-route</span></span>

<span data-ttu-id="10923-139">[asp-route](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.route) 특성은 명명된 경로에 직접 연결되는 URL을 생성하기 위해서 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="10923-139">The [asp-route](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.route) attribute is used for creating a URL linking directly to a named route.</span></span> <span data-ttu-id="10923-140">[라우팅 특성](xref:mvc/controllers/routing#attribute-routing)을 사용하면 경로가 `SpeakerController`에 표시된 이름으로 지정되고 해당 `Evaluations` 메서드에서 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="10923-140">Using [routing attributes](xref:mvc/controllers/routing#attribute-routing), a route can be named as shown in the `SpeakerController` and used in its `Evaluations` action:</span></span>

[!code-csharp[](samples/TagHelpersBuiltIn/Controllers/SpeakerController.cs?range=22-24)]

<span data-ttu-id="10923-141">다음 태그에서 `asp-route` 특성은 명명된 경로를 참조합니다.</span><span class="sxs-lookup"><span data-stu-id="10923-141">In the following markup, the `asp-route` attribute references the named route:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspRoute)]

<span data-ttu-id="10923-142">앵커 태그 도우미는 */Speaker/Evaluations* URL을 사용하여 해당 컨트롤러 액션을 직접 가리키는 경로를 생성합니다.</span><span class="sxs-lookup"><span data-stu-id="10923-142">The Anchor Tag Helper generates a route directly to that controller action using the URL */Speaker/Evaluations*.</span></span> <span data-ttu-id="10923-143">생성된 HTML:</span><span class="sxs-lookup"><span data-stu-id="10923-143">The generated HTML:</span></span>

```html
<a href="/Speaker/Evaluations">Speaker Evaluations</a>
```

<span data-ttu-id="10923-144">`asp-route` 와 `asp-controller` 또는 `asp-action` 을 동시에 지정하면 기대하는 것과 다른 경로가 생성될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="10923-144">If `asp-controller` or `asp-action` is specified in addition to `asp-route`, the route generated may not be what you expect.</span></span> <span data-ttu-id="10923-145">경로 충돌을 방지하려면 `asp-route`를 `asp-controller` 및 `asp-action` 특성과 함께 사용하지 않아야 합니다. </span><span class="sxs-lookup"><span data-stu-id="10923-145">To avoid a route conflict, `asp-route` shouldn't be used with the `asp-controller` and `asp-action` attributes.</span></span>

## <a name="asp-all-route-data"></a><span data-ttu-id="10923-146">asp-all-route-data</span><span class="sxs-lookup"><span data-stu-id="10923-146">asp-all-route-data</span></span>

<span data-ttu-id="10923-147">[asp-all-route-data](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.routevalues) 특성은 키-값 쌍 사전을 만들 수 있도록 지원해줍니다.</span><span class="sxs-lookup"><span data-stu-id="10923-147">The [asp-all-route-data](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.routevalues) attribute supports the creation of a dictionary of key-value pairs.</span></span> <span data-ttu-id="10923-148">키는 매개 변수 이름이고, 값은 매개 변수값입니다.</span><span class="sxs-lookup"><span data-stu-id="10923-148">The key is the parameter name, and the value is the parameter value.</span></span>

<span data-ttu-id="10923-149">다음 예제에서는 사전이 초기화되어 Razor 뷰로 전달됩니다.</span><span class="sxs-lookup"><span data-stu-id="10923-149">In the following example, a dictionary is initialized and passed to a Razor view.</span></span> <span data-ttu-id="10923-150">또는 모델을 사용하여 데이터를 전달할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="10923-150">Alternatively, the data could be passed in with your model.</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspAllRouteData)]

<span data-ttu-id="10923-151">위의 코드로 생성되는 HTML은 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="10923-151">The preceding code generates the following HTML:</span></span>

```html
<a href="/Speaker/EvaluationsCurrent?speakerId=11&currentYear=true">Speaker Evaluations</a>
```

<span data-ttu-id="10923-152">`asp-all-route-data` 사전은 투영되어 오버로드된 `Evaluations` 액션의 요구 사항을 충족하는 쿼리 문자열을 생성합니다.</span><span class="sxs-lookup"><span data-stu-id="10923-152">The `asp-all-route-data` dictionary is flattened to produce a querystring meeting the requirements of the overloaded `Evaluations` action:</span></span>

[!code-csharp[](samples/TagHelpersBuiltIn/Controllers/SpeakerController.cs?range=26-30)]

<span data-ttu-id="10923-153">사전에 존재하는 키가 경로 매개 변수와 일치하면 경로에서 해당 값이 적절하게 대체됩니다.</span><span class="sxs-lookup"><span data-stu-id="10923-153">If any keys in the dictionary match route parameters, those values are substituted in the route as appropriate.</span></span> <span data-ttu-id="10923-154">반면 일치하지 않는 다른 값은 요청 매개 변수로 생성됩니다.</span><span class="sxs-lookup"><span data-stu-id="10923-154">The other non-matching values are generated as request parameters.</span></span>

## <a name="asp-fragment"></a><span data-ttu-id="10923-155">asp-fragment</span><span class="sxs-lookup"><span data-stu-id="10923-155">asp-fragment</span></span>

<span data-ttu-id="10923-156">[asp-fragment](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.fragment) 특성은 URL에 추가할 URL 조각을 정의합니다.</span><span class="sxs-lookup"><span data-stu-id="10923-156">The [asp-fragment](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.fragment) attribute defines a URL fragment to append to the URL.</span></span> <span data-ttu-id="10923-157">앵커 태그 도우미는 해시 문자(#)를 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="10923-157">The Anchor Tag Helper adds the hash character (#).</span></span> <span data-ttu-id="10923-158">다음 태그를 살펴보시기 바랍니다.</span><span class="sxs-lookup"><span data-stu-id="10923-158">Consider the following markup:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspFragment)]

<span data-ttu-id="10923-159">생성된 HTML:</span><span class="sxs-lookup"><span data-stu-id="10923-159">The generated HTML:</span></span>

```html
<a href="/Speaker/Evaluations#SpeakerEvaluations">Speaker Evaluations</a>
```

<span data-ttu-id="10923-160">해시 태그는 클라이언트 측 앱을 만들 때 유용합니다.</span><span class="sxs-lookup"><span data-stu-id="10923-160">Hash tags are useful when building client-side apps.</span></span> <span data-ttu-id="10923-161">예를 들어 JavaScript에서 손쉽게 표시하고 검색하는 데 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="10923-161">They can be used for easy marking and searching in JavaScript, for example.</span></span>

## <a name="asp-area"></a><span data-ttu-id="10923-162">asp-area</span><span class="sxs-lookup"><span data-stu-id="10923-162">asp-area</span></span>

<span data-ttu-id="10923-163">[asp-area](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.area) 특성은 적절한 경로를 설정하기 위해 사용되는 영역 이름을 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="10923-163">The [asp-area](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.area) attribute sets the area name used to set the appropriate route.</span></span> <span data-ttu-id="10923-164">다음 예제에서는 area 특성으로 인해 경로가 다시 매핑되는 방식을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="10923-164">The following example depicts how the area attribute causes a remapping of routes.</span></span> <span data-ttu-id="10923-165">`asp-area`를 "Blogs"로 설정하면 이 앵커 태그에 연결된 컨트롤러 및 뷰의 경로에 *Areas/Blogs* 디렉터리가 접두사로 추가됩니다.</span><span class="sxs-lookup"><span data-stu-id="10923-165">Setting `asp-area` to "Blogs" prefixes the directory *Areas/Blogs* to the routes of the associated controllers and views for this anchor tag.</span></span>

* <span data-ttu-id="10923-166">**{Project name}**</span><span class="sxs-lookup"><span data-stu-id="10923-166">**{Project name}**</span></span>
  * <span data-ttu-id="10923-167">**wwwroot**</span><span class="sxs-lookup"><span data-stu-id="10923-167">**wwwroot**</span></span>
  * <span data-ttu-id="10923-168">**Areas**</span><span class="sxs-lookup"><span data-stu-id="10923-168">**Areas**</span></span>
    * <span data-ttu-id="10923-169">**Blogs**</span><span class="sxs-lookup"><span data-stu-id="10923-169">**Blogs**</span></span>
      * <span data-ttu-id="10923-170">**Controllers**</span><span class="sxs-lookup"><span data-stu-id="10923-170">**Controllers**</span></span>
        * <span data-ttu-id="10923-171">*HomeController.cs*</span><span class="sxs-lookup"><span data-stu-id="10923-171">*HomeController.cs*</span></span>
      * <span data-ttu-id="10923-172">**Views**</span><span class="sxs-lookup"><span data-stu-id="10923-172">**Views**</span></span>
        * <span data-ttu-id="10923-173">**Home**</span><span class="sxs-lookup"><span data-stu-id="10923-173">**Home**</span></span>
          * <span data-ttu-id="10923-174">*AboutBlog.cshtml*</span><span class="sxs-lookup"><span data-stu-id="10923-174">*AboutBlog.cshtml*</span></span>
          * <span data-ttu-id="10923-175">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="10923-175">*Index.cshtml*</span></span>
        * <span data-ttu-id="10923-176">*\_ViewStart.cshtml*</span><span class="sxs-lookup"><span data-stu-id="10923-176">*\_ViewStart.cshtml*</span></span>
  * <span data-ttu-id="10923-177">**Controllers**</span><span class="sxs-lookup"><span data-stu-id="10923-177">**Controllers**</span></span>

<span data-ttu-id="10923-178">위의 디렉터리 계층 구조에서 *AboutBlog.cshtml* 파일을 참조하는 태그는 다음과 같습니다. </span><span class="sxs-lookup"><span data-stu-id="10923-178">Given the preceding directory hierarchy, the markup to reference the *AboutBlog.cshtml* file is:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspArea)]

<span data-ttu-id="10923-179">생성된 HTML:</span><span class="sxs-lookup"><span data-stu-id="10923-179">The generated HTML:</span></span>

```html
<a href="/Blogs/Home/AboutBlog">About Blog</a>
```

> [!TIP]
> <span data-ttu-id="10923-180">MVC 앱에서 작업하기 위한 영역의 경우, 경로 템플릿에 해당 영역에 대한 참조가 포함되어야 합니다 (존재할 경우).</span><span class="sxs-lookup"><span data-stu-id="10923-180">For areas to work in an MVC app, the route template must include a reference to the area, if it exists.</span></span> <span data-ttu-id="10923-181">해당 템플릿은 *Startup.Configure*에서 `routes.MapRoute` 메서드 호출의 두 번째 매개 변수로 표시됩니다.</span><span class="sxs-lookup"><span data-stu-id="10923-181">That template is represented by the second parameter of the `routes.MapRoute` method call in *Startup.Configure*:</span></span>
>
> [!code-csharp[](samples/TagHelpersBuiltIn/Startup.cs?name=snippet_UseMvc&highlight=5)]

## <a name="asp-protocol"></a><span data-ttu-id="10923-182">asp-protocol</span><span class="sxs-lookup"><span data-stu-id="10923-182">asp-protocol</span></span>

<span data-ttu-id="10923-183">[asp-protocol](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.protocol) 특성은 URL의 프로토콜(예: `https`)을 지정하는 데 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="10923-183">The [asp-protocol](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.protocol) attribute is for specifying a protocol (such as `https`) in your URL.</span></span> <span data-ttu-id="10923-184">예:</span><span class="sxs-lookup"><span data-stu-id="10923-184">For example:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspProtocol)]

<span data-ttu-id="10923-185">생성된 HTML:</span><span class="sxs-lookup"><span data-stu-id="10923-185">The generated HTML:</span></span>

```html
<a href="https://localhost/Home/About">About</a>
```

<span data-ttu-id="10923-186">이 예제의 호스트 이름은 localhost지만, 실제로 앵커 태그 도우미가 URL을 생성할 때는 웹 사이트의 공용 도메인을 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="10923-186">The host name in the example is localhost, but the Anchor Tag Helper uses the website's public domain when generating the URL.</span></span>

## <a name="asp-host"></a><span data-ttu-id="10923-187">asp-host</span><span class="sxs-lookup"><span data-stu-id="10923-187">asp-host</span></span>

<span data-ttu-id="10923-188">[asp-host](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.host) 특성은 URL의 호스트 이름을 지정하는 데 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="10923-188">The [asp-host](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.host) attribute is for specifying a host name in your URL.</span></span> <span data-ttu-id="10923-189">예:</span><span class="sxs-lookup"><span data-stu-id="10923-189">For example:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspHost)]

<span data-ttu-id="10923-190">생성된 HTML:</span><span class="sxs-lookup"><span data-stu-id="10923-190">The generated HTML:</span></span>

```html
<a href="https://microsoft.com/Home/About">About</a>
```

## <a name="asp-page"></a><span data-ttu-id="10923-191">asp-page</span><span class="sxs-lookup"><span data-stu-id="10923-191">asp-page</span></span>

<span data-ttu-id="10923-192">[asp-page](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.page) 특성은 Razor 페이지와 함께 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="10923-192">The [asp-page](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.page) attribute is used with Razor Pages.</span></span> <span data-ttu-id="10923-193">이 특성은 앵커 태그의 `href` 특성값을 특정 페이지로 설정하기 위해서 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="10923-193">Use it to set an anchor tag's `href` attribute value to a specific page.</span></span> <span data-ttu-id="10923-194">페이지 이름 앞에 슬래시("/")를 접두사로 사용해서 URL을 생성합니다.</span><span class="sxs-lookup"><span data-stu-id="10923-194">Prefixing the page name with a forward slash ("/") creates the URL.</span></span>

<span data-ttu-id="10923-195">다음 예제는 참석자 Razor 페이지를 가리킵니다.</span><span class="sxs-lookup"><span data-stu-id="10923-195">The following sample points to the attendee Razor Page:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspPage)]

<span data-ttu-id="10923-196">생성된 HTML:</span><span class="sxs-lookup"><span data-stu-id="10923-196">The generated HTML:</span></span>

```html
<a href="/Attendee">All Attendees</a>
```

<span data-ttu-id="10923-197">`asp-page` 특성은 `asp-route`, `asp-controller` 및 `asp-action` 특성과 함께 사용할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="10923-197">The `asp-page` attribute is mutually exclusive with the `asp-route`, `asp-controller`, and `asp-action` attributes.</span></span> <span data-ttu-id="10923-198">그러나 다음 태그에서 볼 수 있는 것처럼 `asp-page`는 `asp-route-{value}`와 함께 사용해서 라우팅을 제어할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="10923-198">However, `asp-page` can be used with `asp-route-{value}` to control routing, as the following markup demonstrates:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspPageAspRouteId)]

<span data-ttu-id="10923-199">생성된 HTML:</span><span class="sxs-lookup"><span data-stu-id="10923-199">The generated HTML:</span></span>

```html
<a href="/Attendee?attendeeid=10">View Attendee</a>
```

## <a name="asp-page-handler"></a><span data-ttu-id="10923-200">asp-page-handler</span><span class="sxs-lookup"><span data-stu-id="10923-200">asp-page-handler</span></span>

<span data-ttu-id="10923-201">[asp-page-handler](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.pagehandler) 특성은 Razor 페이지와 함께 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="10923-201">The [asp-page-handler](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.pagehandler) attribute is used with Razor Pages.</span></span> <span data-ttu-id="10923-202">이 특성을 특정 페이지 처리기에 연결하기 위한 것입니다.</span><span class="sxs-lookup"><span data-stu-id="10923-202">It's intended for linking to specific page handlers.</span></span>

<span data-ttu-id="10923-203">다음 페이지 처리기를 살펴보시기 바랍니다.</span><span class="sxs-lookup"><span data-stu-id="10923-203">Consider the following page handler:</span></span>

[!code-csharp[](samples/TagHelpersBuiltIn/Pages/Attendee.cshtml.cs?name=snippet_OnGetProfileHandler)]

<span data-ttu-id="10923-204">페이지 모델의 관련 태그는 `OnGetProfile` 페이지 처리기에 연결됩니다.</span><span class="sxs-lookup"><span data-stu-id="10923-204">The page model's associated markup links to the `OnGetProfile` page handler.</span></span> <span data-ttu-id="10923-205">페이지 처리기 메서드 이름의 `On<Verb>` 접두사는 `asp-page-handler` 특성값에서 생략되므로 주의하시기 바랍니다.</span><span class="sxs-lookup"><span data-stu-id="10923-205">Note that the `On<Verb>` prefix of the page handler method name is omitted in the `asp-page-handler` attribute value.</span></span> <span data-ttu-id="10923-206">비동기 메서드인 경우 `Async` 접미사도 생략됩니다.</span><span class="sxs-lookup"><span data-stu-id="10923-206">If this were an asynchronous method, the `Async` suffix would be omitted too.</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspPageHandler)]

<span data-ttu-id="10923-207">생성된 HTML:</span><span class="sxs-lookup"><span data-stu-id="10923-207">The generated HTML:</span></span>

```html
<a href="/Attendee?attendeeid=12&handler=Profile">Attendee Profile</a>
```

## <a name="additional-resources"></a><span data-ttu-id="10923-208">추가 자료</span><span class="sxs-lookup"><span data-stu-id="10923-208">Additional resources</span></span>

* <xref:mvc/controllers/areas>
* <xref:razor-pages/index>
