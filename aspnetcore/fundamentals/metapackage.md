---
title: "ASP.NET Core에 대 한 Microsoft.AspNetCore.All metapackage 2.x 이상"
author: Rick-Anderson
description: "Microsoft.AspNetCore.All metapackage 지원 되는 모든 ASP.NET Core 및 Entity Framework Core 패키지를 해당 종속성과 함께 포함 되어 있습니다."
ms.author: riande
manager: wpickett
ms.date: 09/20/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/metapackage
ms.openlocfilehash: 8a44ee7ebb7e6b0112000429f1f080bceb7dc895
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/19/2018
---
#<a name="microsoftaspnetcoreall-metapackage-for-aspnet-core-2x"></a><span data-ttu-id="1b768-103">Microsoft.AspNetCore.All metapackage for ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="1b768-103">Microsoft.AspNetCore.All metapackage for ASP.NET Core 2.x</span></span>

<span data-ttu-id="1b768-104">이 기능을 사용 하려면 ASP.NET Core 2.x 대상.NET 2.x 핵심입니다.</span><span class="sxs-lookup"><span data-stu-id="1b768-104">This feature requires ASP.NET Core 2.x targeting .NET Core 2.x.</span></span>

<span data-ttu-id="1b768-105">ASP.NET Core에 대한 [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) 메타패키지는 다음을 포함합니다.</span><span class="sxs-lookup"><span data-stu-id="1b768-105">The [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) metapackage for ASP.NET Core includes:</span></span>

* <span data-ttu-id="1b768-106">ASP.NET Core 팀에서 지원되는 모든 패키지</span><span class="sxs-lookup"><span data-stu-id="1b768-106">All supported packages by the ASP.NET Core team.</span></span>
* <span data-ttu-id="1b768-107">Entity Framework Core에서 지원되는 모든 패키지</span><span class="sxs-lookup"><span data-stu-id="1b768-107">All supported packages by the Entity Framework Core.</span></span> 
* <span data-ttu-id="1b768-108">ASP.NET Core 및 Entity Framework Core에서 사용하는 내부 및 타사 종속성</span><span class="sxs-lookup"><span data-stu-id="1b768-108">Internal and 3rd-party dependencies used by ASP.NET Core and Entity Framework Core.</span></span> 

<span data-ttu-id="1b768-109">ASP.NET Core의 모든 기능 2.x 및 Entity Framework Core 2.x에 포함 되는 `Microsoft.AspNetCore.All` 패키지 합니다.</span><span class="sxs-lookup"><span data-stu-id="1b768-109">All the features of ASP.NET Core 2.x and Entity Framework Core 2.x are included in the `Microsoft.AspNetCore.All` package.</span></span> <span data-ttu-id="1b768-110">기본 프로젝트 템플릿에이 패키지를 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="1b768-110">The default project templates use this package.</span></span>

<span data-ttu-id="1b768-111">버전 번호는 `Microsoft.AspNetCore.All` metapackage ASP.NET Core 버전 및 Entity Framework Core 버전 (.NET Core 버전으로 정렬 됨)를 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="1b768-111">The version number of the `Microsoft.AspNetCore.All` metapackage represents the ASP.NET Core version and Entity Framework Core version (aligned with the .NET Core version).</span></span>

<span data-ttu-id="1b768-112">사용 하는 응용 프로그램의 `Microsoft.AspNetCore.All` metapackage을 자동으로 이용는 [.NET Core 런타임 저장소](https://docs.microsoft.com/dotnet/core/deploying/runtime-store)합니다.</span><span class="sxs-lookup"><span data-stu-id="1b768-112">Applications that use the `Microsoft.AspNetCore.All` metapackage automatically take advantage of the [.NET Core Runtime Store](https://docs.microsoft.com/dotnet/core/deploying/runtime-store).</span></span> <span data-ttu-id="1b768-113">런타임 저장소 ASP.NET Core 2.x 응용 프로그램을 실행 하는 데 필요한 모든 런타임 자산을 포함 합니다.</span><span class="sxs-lookup"><span data-stu-id="1b768-113">The Runtime Store contains all the runtime assets needed to run ASP.NET Core 2.x applications.</span></span> <span data-ttu-id="1b768-114">사용 하는 경우는 `Microsoft.AspNetCore.All` metapackage, **없습니다** 참조 ASP.NET Core NuGet 패키지가에서 자산에는 응용 프로그램과 함께 배포 되 &mdash; .NET Core 런타임 저장소 이러한 자산을 포함 합니다.</span><span class="sxs-lookup"><span data-stu-id="1b768-114">When you use the `Microsoft.AspNetCore.All` metapackage, **no** assets from the referenced ASP.NET Core NuGet packages are deployed with the application &mdash; the .NET Core Runtime Store contains these assets.</span></span> <span data-ttu-id="1b768-115">응용 프로그램 시작 시간을 개선 하기 위해 자산 런타임 저장소에 미리 컴파일되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="1b768-115">The assets in the Runtime Store are precompiled to improve application startup time.</span></span>

<span data-ttu-id="1b768-116">사용 하지 않는 패키지를 제거 하려면 패키지 해제 프로세스를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="1b768-116">You can use the package trimming process to remove packages that you don't use.</span></span> <span data-ttu-id="1b768-117">트리밍된 패키지는 게시 된 응용 프로그램 출력에서 제외 됩니다.</span><span class="sxs-lookup"><span data-stu-id="1b768-117">Trimmed packages are excluded in published application output.</span></span>

<span data-ttu-id="1b768-118">다음 *.csproj* 파일 참조는 `Microsoft.AspNetCore.All` metapackage ASP.NET Core 용:</span><span class="sxs-lookup"><span data-stu-id="1b768-118">The following *.csproj* file references the `Microsoft.AspNetCore.All` metapackage for ASP.NET Core:</span></span>

[!code-xml[Main](..\mvc\views\view-compilation\sample\MvcRazorCompileOnPublish2.csproj?highlight=9)]
