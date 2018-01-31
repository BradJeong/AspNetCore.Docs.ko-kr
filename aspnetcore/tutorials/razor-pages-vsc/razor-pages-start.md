---
title: "Visual Studio Code를 사용하여 ASP.NET Core에서 Razor 페이지 시작"
author: rick-anderson
description: "Visual Studio Code를 사용하여 ASP.NET Core에서 Razor 페이지 시작"
manager: wpickett
ms.author: riande
ms.date: 08/27/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages-vsc/razor-pages-start
ms.openlocfilehash: 7c01d802e59951281c86c8eab64b7c6b9d646fbf
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/30/2018
---
# <a name="getting-started-with-razor-pages-in-aspnet-core-with-visual-studio-code"></a><span data-ttu-id="ebcfd-103">Visual Studio Code를 사용하여 ASP.NET Core에서 Razor 페이지 시작</span><span class="sxs-lookup"><span data-stu-id="ebcfd-103">Getting started with Razor Pages in ASP.NET Core with Visual Studio Code</span></span>

<span data-ttu-id="ebcfd-104">작성자: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="ebcfd-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="ebcfd-105">이 자습서에서는 ASP.NET Core Razor 페이지 웹앱을 빌드하는 작업의 기본 사항을 설명합니다.</span><span class="sxs-lookup"><span data-stu-id="ebcfd-105">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="ebcfd-106">이 자습서를 시작하기 전에 [Razor 페이지 소개](xref:mvc/razor-pages/index)를 완료하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="ebcfd-106">We recommend you complete [Introduction to Razor Pages](xref:mvc/razor-pages/index) before starting this tutorial.</span></span> <span data-ttu-id="ebcfd-107">Razor 페이지는 ASP.NET Core에서 웹 응용 프로그램 UI를 빌드하는 좋은 방법입니다.</span><span class="sxs-lookup"><span data-stu-id="ebcfd-107">Razor Pages is the recommended way to build UI for web applications in ASP.NET Core.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ebcfd-108">필수 구성 요소</span><span class="sxs-lookup"><span data-stu-id="ebcfd-108">Prerequisites</span></span>

<span data-ttu-id="ebcfd-109">다음을 설치합니다.</span><span class="sxs-lookup"><span data-stu-id="ebcfd-109">Install the following:</span></span>

* <span data-ttu-id="ebcfd-110">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) 이상</span><span class="sxs-lookup"><span data-stu-id="ebcfd-110">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later</span></span>
* [<span data-ttu-id="ebcfd-111">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ebcfd-111">Visual Studio Code</span></span>](https://code.visualstudio.com)
* <span data-ttu-id="ebcfd-112">VS Code [C# 확장](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span><span class="sxs-lookup"><span data-stu-id="ebcfd-112">VS Code [C# extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span></span> 

## <a name="create-a-razor-web-app"></a><span data-ttu-id="ebcfd-113">Razor 웹앱 만들기</span><span class="sxs-lookup"><span data-stu-id="ebcfd-113">Create a Razor web app</span></span>

<span data-ttu-id="ebcfd-114">터미널에서 다음 명령을 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="ebcfd-114">From a terminal, run the following commands:</span></span>

```console
dotnet new razor -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

<span data-ttu-id="ebcfd-115">이전 명령은 [.NET Core CLI](https://docs.microsoft.com/dotnet/core/tools/dotnet)를 사용하여 Razor 페이지 프로젝트를 만들고 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="ebcfd-115">The preceding commands use the [.NET Core CLI](https://docs.microsoft.com/dotnet/core/tools/dotnet) to create and run a Razor Pages project.</span></span> <span data-ttu-id="ebcfd-116">브라우저를 열고 http://localhost:5000으로 이동하여 응용 프로그램을 봅니다.</span><span class="sxs-lookup"><span data-stu-id="ebcfd-116">Open a browser to http://localhost:5000 to view the application.</span></span>

![홈 또는 인덱스 페이지](../razor-pages/razor-pages-start/_static/home.png)

[!INCLUDE[razor-pages-start](../../includes/RP/razor-pages-start.md)]

## <a name="open-the-project"></a><span data-ttu-id="ebcfd-118">프로젝트 열기</span><span class="sxs-lookup"><span data-stu-id="ebcfd-118">Open the project</span></span>

<span data-ttu-id="ebcfd-119">Ctrl+C를 눌러 응용 프로그램을 종료합니다.</span><span class="sxs-lookup"><span data-stu-id="ebcfd-119">Press Ctrl+C to shut down the application.</span></span>

<span data-ttu-id="ebcfd-120">VS Code(Visual Studio Code)에서 **파일 > 폴더 열기**를 선택하고 *RazorPagesMovie* 폴더를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="ebcfd-120">From Visual Studio Code (VS Code), select **File > Open Folder**, and then select the *RazorPagesMovie* folder.</span></span>

- <span data-ttu-id="ebcfd-121">다음 **경고** 메시지에 대해 **예**를 선택합니다. “빌드 및 디버그에 필요한 자산이 ‘RazorPagesMovie’에서 누락되었습니다.</span><span class="sxs-lookup"><span data-stu-id="ebcfd-121">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'RazorPagesMovie'.</span></span> <span data-ttu-id="ebcfd-122">추가할까요?”</span><span class="sxs-lookup"><span data-stu-id="ebcfd-122">Add them?"</span></span>
- <span data-ttu-id="ebcfd-123">다음 **정보** 메시지에 대해 **복원**을 선택합니다. “확인되지 않은 종속성이 있습니다.”</span><span class="sxs-lookup"><span data-stu-id="ebcfd-123">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="ebcfd-124">앱 시작</span><span class="sxs-lookup"><span data-stu-id="ebcfd-124">Launch the app</span></span>

<span data-ttu-id="ebcfd-125">Ctrl+F5를 눌러 디버깅 없이 앱을 시작합니다.</span><span class="sxs-lookup"><span data-stu-id="ebcfd-125">Press Ctrl+F5 to start the app without debugging.</span></span> <span data-ttu-id="ebcfd-126">또는 **디버그** 메뉴에서 **디버깅하지 않고 시작**을 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="ebcfd-126">Alternatively, from the **Debug** menu, select **Start Without Debugging**.</span></span>

<span data-ttu-id="ebcfd-127">다음 자습서에서는 프로젝트에 모델을 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="ebcfd-127">In the next tutorial, we add a model to the project.</span></span> 

>[!div class="step-by-step"]
[<span data-ttu-id="ebcfd-128">다음: 모델 추가</span><span class="sxs-lookup"><span data-stu-id="ebcfd-128">Next: Adding a model</span></span>](xref:tutorials/razor-pages-vsc/model)  
