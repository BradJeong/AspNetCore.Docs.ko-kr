---
title: "레이아웃"
author: ardalis
description: 
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/layout
ms.openlocfilehash: f225e2a93edfc552961f9f16294bc0ace6eb0002
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/19/2018
---
# <a name="layout"></a><span data-ttu-id="e25c6-102">레이아웃</span><span class="sxs-lookup"><span data-stu-id="e25c6-102">Layout</span></span>

<span data-ttu-id="e25c6-103">으로 [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="e25c6-103">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="e25c6-104">뷰는 자주 visual 및 프로그래밍 요소를 공유합니다.</span><span class="sxs-lookup"><span data-stu-id="e25c6-104">Views frequently share visual and programmatic elements.</span></span> <span data-ttu-id="e25c6-105">이 문서에서는 일반적인 레이아웃을 사용 하 여 지시문을 공유 하 고 ASP.NET 응용 프로그램에서 뷰 렌더링 하기 전에 일반적인 코드를 실행 하는 방법에 알아봅니다.</span><span class="sxs-lookup"><span data-stu-id="e25c6-105">In this article, you'll learn how to use common layouts, share directives, and run common code before rendering views in your ASP.NET app.</span></span>

## <a name="what-is-a-layout"></a><span data-ttu-id="e25c6-106">레이아웃 무엇입니까</span><span class="sxs-lookup"><span data-stu-id="e25c6-106">What is a Layout</span></span>

<span data-ttu-id="e25c6-107">대부분의 웹 응용 프로그램은 사용자가 페이지를 탐색 하면서 일관 된 환경을 제공 하는 일반적인 레이아웃이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e25c6-107">Most web apps have a common layout that provides the user with a consistent experience as they navigate from page to page.</span></span> <span data-ttu-id="e25c6-108">레이아웃에는 일반적으로 응용 프로그램, 탐색 또는 메뉴 요소 머리글과 바닥글을 같은 공통 사용자 인터페이스 요소가 포함 됩니다.</span><span class="sxs-lookup"><span data-stu-id="e25c6-108">The layout typically includes common user interface elements such as the app header, navigation or menu elements, and footer.</span></span>

![페이지 레이아웃 예제](layout/_static/page-layout.png)

<span data-ttu-id="e25c6-110">스크립트 및 스타일 시트와 같은 일반적인 HTML 구조는 응용 프로그램 내에서 여러 페이지에 의해 자주 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="e25c6-110">Common HTML structures such as scripts and stylesheets are also frequently used by many pages within an app.</span></span> <span data-ttu-id="e25c6-111">이러한 공유 요소를 모두 정의 되어 있을 수는 *레이아웃* 파일에는 앱 내에서 사용 되는 모든 보기에서 참조할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e25c6-111">All of these shared elements may be defined in a *layout* file, which can then be referenced by any view used within the app.</span></span> <span data-ttu-id="e25c6-112">레이아웃에 따라 든이 터 뷰에서 중복 코드를 줄이기는 [하지 반복 직접 (드라이) 원칙](http://deviq.com/don-t-repeat-yourself/)합니다.</span><span class="sxs-lookup"><span data-stu-id="e25c6-112">Layouts reduce duplicate code in views, helping them follow the [Don't Repeat Yourself (DRY) principle](http://deviq.com/don-t-repeat-yourself/).</span></span>

<span data-ttu-id="e25c6-113">ASP.NET 응용 프로그램에 대 한 기본 레이아웃 규칙에 따라 라는 `_Layout.cshtml`합니다.</span><span class="sxs-lookup"><span data-stu-id="e25c6-113">By convention, the default layout for an ASP.NET app is named `_Layout.cshtml`.</span></span> <span data-ttu-id="e25c6-114">Visual Studio ASP.NET Core MVC 프로젝트 템플릿은 포함이 레이아웃 파일에는 `Views/Shared` 폴더:</span><span class="sxs-lookup"><span data-stu-id="e25c6-114">The Visual Studio ASP.NET Core MVC project template includes this layout file in the `Views/Shared` folder:</span></span>

![솔루션 탐색기에서 views 폴더](layout/_static/web-project-views.png)

<span data-ttu-id="e25c6-116">이 레이아웃 앱의 뷰에 대 한 최상위 수준 템플릿을 정의합니다.</span><span class="sxs-lookup"><span data-stu-id="e25c6-116">This layout defines a top level template for views in the app.</span></span> <span data-ttu-id="e25c6-117">앱에는 레이아웃 필요 하지 않습니다 하 고 앱 다양 한 레이아웃을 지정 하는 다른 뷰를 둘 이상의 레이아웃을 정의할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e25c6-117">Apps do not require a layout, and apps can define more than one layout, with different views specifying different layouts.</span></span>

<span data-ttu-id="e25c6-118">예로 `_Layout.cshtml`:</span><span class="sxs-lookup"><span data-stu-id="e25c6-118">An example `_Layout.cshtml`:</span></span>

[!code-html[Main](../../common/samples/WebApplication1/Views/Shared/_Layout.cshtml?highlight=42,66)]

## <a name="specifying-a-layout"></a><span data-ttu-id="e25c6-119">레이아웃을 지정합니다.</span><span class="sxs-lookup"><span data-stu-id="e25c6-119">Specifying a Layout</span></span>

<span data-ttu-id="e25c6-120">Razor 보기에 `Layout` 속성입니다.</span><span class="sxs-lookup"><span data-stu-id="e25c6-120">Razor views have a `Layout` property.</span></span> <span data-ttu-id="e25c6-121">이 속성을 설정 하 여 레이아웃을 지정 하는 개별 보기:</span><span class="sxs-lookup"><span data-stu-id="e25c6-121">Individual views specify a layout by setting this property:</span></span>

[!code-html[Main](../../common/samples/WebApplication1/Views/_ViewStart.cshtml?highlight=2)]

<span data-ttu-id="e25c6-122">지정한 레이아웃 전체 경로 사용할 수 있습니다 (예: `/Views/Shared/_Layout.cshtml`) 또는 이름의 일부 (예: `_Layout`).</span><span class="sxs-lookup"><span data-stu-id="e25c6-122">The layout specified can use a full path (example: `/Views/Shared/_Layout.cshtml`) or a partial name (example: `_Layout`).</span></span> <span data-ttu-id="e25c6-123">이름의 일부 제공 되 면 해당 표준 검색 프로세스를 사용 하 여 레이아웃 파일에 대 한 Razor 보기 엔진이 검색 됩니다.</span><span class="sxs-lookup"><span data-stu-id="e25c6-123">When a partial name is provided, the Razor view engine will search for the layout file using its standard discovery process.</span></span> <span data-ttu-id="e25c6-124">컨트롤러 관련 폴더를 먼저 검색, 뒤의 `Shared` 폴더입니다.</span><span class="sxs-lookup"><span data-stu-id="e25c6-124">The controller-associated folder is searched first, followed by the `Shared` folder.</span></span> <span data-ttu-id="e25c6-125">이 검색 프로세스는 검색에 사용 되는 컬렉션과 동일한 [부분 뷰](partial.md)합니다.</span><span class="sxs-lookup"><span data-stu-id="e25c6-125">This discovery process is identical to the one used to discover [partial views](partial.md).</span></span>

<span data-ttu-id="e25c6-126">기본적으로 모든 레이아웃 호출 해야 `RenderBody`합니다.</span><span class="sxs-lookup"><span data-stu-id="e25c6-126">By default, every layout must call `RenderBody`.</span></span> <span data-ttu-id="e25c6-127">모든 위치에 대 한 호출 `RenderBody` 은 배치 하는 보기의 내용을 렌더링 됩니다.</span><span class="sxs-lookup"><span data-stu-id="e25c6-127">Wherever the call to `RenderBody` is placed, the contents of the view will be rendered.</span></span>

<a name="layout-sections-label"></a>

### <a name="sections"></a><span data-ttu-id="e25c6-128">섹션</span><span class="sxs-lookup"><span data-stu-id="e25c6-128">Sections</span></span>

<span data-ttu-id="e25c6-129">하나 이상의 레이아웃을 참조할 필요에 따라 수 *섹션*를 호출 하 여 `RenderSection`합니다.</span><span class="sxs-lookup"><span data-stu-id="e25c6-129">A layout can optionally reference one or more *sections*, by calling `RenderSection`.</span></span> <span data-ttu-id="e25c6-130">섹션에서는 특정 페이지 요소를 배치할 위치를 구성 하는 방법을 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="e25c6-130">Sections provide a way to organize where certain page elements should be placed.</span></span> <span data-ttu-id="e25c6-131">호출할 때마다 `RenderSection` 않은 필수 또는 선택 여부를 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e25c6-131">Each call to `RenderSection` can specify whether that section is required or optional.</span></span> <span data-ttu-id="e25c6-132">필수 세션 발견 되지 않으면 예외가 throw 됩니다.</span><span class="sxs-lookup"><span data-stu-id="e25c6-132">If a required section is not found, an exception will be thrown.</span></span> <span data-ttu-id="e25c6-133">사용 하 여 섹션 내에서 렌더링할 콘텐츠를 지정 하는 개별 뷰에 `@section` Razor 구문입니다.</span><span class="sxs-lookup"><span data-stu-id="e25c6-133">Individual views specify the content to be rendered within a section using the `@section` Razor syntax.</span></span> <span data-ttu-id="e25c6-134">뷰 섹션을 정의 하는 경우 렌더링 해야 하거나 오류가 발생 합니다.</span><span class="sxs-lookup"><span data-stu-id="e25c6-134">If a view defines a section, it must be rendered (or an error will occur).</span></span>

<span data-ttu-id="e25c6-135">예 `@section` 뷰의 정의:</span><span class="sxs-lookup"><span data-stu-id="e25c6-135">An example `@section` definition in a view:</span></span>

```html
@section Scripts {
     <script type="text/javascript" src="/scripts/main.js"></script>
   }
   ```

<span data-ttu-id="e25c6-136">위의 코드에서 유효성 검사 스크립트에 추가 됩니다는 `scripts` 폼을 포함 하는 보기에는 섹션입니다.</span><span class="sxs-lookup"><span data-stu-id="e25c6-136">In the code above, validation scripts are added to the `scripts` section on a view that includes a form.</span></span> <span data-ttu-id="e25c6-137">동일한 응용 프로그램에서 다른 보기에는 추가 스크립트, 필요 하지 않은 스크립트 섹션을 정의 하려면 필요 하므로.</span><span class="sxs-lookup"><span data-stu-id="e25c6-137">Other views in the same application might not require any additional scripts, and so wouldn't need to define a scripts section.</span></span>

<span data-ttu-id="e25c6-138">뷰에 정의 된 섹션은 있으며 즉시 레이아웃 페이지 에서만 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e25c6-138">Sections defined in a view are available only in its immediate layout page.</span></span> <span data-ttu-id="e25c6-139">부분, 구성 요소 보기 또는 보기 시스템의 다른 부분에서 참조할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="e25c6-139">They cannot be referenced from partials, view components, or other parts of the view system.</span></span>

### <a name="ignoring-sections"></a><span data-ttu-id="e25c6-140">섹션을 무시합니다.</span><span class="sxs-lookup"><span data-stu-id="e25c6-140">Ignoring sections</span></span>

<span data-ttu-id="e25c6-141">기본적으로 본문 및 콘텐츠 페이지의 모든 섹션 모두 렌더링 해야 합니다는 레이아웃 페이지에서.</span><span class="sxs-lookup"><span data-stu-id="e25c6-141">By default, the body and all sections in a content page must all be rendered by the layout page.</span></span> <span data-ttu-id="e25c6-142">Razor 뷰 엔진 본문과 각 섹션 렌더링 될 지 여부를 추적 하 여이 적용 합니다.</span><span class="sxs-lookup"><span data-stu-id="e25c6-142">The Razor view engine enforces this by tracking whether the body and each section have been rendered.</span></span>

<span data-ttu-id="e25c6-143">에 대 한 본문 또는 섹션을 무시 하는 뷰 엔진이 명령, 호출의 `IgnoreBody` 및 `IgnoreSection` 메서드.</span><span class="sxs-lookup"><span data-stu-id="e25c6-143">To instruct the view engine to ignore the body or sections, call the `IgnoreBody` and `IgnoreSection` methods.</span></span>

<span data-ttu-id="e25c6-144">본문 및 Razor 페이지에서 모든 섹션 렌더링 하거나 무시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="e25c6-144">The body and every section in a Razor page must be either rendered or ignored.</span></span>

<a name="viewimports"></a>

## <a name="importing-shared-directives"></a><span data-ttu-id="e25c6-145">공유 지시문 가져오기</span><span class="sxs-lookup"><span data-stu-id="e25c6-145">Importing Shared Directives</span></span>

<span data-ttu-id="e25c6-146">뷰 또는 네임 스페이스 가져오기 등의 많은 작업을 Razor 지시문을 사용할 수 있습니다 [종속성 주입](dependency-injection.md)합니다.</span><span class="sxs-lookup"><span data-stu-id="e25c6-146">Views can use Razor directives to do many things, such as importing namespaces or performing [dependency injection](dependency-injection.md).</span></span> <span data-ttu-id="e25c6-147">일반적인 많은 보기에서 공유 하는 지시문을 지정할 수 있습니다 `_ViewImports.cshtml` 파일입니다.</span><span class="sxs-lookup"><span data-stu-id="e25c6-147">Directives shared by many views may be specified in a common `_ViewImports.cshtml` file.</span></span> <span data-ttu-id="e25c6-148">`_ViewImports` 파일은 다음 지시문을 지원 합니다.</span><span class="sxs-lookup"><span data-stu-id="e25c6-148">The `_ViewImports` file supports the following directives:</span></span>

* `@addTagHelper`

* `@removeTagHelper`

* `@tagHelperPrefix`

* `@using`

* `@model`

* `@inherits`

* `@inject`

<span data-ttu-id="e25c6-149">파일 섹션 정의 함수 등의 다른 Razor 기능을 지원 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="e25c6-149">The file does not support other Razor features, such as functions and section definitions.</span></span>

<span data-ttu-id="e25c6-150">샘플 `_ViewImports.cshtml` 파일:</span><span class="sxs-lookup"><span data-stu-id="e25c6-150">A sample `_ViewImports.cshtml` file:</span></span>

[!code-html[Main](../../common/samples/WebApplication1/Views/_ViewImports.cshtml)]

<span data-ttu-id="e25c6-151">`_ViewImports.cshtml` 파일 ASP.NET Core MVC 응용 프로그램에 일반적으로 배치 되는 `Views` 폴더입니다.</span><span class="sxs-lookup"><span data-stu-id="e25c6-151">The `_ViewImports.cshtml` file for an ASP.NET Core MVC app is typically placed in the `Views` folder.</span></span> <span data-ttu-id="e25c6-152">A `_ViewImports.cshtml` 경우에만 적용 됩니다 뷰는 해당 폴더와 하위 폴더 내에서 각 폴더에서 파일을 배치할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e25c6-152">A `_ViewImports.cshtml` file can be placed within any folder, in which case it will only be applied to views within that folder and its subfolders.</span></span> <span data-ttu-id="e25c6-153">`_ViewImports`루트 수준에서 시작 파일은 처리 있으며 다음 최대 선행 각 폴더에 대 한 뷰 자체의 위치 루트 수준에서 설정을 지정할 수 있습니다 재정의할 수 폴더 수준에서.</span><span class="sxs-lookup"><span data-stu-id="e25c6-153">`_ViewImports` files are processed starting at the root level, and then for each folder leading up to the location of the view itself, so settings specified at the root level may be overridden at the folder level.</span></span>

<span data-ttu-id="e25c6-154">예를 들어 루트 수준 `_ViewImports.cshtml` 파일 지정 `@model` 및 `@addTagHelper`, 또 다른 `_ViewImports.cshtml` 파일 보기의 컨트롤러 관련 폴더를 다른 지정 `@model` 다른 추가 `@addTagHelper`, 보기 두 태그 도우미에 액세스할 수 있는 및 후자 ´ ֲ `@model`합니다.</span><span class="sxs-lookup"><span data-stu-id="e25c6-154">For example, if a root level `_ViewImports.cshtml` file specifies `@model` and `@addTagHelper`, and another `_ViewImports.cshtml` file in the controller-associated folder of the view specifies a different `@model` and adds another `@addTagHelper`, the view will have access to both tag helpers and will use the latter `@model`.</span></span>

<span data-ttu-id="e25c6-155">여러 개인 경우 `_ViewImports.cshtml` 파일 보기에 대 한 실행에 포함 된 지시문의 동작을 결합 된 `ViewImports.cshtml` 파일이 다음과 같이 됩니다.</span><span class="sxs-lookup"><span data-stu-id="e25c6-155">If multiple `_ViewImports.cshtml` files are run for a view, combined behavior of the directives included in the `ViewImports.cshtml` files will be as follows:</span></span>

* <span data-ttu-id="e25c6-156">`@addTagHelper``@removeTagHelper`: 순서로 모든 실행</span><span class="sxs-lookup"><span data-stu-id="e25c6-156">`@addTagHelper`, `@removeTagHelper`: all run, in order</span></span>

* <span data-ttu-id="e25c6-157">`@tagHelperPrefix`: 모든 다른 보기에 가장 가까운 스택을 재정의</span><span class="sxs-lookup"><span data-stu-id="e25c6-157">`@tagHelperPrefix`: the closest one to the view overrides any others</span></span>

* <span data-ttu-id="e25c6-158">`@model`: 모든 다른 보기에 가장 가까운 스택을 재정의</span><span class="sxs-lookup"><span data-stu-id="e25c6-158">`@model`: the closest one to the view overrides any others</span></span>

* <span data-ttu-id="e25c6-159">`@inherits`: 모든 다른 보기에 가장 가까운 스택을 재정의</span><span class="sxs-lookup"><span data-stu-id="e25c6-159">`@inherits`: the closest one to the view overrides any others</span></span>

* <span data-ttu-id="e25c6-160">`@using`: 모든 포함 됩니다. 중복 항목은 무시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="e25c6-160">`@using`: all are included; duplicates are ignored</span></span>

* <span data-ttu-id="e25c6-161">`@inject`: 각 속성에 대 한 보기에 가장 가까운 스택을 속성 이름이 같은 다른 재정의</span><span class="sxs-lookup"><span data-stu-id="e25c6-161">`@inject`: for each property, the closest one to the view overrides any others with the same property name</span></span>

<a name="viewstart"></a>

## <a name="running-code-before-each-view"></a><span data-ttu-id="e25c6-162">각 보기 전에 코드를 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="e25c6-162">Running Code Before Each View</span></span>

<span data-ttu-id="e25c6-163">모든 보기 전에 실행 해야 할 코드가 있으면에 배치 해야는 `_ViewStart.cshtml` 파일입니다.</span><span class="sxs-lookup"><span data-stu-id="e25c6-163">If you have code you need to run before every view, this should be placed in the `_ViewStart.cshtml` file.</span></span> <span data-ttu-id="e25c6-164">일반적으로는 `_ViewStart.cshtml` 파일은 `Views` 폴더입니다.</span><span class="sxs-lookup"><span data-stu-id="e25c6-164">By convention, the `_ViewStart.cshtml` file is located in the `Views` folder.</span></span> <span data-ttu-id="e25c6-165">에 나열 된 문을 `_ViewStart.cshtml` 마다 전체 보기 (하지 레이아웃 및 아님 뷰) 보다 먼저 실행 됩니다.</span><span class="sxs-lookup"><span data-stu-id="e25c6-165">The statements listed in `_ViewStart.cshtml` are run before every full view (not layouts, and not partial views).</span></span> <span data-ttu-id="e25c6-166">마찬가지로 [ViewImports.cshtml](xref:mvc/views/layout#viewimports), `_ViewStart.cshtml` 계층적입니다.</span><span class="sxs-lookup"><span data-stu-id="e25c6-166">Like [ViewImports.cshtml](xref:mvc/views/layout#viewimports), `_ViewStart.cshtml` is hierarchical.</span></span> <span data-ttu-id="e25c6-167">경우는 `_ViewStart.cshtml` 파일 보기 컨트롤러 관련 폴더에 정의 되어의 루트에 정의 된 후 실행 됩니다는 `Views` 폴더 (있는 경우).</span><span class="sxs-lookup"><span data-stu-id="e25c6-167">If a `_ViewStart.cshtml` file is defined in the controller-associated view folder, it will be run after the one defined in the root of the `Views` folder (if any).</span></span>

<span data-ttu-id="e25c6-168">샘플 `_ViewStart.cshtml` 파일:</span><span class="sxs-lookup"><span data-stu-id="e25c6-168">A sample `_ViewStart.cshtml` file:</span></span>

[!code-html[Main](../../common/samples/WebApplication1/Views/_ViewStart.cshtml)]

<span data-ttu-id="e25c6-169">위의 파일 모든 보기에서 사용 되도록 지정 된 `_Layout.cshtml` 레이아웃 합니다.</span><span class="sxs-lookup"><span data-stu-id="e25c6-169">The file above specifies that all views will use the `_Layout.cshtml` layout.</span></span>

> [!NOTE]
> <span data-ttu-id="e25c6-170">모두 `_ViewStart.cshtml` 나 `_ViewImports.cshtml` 에 일반적으로 배치 되는 `/Views/Shared` 폴더입니다.</span><span class="sxs-lookup"><span data-stu-id="e25c6-170">Neither `_ViewStart.cshtml` nor `_ViewImports.cshtml` are typically placed in the `/Views/Shared` folder.</span></span> <span data-ttu-id="e25c6-171">이러한 파일의 버전을 앱 수준에서 직접 배치 해야는 `/Views` 폴더입니다.</span><span class="sxs-lookup"><span data-stu-id="e25c6-171">The app-level versions of these files should be placed directly in the `/Views` folder.</span></span>
