---
title: "ASP.NET Core의 Razor 페이지 소개"
author: Rick-Anderson
description: "이 문서는 ASP.NET Core의 Razor 페이지를 사용하여 페이지에 초점을 맞춘 시나리오의 손쉬운 개발에 관한 개요를 제공합니다."
keywords: "ASP.NET Core, Razor 페이지"
ms.author: riande
manager: wpickett
ms.date: 09/12/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/razor-pages/index
ms.openlocfilehash: 31d8b1f662d3d5e7dad8f459d951c7b8181148b8
ms.sourcegitcommit: 5834afb87e4262b9b88e60e3fe6c735e61a1e08d
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/20/2017
---
# <a name="introduction-to-razor-pages-in-aspnet-core"></a><span data-ttu-id="6a426-104">ASP.NET Core의 Razor 페이지 소개</span><span class="sxs-lookup"><span data-stu-id="6a426-104">Introduction to Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="6a426-105">작성자: [Rick Anderson](https://twitter.com/RickAndMSFT) 및 [Ryan Nowak](https://github.com/rynowak)</span><span class="sxs-lookup"><span data-stu-id="6a426-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Ryan Nowak](https://github.com/rynowak)</span></span>

<span data-ttu-id="6a426-106">Razor 페이지는 더 쉽고 더 생산적으로 코딩 페이지에 초점을 맞춘 시나리오를 만드는 ASP.NET Core MVC의 새로운 기능입니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-106">Razor Pages is a new feature of ASP.NET Core MVC that makes coding page-focused scenarios easier and more productive.</span></span>

<span data-ttu-id="6a426-107">모델-뷰-컨트롤러 방법을 사용하는 자습서를 검색할 경우 [ASP.NET Core MVC 시작](xref:tutorials/first-mvc-app/start-mvc)을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="6a426-107">If you're looking for a tutorial that uses the Model-View-Controller approach, see [Getting started with ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc).</span></span>

<span data-ttu-id="6a426-108">이 문서에서는 Razor 페이지를 소개합니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-108">This document provides an introduction to Razor Pages.</span></span> <span data-ttu-id="6a426-109">이 문서는 단계별 자습서가 아닙니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-109">It's not a step by step tutorial.</span></span> <span data-ttu-id="6a426-110">이해하기 어려운 섹션이 있는 경우 [Razor 페이지 시작](xref:tutorials/razor-pages/razor-pages-start)을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="6a426-110">If you find some of the sections difficult to follow, see [Getting started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start).</span></span>

<a name="prerequisites"></a>

## <a name="aspnet-core-20-prerequisites"></a><span data-ttu-id="6a426-111">ASP.NET Core 2.0 필요 구성 요소</span><span class="sxs-lookup"><span data-stu-id="6a426-111">ASP.NET Core 2.0 prerequisites</span></span>

<span data-ttu-id="6a426-112">[.NET Core](https://www.microsoft.com/net/core) 2.0.0 이상을 설치합니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-112">Install [.NET Core](https://www.microsoft.com/net/core) 2.0.0 or later.</span></span>

<span data-ttu-id="6a426-113">Visual Studio를 사용할 경우 다음 워크로드가 포함된 [Visual Studio](https://www.visualstudio.com/vs/) 2017 버전 15.3 이상을 설치합니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-113">If you're using Visual Studio, install [Visual Studio](https://www.visualstudio.com/vs/) 2017 version 15.3 or later with the following workloads:</span></span>

* <span data-ttu-id="6a426-114">**ASP.NET 및 웹 개발**</span><span class="sxs-lookup"><span data-stu-id="6a426-114">**ASP.NET and web development**</span></span>
* <span data-ttu-id="6a426-115">**.NET Core 플랫폼 간 개발**</span><span class="sxs-lookup"><span data-stu-id="6a426-115">**.NET Core cross-platform development**</span></span>

<a name="rpvs17"></a>

## <a name="creating-a-razor-pages-project"></a><span data-ttu-id="6a426-116">Razor 페이지 프로젝트 만들기</span><span class="sxs-lookup"><span data-stu-id="6a426-116">Creating a Razor Pages project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6a426-117">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6a426-117">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="6a426-118">Visual Studio를 사용하여 Razor 페이지 프로젝트를 만드는 방법에 대한 자세한 내용은 [Razor 페이지 시작](xref:tutorials/razor-pages/razor-pages-start)을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="6a426-118">See [Getting started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) for detailed instructions on how to create a Razor Pages project using Visual Studio.</span></span>

#   <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="6a426-119">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="6a426-119">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="6a426-120">명령줄에서 `dotnet new razor`를 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-120">Run `dotnet new razor` from the command line.</span></span>

<span data-ttu-id="6a426-121">Mac용 Visual Studio에서 생성된 *.csproj* 파일을 엽니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-121">Open the generated *.csproj* file from Visual Studio for Mac.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="6a426-122">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6a426-122">Visual Studio Code</span></span>](#tab/visual-studio-code) 

<span data-ttu-id="6a426-123">명령줄에서 `dotnet new razor`를 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-123">Run `dotnet new razor` from the command line.</span></span>

#   <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="6a426-124">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="6a426-124">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="6a426-125">명령줄에서 `dotnet new razor`를 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-125">Run `dotnet new razor` from the command line.</span></span>

---

## <a name="razor-pages"></a><span data-ttu-id="6a426-126">Razor 페이지</span><span class="sxs-lookup"><span data-stu-id="6a426-126">Razor Pages</span></span>

<span data-ttu-id="6a426-127">Razor 페이지는 *Startup.cs*에서 사용하도록 설정됩니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-127">Razor Pages is enabled in *Startup.cs*:</span></span>

[!code-cs[main](index/sample/RazorPagesIntro/Startup.cs?name=snippet_Startup)]

<span data-ttu-id="6a426-128">기본 페이지를 고려해 봅니다. <a name="OnGet"></a></span><span class="sxs-lookup"><span data-stu-id="6a426-128">Consider a basic page: <a name="OnGet"></a></span></span>

[!code-cshtml[main](index/sample/RazorPagesIntro/Pages/Index.cshtml)]

<span data-ttu-id="6a426-129">이전 코드는 Razor 뷰 파일과 매우 비슷합니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-129">The preceding code looks a lot like a Razor view file.</span></span> <span data-ttu-id="6a426-130">차이점은 `@page` 지시문입니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-130">What makes it different is the `@page` directive.</span></span> <span data-ttu-id="6a426-131">`@page`는 파일을 MVC 작업으로 만듭니다. 즉, 컨트롤러를 거치지 않고 요청을 직접 처리합니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-131">`@page` makes the file into an MVC action - which means that it handles requests directly, without going through a controller.</span></span> <span data-ttu-id="6a426-132">`@page`는 페이지의 첫 번째 Razor 지시문이어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-132">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="6a426-133">`@page`는 다른 Razor 생성자의 동작에 영향을 미칩니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-133">`@page` affects the behavior of other Razor constructs.</span></span>

<span data-ttu-id="6a426-134">`PageModel` 클래스를 사용하는 비슷한 페이지는 다음 두 파일에 표시됩니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-134">A similar page, using a `PageModel` class, is shown in the following two files.</span></span> <span data-ttu-id="6a426-135">*Pages/Index2.cshtml* 파일:</span><span class="sxs-lookup"><span data-stu-id="6a426-135">The *Pages/Index2.cshtml* file:</span></span>

[!code-cshtml[main](index/sample/RazorPagesIntro/Pages/Index2.cshtml)]

<span data-ttu-id="6a426-136">*Pages/Index2.cshtml.cs* "코드 숨김" 파일:</span><span class="sxs-lookup"><span data-stu-id="6a426-136">The *Pages/Index2.cshtml.cs* "code-behind" file:</span></span>

[!code-cs[main](index/sample/RazorPagesIntro/Pages/Index2.cshtml.cs)]

<span data-ttu-id="6a426-137">일반적으로 `PageModel` 클래스 파일의 이름은 Razor 페이지 파일과 동일하고 *.cs*가 추가됩니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-137">By convention, the `PageModel` class file has the same name as the Razor Page file with *.cs* appended.</span></span> <span data-ttu-id="6a426-138">예를 들어 이전 Razor 페이지는 *Pages/Index2.cshtml*입니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-138">For example, the previous Razor Page is *Pages/Index2.cshtml*.</span></span> <span data-ttu-id="6a426-139">`PageModel` 클래스가 포함된 파일의 이름은 *Pages/Index2.cshtml.cs*입니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-139">The file containing the `PageModel` class is named *Pages/Index2.cshtml.cs*.</span></span>

<span data-ttu-id="6a426-140">페이지에 대한 URL 경로 연결은 파일 시스템의 페이지 위치에 따라 결정됩니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-140">The associations of URL paths to pages are determined by the page's location in the file system.</span></span> <span data-ttu-id="6a426-141">다음 표에서는 Razor 페이지 경로 및 일치하는 URL을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-141">The following table shows a Razor Page path and the matching URL:</span></span>

| <span data-ttu-id="6a426-142">파일 이름 및 경로</span><span class="sxs-lookup"><span data-stu-id="6a426-142">File name and path</span></span>               | <span data-ttu-id="6a426-143">일치하는 URL</span><span class="sxs-lookup"><span data-stu-id="6a426-143">matching URL</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="6a426-144">*/Pages/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="6a426-144">*/Pages/Index.cshtml*</span></span> | <span data-ttu-id="6a426-145">`/` 또는 `/Index`</span><span class="sxs-lookup"><span data-stu-id="6a426-145">`/` or `/Index`</span></span> |
| <span data-ttu-id="6a426-146">*/Pages/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="6a426-146">*/Pages/Contact.cshtml*</span></span> | `/Contact` |
| <span data-ttu-id="6a426-147">*/Pages/Store/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="6a426-147">*/Pages/Store/Contact.cshtml*</span></span> | `/Store/Contact` |
| <span data-ttu-id="6a426-148">*/Pages/Store/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="6a426-148">*/Pages/Store/Index.cshtml*</span></span> | <span data-ttu-id="6a426-149">`/Store` 또는 `/Store/Index`</span><span class="sxs-lookup"><span data-stu-id="6a426-149">`/Store` or `/Store/Index`</span></span> |

<span data-ttu-id="6a426-150">메모:</span><span class="sxs-lookup"><span data-stu-id="6a426-150">Notes:</span></span>

* <span data-ttu-id="6a426-151">런타임은 기본적으로 *Pages* 폴더에서 Razor 페이지 파일을 검색합니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-151">The runtime looks for Razor Pages files in the *Pages* folder by default.</span></span>
* <span data-ttu-id="6a426-152">URL에 페이지가 포함되어 있지 않을 경우 `Index`가 기본 페이지입니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-152">`Index` is the default page when a URL doesn't include a page.</span></span>

## <a name="writing-a-basic-form"></a><span data-ttu-id="6a426-153">기본 폼 작성</span><span class="sxs-lookup"><span data-stu-id="6a426-153">Writing a basic form</span></span>

<span data-ttu-id="6a426-154">Razor 페이지 기능은 웹 브라우저에서 사용되는 일반 패턴을 쉽게 만들도록 고안되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-154">Razor Pages features are designed to make common patterns used with web browsers easy.</span></span> <span data-ttu-id="6a426-155">[모델 바인딩](xref:mvc/models/model-binding), [태그 도우미](xref:mvc/views/tag-helpers/intro) 및 HTML 도우미는 모두 Razor 페이지 클래스에 정의된 속성에서 *제대로 작동*합니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-155">[Model binding](xref:mvc/models/model-binding), [Tag Helpers](xref:mvc/views/tag-helpers/intro), and HTML helpers all *just work* with the properties defined in a Razor Page class.</span></span> <span data-ttu-id="6a426-156">`Contact` 모델에 대한 기본 “연락처” 폼을 구현하는 페이지를 고려해 봅니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-156">Consider a page that implements a basic "contact us" form for the `Contact` model:</span></span>

<span data-ttu-id="6a426-157">이 문서에 있는 샘플의 경우 `DbContext`는 [Startup.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16) 파일에서 초기화됩니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-157">For the samples in this document, the `DbContext` is initialized in the [Startup.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16) file.</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Startup.cs?highlight=15-16)]

<span data-ttu-id="6a426-158">데이터 모델:</span><span class="sxs-lookup"><span data-stu-id="6a426-158">The data model:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Data/Customer.cs)]

<span data-ttu-id="6a426-159">db 컨텍스트:</span><span class="sxs-lookup"><span data-stu-id="6a426-159">The db context:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Data/AppDbContext.cs)]

<span data-ttu-id="6a426-160">*Pages/Create.cshtml* 뷰 파일:</span><span class="sxs-lookup"><span data-stu-id="6a426-160">The *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Create.cshtml)]

<span data-ttu-id="6a426-161">뷰에 대한 *Pages/Create.cshtml.cs* 코드 숨김 파일:</span><span class="sxs-lookup"><span data-stu-id="6a426-161">The *Pages/Create.cshtml.cs* code-behind file for the view:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_ALL)]

<span data-ttu-id="6a426-162">일반적으로 `PageModel` 클래스를 `<PageName>Model`이라고 하고 이 클래스는 페이지와 동일한 네임스페이스에 있습니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-162">By convention, the `PageModel` class is called `<PageName>Model` and is in the same namespace as the page.</span></span>

<span data-ttu-id="6a426-163">`PageModel` 클래스를 사용하면 해당 프레젠테이션에서 페이지의 논리를 분리합니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-163">The `PageModel` class allows separation of the logic of a page from its presentation.</span></span> <span data-ttu-id="6a426-164">페이지에 전송된 요청 및 페이지를 렌더링하는 데 사용되는 데이터에 대한 페이지 처리기를 정의합니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-164">It defines page handlers for requests sent to the page and the data used to render the page.</span></span> <span data-ttu-id="6a426-165">이렇게 분리하면 [종속성 주입](xref:fundamentals/dependency-injection)을 통해 페이지 종속성을 관리할 수 있고 [단위 테스트](xref:testing/razor-pages-testing)를 페이지로 관리할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-165">This separation allows you to manage page dependencies through [dependency injection](xref:fundamentals/dependency-injection) and to [unit test](xref:testing/razor-pages-testing) the pages.</span></span>

<span data-ttu-id="6a426-166">페이지에는 `POST` 요청에서 실행되는 `OnPostAsync` *처리기 메서드*가 있습니다(사용자가 폼을 게시할 때).</span><span class="sxs-lookup"><span data-stu-id="6a426-166">The page has an `OnPostAsync` *handler method*, which runs on `POST` requests (when a user posts the form).</span></span> <span data-ttu-id="6a426-167">HTTP 동사에 대한 처리기 메서드를 추가할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-167">You can add handler methods for any HTTP verb.</span></span> <span data-ttu-id="6a426-168">가장 일반적인 처리기는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-168">The most common handlers are:</span></span>

* <span data-ttu-id="6a426-169">`OnGet` - 페이지에 필요한 상태를 초기화합니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-169">`OnGet` to initialize state needed for the page.</span></span> <span data-ttu-id="6a426-170">[OnGet](#OnGet) 샘플.</span><span class="sxs-lookup"><span data-stu-id="6a426-170">[OnGet](#OnGet) sample.</span></span>
* <span data-ttu-id="6a426-171">`OnPost` - 제출에서 처리합니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-171">`OnPost` to handle form submissions.</span></span>

<span data-ttu-id="6a426-172">`Async` 명명 접미사는 선택 사항이지만 비동기 함수에 대한 규칙에서 종종 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-172">The `Async` naming suffix is optional but is often used by convention for asynchronous functions.</span></span> <span data-ttu-id="6a426-173">이전 예제의 `OnPostAsync` 코드는 일반적으로 컨트롤러에서 작성되는 것과 비슷해 보입니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-173">The `OnPostAsync` code in the preceding example looks similar to what you would normally write in a controller.</span></span> <span data-ttu-id="6a426-174">이전 코드는 Razor 페이지에 일반적인 코드입니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-174">The preceding code is typical for Razor Pages.</span></span> <span data-ttu-id="6a426-175">[모델 바인딩](xref:mvc/models/model-binding), [유효성 검사](xref:mvc/models/validation) 및 작업 결과 같은 대부분의 MVC 기본 형식이 공유됩니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-175">Most of the MVC primitives like [model binding](xref:mvc/models/model-binding), [validation](xref:mvc/models/validation), and action results are shared.</span></span>  <!-- Review: Ryan, can we get a list of what is shared and what isn't? -->

<span data-ttu-id="6a426-176">이전 `OnPostAsync` 메서드:</span><span class="sxs-lookup"><span data-stu-id="6a426-176">The previous `OnPostAsync` method:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="6a426-177">`OnPostAsync`의 기본 흐름:</span><span class="sxs-lookup"><span data-stu-id="6a426-177">The basic flow of `OnPostAsync`:</span></span>

<span data-ttu-id="6a426-178">유효성 검사 오류를 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-178">Check for validation errors.</span></span>

*  <span data-ttu-id="6a426-179">오류가 없는 경우 데이터를 저장하고 리디렉션됩니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-179">If there are no errors, save the data and redirect.</span></span>
*  <span data-ttu-id="6a426-180">오류가 있는 경우 유효성 검사 메시지가 포함된 페이지를 다시 표시합니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-180">If there are errors, show the page again with validation messages.</span></span> <span data-ttu-id="6a426-181">클라이언트 쪽 유효성 검사는 기존 ASP.NET Core MVC 응용 프로그램과 동일합니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-181">Client-side validation is identical to traditional ASP.NET Core MVC applications.</span></span> <span data-ttu-id="6a426-182">대부분의 경우 유효성 검사 오류는 클라이언트에서 검색되고 서버에는 제출되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-182">In many cases, validation errors would be detected on the client, and never submitted to the server.</span></span>

<span data-ttu-id="6a426-183">데이터를 성공적으로 입력하면 `OnPostAsync` 처리기 메서드는 `RedirectToPage` 도우미 메서드를 호출하여 `RedirectToPageResult` 인스턴스를 반환합니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-183">When the data is entered successfully, the `OnPostAsync` handler method calls the `RedirectToPage` helper method to return an instance of `RedirectToPageResult`.</span></span> <span data-ttu-id="6a426-184">`RedirectToPage`는 `RedirectToAction` 또는 `RedirectToRoute`와 비슷하지만 페이지에 대해 사용자 지정된 새 작업 결과입니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-184">`RedirectToPage` is a new action result, similar to `RedirectToAction` or `RedirectToRoute`, but customized for pages.</span></span> <span data-ttu-id="6a426-185">이전 샘플에서 이 메서드는 루트 인덱스 페이지(`/Index`)로 리디렉션됩니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-185">In the preceding sample, it redirects to the root Index page (`/Index`).</span></span> <span data-ttu-id="6a426-186">`RedirectToPage`는 [페이지에 대한 URL 생성](#url_gen) 섹션에서 자세히 설명합니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-186">`RedirectToPage` is detailed in the [URL generation for Pages](#url_gen) section.</span></span>

<span data-ttu-id="6a426-187">서버에 전달되는 유효성 오류가 제출된 폼에 있는 경우 `OnPostAsync` 처리기 메서드는 `Page` 도우미 메서드를 호출합니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-187">When the submitted form has validation errors (that are passed to the server), the`OnPostAsync` handler method calls the `Page` helper method.</span></span> <span data-ttu-id="6a426-188">`Page`는 `PageResult` 인스턴스를 반환합니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-188">`Page` returns an instance of `PageResult`.</span></span> <span data-ttu-id="6a426-189">`Page` 반환은 컨트롤의 작업이 `View`를 반환하는 방식과 비슷합니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-189">Returning `Page` is similar to how actions in controllers return `View`.</span></span> <span data-ttu-id="6a426-190">`PageResult`는 처리기 메서드의 기본 <!-- Review  --> 반환 형식입니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-190">`PageResult` is the default <!-- Review  --> return type for a handler method.</span></span> <span data-ttu-id="6a426-191">`void`를 반환하는 처리기 메서드는 페이지를 렌더링합니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-191">A handler method that returns `void` renders the page.</span></span>

<span data-ttu-id="6a426-192">`Customer` 속성은 `[BindProperty]` 특성을 사용하여 모델 바인딩으로 옵트인(opt in)합니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-192">The `Customer` property uses `[BindProperty]` attribute to opt in to model binding.</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_PageModel&highlight=10-11)]

<span data-ttu-id="6a426-193">Razor 페이지는 기본적으로 GET이 아닌 동사에만 속성을 바인딩합니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-193">Razor Pages, by default, bind properties only with non-GET verbs.</span></span> <span data-ttu-id="6a426-194">속성에 바인딩하면 작성해야 하는 코드 양이 감소할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-194">Binding to properties can reduce the amount of code you have to write.</span></span> <span data-ttu-id="6a426-195">바인딩은 동일한 속성을 사용하여 폼 필드(`<input asp-for="Customer.Name" />`)를 렌더링하고 입력을 허용하는 방식으로 코드를 줄입니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-195">Binding reduces code by using the same property to render form fields (`<input asp-for="Customer.Name" />`) and accept the input.</span></span>

<span data-ttu-id="6a426-196">홈페이지(*Index.cshtml*):</span><span class="sxs-lookup"><span data-stu-id="6a426-196">The home page (*Index.cshtml*):</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Index.cshtml)]

<span data-ttu-id="6a426-197">코드 숨김 *Index.cshtml.cs* 파일:</span><span class="sxs-lookup"><span data-stu-id="6a426-197">The code behind *Index.cshtml.cs* file:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs)]

<span data-ttu-id="6a426-198">*Index.cshtml* 파일에는 각 연락처의 편집 링크를 만들기 위해 다음 태그가 포함됩니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-198">The *Index.cshtml* file contains the following markup to create an edit link for each contact:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=21)]

<span data-ttu-id="6a426-199">[앵커 태그 도우미](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper)는 `asp-route-{value}` 특성을 사용하여 편집 페이지 링크를 생성했습니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-199">The [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) used the `asp-route-{value}` attribute to generate a link to the Edit page.</span></span> <span data-ttu-id="6a426-200">링크에는 연락처 ID와 함께 경로 데이터가 포함됩니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-200">The link contains route data with the contact ID.</span></span> <span data-ttu-id="6a426-201">예를 들어, `http://localhost:5000/Edit/1`을 입력합니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-201">For example, `http://localhost:5000/Edit/1`.</span></span>

<span data-ttu-id="6a426-202">*Pages/Edit.cshtml* 파일:</span><span class="sxs-lookup"><span data-stu-id="6a426-202">The *Pages/Edit.cshtml* file:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Edit.cshtml?highlight=1)]

<span data-ttu-id="6a426-203">첫 번째 줄에는 `@page "{id:int}"` 지시문이 포함됩니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-203">The first line contains the `@page "{id:int}"` directive.</span></span> <span data-ttu-id="6a426-204">라우팅 제약 조건 `"{id:int}"`는 `int` 경로 데이터가 포함된 페이지에 대한 요청을 허용하도록 페이지에 지시합니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-204">The routing constraint`"{id:int}"` tells the page to accept requests to the page that contain `int` route data.</span></span> <span data-ttu-id="6a426-205">페이지에 대한 요청에 `int`로 변환될 수 있는 경로 데이터가 없으면 런타임은 HTTP 404(찾을 수 없음) 오류를 반환합니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-205">If a request to the page doesn't contain route data that can be converted to an `int`, the runtime returns an HTTP 404 (not found) error.</span></span>

<span data-ttu-id="6a426-206">*Pages/Edit.cshtml.cs* 파일:</span><span class="sxs-lookup"><span data-stu-id="6a426-206">The *Pages/Edit.cshtml.cs* file:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Edit.cshtml.cs)]

<span data-ttu-id="6a426-207">또한 *Index.cshtml* 파일에는 각 고객 연락처의 삭제 단추를 만드는 표시가 포함됩니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-207">The *Index.cshtml* file also contains markup to create a delete button for each customer contact:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=22-23)]

<span data-ttu-id="6a426-208">삭제 단추가 HTML로 렌더링되는 경우 해당 `formaction`에는 다음을 위한 매개 변수가 포함됩니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-208">When the delete button is rendered in HTML, its `formaction` includes parameters for:</span></span>

* <span data-ttu-id="6a426-209">`asp-route-id` 특성에서 지정된 고객 연락처 ID</span><span class="sxs-lookup"><span data-stu-id="6a426-209">The customer contact ID specified by the `asp-route-id` attribute.</span></span>
* <span data-ttu-id="6a426-210">`asp-page-handler` 특성에서 지정된 `handler`</span><span class="sxs-lookup"><span data-stu-id="6a426-210">The `handler` specified by the `asp-page-handler` attribute.</span></span>

<span data-ttu-id="6a426-211">`1`이라는 고객 연락처 ID를 포함한 렌더링된 삭제 단추의 예는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-211">Here is an example of a rendered delete button with a customer contact ID of `1`:</span></span>

```html
<button type="submit" formaction="/?id=1&amp;handler=delete">delete</button>
```

<span data-ttu-id="6a426-212">단추를 선택하면 양식 `POST` 요청이 서버에 전송됩니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-212">When the button is selected, a form `POST` request is sent to the server.</span></span> <span data-ttu-id="6a426-213">이름 규칙에 따라 `OnPost[handler]Async` 구성표에 해당하는 `handler` 매개 변수 값을 기반으로 처리기 메서드의 이름을 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-213">By convention, the name of the handler method is selected based the value of the `handler` parameter according to the scheme `OnPost[handler]Async`.</span></span>

<span data-ttu-id="6a426-214">`handler`가 이 예제에서 `delete`이기 때문에 `OnPostDeleteAsync` 처리기 메서드는 `POST` 요청을 처리하는 데 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-214">Because the `handler` is `delete` in this example, the `OnPostDeleteAsync` handler method is used to process the `POST` request.</span></span> <span data-ttu-id="6a426-215">`asp-page-handler`가 `remove`와 같은 다른 값으로 설정되면 `OnPostRemoveAsync`라는 이름의 페이지 처리기 메서드를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-215">If the `asp-page-handler` is set to a different value, such as `remove`, a page handler method with the name `OnPostRemoveAsync` is selected.</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs?range=26-37)]

<span data-ttu-id="6a426-216">`OnPostDeleteAsync` 메서드는 다음과 같은 작업을 수행합니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-216">The `OnPostDeleteAsync` method:</span></span>

* <span data-ttu-id="6a426-217">쿼리 문자열에서 `id`를 수용합니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-217">Accepts the `id` from the query string.</span></span>
* <span data-ttu-id="6a426-218">`FindAsync`를 사용하여 고객 연락처의 데이터베이스를 쿼리합니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-218">Queries the database for the customer contact with `FindAsync`.</span></span>
* <span data-ttu-id="6a426-219">고객 연락처를 찾으면 고객 연락처의 목록에서 제거됩니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-219">If the customer contact is found, they're removed from the list of customer contacts.</span></span> <span data-ttu-id="6a426-220">데이터베이스가 업데이트됩니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-220">The database is updated.</span></span>
* <span data-ttu-id="6a426-221">`RedirectToPage`를 호출하여 루트 인덱스 페이지(`/Index`)를 리디렉션합니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-221">Calls `RedirectToPage` to redirect to the root Index page (`/Index`).</span></span>

<a name="xsrf"></a>

## <a name="xsrfcsrf-and-razor-pages"></a><span data-ttu-id="6a426-222">XSRF/CSRF 및 Razor 페이지</span><span class="sxs-lookup"><span data-stu-id="6a426-222">XSRF/CSRF and Razor Pages</span></span>

<span data-ttu-id="6a426-223">[위조 방지 유효성 검사](xref:security/anti-request-forgery)에 대한 코드를 작성할 필요가 없습니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-223">You don't have to write any code for [antiforgery validation](xref:security/anti-request-forgery).</span></span> <span data-ttu-id="6a426-224">위조 방지 토큰 생성 및 유효성 검사는 Razor 페이지에 자동으로 포함됩니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-224">Antiforgery token generation and validation are automatically included in Razor Pages.</span></span>

<a name="layout"></a>
## <a name="using-layouts-partials-templates-and-tag-helpers-with-razor-pages"></a><span data-ttu-id="6a426-225">Razor 페이지와 함께 레이아웃, 부분, 템플릿 및 태그 도우미 사용</span><span class="sxs-lookup"><span data-stu-id="6a426-225">Using Layouts, partials, templates, and Tag Helpers with Razor Pages</span></span>

<span data-ttu-id="6a426-226">페이지는 Razor 뷰 엔진의 모든 기능을 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-226">Pages work with all the features of the Razor view engine.</span></span> <span data-ttu-id="6a426-227">레이아웃, 부분, 템플릿, 태그 도우미, *_ViewStart.cshtml*, *_ViewImports.cshtml*은 기존 Razor 뷰와 동일한 방식으로 작동합니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-227">Layouts, partials, templates, Tag Helpers, *_ViewStart.cshtml*, *_ViewImports.cshtml* work in the same way they do for conventional Razor views.</span></span>

<span data-ttu-id="6a426-228">해당 기능 중 일부를 활용하여 이 페이지의 문제를 해결해 보겠습니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-228">Let's declutter this page by taking advantage of some of those features.</span></span>

<span data-ttu-id="6a426-229">[레이아웃 페이지](xref:mvc/views/layout)를 *Pages/_Layout.cshtml*에 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-229">Add a [layout page](xref:mvc/views/layout) to *Pages/_Layout.cshtml*:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/_LayoutSimple.cshtml)]

<span data-ttu-id="6a426-230">[레이아웃](xref:mvc/views/layout):</span><span class="sxs-lookup"><span data-stu-id="6a426-230">The [Layout](xref:mvc/views/layout):</span></span>

* <span data-ttu-id="6a426-231">각 페이지의 레이아웃을 제어합니다(페이지가 레이아웃에서 옵트아웃(opt out)되지 않는 경우).</span><span class="sxs-lookup"><span data-stu-id="6a426-231">Controls the layout of each page (unless the page opts out of layout).</span></span>
* <span data-ttu-id="6a426-232">JavaScript 및 스타일시트 같은 HTML 구조를 가져옵니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-232">Imports HTML structures such as JavaScript and stylesheets.</span></span>

<span data-ttu-id="6a426-233">자세한 내용은 [레이아웃 페이지](xref:mvc/views/layout)를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="6a426-233">See [layout page](xref:mvc/views/layout) for more information.</span></span>

<span data-ttu-id="6a426-234">[Layout](xref:mvc/views/layout#specifying-a-layout) 속성은 *Pages/_ViewStart.cshtml*에서 설정됩니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-234">The [Layout](xref:mvc/views/layout#specifying-a-layout) property is set in *Pages/_ViewStart.cshtml*:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/_ViewStart.cshtml)]

<span data-ttu-id="6a426-235">**참고:** 레이아웃은 *Pages* 폴더에 있습니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-235">**Note:** The layout is in the *Pages* folder.</span></span> <span data-ttu-id="6a426-236">페이지는 현재 페이지와 동일한 폴더에서 시작하여 다른 뷰(레이아웃, 템플릿, 부분)를 계층 구조로 검색합니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-236">Pages look for other views (layouts, templates, partials) hierarchically, starting in the same folder as the current page.</span></span> <span data-ttu-id="6a426-237">*Pages* 폴더의 레이아웃은 *Pages* 폴더 아래의 Razor 페이지에서 사용될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-237">A layout in the *Pages* folder can be used from any Razor page under the *Pages* folder.</span></span>

<span data-ttu-id="6a426-238">레이아웃 파일은 *Views/Shared* 폴더에 두지 **않는** 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-238">We recommend you **not** put the layout file in the *Views/Shared* folder.</span></span> <span data-ttu-id="6a426-239">*Views/Shared*는 MVC 뷰 패턴입니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-239">*Views/Shared* is an MVC views pattern.</span></span> <span data-ttu-id="6a426-240">Razor 페이지는 경로 규칙이 아니라 폴더 계층 구조를 사용해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-240">Razor Pages are meant to rely on folder hierarchy, not path conventions.</span></span>

<span data-ttu-id="6a426-241">Razor 페이지의 뷰 검색에는 *Pages* 폴더가 포함됩니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-241">View search from a Razor Page includes the *Pages* folder.</span></span> <span data-ttu-id="6a426-242">MVC 컨트롤러 및 기존 Razor 뷰에서 사용 중인 레이아웃, 템플릿 및 부분이 *제대로 작동*합니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-242">The layouts, templates, and partials you're using with MVC controllers and conventional Razor views *just work*.</span></span>

<span data-ttu-id="6a426-243">*Pages/_ViewImports.cshtml* 파일을 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-243">Add a *Pages/_ViewImports.cshtml* file:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml)]

<span data-ttu-id="6a426-244">`@namespace`는 자습서의 뒷부분에서 설명합니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-244">`@namespace` is explained later in the tutorial.</span></span> <span data-ttu-id="6a426-245">`@addTagHelper` 지시문은 [기본 제공 태그 도우미](xref:mvc/views/tag-helpers/builtin-th/Index)를 *Pages* 폴더의 모든 페이지에 도입합니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-245">The `@addTagHelper` directive brings in the [built-in Tag Helpers](xref:mvc/views/tag-helpers/builtin-th/Index) to all the pages in the *Pages* folder.</span></span>

<a name="namespace"></a>

<span data-ttu-id="6a426-246">`@namespace` 지시문은 페이지에서 명시적으로 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-246">When the `@namespace` directive is used explicitly on a page:</span></span>

[!code-cshtml[main](index/sample/RazorPagesIntro/Pages/Customers/Namespace2.cshtml?highlight=2)]

<span data-ttu-id="6a426-247">이 지시문은 페이지에 대한 네임스페이스를 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-247">The directive sets the namespace for the page.</span></span> <span data-ttu-id="6a426-248">`@model` 지시문에는 네임스페이스가 포함될 필요가 없습니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-248">The `@model` directive doesn't need to include the namespace.</span></span>

<span data-ttu-id="6a426-249">`@namespace` 지시문이 *_ViewImports.cshtml*에 포함되면 지정된 네임스페이스는 `@namespace` 지시문을 가져오는 페이지의 생성된 네임스페이스에 대한 접두사를 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-249">When the `@namespace` directive is contained in *_ViewImports.cshtml*, the specified namespace supplies the prefix for the generated namespace in the Page that imports the `@namespace` directive.</span></span> <span data-ttu-id="6a426-250">생성된 네임스페이스의 나머지 부분(접미사 부분)은 *_ViewImports.cshtml*이 포함된 폴더와 페이지가 포함된 폴더 사이의 점으로 구분된 상대 경로입니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-250">The rest of the generated namespace (the suffix portion) is the dot-separated relative path between the folder containing *_ViewImports.cshtml* and the folder containing the page.</span></span>

<span data-ttu-id="6a426-251">예를 들어 코드 숨김 파일 *Pages/Customers/Edit.cshtml.cs*는 네임스페이스를 명시적으로 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-251">For example, the code behind file *Pages/Customers/Edit.cshtml.cs* explicitly sets the namespace:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts2/Pages/Customers/Edit.cshtml.cs?name=snippet_namespace)]

<span data-ttu-id="6a426-252">*Pages/_ViewImports.cshtml* 파일은 다음 네임스페이스를 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-252">The *Pages/_ViewImports.cshtml* file sets the following namespace:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml?highlight=1)]

<span data-ttu-id="6a426-253">*Pages/Customers/Edit.cshtml* Razor 페이지에 대한 생성된 네임스페이스는 코드 숨김 파일과 동일합니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-253">The generated namespace for the *Pages/Customers/Edit.cshtml* Razor Page is the same as the code behind file.</span></span> <span data-ttu-id="6a426-254">`@namespace` 지시문은 코드 숨김 파일에 대한 `@using` 지시문을 추가할 필요 없이 프로젝트 및 페이지 생성 코드에 추가된 C# 클래스가 *제대로 작동*하도록 고안되었습니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-254">The `@namespace` directive was designed so the C# classes added to a project and pages-generated code *just work* without having to add an `@using` directive for the code behind file.</span></span>

<span data-ttu-id="6a426-255">**참고:** `@namespace`는 기존 Razor 뷰에서도 작동합니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-255">**Note:** `@namespace` also works with conventional Razor views.</span></span>

<span data-ttu-id="6a426-256">원래 *Pages/Create.cshtml* 뷰 파일:</span><span class="sxs-lookup"><span data-stu-id="6a426-256">The original *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Create.cshtml?highlight=2)]

<span data-ttu-id="6a426-257">업데이트된 *Pages/Create.cshtml* 뷰 파일:</span><span class="sxs-lookup"><span data-stu-id="6a426-257">The updated *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/Customers/Create.cshtml?highlight=2)]

<span data-ttu-id="6a426-258">[Razor 페이지 시작 프로젝트](#rpvs17)에는 클라이언트 쪽 유효성 검사를 연결하는 *Pages/_ValidationScriptsPartial.cshtml*이 포함됩니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-258">The [Razor Pages starter project](#rpvs17) contains the *Pages/_ValidationScriptsPartial.cshtml*, which hooks up client-side validation.</span></span>

<a name="url_gen"></a>

## <a name="url-generation-for-pages"></a><span data-ttu-id="6a426-259">페이지에 대한 URL 생성</span><span class="sxs-lookup"><span data-stu-id="6a426-259">URL generation for Pages</span></span>

<span data-ttu-id="6a426-260">이전에 표시된 `Create` 페이지에서는 `RedirectToPage`를 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-260">The `Create` page, shown previously, uses `RedirectToPage`:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync&highlight=10)]

<span data-ttu-id="6a426-261">앱에는 다음 파일/폴더 구조가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-261">The app has the following file/folder structure:</span></span>

* <span data-ttu-id="6a426-262">*/Pages*</span><span class="sxs-lookup"><span data-stu-id="6a426-262">*/Pages*</span></span>

  * <span data-ttu-id="6a426-263">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="6a426-263">*Index.cshtml*</span></span>
  * <span data-ttu-id="6a426-264">*/Customer*</span><span class="sxs-lookup"><span data-stu-id="6a426-264">*/Customer*</span></span>

    * <span data-ttu-id="6a426-265">*Create.cshtml*</span><span class="sxs-lookup"><span data-stu-id="6a426-265">*Create.cshtml*</span></span>
    * <span data-ttu-id="6a426-266">*Edit.cshtml*</span><span class="sxs-lookup"><span data-stu-id="6a426-266">*Edit.cshtml*</span></span>
    * <span data-ttu-id="6a426-267">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="6a426-267">*Index.cshtml*</span></span>

<span data-ttu-id="6a426-268">*Pages/Customers/Create.cshtml* 및 *Pages/Customers/Edit.cshtml* 페이지는 성공 후에 *Pages/Index.cshtml*로 리디렉션됩니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-268">The *Pages/Customers/Create.cshtml* and *Pages/Customers/Edit.cshtml* pages redirect to *Pages/Index.cshtml* after success.</span></span> <span data-ttu-id="6a426-269">문자열 `/Index`는 이전 페이지에 액세스하기 위한 URI 부분입니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-269">The string `/Index` is part of the URI to access the preceding page.</span></span> <span data-ttu-id="6a426-270">문자열 `/Index`는 *Pages/Index.cshtml* 페이지의 URI를 생성하는 데 사용될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-270">The string `/Index` can be used to generate URIs to the *Pages/Index.cshtml* page.</span></span> <span data-ttu-id="6a426-271">예:</span><span class="sxs-lookup"><span data-stu-id="6a426-271">For example:</span></span>

* `Url.Page("/Index", ...)`
* `<a asp-page="/Index">My Index Page</a>`
* `RedirectToPage("/Index")`

<span data-ttu-id="6a426-272">페이지 이름은 루트 */Pages* 폴더에서 시작되는 페이지 경로입니다(선행 `/` 포함, 예: `/Index`).</span><span class="sxs-lookup"><span data-stu-id="6a426-272">The page name is the path to the page from the root */Pages* folder (including a leading `/`, for example `/Index`).</span></span> <span data-ttu-id="6a426-273">이전 URL 생성 샘플은 URL 하드 코딩보다 훨씬 더 기능이 다양합니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-273">The preceding URL generation samples are much more feature rich than just hardcoding a URL.</span></span> <span data-ttu-id="6a426-274">URL 생성은 [라우팅](xref:mvc/controllers/routing)을 사용하며 경로가 대상 경로에서 정의되는 방식에 따라 매개 변수를 생성하고 인코딩할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-274">URL generation uses [routing](xref:mvc/controllers/routing) and can generate and encode parameters according to how the route is defined in the destination path.</span></span>

<span data-ttu-id="6a426-275">페이지에 대한 URL 생성은 상대적 이름을 지원합니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-275">URL generation for pages supports relative names.</span></span> <span data-ttu-id="6a426-276">다음 표에서는 *Pages/Customers/Create.cshtml*에서 다른 `RedirectToPage` 매개 변수를 통해 선택되는 인덱스 페이지를 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-276">The following table shows which Index page is selected with different `RedirectToPage` parameters from *Pages/Customers/Create.cshtml*:</span></span>

| <span data-ttu-id="6a426-277">RedirectToPage(x)</span><span class="sxs-lookup"><span data-stu-id="6a426-277">RedirectToPage(x)</span></span>| <span data-ttu-id="6a426-278">페이지</span><span class="sxs-lookup"><span data-stu-id="6a426-278">Page</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="6a426-279">RedirectToPage("/Index")</span><span class="sxs-lookup"><span data-stu-id="6a426-279">RedirectToPage("/Index")</span></span> | <span data-ttu-id="6a426-280">*Pages/Index*</span><span class="sxs-lookup"><span data-stu-id="6a426-280">*Pages/Index*</span></span> |
| <span data-ttu-id="6a426-281">RedirectToPage("./Index");</span><span class="sxs-lookup"><span data-stu-id="6a426-281">RedirectToPage("./Index");</span></span> | <span data-ttu-id="6a426-282">*Pages/Customers/Index*</span><span class="sxs-lookup"><span data-stu-id="6a426-282">*Pages/Customers/Index*</span></span> |
| <span data-ttu-id="6a426-283">RedirectToPage("../Index")</span><span class="sxs-lookup"><span data-stu-id="6a426-283">RedirectToPage("../Index")</span></span> | <span data-ttu-id="6a426-284">*Pages/Index*</span><span class="sxs-lookup"><span data-stu-id="6a426-284">*Pages/Index*</span></span> |
| <span data-ttu-id="6a426-285">RedirectToPage("Index")</span><span class="sxs-lookup"><span data-stu-id="6a426-285">RedirectToPage("Index")</span></span>  | <span data-ttu-id="6a426-286">*Pages/Customers/Index*</span><span class="sxs-lookup"><span data-stu-id="6a426-286">*Pages/Customers/Index*</span></span> |

<span data-ttu-id="6a426-287">`RedirectToPage("Index")`, `RedirectToPage("./Index")` 및 `RedirectToPage("../Index")`는 *상대적 이름*입니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-287">`RedirectToPage("Index")`, `RedirectToPage("./Index")`, and `RedirectToPage("../Index")`  are *relative names*.</span></span> <span data-ttu-id="6a426-288">`RedirectToPage` 매개 변수는 현재 페이지의 경로와 *결합*되어 대상 페이지의 이름을 계산합니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-288">The `RedirectToPage` parameter is *combined* with the path of the current page to compute the name of the destination page.</span></span>  <!-- Review: Original had The provided string is combined with the page name of the current page to compute the name of the destination page. -- page name, not page path -->

<span data-ttu-id="6a426-289">상대적 이름 연결은 구조가 복잡한 사이트를 빌드할 때 유용합니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-289">Relative name linking is useful when building sites with a complex structure.</span></span> <span data-ttu-id="6a426-290">상대적 이름을 사용하여 한 폴더의 여러 페이지 간을 연결하는 경우 해당 폴더의 이름을 바꿀 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-290">If you use relative names to link between pages in a folder, you can rename that folder.</span></span> <span data-ttu-id="6a426-291">그래도 모든 링크가 작동합니다(폴더 이름을 포함하지 않기 때문).</span><span class="sxs-lookup"><span data-stu-id="6a426-291">All the links still work (because they didn't include the folder name).</span></span>

## <a name="tempdata"></a><span data-ttu-id="6a426-292">TempData</span><span class="sxs-lookup"><span data-stu-id="6a426-292">TempData</span></span>

<span data-ttu-id="6a426-293">ASP.NET Core는 [TempData](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) 속성을 [컨트롤러](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller)에서 노출합니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-293">ASP.NET Core exposes the [TempData](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) property on a [controller](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller).</span></span> <span data-ttu-id="6a426-294">이 속성은 판독될 때까지 데이터를 저장합니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-294">This property stores data until it is read.</span></span> <span data-ttu-id="6a426-295">`Keep` 및 `Peek` 메서드를 사용하여 삭제 없이 데이터를 검사할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-295">The `Keep` and `Peek` methods can be used to examine the data without deletion.</span></span> <span data-ttu-id="6a426-296">`TempData`는 두 개 이상의 요청에 대한 데이터가 필요할 경우 리디렉션에 유용합니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-296">`TempData` is  useful for redirection, when data is needed for more than a single request.</span></span>

<span data-ttu-id="6a426-297">`[TempData]` 특성은 ASP.NET Core 2.0의 새로운 기능이고 컨트롤러 및 페이지에서 지원됩니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-297">The `[TempData]` attribute is new in ASP.NET Core 2.0 and is supported on controllers and pages.</span></span>

<span data-ttu-id="6a426-298">다음 코드는 `TempData`를 사용하여 `Message` 값을 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-298">The following code sets the value of `Message` using `TempData`:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateDot.cshtml.cs?highlight=10-11,25&name=snippet_Temp)]

<span data-ttu-id="6a426-299">*Pages/Customers/Index.cshtml* 파일의 다음 태그는 `TempData`를 사용하여 `Message` 값을 표시합니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-299">The following markup in the *Pages/Customers/Index.cshtml* file displays the value of `Message` using `TempData`.</span></span>

```cshtml
<h3>Msg: @Model.Message</h3>
```

<span data-ttu-id="6a426-300">*Pages/Customers/Index.cshtml.cs* 코드 숨김 파일은 `[TempData]` 특성을 `Message` 속성에 적용합니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-300">The *Pages/Customers/Index.cshtml.cs* code-behind file applies the `[TempData]` attribute to the `Message` property.</span></span>

```cs
[TempData]
public string Message { get; set; }
```

<span data-ttu-id="6a426-301">자세한 내용은 [TempData](xref:fundamentals/app-state#temp)를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="6a426-301">See [TempData](xref:fundamentals/app-state#temp) for more information.</span></span>

<a name="mhpp"></a>
## <a name="multiple-handlers-per-page"></a><span data-ttu-id="6a426-302">페이지당 여러 처리기</span><span class="sxs-lookup"><span data-stu-id="6a426-302">Multiple handlers per page</span></span>

<span data-ttu-id="6a426-303">다음 페이지는 `asp-page-handler` 태그 도우미를 사용하여 두 개의 페이지 처리기에 대한 태그를 생성합니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-303">The following page generates markup for two page handlers using the `asp-page-handler` Tag Helper:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?highlight=12-13)]

<!-- Review: the FormActionTagHelper applies to all <form /> elements on a Razor page, even when there is no `asp-` attribute   -->

<span data-ttu-id="6a426-304">이전 예제의 폼에는 두 개의 제출 단추가 있고, 각 단추는 `FormActionTagHelper`를 사용하여 다른 URL에 제출됩니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-304">The form in the preceding example has two submit buttons, each using the `FormActionTagHelper` to submit to a different URL.</span></span> <span data-ttu-id="6a426-305">`asp-page-handler` 특성은 `asp-page`와 함께 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-305">The `asp-page-handler` attribute is a companion to `asp-page`.</span></span> <span data-ttu-id="6a426-306">`asp-page-handler`는 페이지에서 정의된 각 처리기 메서드에 제출되는 URL을 생성합니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-306">`asp-page-handler` generates URLs that submit to each of the handler methods defined by a page.</span></span> <span data-ttu-id="6a426-307">샘플이 현재 페이지에 연결되어 있으므로 `asp-page`가 지정되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-307">`asp-page` is not specified because the sample is linking to the current page.</span></span>

<span data-ttu-id="6a426-308">코드 숨김 파일:</span><span class="sxs-lookup"><span data-stu-id="6a426-308">The code-behind file:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml.cs?highlight=20,32)]

<span data-ttu-id="6a426-309">이전 코드는 *명명된 처리기 메서드*를 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-309">The preceding code uses *named handler methods*.</span></span> <span data-ttu-id="6a426-310">명명된 처리기 메서드를 만들려면 `On<HTTP Verb>` 뒤와 `Async`(있는 경우) 앞에 이름의 텍스트를 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-310">Named handler methods are created by taking the text in the name after `On<HTTP Verb>` and before `Async` (if present).</span></span> <span data-ttu-id="6a426-311">이전 예제에서 페이지 메서드는 OnPost**JoinList**Async 및 OnPost**JoinListUC**Async입니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-311">In the preceding example, the page methods are OnPost**JoinList**Async and OnPost**JoinListUC**Async.</span></span> <span data-ttu-id="6a426-312">*OnPost* 및 *Async*가 제거된 처리기 이름은 `JoinList` 및 `JoinListUC`입니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-312">With *OnPost* and *Async* removed, the handler names are `JoinList` and `JoinListUC`.</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?range=12-13)]

<span data-ttu-id="6a426-313">이전 코드를 사용할 경우 `OnPostJoinListAsync`에 제출되는 URL 경로는 `http://localhost:5000/Customers/CreateFATH?handler=JoinList`입니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-313">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `http://localhost:5000/Customers/CreateFATH?handler=JoinList`.</span></span> <span data-ttu-id="6a426-314">`OnPostJoinListUCAsync`에 제출되는 URL 경로는 `http://localhost:5000/Customers/CreateFATH?handler=JoinListUC`입니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-314">The URL path that submits to `OnPostJoinListUCAsync` is `http://localhost:5000/Customers/CreateFATH?handler=JoinListUC`.</span></span>

## <a name="customizing-routing"></a><span data-ttu-id="6a426-315">라우팅 사용자 지정</span><span class="sxs-lookup"><span data-stu-id="6a426-315">Customizing Routing</span></span>

<span data-ttu-id="6a426-316">URL에서 쿼리 문자열 `?handler=JoinList`를 사용하지 않으려면 경로를 변경하여 처리기 이름을 URL의 경로 부분에 넣습니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-316">If you don't like the query string `?handler=JoinList` in the URL, you can change the route to put the handler name in the path portion of the URL.</span></span> <span data-ttu-id="6a426-317">`@page` 지시문 뒤에 큰따옴표로 묶은 경로 템플릿을 추가하여 경로를 사용자 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-317">You can customize the route by adding a route template enclosed in double quotes after the `@page` directive.</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateRoute.cshtml?highlight=1)]

<span data-ttu-id="6a426-318">이전 경로는 쿼리 문자열 대신 URL 경로에 처리기 이름을 넣습니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-318">The preceding route puts the handler name in the URL path instead of the query string.</span></span> <span data-ttu-id="6a426-319">`handler` 뒤의 `?`는 경로 매개 변수가 선택 사항임을 의미합니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-319">The `?` following `handler` means the route parameter is optional.</span></span>

<span data-ttu-id="6a426-320">`@page`를 사용하여 페이지 경로에 다른 세그먼트 및 매개 변수를 추가할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-320">You can use `@page` to add additional segments and parameters to a page's route.</span></span> <span data-ttu-id="6a426-321">무엇이든 페이지의 기본 경로에 **추가**됩니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-321">Whatever's there is **appended** to the default route of the page.</span></span> <span data-ttu-id="6a426-322">절대 또는 가상 경로를 사용하여 페이지 경로를 변경하는 기능(예: `"~/Some/Other/Path"`)은 지원되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-322">Using an absolute or virtual path to change the page's route (like `"~/Some/Other/Path"`) is not supported.</span></span>

## <a name="configuration-and-settings"></a><span data-ttu-id="6a426-323">구성 및 설정</span><span class="sxs-lookup"><span data-stu-id="6a426-323">Configuration and settings</span></span>

<span data-ttu-id="6a426-324">고급 옵션을 구성하려면 MVC 빌더에서 확장 메서드 `AddRazorPagesOptions`를 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-324">To configure advanced options, use the extension method `AddRazorPagesOptions` on the MVC builder:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/StartupAdvanced.cs?name=snippet_1)]

<span data-ttu-id="6a426-325">현재 `RazorPagesOptions`를 사용하여 페이지의 루트 디렉터리를 설정하거나 페이지에 대한 응용 프로그램 모델 규칙을 추가할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-325">Currently you can use the `RazorPagesOptions` to set the root directory for pages, or add application model conventions for pages.</span></span> <span data-ttu-id="6a426-326">나중에 이 방법으로 더 큰 확장성을 지원할 예정입니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-326">We'll enable more extensibility this way in the future.</span></span>

<span data-ttu-id="6a426-327">뷰를 미리 컴파일하려면 [Razor 뷰 컴파일](xref:mvc/views/view-compilation)을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="6a426-327">To precompile views, see [Razor view compilation](xref:mvc/views/view-compilation) .</span></span>

<span data-ttu-id="6a426-328">[샘플 코드를 다운로드하거나 봅니다](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/index/sample).</span><span class="sxs-lookup"><span data-stu-id="6a426-328">[Download or view sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/index/sample).</span></span>

<span data-ttu-id="6a426-329">이 소개에 따라 빌드되는 [ASP.NET Core의 Razor 페이지 시작](xref:tutorials/razor-pages/razor-pages-start)을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="6a426-329">See [Getting started with Razor Pages in ASP.NET Core](xref:tutorials/razor-pages/razor-pages-start), which builds on this introduction.</span></span>

### <a name="specify-that-razor-pages-are-at-the-content-root"></a><span data-ttu-id="6a426-330">Razor 페이지를 콘텐츠 루트로 지정</span><span class="sxs-lookup"><span data-stu-id="6a426-330">Specify that Razor Pages are at the content root</span></span>

<span data-ttu-id="6a426-331">기본적으로 Razor 페이지의 루트 경로는 */Pages* 디렉터리입니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-331">By default, Razor Pages are rooted in the */Pages* directory.</span></span> <span data-ttu-id="6a426-332">[AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_)에 [WithRazorPagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot)를 추가하여 Razor 페이지를 앱의 콘텐츠 루트([ContentRootPath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath))로 지정합니다.</span><span class="sxs-lookup"><span data-stu-id="6a426-332">Add [WithRazorPagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot) to [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) to specify that your Razor Pages are at the content root ([ContentRootPath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)) of the app:</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesAtContentRoot();
```

### <a name="specify-that-razor-pages-are-at-a-custom-root-directory"></a><span data-ttu-id="6a426-333">Razor 페이지가 사용자 지정 루트 디렉터리에 있도록 지정</span><span class="sxs-lookup"><span data-stu-id="6a426-333">Specify that Razor Pages are at a custom root directory</span></span>

<span data-ttu-id="6a426-334">[AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_)에 [WithRazorPagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot)를 추가하여 Razor 페이지가 앱의 사용자 지정 루트 디렉터리에 있도록 지정합니다(상대 경로 제공).</span><span class="sxs-lookup"><span data-stu-id="6a426-334">Add [WithRazorPagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) to [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) to specify that your Razor Pages are at a custom root directory in the app (provide a relative path):</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesRoot("/path/to/razor/pages");
```

## <a name="see-also"></a><span data-ttu-id="6a426-335">참고 항목</span><span class="sxs-lookup"><span data-stu-id="6a426-335">See also</span></span>

* [<span data-ttu-id="6a426-336">Razor 페이지 시작</span><span class="sxs-lookup"><span data-stu-id="6a426-336">Getting started with Razor Pages</span></span>](xref:tutorials/razor-pages/razor-pages-start)
* [<span data-ttu-id="6a426-337">Razor 페이지 권한 부여 규칙</span><span class="sxs-lookup"><span data-stu-id="6a426-337">Razor Pages authorization conventions</span></span>](xref:security/authorization/razor-pages-authorization)
* [<span data-ttu-id="6a426-338">Razor 페이지 사용자 지정 경로 및 페이지 모델 공급자</span><span class="sxs-lookup"><span data-stu-id="6a426-338">Razor Pages custom route and page model providers</span></span>](xref:mvc/razor-pages/razor-pages-convention-features)
* [<span data-ttu-id="6a426-339">Razor 페이지 단위 및 통합 테스트</span><span class="sxs-lookup"><span data-stu-id="6a426-339">Razor Pages unit and integration testing</span></span>](xref:testing/razor-pages-testing)
