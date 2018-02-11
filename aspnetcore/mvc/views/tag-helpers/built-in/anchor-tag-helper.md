---
title: "앵커 태그 도우미"
author: pkellner
description: "ASP.NET Core 앵커 태그 도우미 특성 및 HTML 앵커 태그의 동작을 확장할 때 각 특성이 담당하는 역할을 검색합니다."
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 01/31/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/tag-helpers/builtin-th/anchor-tag-helper
ms.openlocfilehash: f3b704174c3287edda12725b7973a2464e485bac
ms.sourcegitcommit: f2a11a89037471a77ad68a67533754b7bb8303e2
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/01/2018
---
# <a name="anchor-tag-helper"></a><span data-ttu-id="f71ef-103">앵커 태그 도우미</span><span class="sxs-lookup"><span data-stu-id="f71ef-103">Anchor Tag Helper</span></span>

<span data-ttu-id="f71ef-104">작성자: [Peter Kellner](http://peterkellner.net) 및 [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="f71ef-104">By [Peter Kellner](http://peterkellner.net) and [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="f71ef-105">[샘플 코드 보기 또는 다운로드](https://github.com/aspnet/Docs/tree/master/aspnetcore/tag-helpers/built-in/samples/TagHelpersBuiltInAspNetCore)([다운로드 방법](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="f71ef-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tag-helpers/built-in/samples/TagHelpersBuiltInAspNetCore) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="f71ef-106">[앵커 태그 도우미](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper)는 새 특성을 추가하여 표준 HTML 앵커(`<a ... ></a>`) 태그를 향상시킵니다.</span><span class="sxs-lookup"><span data-stu-id="f71ef-106">The [Anchor Tag Helper](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper) enhances the standard HTML anchor (`<a ... ></a>`) tag by adding new attributes.</span></span> <span data-ttu-id="f71ef-107">규칙에 따라 특성 이름의 접두사는 `asp-`입니다.</span><span class="sxs-lookup"><span data-stu-id="f71ef-107">By convention, the attribute names are prefixed with `asp-`.</span></span> <span data-ttu-id="f71ef-108">렌더링된 앵커 요소의 `href` 특성 값은 `asp-` 특성 값에 따라 결정됩니다.</span><span class="sxs-lookup"><span data-stu-id="f71ef-108">The rendered anchor element's `href` attribute value is determined by the values of the `asp-` attributes.</span></span>

<span data-ttu-id="f71ef-109">*SpeakerController*는 이 문서 전반의 샘플에서 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="f71ef-109">*SpeakerController* is used in samples throughout this document:</span></span>

[!code-csharp[](samples/TagHelpersBuiltInAspNetCore/Controllers/SpeakerController.cs?name=snippet_SpeakerController)]

<span data-ttu-id="f71ef-110">`asp-` 특성의 인벤토리는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="f71ef-110">An inventory of the `asp-` attributes follows.</span></span>

## <a name="asp-controller"></a><span data-ttu-id="f71ef-111">asp-controller</span><span class="sxs-lookup"><span data-stu-id="f71ef-111">asp-controller</span></span>

<span data-ttu-id="f71ef-112">[asp-controller](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.controller) 특성은 URL 생성에 사용되는 컨트롤러를 할당합니다.</span><span class="sxs-lookup"><span data-stu-id="f71ef-112">The [asp-controller](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.controller) attribute assigns the controller used for generating the URL.</span></span> <span data-ttu-id="f71ef-113">다음 태그는 모든 스피커를 나열합니다.</span><span class="sxs-lookup"><span data-stu-id="f71ef-113">The following markup lists all speakers:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltInAspNetCore/Views/Home/Index.cshtml?name=snippet_AspController)]

<span data-ttu-id="f71ef-114">생성된 코드:</span><span class="sxs-lookup"><span data-stu-id="f71ef-114">The generated HTML:</span></span>

```html
<a href="/Speaker">All Speakers</a>
```

<span data-ttu-id="f71ef-115">`asp-controller` 특성이 지정되고 `asp-action`이 지정되지 않으면, 기본 `asp-action` 값은 현재 실행 중인 보기와 연결된 컨트롤러 작업입니다.</span><span class="sxs-lookup"><span data-stu-id="f71ef-115">If the `asp-controller` attribute is specified and `asp-action` isn't, the default `asp-action` value is the controller action associated with the currently executing view.</span></span> <span data-ttu-id="f71ef-116">위의 태그에서 `asp-action`이 생략되고 *HomeController*의 *인덱스* 보기(*/Home*)에서 앵커 태그 도우미가 사용되는 경우 생성된 HTML은 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="f71ef-116">If `asp-action` is omitted from the preceding markup, and the Anchor Tag Helper is used in *HomeController*'s *Index* view (*/Home*), the generated HTML is:</span></span>

```html
<a href="/Home">All Speakers</a>
```

## <a name="asp-action"></a><span data-ttu-id="f71ef-117">asp-action</span><span class="sxs-lookup"><span data-stu-id="f71ef-117">asp-action</span></span>

<span data-ttu-id="f71ef-118">[asp-action](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.action) 특성 값은 생성된 `href` 특성에 포함된 컨트롤러 작업 이름을 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="f71ef-118">The [asp-action](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.action) attribute value represents the controller action name included in the generated `href` attribute.</span></span> <span data-ttu-id="f71ef-119">다음 태그는 생성된 `href` 특성 값을 스피커 평가 페이지로 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="f71ef-119">The following markup sets the generated `href` attribute value to the speaker evaluations page:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltInAspNetCore/Views/Home/Index.cshtml?name=snippet_AspAction)]

<span data-ttu-id="f71ef-120">생성된 코드:</span><span class="sxs-lookup"><span data-stu-id="f71ef-120">The generated HTML:</span></span>

```html
<a href="/Speaker/Evaluations">Speaker Evaluations</a>
```

<span data-ttu-id="f71ef-121">`asp-controller` 특성을 지정하지 않으면 현재 보기를 실행 중인 보기를 호출하는 기본 컨트롤러가 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="f71ef-121">If no `asp-controller` attribute is specified, the default controller calling the view executing the current view is used.</span></span>

<span data-ttu-id="f71ef-122">`asp-action` 특성 값이 `Index`이면 URL에 아무 작업도 추가되지 않으므로 기본 `Index` 작업이 호출됩니다.</span><span class="sxs-lookup"><span data-stu-id="f71ef-122">If the `asp-action` attribute value is `Index`, then no action is appended to the URL, leading to the invocation of the default `Index` action.</span></span> <span data-ttu-id="f71ef-123">지정된(또는 기본값으로 설정된) 작업은 `asp-controller`에서 참조되는 컨트롤러에 있어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="f71ef-123">The action specified (or defaulted), must exist in the controller referenced in `asp-controller`.</span></span>

## <a name="asp-route-value"></a><span data-ttu-id="f71ef-124">asp-route-{value}</span><span class="sxs-lookup"><span data-stu-id="f71ef-124">asp-route-{value}</span></span>

<span data-ttu-id="f71ef-125">[asp-route-{value}](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.routevalues) 특성은 와일드카드 경로 접두사를 사용하도록 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="f71ef-125">The [asp-route-{value}](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.routevalues) attribute enables a wildcard route prefix.</span></span> <span data-ttu-id="f71ef-126">`{value}` 자리 표시자에 포함되는 모든 값은 잠재적인 경로 매개 변수로 해석됩니다.</span><span class="sxs-lookup"><span data-stu-id="f71ef-126">Any value occupying the `{value}` placeholder is interpreted as a potential route parameter.</span></span> <span data-ttu-id="f71ef-127">기본 경로가 없으면 이 경로 접두사가 생성되는 `href` 특성에 요청 매개 변수 및 값으로 추가됩니다.</span><span class="sxs-lookup"><span data-stu-id="f71ef-127">If a default route isn't found, this route prefix is appended to the generated `href` attribute as a request parameter and value.</span></span> <span data-ttu-id="f71ef-128">그렇지 않으면 경로 템플릿에서 이 경로 접두사가 대체됩니다.</span><span class="sxs-lookup"><span data-stu-id="f71ef-128">Otherwise, it's substituted in the route template.</span></span>

<span data-ttu-id="f71ef-129">다음 컨트롤러 작업을 살펴보세요.</span><span class="sxs-lookup"><span data-stu-id="f71ef-129">Consider the following controller action:</span></span>

[!code-csharp[](samples/TagHelpersBuiltInAspNetCore/Controllers/BuiltInTagController.cs?name=snippet_AnchorTagHelperAction)]

<span data-ttu-id="f71ef-130">*Startup.Configure*에 정의된 기본 경로 템플릿을 사용하는 경우 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="f71ef-130">With a default route template defined in *Startup.Configure*:</span></span>

[!code-csharp[](samples/TagHelpersBuiltInAspNetCore/Startup.cs?name=snippet_UseMvc&highlight=8-10)]

<span data-ttu-id="f71ef-131">MVC 보기는 다음과 같이 작업을 통해 제공되는 모델을 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="f71ef-131">The MVC view uses the model, provided by the action, as follows:</span></span>

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

<span data-ttu-id="f71ef-132">기본 경로의 `{id?}` 자리 표시자가 일치했습니다.</span><span class="sxs-lookup"><span data-stu-id="f71ef-132">The default route's `{id?}` placeholder was matched.</span></span> <span data-ttu-id="f71ef-133">생성된 코드:</span><span class="sxs-lookup"><span data-stu-id="f71ef-133">The generated HTML:</span></span>

```html
<a href="/Speaker/Detail/12">SpeakerId: 12</a>
```

<span data-ttu-id="f71ef-134">다음 MVC 보기와 마찬가지로 경로 접두사가 라우팅 템플릿의 일부와 일치하지 않는다고 가정합니다.</span><span class="sxs-lookup"><span data-stu-id="f71ef-134">Assume the route prefix isn't part of the matching routing template, as with the following MVC view:</span></span>

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

<span data-ttu-id="f71ef-135">일치하는 경로에서 `speakerid`를 찾을 수 없기 때문에 생성된 HTML은 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="f71ef-135">The following HTML is generated because `speakerid` wasn't found in the matching route:</span></span>

```html
<a href="/Speaker/Detail?speakerid=12">SpeakerId: 12</a>
```

<span data-ttu-id="f71ef-136">`asp-controller` 또는 `asp-action`이 지정되지 않으면 `asp-route` 특성과 동일한 기본 처리가 그대로 수행됩니다.</span><span class="sxs-lookup"><span data-stu-id="f71ef-136">If either `asp-controller` or `asp-action` aren't specified, then the same default processing is followed as is in the `asp-route` attribute.</span></span>

## <a name="asp-route"></a><span data-ttu-id="f71ef-137">asp-route</span><span class="sxs-lookup"><span data-stu-id="f71ef-137">asp-route</span></span>

<span data-ttu-id="f71ef-138">[asp-route](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.route) 특성은 명명된 경로에 직접 연결되는 URL을 만드는 데 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="f71ef-138">The [asp-route](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.route) attribute is used for creating a URL linking directly to a named route.</span></span> <span data-ttu-id="f71ef-139">[라우팅 특성](xref:mvc/controllers/routing#attribute-routing)을 사용하면 경로가 `SpeakerController`에 표시된 이름으로 지정되고 해당 `Evaluations` 메서드에서 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f71ef-139">Using [routing attributes](xref:mvc/controllers/routing#attribute-routing), a route can be named as shown in the `SpeakerController` and used in its `Evaluations` action:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltInAspNetCore/Controllers/SpeakerController.cs?range=22-24)]

<span data-ttu-id="f71ef-140">다음 태그에서 `asp-route` 특성은 명명된 경로를 참조합니다.</span><span class="sxs-lookup"><span data-stu-id="f71ef-140">In the following markup, the `asp-route` attribute references the named route:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltInAspNetCore/Views/Home/Index.cshtml?name=snippet_AspRoute)]

<span data-ttu-id="f71ef-141">앵커 태그 도우미는 */Speaker/Evaluations* URL을 사용하여 해당 컨트롤러 작업을 직접 가리키는 경로를 생성합니다.</span><span class="sxs-lookup"><span data-stu-id="f71ef-141">The Anchor Tag Helper generates a route directly to that controller action using the URL */Speaker/Evaluations*.</span></span> <span data-ttu-id="f71ef-142">생성된 코드:</span><span class="sxs-lookup"><span data-stu-id="f71ef-142">The generated HTML:</span></span>

```html
<a href="/Speaker/Evaluations">Speaker Evaluations</a>
```

<span data-ttu-id="f71ef-143">`asp-route` 외에 `asp-controller` 또는 `asp-action`을 지정하면 예상과 다른 경로가 생성될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f71ef-143">If `asp-controller` or `asp-action` is specified in addition to `asp-route`, the route generated may not be what you expect.</span></span> <span data-ttu-id="f71ef-144">경로 충돌을 방지하기 위해 `asp-route`는 `asp-controller` 및 `asp-action` 특성과 함께 사용하지 않아야 합니다.</span><span class="sxs-lookup"><span data-stu-id="f71ef-144">To avoid a route conflict, `asp-route` shouldn't be used with the `asp-controller` and `asp-action` attributes.</span></span>

## <a name="asp-all-route-data"></a><span data-ttu-id="f71ef-145">asp-all-route-data</span><span class="sxs-lookup"><span data-stu-id="f71ef-145">asp-all-route-data</span></span>

<span data-ttu-id="f71ef-146">[asp-all-route-data](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.routevalues) 특성은 키-값 쌍 사전을 만들도록 지원합니다.</span><span class="sxs-lookup"><span data-stu-id="f71ef-146">The [asp-all-route-data](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.routevalues) attribute supports the creation of a dictionary of key-value pairs.</span></span> <span data-ttu-id="f71ef-147">키는 매개 변수 이름이고, 값은 매개 변수 값입니다.</span><span class="sxs-lookup"><span data-stu-id="f71ef-147">The key is the parameter name, and the value is the parameter value.</span></span>

<span data-ttu-id="f71ef-148">다음 예제에서는 사전이 초기화되어 Razor 보기로 전달됩니다.</span><span class="sxs-lookup"><span data-stu-id="f71ef-148">In the following example, a dictionary is initialized and passed to a Razor view.</span></span> <span data-ttu-id="f71ef-149">또는 모델을 사용하여 데이터를 전달할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f71ef-149">Alternatively, the data could be passed in with your model.</span></span>

[!code-cshtml[](samples/TagHelpersBuiltInAspNetCore/Views/Home/Index.cshtml?name=snippet_AspAllRouteData)]

<span data-ttu-id="f71ef-150">앞의 코드에서 생성된 HTML은 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="f71ef-150">The preceding code generates the following HTML:</span></span>

```html
<a href="/Speaker/EvaluationsCurrent?speakerId=11&currentYear=true">Speaker Evaluations</a>
```

<span data-ttu-id="f71ef-151">`asp-all-route-data` 사전을 평면화하여 오버로드된 `Evaluations` 작업의 요구 사항을 충족하는 쿼리 문자열을 생성합니다.</span><span class="sxs-lookup"><span data-stu-id="f71ef-151">The `asp-all-route-data` dictionary is flattened to produce a querystring meeting the requirements of the overloaded `Evaluations` action:</span></span>

[!code-csharp[](samples/TagHelpersBuiltInAspNetCore/Controllers/SpeakerController.cs?range=26-30)]

<span data-ttu-id="f71ef-152">사전에 있는 키가 경로 매개 변수와 일치하면 해당 값이 경로에서 적절하게 대체됩니다.</span><span class="sxs-lookup"><span data-stu-id="f71ef-152">If any keys in the dictionary match route parameters, those values are substituted in the route as appropriate.</span></span> <span data-ttu-id="f71ef-153">일치하지 않는 다른 값은 요청 매개 변수로 생성됩니다.</span><span class="sxs-lookup"><span data-stu-id="f71ef-153">The other non-matching values are generated as request parameters.</span></span>

## <a name="asp-fragment"></a><span data-ttu-id="f71ef-154">asp-fragment</span><span class="sxs-lookup"><span data-stu-id="f71ef-154">asp-fragment</span></span>

<span data-ttu-id="f71ef-155">[asp-fragment](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.fragment) 특성은 URL에 추가할 URL 조각을 정의합니다.</span><span class="sxs-lookup"><span data-stu-id="f71ef-155">The [asp-fragment](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.fragment) attribute defines a URL fragment to append to the URL.</span></span> <span data-ttu-id="f71ef-156">앵커 태그 도우미는 해시 문자(#)를 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="f71ef-156">The Anchor Tag Helper adds the hash character (#).</span></span> <span data-ttu-id="f71ef-157">다음 태그를 살펴보세요.</span><span class="sxs-lookup"><span data-stu-id="f71ef-157">Consider the following markup:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltInAspNetCore/Views/Home/Index.cshtml?name=snippet_AspFragment)]

<span data-ttu-id="f71ef-158">생성된 코드:</span><span class="sxs-lookup"><span data-stu-id="f71ef-158">The generated HTML:</span></span>

```html
<a href="/Speaker/Evaluations#SpeakerEvaluations">Speaker Evaluations</a>
```

<span data-ttu-id="f71ef-159">해시 태그는 클라이언트 쪽 앱을 빌드할 때 유용합니다.</span><span class="sxs-lookup"><span data-stu-id="f71ef-159">Hash tags are useful when building client-side apps.</span></span> <span data-ttu-id="f71ef-160">다음과 같이 JavaScript에서 손쉽게 만들고 검색하는 데 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f71ef-160">They can be used for easy marking and searching in JavaScript, for example.</span></span>

## <a name="asp-area"></a><span data-ttu-id="f71ef-161">asp-area</span><span class="sxs-lookup"><span data-stu-id="f71ef-161">asp-area</span></span>

<span data-ttu-id="f71ef-162">[asp-area](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.area) 특성은 적절한 경로를 설정하는 데 사용되는 영역 이름을 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="f71ef-162">The [asp-area](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.area) attribute sets the area name used to set the appropriate route.</span></span> <span data-ttu-id="f71ef-163">다음 예제에서는 area 특성으로 인해 경로가 다시 매핑되는 방식을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="f71ef-163">The following example depicts how the area attribute causes a remapping of routes.</span></span> <span data-ttu-id="f71ef-164">`asp-area`를 "Blogs"로 설정하면 이 앵커 태그에 대해 연결된 컨트롤러 및 보기의 경로에 *Areas/Blogs* 디렉터리가 접두사로 붙습니다.</span><span class="sxs-lookup"><span data-stu-id="f71ef-164">Setting `asp-area` to "Blogs" prefixes the directory *Areas/Blogs* to the routes of the associated controllers and views for this anchor tag.</span></span>

* <span data-ttu-id="f71ef-165">**<프로젝트 이름\>**</span><span class="sxs-lookup"><span data-stu-id="f71ef-165">**<Project name\>**</span></span>
  * <span data-ttu-id="f71ef-166">**wwwroot**</span><span class="sxs-lookup"><span data-stu-id="f71ef-166">**wwwroot**</span></span>
  * <span data-ttu-id="f71ef-167">**영역**</span><span class="sxs-lookup"><span data-stu-id="f71ef-167">**Areas**</span></span>
    * <span data-ttu-id="f71ef-168">**블로그**</span><span class="sxs-lookup"><span data-stu-id="f71ef-168">**Blogs**</span></span>
      * <span data-ttu-id="f71ef-169">**컨트롤러**</span><span class="sxs-lookup"><span data-stu-id="f71ef-169">**Controllers**</span></span>
        * <span data-ttu-id="f71ef-170">*HomeController.cs*</span><span class="sxs-lookup"><span data-stu-id="f71ef-170">*HomeController.cs*</span></span>
      * <span data-ttu-id="f71ef-171">**뷰**</span><span class="sxs-lookup"><span data-stu-id="f71ef-171">**Views**</span></span>
        * <span data-ttu-id="f71ef-172">**Home**</span><span class="sxs-lookup"><span data-stu-id="f71ef-172">**Home**</span></span>
          * <span data-ttu-id="f71ef-173">*AboutBlog.cshtml*</span><span class="sxs-lookup"><span data-stu-id="f71ef-173">*AboutBlog.cshtml*</span></span>
          * <span data-ttu-id="f71ef-174">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="f71ef-174">*Index.cshtml*</span></span>
        * <span data-ttu-id="f71ef-175">*_ViewStart.cshtml*</span><span class="sxs-lookup"><span data-stu-id="f71ef-175">*_ViewStart.cshtml*</span></span>
  * <span data-ttu-id="f71ef-176">**컨트롤러**</span><span class="sxs-lookup"><span data-stu-id="f71ef-176">**Controllers**</span></span>

<span data-ttu-id="f71ef-177">앞의 디렉터리 계층 구조에서 *AboutBlog.cshtml* 파일을 참조하는 태그는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="f71ef-177">Given the preceding directory hierarchy, the markup to reference the *AboutBlog.cshtml* file is:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltInAspNetCore/Views/Home/Index.cshtml?name=snippet_AspArea)]

<span data-ttu-id="f71ef-178">생성된 코드:</span><span class="sxs-lookup"><span data-stu-id="f71ef-178">The generated HTML:</span></span>

```html
<a href="/Blogs/Home/AboutBlog">About Blog</a>
```

> [!TIP]
> <span data-ttu-id="f71ef-179">MVC 앱에서 작업할 영역이 있는 경우 경로 템플릿에 해당 영역에 대한 참조가 포함되어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="f71ef-179">For areas to work in an MVC app, the route template must include a reference to the area, if it exists.</span></span> <span data-ttu-id="f71ef-180">해당 템플릿은 *Startup.Configure*에서 `routes.MapRoute` 메서드 호출의 두 번째 매개 변수([!code-csharp[](samples/TagHelpersBuiltInAspNetCore/Startup.cs?name=snippet_UseMvc&highlight=5)])로 표시됩니다.</span><span class="sxs-lookup"><span data-stu-id="f71ef-180">That template is represented by the second parameter of the `routes.MapRoute` method call in *Startup.Configure*: [!code-csharp[](samples/TagHelpersBuiltInAspNetCore/Startup.cs?name=snippet_UseMvc&highlight=5)]</span></span>

## <a name="asp-protocol"></a><span data-ttu-id="f71ef-181">asp-protocol</span><span class="sxs-lookup"><span data-stu-id="f71ef-181">asp-protocol</span></span>

<span data-ttu-id="f71ef-182">[asp-protocol](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.protocol) 특성은 URL에서 프로토콜(예: `https`)을 지정하는 데 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="f71ef-182">The [asp-protocol](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.protocol) attribute is for specifying a protocol (such as `https`) in your URL.</span></span> <span data-ttu-id="f71ef-183">예:</span><span class="sxs-lookup"><span data-stu-id="f71ef-183">For example:</span></span>

<span data-ttu-id="f71ef-184">[!code-cshtml[samples/TagHelpersBuiltInAspNetCore/Views/Index.cshtml?name=snippet_AspProtocol]]</span><span class="sxs-lookup"><span data-stu-id="f71ef-184">[!code-cshtml[samples/TagHelpersBuiltInAspNetCore/Views/Index.cshtml?name=snippet_AspProtocol]]</span></span>

<span data-ttu-id="f71ef-185">생성된 코드:</span><span class="sxs-lookup"><span data-stu-id="f71ef-185">The generated HTML:</span></span>

```html
<a href="https://localhost/Home/About">About</a>
```

<span data-ttu-id="f71ef-186">이 예제의 호스트 이름은 localhost이지만, 앵커 태그 도우미는 URL을 생성할 때 웹 사이트의 공용 도메인을 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="f71ef-186">The host name in the example is localhost, but the Anchor Tag Helper uses the website's public domain when generating the URL.</span></span>

## <a name="asp-host"></a><span data-ttu-id="f71ef-187">asp-host</span><span class="sxs-lookup"><span data-stu-id="f71ef-187">asp-host</span></span>

<span data-ttu-id="f71ef-188">[asp-host](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.host) 특성은 URL에 호스트 이름을 지정하는 데 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="f71ef-188">The [asp-host](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.host) attribute is for specifying a host name in your URL.</span></span> <span data-ttu-id="f71ef-189">예:</span><span class="sxs-lookup"><span data-stu-id="f71ef-189">For example:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltInAspNetCore/Views/Home/Index.cshtml?name=snippet_AspHost)]

<span data-ttu-id="f71ef-190">생성된 코드:</span><span class="sxs-lookup"><span data-stu-id="f71ef-190">The generated HTML:</span></span>

```html
<a href="https://microsoft.com/Home/About">About</a>
```

## <a name="asp-page"></a><span data-ttu-id="f71ef-191">asp-page</span><span class="sxs-lookup"><span data-stu-id="f71ef-191">asp-page</span></span>

<span data-ttu-id="f71ef-192">[asp-page](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.page) 특성은 Razor 페이지와 함께 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="f71ef-192">The [asp-page](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.page) attribute is used with Razor Pages.</span></span> <span data-ttu-id="f71ef-193">이는 앵커 태그의 `href` 특성 값을 특정 페이지로 설정하는 데 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="f71ef-193">Use it to set an anchor tag's `href` attribute value to a specific page.</span></span> <span data-ttu-id="f71ef-194">페이지 이름 앞에 슬래시("/")를 접두사로 사용하여 URL을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="f71ef-194">Prefixing the page name with a forward slash ("/") creates the URL.</span></span>

<span data-ttu-id="f71ef-195">다음 샘플은 참석자 Razor 페이지를 가리킵니다.</span><span class="sxs-lookup"><span data-stu-id="f71ef-195">The following sample points to the attendee Razor Page:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltInAspNetCore/Views/Home/Index.cshtml?name=snippet_AspPage)]

<span data-ttu-id="f71ef-196">생성된 코드:</span><span class="sxs-lookup"><span data-stu-id="f71ef-196">The generated HTML:</span></span>

```html
<a href="/Attendee">All Attendees</a>
```

<span data-ttu-id="f71ef-197">`asp-page` 특성은 `asp-route`, `asp-controller` 및 `asp-action` 특성과 함께 사용할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="f71ef-197">The `asp-page` attribute is mutually exclusive with the `asp-route`, `asp-controller`, and `asp-action` attributes.</span></span> <span data-ttu-id="f71ef-198">그러나 다음 태그에서 보여 주듯이 `asp-page`는 `asp-route-{value}`와 함께 사용하여 라우팅을 제어할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f71ef-198">However, `asp-page` can be used with `asp-route-{value}` to control routing, as the following markup demonstrates:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltInAspNetCore/Views/Home/Index.cshtml?name=snippet_AspPageAspRouteId)]

<span data-ttu-id="f71ef-199">생성된 코드:</span><span class="sxs-lookup"><span data-stu-id="f71ef-199">The generated HTML:</span></span>

```html
<a href="/Attendee?attendeeid=10">View Attendee</a>
```

## <a name="asp-page-handler"></a><span data-ttu-id="f71ef-200">asp-page-handler</span><span class="sxs-lookup"><span data-stu-id="f71ef-200">asp-page-handler</span></span>

<span data-ttu-id="f71ef-201">[asp-page-handler](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.pagehandler) 특성은 Razor 페이지와 함께 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="f71ef-201">The [asp-page-handler](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.pagehandler) attribute is used with Razor Pages.</span></span> <span data-ttu-id="f71ef-202">특정 페이지 처리기에 연결하기 위한 것입니다.</span><span class="sxs-lookup"><span data-stu-id="f71ef-202">It's intended for linking to specific page handlers.</span></span>

<span data-ttu-id="f71ef-203">다음 페이지 처리기를 살펴보세요.</span><span class="sxs-lookup"><span data-stu-id="f71ef-203">Consider the following page handler:</span></span>

[!code-csharp[](samples/TagHelpersBuiltInAspNetCore/Pages/Attendee.cshtml.cs?name=snippet_OnGetProfileHandler)]

<span data-ttu-id="f71ef-204">페이지 모델 관련 태그는 `OnGetProfile` 페이지 처리기에 연결됩니다.</span><span class="sxs-lookup"><span data-stu-id="f71ef-204">The page model's associated markup links to the `OnGetProfile` page handler.</span></span> <span data-ttu-id="f71ef-205">페이지 처리기 메서드 이름의 `On<Verb>` 접두사는 `asp-page-handler` 특성 값에서 생략됩니다.</span><span class="sxs-lookup"><span data-stu-id="f71ef-205">Note that the `On<Verb>` prefix of the page handler method name is omitted in the `asp-page-handler` attribute value.</span></span> <span data-ttu-id="f71ef-206">비동기 메서드인 경우 `Async` 접미사도 생략됩니다.</span><span class="sxs-lookup"><span data-stu-id="f71ef-206">If this were an asynchronous method, the `Async` suffix would be omitted too.</span></span>

[!code-cshtml[](samples/TagHelpersBuiltInAspNetCore/Views/Home/Index.cshtml?name=snippet_AspPageHandler)]

<span data-ttu-id="f71ef-207">생성된 코드:</span><span class="sxs-lookup"><span data-stu-id="f71ef-207">The generated HTML:</span></span>

```html
<a href="/Attendee?attendeeid=12&handler=Profile">Attendee Profile</a>
```

## <a name="additional-resources"></a><span data-ttu-id="f71ef-208">추가 리소스</span><span class="sxs-lookup"><span data-stu-id="f71ef-208">Additional resources</span></span>

* [<span data-ttu-id="f71ef-209">영역</span><span class="sxs-lookup"><span data-stu-id="f71ef-209">Areas</span></span>](xref:mvc/controllers/areas)
* [<span data-ttu-id="f71ef-210">Razor 페이지 소개</span><span class="sxs-lookup"><span data-stu-id="f71ef-210">Intro to Razor Pages</span></span>](xref:mvc/razor-pages/index)
