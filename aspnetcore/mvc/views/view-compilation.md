---
title: ASP.NET Core에서 Razor 파일 컴파일 및 미리 컴파일
author: rick-anderson
description: Razor 파일을 미리 컴파일할 경우의 이점과 ASP.NET Core 앱에서 Razor 파일 미리 컴파일을 구현하는 방법에 대해 알아보세요.
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/17/2018
uid: mvc/views/view-compilation
ms.openlocfilehash: 6ef450a24f57c721021f77f6df5088574caa2645
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36274042"
---
# <a name="razor-file-compilation-in-aspnet-core"></a><span data-ttu-id="31998-103">ASP.NET Core의 Razor 파일 컴파일</span><span class="sxs-lookup"><span data-stu-id="31998-103">Razor file compilation in ASP.NET Core</span></span>

<span data-ttu-id="31998-104">작성자: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="31998-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range="= aspnetcore-1.1"
<span data-ttu-id="31998-105">Razor 파일은 런타임 시 관련 MVC 뷰가 호출되면 컴파일됩니다.</span><span class="sxs-lookup"><span data-stu-id="31998-105">A Razor file is compiled at runtime, when the associated MVC view is invoked.</span></span> <span data-ttu-id="31998-106">빌드 시 Razor 파일 게시는 지원되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="31998-106">Build-time Razor file publishing is unsupported.</span></span> <span data-ttu-id="31998-107">Razor 파일은 필요에 따라 미리 컴파일 도구를 사용하여 앱 게시 및 배포 시 컴파일될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="31998-107">Razor files can optionally be compiled at publish time and deployed with the app&mdash;using the precompilation tool.</span></span>
::: moniker-end
::: moniker range="= aspnetcore-2.0"
<span data-ttu-id="31998-108">Razor 파일은 런타임 시 관련 Razor 페이지 또는 MVC 뷰가 호출되면 컴파일됩니다.</span><span class="sxs-lookup"><span data-stu-id="31998-108">A Razor file is compiled at runtime, when the associated Razor Page or MVC view is invoked.</span></span> <span data-ttu-id="31998-109">빌드 시 Razor 파일 게시는 지원되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="31998-109">Build-time Razor file publishing is unsupported.</span></span> <span data-ttu-id="31998-110">Razor 파일은 필요에 따라 미리 컴파일 도구를 사용하여 앱 게시 및 배포 시 컴파일될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="31998-110">Razor files can optionally be compiled at publish time and deployed with the app&mdash;using the precompilation tool.</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="31998-111">Razor 파일은 런타임 시 관련 Razor 페이지 또는 MVC 뷰가 호출되면 컴파일됩니다.</span><span class="sxs-lookup"><span data-stu-id="31998-111">A Razor file is compiled at runtime, when the associated Razor Page or MVC view is invoked.</span></span> <span data-ttu-id="31998-112">Razor 파일은 [Razor SDK](xref:razor-pages/sdk)를 사용하여 빌드 및 게시 시 컴파일됩니다.</span><span class="sxs-lookup"><span data-stu-id="31998-112">Razor files are compiled at both build and publish time using the [Razor SDK](xref:razor-pages/sdk).</span></span>
::: moniker-end

## <a name="precompilation-considerations"></a><span data-ttu-id="31998-113">미리 컴파일 고려 사항</span><span class="sxs-lookup"><span data-stu-id="31998-113">Precompilation considerations</span></span>

<span data-ttu-id="31998-114">다음은 Razor 파일을 미리 컴파일할 경우의 부작용입니다.</span><span class="sxs-lookup"><span data-stu-id="31998-114">The following are side effects of precompiling Razor files:</span></span>

* <span data-ttu-id="31998-115">더 작게 게시되는 번들</span><span class="sxs-lookup"><span data-stu-id="31998-115">A smaller published bundle</span></span>
* <span data-ttu-id="31998-116">더 빠른 시작 시간</span><span class="sxs-lookup"><span data-stu-id="31998-116">A faster startup time</span></span>
* <span data-ttu-id="31998-117">게시된 번들에 연관 콘텐츠가 없기 때문에 Razor 파일을 편집할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="31998-117">You can't edit Razor files&mdash;the associated content is absent from the published bundle.</span></span>

## <a name="deploy-precompiled-files"></a><span data-ttu-id="31998-118">미리 컴파일된 파일 배포</span><span class="sxs-lookup"><span data-stu-id="31998-118">Deploy precompiled files</span></span>

::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="31998-119">Razor 파일의 빌드 및 게시 시점 컴파일은 Razor SDK에서 기본적으로 사용하도록 설정됩니다.</span><span class="sxs-lookup"><span data-stu-id="31998-119">Build- and publish-time compilation of Razor files is enabled by default by the Razor SDK.</span></span> <span data-ttu-id="31998-120">업데이트된 후에 Razor 파일을 편집하는 작업은 빌드 시 지원됩니다.</span><span class="sxs-lookup"><span data-stu-id="31998-120">Editing Razor files after they're updated is supported at build time.</span></span> <span data-ttu-id="31998-121">기본적으로 컴파일된 *Views.dll*만이 앱과 함께 배포되고 *cshtml* 파일은 배포되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="31998-121">By default, only the compiled *Views.dll* and no *.cshtml* files are deployed with your app.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="31998-122">Razor SDK는 미리 컴파일 특정 속성이 프로젝트 파일에서 설정되지 않은 경우에만 유효합니다.</span><span class="sxs-lookup"><span data-stu-id="31998-122">The Razor SDK is effective only when no precompilation-specific properties are set in the project file.</span></span> <span data-ttu-id="31998-123">예를 들어 *.csproj* 파일의 `MvcRazorCompileOnPublish` 속성을 `true`로 설정하면 Razor SDK가 비활성화됩니다.</span><span class="sxs-lookup"><span data-stu-id="31998-123">For instance, setting the *.csproj* file's `MvcRazorCompileOnPublish` property to `true` disables the Razor SDK.</span></span>
::: moniker-end

::: moniker range="= aspnetcore-2.0"
<span data-ttu-id="31998-124">프로젝트가 .NET Framework를 대상으로 하는 경우 [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) NuGet 패키지를 설치하세요.</span><span class="sxs-lookup"><span data-stu-id="31998-124">If your project targets .NET Framework, install the [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) NuGet package:</span></span>

<span data-ttu-id="31998-125">[!code-xml[](view-compilation/sample/DotNetFrameworkProject.csproj?name=snippet_ViewCompilationPackage)]</span><span class="sxs-lookup"><span data-stu-id="31998-125">[!code-xml[](view-compilation/sample/DotNetFrameworkProject.csproj?name=snippet_ViewCompilationPackage)]</span></span>

<span data-ttu-id="31998-126">프로젝트가 .NET Core를 대상으로 하는 경우 변경 작업이 필요하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="31998-126">If your project targets .NET Core, no changes are necessary.</span></span>

<span data-ttu-id="31998-127">ASP.NET Core 2.x 프로젝트 템플릿은 기본적으로 `MvcRazorCompileOnPublish` 속성을 암시적으로 `true`로 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="31998-127">The ASP.NET Core 2.x project templates implicitly set the `MvcRazorCompileOnPublish` property to `true` by default.</span></span> <span data-ttu-id="31998-128">다라서 이 요소는 *.csproj* 파일에서 안전하게 제거할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="31998-128">Consequently, this element can be safely removed from the *.csproj* file.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="31998-129">ASP.NET Core 2.0에서 [SCD(자체 포함 배포)](/dotnet/core/deploying/#self-contained-deployments-scd)를 수행하는 경우 Razor 파일 미리 컴파일은 사용 불가능합니다.</span><span class="sxs-lookup"><span data-stu-id="31998-129">Razor file precompilation is unavailable when performing a [self-contained deployment (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) in ASP.NET Core 2.0.</span></span>
::: moniker-end

::: moniker range="= aspnetcore-1.1"
<span data-ttu-id="31998-130">`MvcRazorCompileOnPublish` 속성을 `true`로 설정하고 [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) NuGet 패키지를 설치하세요.</span><span class="sxs-lookup"><span data-stu-id="31998-130">Set the `MvcRazorCompileOnPublish` property to `true`, and install the [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) NuGet package.</span></span> <span data-ttu-id="31998-131">다음 *.csproj* 샘플은 이러한 설정을 강조 표시합니다.</span><span class="sxs-lookup"><span data-stu-id="31998-131">The following *.csproj* sample highlights these settings:</span></span>

<span data-ttu-id="31998-132">[!code-xml[](view-compilation/sample/MvcRazorCompileOnPublish.csproj?highlight=4,10)]</span><span class="sxs-lookup"><span data-stu-id="31998-132">[!code-xml[](view-compilation/sample/MvcRazorCompileOnPublish.csproj?highlight=4,10)]</span></span>
::: moniker-end

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="31998-133">[.NET Core CLI 게시 명령](/dotnet/core/tools/dotnet-publish)을 사용하여 [프레임워크 종속 배포](/dotnet/core/deploying/#framework-dependent-deployments-fdd)에 대한 앱을 준비합니다.</span><span class="sxs-lookup"><span data-stu-id="31998-133">Prepare the app for a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd) with the [.NET Core CLI publish command](/dotnet/core/tools/dotnet-publish).</span></span> <span data-ttu-id="31998-134">예를 들어 프로젝트 루트에서 다음 명령을 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="31998-134">For example, execute the following command at the project root:</span></span>

```console
dotnet publish -c Release
```

<span data-ttu-id="31998-135">미리 컴파일에 성공하면 컴파일된 Razor 파일이 포함된 *<project_name>.PrecompiledViews.dll* 파일이 생성됩니다.</span><span class="sxs-lookup"><span data-stu-id="31998-135">A *<project_name>.PrecompiledViews.dll* file, containing the compiled Razor files, is produced when precompilation succeeds.</span></span> <span data-ttu-id="31998-136">예를 들어 아래 스크린샷은 *WebApplication1.PrecompiledViews.dll* 내부에서 *Index.cshtml*의 내용을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="31998-136">For example, the screenshot below depicts the contents of *Index.cshtml* within *WebApplication1.PrecompiledViews.dll*:</span></span>

![DLL 내에서 Razor 뷰](view-compilation/_static/razor-views-in-dll.png)
::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="31998-138">추가 자료</span><span class="sxs-lookup"><span data-stu-id="31998-138">Additional resources</span></span>

::: moniker range="= aspnetcore-1.1"
* <xref:mvc/views/overview>
::: moniker-end

::: moniker range="= aspnetcore-2.0"
* <xref:razor-pages/index>
* <xref:mvc/views/overview>
::: moniker-end

::: moniker range=">= aspnetcore-2.1"
* <xref:razor-pages/index>
* <xref:mvc/views/overview>
* <xref:razor-pages/sdk>
::: moniker-end