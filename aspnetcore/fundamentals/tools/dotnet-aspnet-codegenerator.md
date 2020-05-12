---
title: dotnet aspnet-codegenerator 명령
author: rick-anderson
description: dotnet aspnet codegenerator 명령은 ASP.NET Core 프로젝트를 스캐폴딩합니다.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 07/04/2019
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: fundamentals/tools/dotnet-aspnet-codegenerator
ms.openlocfilehash: 58f7aa30d3e916307437d56c61e80765ac0c21cf
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/04/2020
ms.locfileid: "82766474"
---
# <a name="dotnet-aspnet-codegenerator"></a><span data-ttu-id="8f900-103">dotnet aspnet-codegenerator</span><span class="sxs-lookup"><span data-stu-id="8f900-103">dotnet aspnet-codegenerator</span></span>

<span data-ttu-id="8f900-104">작성자: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="8f900-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="8f900-105">`dotnet aspnet-codegenerator`는 ASP.NET Core 스캐폴딩 엔진을 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="8f900-105">`dotnet aspnet-codegenerator` - Runs the ASP.NET Core scaffolding engine.</span></span> <span data-ttu-id="8f900-106">`dotnet aspnet-codegenerator`는 명령줄에서 스캐폴딩하는 경우에만 필요하며, Visual Studio에서 스캐폴딩을 사용하는 경우에는 필요하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="8f900-106">`dotnet aspnet-codegenerator` is only required to scaffold from the command line, it's not needed to use scaffolding with Visual Studio.</span></span>

<span data-ttu-id="8f900-107">이 문서는 [.NET Core 2.1 SDK](https://dotnet.microsoft.com/download/dotnet-core/2.1) 이상에 적용됩니다.</span><span class="sxs-lookup"><span data-stu-id="8f900-107">This article applies to [.NET Core 2.1 SDK](https://dotnet.microsoft.com/download/dotnet-core/2.1) and later.</span></span>

## <a name="installing-aspnet-codegenerator"></a><span data-ttu-id="8f900-108">aspnet-codegenerator 설치</span><span class="sxs-lookup"><span data-stu-id="8f900-108">Installing aspnet-codegenerator</span></span>

<span data-ttu-id="8f900-109">`dotnet-aspnet-codegenerator`는 설치가 필요한 [전역 도구](/dotnet/core/tools/global-tools)입니다.</span><span class="sxs-lookup"><span data-stu-id="8f900-109">`dotnet-aspnet-codegenerator` is a [global tool](/dotnet/core/tools/global-tools) that must be installed.</span></span> <span data-ttu-id="8f900-110">다음 명령은 `dotnet-aspnet-codegenerator` 도구의 안정적인 최신 버전을 설치합니다.</span><span class="sxs-lookup"><span data-stu-id="8f900-110">The following command installs the latest stable version of the `dotnet-aspnet-codegenerator` tool:</span></span>

```dotnetcli
dotnet tool install -g dotnet-aspnet-codegenerator
```

<span data-ttu-id="8f900-111">다음 명령은 `dotnet-aspnet-codegenerator`를 설치된 .NET Core SDK에서 사용 가능한 안정적인 최신 버전으로 업데이트합니다.</span><span class="sxs-lookup"><span data-stu-id="8f900-111">The following command updates `dotnet-aspnet-codegenerator` to the latest stable version available from the installed .NET Core SDKs:</span></span>

```dotnetcli
dotnet tool update -g dotnet-aspnet-codegenerator
```

## <a name="synopsis"></a><span data-ttu-id="8f900-112">개요</span><span class="sxs-lookup"><span data-stu-id="8f900-112">Synopsis</span></span>

```
dotnet aspnet-codegenerator [arguments] [-p|--project] [-n|--nuget-package-dir] [-c|--configuration] [-tfm|--target-framework] [-b|--build-base-path] [--no-build] 
dotnet aspnet-codegenerator [-h|--help]
```

## <a name="description"></a><span data-ttu-id="8f900-113">설명</span><span class="sxs-lookup"><span data-stu-id="8f900-113">Description</span></span>

<span data-ttu-id="8f900-114">`dotnet aspnet-codegenerator` 전역 명령은 ASP.NET Core 코드 생성기 및 스캐폴딩 엔진을 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="8f900-114">The `dotnet aspnet-codegenerator` global command runs the ASP.NET Core code generator and scaffolding engine.</span></span>

## <a name="arguments"></a><span data-ttu-id="8f900-115">인수</span><span class="sxs-lookup"><span data-stu-id="8f900-115">Arguments</span></span>

`generator`

<span data-ttu-id="8f900-116">실행할 코드 생성기입니다.</span><span class="sxs-lookup"><span data-stu-id="8f900-116">The code generator to run.</span></span> <span data-ttu-id="8f900-117">다음 생성기를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8f900-117">The following generators are available:</span></span>

| <span data-ttu-id="8f900-118">생성기</span><span class="sxs-lookup"><span data-stu-id="8f900-118">Generator</span></span> | <span data-ttu-id="8f900-119">연산</span><span class="sxs-lookup"><span data-stu-id="8f900-119">Operation</span></span> |
| ----------------- | ------------ | 
| <span data-ttu-id="8f900-120">area</span><span class="sxs-lookup"><span data-stu-id="8f900-120">area</span></span>      | [<span data-ttu-id="8f900-121">영역 스캐폴딩</span><span class="sxs-lookup"><span data-stu-id="8f900-121">Scaffolds an Area</span></span>](/aspnet/core/mvc/controllers/areas) |
  <span data-ttu-id="8f900-122">Controller</span><span class="sxs-lookup"><span data-stu-id="8f900-122">controller</span></span>| [<span data-ttu-id="8f900-123">컨트롤러 스캐폴딩</span><span class="sxs-lookup"><span data-stu-id="8f900-123">Scaffolds a controller</span></span>](/aspnet/core/tutorials/first-mvc-app/adding-model) |
  <span data-ttu-id="8f900-124">identity</span><span class="sxs-lookup"><span data-stu-id="8f900-124">identity</span></span>  | [<span data-ttu-id="8f900-125">Identity 스캐폴딩</span><span class="sxs-lookup"><span data-stu-id="8f900-125">Scaffolds Identity</span></span>](/aspnet/core/security/authentication/scaffold-identity) |
  <span data-ttu-id="8f900-126">razorpage</span><span class="sxs-lookup"><span data-stu-id="8f900-126">razorpage</span></span> | [<span data-ttu-id="8f900-127">Razor 페이지 스캐폴딩</span><span class="sxs-lookup"><span data-stu-id="8f900-127">Scaffolds Razor Pages</span></span>](/aspnet/core/tutorials/razor-pages/model) |
  <span data-ttu-id="8f900-128">view</span><span class="sxs-lookup"><span data-stu-id="8f900-128">view</span></span>      | [<span data-ttu-id="8f900-129">보기 스캐폴딩</span><span class="sxs-lookup"><span data-stu-id="8f900-129">Scaffolds a view</span></span>](/aspnet/core/mvc/views/overview) |

## <a name="options"></a><span data-ttu-id="8f900-130">옵션</span><span class="sxs-lookup"><span data-stu-id="8f900-130">Options</span></span>

`-n|--nuget-package-dir`

<span data-ttu-id="8f900-131">NuGet 패키지 디렉터리를 지정합니다.</span><span class="sxs-lookup"><span data-stu-id="8f900-131">Specifies the NuGet package directory.</span></span>

`-c|--configuration {Debug|Release}`

<span data-ttu-id="8f900-132">빌드 구성을 정의합니다.</span><span class="sxs-lookup"><span data-stu-id="8f900-132">Defines the build configuration.</span></span> <span data-ttu-id="8f900-133">기본값은 `Debug`입니다.</span><span class="sxs-lookup"><span data-stu-id="8f900-133">The default value is `Debug`.</span></span>

`-tfm|--target-framework`

<span data-ttu-id="8f900-134">사용할 대상 [프레임워크](/dotnet/standard/frameworks)입니다.</span><span class="sxs-lookup"><span data-stu-id="8f900-134">Target [Framework](/dotnet/standard/frameworks) to use.</span></span> <span data-ttu-id="8f900-135">예: `net46`.</span><span class="sxs-lookup"><span data-stu-id="8f900-135">For example, `net46`.</span></span>

`-b|--build-base-path`

<span data-ttu-id="8f900-136">빌드 기본 경로입니다.</span><span class="sxs-lookup"><span data-stu-id="8f900-136">The build base path.</span></span>

`-h|--help`

<span data-ttu-id="8f900-137">명령에 대한 간단한 도움말을 출력합니다.</span><span class="sxs-lookup"><span data-stu-id="8f900-137">Prints out a short help for the command.</span></span>

`--no-build`

<span data-ttu-id="8f900-138">실행하기 전에 프로젝트를 빌드하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="8f900-138">Doesn't build the project before running.</span></span> <span data-ttu-id="8f900-139">또한 암시적으로 `--no-restore` 플래그를 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="8f900-139">It also implicitly sets the `--no-restore` flag.</span></span>

`-p|--project <PATH>`

<span data-ttu-id="8f900-140">실행할 프로젝트 파일의 경로를 지정합니다(폴더 이름 또는 전체 경로).</span><span class="sxs-lookup"><span data-stu-id="8f900-140">Specifies the path of the project file to run (folder name or full path).</span></span> <span data-ttu-id="8f900-141">지정하지 않으면 현재 디렉터리로 기본 설정됩니다.</span><span class="sxs-lookup"><span data-stu-id="8f900-141">If not specified, it defaults to the current directory.</span></span>

## <a name="generator-options"></a><span data-ttu-id="8f900-142">생성기 옵션</span><span class="sxs-lookup"><span data-stu-id="8f900-142">Generator options</span></span>

<span data-ttu-id="8f900-143">다음 섹션에서는 지원되는 생성기에 사용 가능한 옵션에서 설명합니다.</span><span class="sxs-lookup"><span data-stu-id="8f900-143">The following sections detail the options available for the supported generators:</span></span>

* <span data-ttu-id="8f900-144">Area</span><span class="sxs-lookup"><span data-stu-id="8f900-144">Area</span></span>
* <span data-ttu-id="8f900-145">Controller</span><span class="sxs-lookup"><span data-stu-id="8f900-145">Controller</span></span>
* <span data-ttu-id="8f900-146">Identity</span><span class="sxs-lookup"><span data-stu-id="8f900-146">Identity</span></span>  
* <span data-ttu-id="8f900-147">Razorpage</span><span class="sxs-lookup"><span data-stu-id="8f900-147">Razorpage</span></span>
* <span data-ttu-id="8f900-148">View</span><span class="sxs-lookup"><span data-stu-id="8f900-148">View</span></span>

<a name="area"></a>

### <a name="area-options"></a><span data-ttu-id="8f900-149">Area 옵션</span><span class="sxs-lookup"><span data-stu-id="8f900-149">Area options</span></span>

<span data-ttu-id="8f900-150">이 도구는 컨트롤러와 보기가 포함된 ASP.NET Core 웹 프로젝트에 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8f900-150">This tool is intended for ASP.NET Core web projects with controllers and views.</span></span> <span data-ttu-id="8f900-151">Razor Pages 앱에는 사용할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="8f900-151">It's not intended for Razor Pages apps.</span></span>

<span data-ttu-id="8f900-152">사용법: `dotnet aspnet-codegenerator area AreaNameToGenerate`</span><span class="sxs-lookup"><span data-stu-id="8f900-152">Usage: `dotnet aspnet-codegenerator area AreaNameToGenerate`</span></span>

<span data-ttu-id="8f900-153">앞의 명령은 다음 폴더를 생성합니다.</span><span class="sxs-lookup"><span data-stu-id="8f900-153">The preceding command generates the following folders:</span></span>

* <span data-ttu-id="8f900-154">*Areas*</span><span class="sxs-lookup"><span data-stu-id="8f900-154">*Areas*</span></span>
  * <span data-ttu-id="8f900-155">*AreaNameToGenerate*</span><span class="sxs-lookup"><span data-stu-id="8f900-155">*AreaNameToGenerate*</span></span>
    * <span data-ttu-id="8f900-156">*컨트롤러*</span><span class="sxs-lookup"><span data-stu-id="8f900-156">*Controllers*</span></span>
    * <span data-ttu-id="8f900-157">*Data*</span><span class="sxs-lookup"><span data-stu-id="8f900-157">*Data*</span></span>
    * <span data-ttu-id="8f900-158">*Models*</span><span class="sxs-lookup"><span data-stu-id="8f900-158">*Models*</span></span>
    * <span data-ttu-id="8f900-159">*Views*</span><span class="sxs-lookup"><span data-stu-id="8f900-159">*Views*</span></span>

<a name="ctl"></a>

### <a name="controller-options"></a><span data-ttu-id="8f900-160">Controller 옵션</span><span class="sxs-lookup"><span data-stu-id="8f900-160">Controller options</span></span>

<span data-ttu-id="8f900-161">다음 표는 `aspnet-codegenerator` `controller` 및 `razorpage`의 옵션 목록을 보여줍니다.</span><span class="sxs-lookup"><span data-stu-id="8f900-161">The following table lists options for  `aspnet-codegenerator` `controller` and `razorpage`:</span></span>

[!INCLUDE [aspnet-codegenerator-args-md.md](~/includes/aspnet-codegenerator-args-md.md)]

<span data-ttu-id="8f900-162">다음 표는 `aspnet-codegenerator controller`에 고유한 옵션 목록을 보여줍니다.</span><span class="sxs-lookup"><span data-stu-id="8f900-162">The following table lists options unique to  `aspnet-codegenerator controller`:</span></span>

| <span data-ttu-id="8f900-163">옵션</span><span class="sxs-lookup"><span data-stu-id="8f900-163">Option</span></span>               | <span data-ttu-id="8f900-164">설명</span><span class="sxs-lookup"><span data-stu-id="8f900-164">Description</span></span>|
| ----------------- | ------------ |
| <span data-ttu-id="8f900-165">--controllerName 또는 -name</span><span class="sxs-lookup"><span data-stu-id="8f900-165">--controllerName or -name</span></span> | <span data-ttu-id="8f900-166">컨트롤러의 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="8f900-166">Name of the controller.</span></span> |
| <span data-ttu-id="8f900-167">--useAsyncActions 또는 -async</span><span class="sxs-lookup"><span data-stu-id="8f900-167">--useAsyncActions or -async</span></span> | <span data-ttu-id="8f900-168">비동기 컨트롤러 작업을 생성합니다.</span><span class="sxs-lookup"><span data-stu-id="8f900-168">Generate async controller actions.</span></span> |
| <span data-ttu-id="8f900-169">--noViews 또는 -nv</span><span class="sxs-lookup"><span data-stu-id="8f900-169">--noViews or -nv</span></span> | <span data-ttu-id="8f900-170">보기를 생성하지 **않습니다**.</span><span class="sxs-lookup"><span data-stu-id="8f900-170">Generate **no** views.</span></span> |
| <span data-ttu-id="8f900-171">--restWithNoViews 또는 -api</span><span class="sxs-lookup"><span data-stu-id="8f900-171">--restWithNoViews or -api</span></span>  | <span data-ttu-id="8f900-172">REST 스타일 API를 사용하여 컨트롤러를 생성합니다.</span><span class="sxs-lookup"><span data-stu-id="8f900-172">Generate a Controller with REST style API.</span></span> <span data-ttu-id="8f900-173">`noViews` 옵션이 지정된 것으로 간주되며 모든 보기 관련된 옵션이 무시됩니다.</span><span class="sxs-lookup"><span data-stu-id="8f900-173">`noViews` is assumed and any view related options are ignored.</span></span> |
| <span data-ttu-id="8f900-174">--readWriteActions 또는 -actions</span><span class="sxs-lookup"><span data-stu-id="8f900-174">--readWriteActions or -actions</span></span> | <span data-ttu-id="8f900-175">모델 없이 읽기/쓰기 동작이 포함된 컨트롤러를 생성합니다.</span><span class="sxs-lookup"><span data-stu-id="8f900-175">Generate controller with read/write actions without a model.</span></span> |

<span data-ttu-id="8f900-176">`-h` 스위치를 사용하여 `aspnet-codegenerator controller` 명령에 대한 도움말을 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="8f900-176">Use the `-h` switch for help on the `aspnet-codegenerator controller` command:</span></span>

```dotnetcli
dotnet aspnet-codegenerator controller -h
```

<span data-ttu-id="8f900-177">`dotnet aspnet-codegenerator controller`의 예제는 [동영상 모델 스캐폴드](/aspnet/core/tutorials/razor-pages/model)를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="8f900-177">See [Scaffold the movie model](/aspnet/core/tutorials/razor-pages/model) for an example of `dotnet aspnet-codegenerator controller`.</span></span>

### <a name="razorpage"></a><span data-ttu-id="8f900-178">Razorpage</span><span class="sxs-lookup"><span data-stu-id="8f900-178">Razorpage</span></span>

<a name="rp"></a>

<span data-ttu-id="8f900-179">Razor Pages는 새 페이지 이름 및 사용할 템플릿을 지정하여 개별적으로 스캐폴딩할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8f900-179">Razor Pages can be individually scaffolded by specifying the name of the new page and the template to use.</span></span> <span data-ttu-id="8f900-180">지원되는 템플릿은 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="8f900-180">The supported templates are:</span></span>

* `Empty`
* `Create`
* `Edit`
* `Delete`
* `Details`
* `List`

<span data-ttu-id="8f900-181">예를 들어 다음 명령은 Edit 템플릿을 사용하여 *MyEdit.cshtml* 및 *MyEdit.cshtml.cs*를 생성합니다.</span><span class="sxs-lookup"><span data-stu-id="8f900-181">For example, the following command uses the Edit template to generate *MyEdit.cshtml* and *MyEdit.cshtml.cs*:</span></span>

```dotnetcli
dotnet aspnet-codegenerator razorpage MyEdit Edit -m Movie -dc RazorPagesMovieContext -outDir Pages/Movies
```

<span data-ttu-id="8f900-182">일반적으로 템플릿 및 생성된 파일 이름은 지정되지 않으며 다음 템플릿이 생성됩니다.</span><span class="sxs-lookup"><span data-stu-id="8f900-182">Typically, the template and generated file name is not specified, and the following templates are created:</span></span>

* `Create`
* `Edit`
* `Delete`
* `Details`
* `List`

<span data-ttu-id="8f900-183">다음 표는 `aspnet-codegenerator` `razorpage` 및 `controller`의 옵션 목록을 보여줍니다.</span><span class="sxs-lookup"><span data-stu-id="8f900-183">The following table lists options for  `aspnet-codegenerator` `razorpage` and `controller`:</span></span>

[!INCLUDE [aspnet-codegenerator-args-md.md](~/includes/aspnet-codegenerator-args-md.md)]

<span data-ttu-id="8f900-184">다음 표는 `aspnet-codegenerator razorpage`에 고유한 옵션 목록을 보여줍니다.</span><span class="sxs-lookup"><span data-stu-id="8f900-184">The following table lists options unique to  `aspnet-codegenerator razorpage`:</span></span>

| <span data-ttu-id="8f900-185">옵션</span><span class="sxs-lookup"><span data-stu-id="8f900-185">Option</span></span>               | <span data-ttu-id="8f900-186">설명</span><span class="sxs-lookup"><span data-stu-id="8f900-186">Description</span></span>|
| ----------------- | ------------ |
|   <span data-ttu-id="8f900-187">--namespaceName 또는 -namespace</span><span class="sxs-lookup"><span data-stu-id="8f900-187">--namespaceName or -namespace</span></span> | <span data-ttu-id="8f900-188">생성된 PageModel에 사용할 네임스페이스의 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="8f900-188">The name of the namespace to use for the generated PageModel</span></span> |
| <span data-ttu-id="8f900-189">--partialView 또는 -partial</span><span class="sxs-lookup"><span data-stu-id="8f900-189">--partialView or -partial</span></span> | <span data-ttu-id="8f900-190">부분 보기를 생성합니다.</span><span class="sxs-lookup"><span data-stu-id="8f900-190">Generate a partial view.</span></span> <span data-ttu-id="8f900-191">이 옵션이 지정되면 레이아웃 옵션 -l 및 -udl은 무시됩니다.</span><span class="sxs-lookup"><span data-stu-id="8f900-191">Layout options -l and -udl are ignored if this is specified.</span></span> |
| <span data-ttu-id="8f900-192">--noPageModel 또는 -npm</span><span class="sxs-lookup"><span data-stu-id="8f900-192">--noPageModel or -npm</span></span> | <span data-ttu-id="8f900-193">빈 템플릿에 대한 PageModel 클래스를 생성하지 않도록 전환합니다.</span><span class="sxs-lookup"><span data-stu-id="8f900-193">Switch to not generate a PageModel class for Empty template</span></span> |

<span data-ttu-id="8f900-194">`-h` 스위치를 사용하여 `aspnet-codegenerator razorpage` 명령에 대한 도움말을 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="8f900-194">Use the `-h` switch for help on the `aspnet-codegenerator razorpage` command:</span></span>

```dotnetcli
dotnet aspnet-codegenerator razorpage -h
```

<span data-ttu-id="8f900-195">`dotnet aspnet-codegenerator razorpage`의 예제는 [동영상 모델 스캐폴드](/aspnet/core/tutorials/razor-pages/model)를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="8f900-195">See [Scaffold the movie model](/aspnet/core/tutorials/razor-pages/model) for an example of `dotnet aspnet-codegenerator razorpage`.</span></span>

### Identity

<span data-ttu-id="8f900-196">[스캐폴드Identity](/aspnet/core/security/authentication/scaffold-identity)를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="8f900-196">See [Scaffold Identity](/aspnet/core/security/authentication/scaffold-identity)</span></span>
