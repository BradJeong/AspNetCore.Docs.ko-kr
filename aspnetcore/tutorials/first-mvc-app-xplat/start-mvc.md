---
title: "Mac, Linux 또는 Windows에서 ASP.NET Core MVC 소개"
author: rick-anderson
description: "Mac, Linux 또는 Windows에서 ASP.NET Core MVC 및 Visual Studio Code 시작"
ms.author: riande
manager: wpickett
ms.date: 07/07/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app-xplat/start-mvc
ms.openlocfilehash: dae1b0f7c8700bbd99080752fb4bb37f93441c9a
ms.sourcegitcommit: 18ff1fdaa3e1ae204ed6a2ba9351ce8cf1371c85
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/29/2018
---
# <a name="getting-started-with-aspnet-core-mvc--on-mac-linux-or-windows"></a><span data-ttu-id="78e91-103">Mac, Linux 또는 Windows에서 ASP.NET Core MVC 시작</span><span class="sxs-lookup"><span data-stu-id="78e91-103">Getting started with ASP.NET Core MVC  on Mac, Linux, or Windows</span></span>

<span data-ttu-id="78e91-104">작성자: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="78e91-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="78e91-105">이 자습서에서는 [Visual Studio Code](https://code.visualstudio.com)(VS Code)를 사용하여 ASP.NET Core MVC 웹앱을 빌드하는 기본 사항에 대해 알아봅니다.</span><span class="sxs-lookup"><span data-stu-id="78e91-105">This tutorial will teach you the basics of building an ASP.NET Core MVC web app using [Visual Studio Code](https://code.visualstudio.com) (VS Code).</span></span> <span data-ttu-id="78e91-106">이 자습서에서는 VS 코드를 잘 알고 있다고 가정합니다.</span><span class="sxs-lookup"><span data-stu-id="78e91-106">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="78e91-107">자세한 내용은 [VS 코드 시작](https://code.visualstudio.com/docs) 및 [Visual Studio Code 도움말](#visual-studio-code-help)을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="78e91-107">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span> 

[!INCLUDE[consider RP](../../includes/razor.md)]

<span data-ttu-id="78e91-108">이 자습서는 세 가지 버전이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="78e91-108">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="78e91-109">macOS: [Mac용 Visual Studio를 사용하여 ASP.NET Core MVC 앱 만들기](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="78e91-109">macOS: [Create an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="78e91-110">Windows: [Visual Studio를 사용하여 ASP.NET Core MVC 앱 만들기](xref:tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="78e91-110">Windows: [Create an ASP.NET Core MVC app with Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="78e91-111">macOS, Linux 및 Windows: [Visual Studio Code를 사용하여 ASP.NET Core MVC 앱 만들기](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="78e91-111">macOS, Linux, and Windows: [Create an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span> 

## <a name="install-vs-code-and-net-core"></a><span data-ttu-id="78e91-112">VS Code 및 .NET Core 설치 </span><span class="sxs-lookup"><span data-stu-id="78e91-112">Install VS Code and .NET Core</span></span>

<span data-ttu-id="78e91-113">이 자습서에서는 [.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) 이상이 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="78e91-113">This tutorial requires the [.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later.</span></span> <span data-ttu-id="78e91-114">ASP.NET Core 1.1 버전에 대해서는 [PDF](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/first-mvc-app-mac/start-mvc/8-23-17.pdf)를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="78e91-114">See [the pdf](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/first-mvc-app-mac/start-mvc/8-23-17.pdf) for the ASP.NET Core 1.1 version.</span></span>

<span data-ttu-id="78e91-115">다음을 설치합니다.</span><span class="sxs-lookup"><span data-stu-id="78e91-115">Install the following:</span></span>

* <span data-ttu-id="78e91-116">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) 이상.</span><span class="sxs-lookup"><span data-stu-id="78e91-116">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later.</span></span>
* [<span data-ttu-id="78e91-117">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="78e91-117">Visual Studio Code</span></span>](https://code.visualstudio.com)
* <span data-ttu-id="78e91-118">VS Code [C# 확장](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span><span class="sxs-lookup"><span data-stu-id="78e91-118">VS Code [C# extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span></span> 

## <a name="create-a-web-app-with-dotnet"></a><span data-ttu-id="78e91-119">Dotnet와 웹앱 만들기</span><span class="sxs-lookup"><span data-stu-id="78e91-119">Create a web app with dotnet</span></span>

<span data-ttu-id="78e91-120">터미널에서 다음 명령을 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="78e91-120">From a terminal, run the following commands:</span></span>

```console
mkdir MvcMovie
cd MvcMovie
dotnet new mvc
```

<span data-ttu-id="78e91-121">VS Code(Visual Studio Code)에서 *MvcMovie* 폴더를 열고 *Startup.cs* 파일을 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="78e91-121">Open the *MvcMovie* folder in Visual Studio Code (VS Code) and select the *Startup.cs* file.</span></span>

- <span data-ttu-id="78e91-122">다음 **경고** 메시지에 대해 **예**를 선택합니다. “빌드 및 디버그에 필요한 자산이 'MvcMovie'에서 누락되었습니다.</span><span class="sxs-lookup"><span data-stu-id="78e91-122">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'MvcMovie'.</span></span> <span data-ttu-id="78e91-123">추가할까요?”</span><span class="sxs-lookup"><span data-stu-id="78e91-123">Add them?"</span></span>
- <span data-ttu-id="78e91-124">다음 **정보** 메시지에 대해 **복원**을 선택합니다. “확인되지 않은 종속성이 있습니다.”</span><span class="sxs-lookup"><span data-stu-id="78e91-124">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

![VS Code, 경고: 빌드 및 디버그에 필요한 자산이 'MvcMovie'에서 누락되었습니다. ](../web-api-vsc/_static/vsc_restore.png)

<span data-ttu-id="78e91-128">**디버그**(F5) 키를 눌러 프로그램을 빌드하고 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="78e91-128">Press **Debug** (F5) to build and run the program.</span></span>

![앱 실행](../first-mvc-app/start-mvc/_static/1.png)

<span data-ttu-id="78e91-130">VS Code가 [Kestrel](xref:fundamentals/servers/kestrel) 웹 서버를 시작하고 앱을 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="78e91-130">VS Code starts the [Kestrel](xref:fundamentals/servers/kestrel) web server and runs your app.</span></span> <span data-ttu-id="78e91-131">주소 표시줄에 `localhost:5000`이 표시되고 `example.com` 등은 표시되지 않음을 알 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="78e91-131">Notice that the address bar shows `localhost:5000` and not something like `example.com`.</span></span> <span data-ttu-id="78e91-132">그 이유는 `localhost`가 로컬 컴퓨터의 표준 이름이기 때문입니다.</span><span class="sxs-lookup"><span data-stu-id="78e91-132">That's because `localhost` is the standard hostname for your local computer.</span></span>

<span data-ttu-id="78e91-133">기본 템플릿은 작동하는 **홈, 정보** 및 **연락처** 링크를 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="78e91-133">The default template gives you working **Home, About** and **Contact** links.</span></span> <span data-ttu-id="78e91-134">위의 브라우저 이미지에는 이러한 링크가 표시되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="78e91-134">The browser image above doesn't show these links.</span></span> <span data-ttu-id="78e91-135">브라우저 크기에 따라 탐색 아이콘을 클릭하여 링크를 표시해야 할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="78e91-135">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![오른쪽 위의 탐색 아이콘](../first-mvc-app/start-mvc/_static/2.png)

<span data-ttu-id="78e91-137">이 자습서의 다음 부분에서는 MVC에 대해 알아보고 일부 코드 작성을 시작합니다.</span><span class="sxs-lookup"><span data-stu-id="78e91-137">In the next part of this tutorial, we'll learn about MVC and start writing some code.</span></span>

## <a name="visual-studio-code-help"></a><span data-ttu-id="78e91-138">Visual Studio Code 도움말</span><span class="sxs-lookup"><span data-stu-id="78e91-138">Visual Studio Code help</span></span>

- [<span data-ttu-id="78e91-139">시작</span><span class="sxs-lookup"><span data-stu-id="78e91-139">Getting started</span></span>](https://code.visualstudio.com/docs)
- [<span data-ttu-id="78e91-140">디버깅</span><span class="sxs-lookup"><span data-stu-id="78e91-140">Debugging</span></span>](https://code.visualstudio.com/docs/editor/debugging)
- [<span data-ttu-id="78e91-141">통합 터미널</span><span class="sxs-lookup"><span data-stu-id="78e91-141">Integrated terminal</span></span>](https://code.visualstudio.com/docs/editor/integrated-terminal)
- [<span data-ttu-id="78e91-142">바로 가기 키</span><span class="sxs-lookup"><span data-stu-id="78e91-142">Keyboard shortcuts</span></span>](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  - [<span data-ttu-id="78e91-143">Mac 바로 가기 키</span><span class="sxs-lookup"><span data-stu-id="78e91-143">Mac keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  - [<span data-ttu-id="78e91-144">Linux 바로 가기 키</span><span class="sxs-lookup"><span data-stu-id="78e91-144">Linux keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  - [<span data-ttu-id="78e91-145">Windows 바로 가기 키</span><span class="sxs-lookup"><span data-stu-id="78e91-145">Windows keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

>[!div class="step-by-step"]
[<span data-ttu-id="78e91-146">다음 - 컨트롤러 추가</span><span class="sxs-lookup"><span data-stu-id="78e91-146">Next - Add a controller</span></span>](adding-controller.md)
