---
title: '자습서: ASP.NET Core MVC를 사용하여 웹 API 만들기'
author: rick-anderson
description: ASP.NET Core MVC를 사용하여 웹 API 빌드
ms.author: riande
ms.custom: mvc
ms.date: 12/10/2018
uid: tutorials/first-web-api
ms.openlocfilehash: 03936ee74836c7b214cb3dc4023a6e3c252f2a26
ms.sourcegitcommit: cec77d5ad8a0cedb1ecbec32834111492afd0cd2
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/10/2019
ms.locfileid: "54207449"
---
# <a name="tutorial-create-a-web-api-with-aspnet-core-mvc"></a><span data-ttu-id="79cf0-103">자습서: ASP.NET Core MVC를 사용하여 웹 API 만들기</span><span class="sxs-lookup"><span data-stu-id="79cf0-103">Tutorial: Create a web API with ASP.NET Core MVC</span></span>

<span data-ttu-id="79cf0-104">작성자: [Rick Anderson](https://twitter.com/RickAndMSFT) 및 [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="79cf0-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="79cf0-105">이 자습서에서는 ASP.NET Core를 사용하여 웹 API를 빌드하는 작업의 기본 사항을 설명합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-105">This tutorial teaches the basics of building a web API with ASP.NET Core.</span></span>

<span data-ttu-id="79cf0-106">이 자습서에서는 다음과 같은 작업을 수행하는 방법을 살펴봅니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-106">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="79cf0-107">웹 API 프로젝트를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-107">Create a web API project.</span></span>
> * <span data-ttu-id="79cf0-108">모델 클래스 추가</span><span class="sxs-lookup"><span data-stu-id="79cf0-108">Add a model class.</span></span>
> * <span data-ttu-id="79cf0-109">데이터베이스 컨텍스트 만들기</span><span class="sxs-lookup"><span data-stu-id="79cf0-109">Create the database context.</span></span>
> * <span data-ttu-id="79cf0-110">데이터베이스 컨텍스트 등록</span><span class="sxs-lookup"><span data-stu-id="79cf0-110">Register the database context.</span></span>
> * <span data-ttu-id="79cf0-111">컨트롤러 추가</span><span class="sxs-lookup"><span data-stu-id="79cf0-111">Add a controller.</span></span>
> * <span data-ttu-id="79cf0-112">CRUD 메서드 추가</span><span class="sxs-lookup"><span data-stu-id="79cf0-112">Add CRUD methods.</span></span>
> * <span data-ttu-id="79cf0-113">라우팅 및 URL 경로 구성</span><span class="sxs-lookup"><span data-stu-id="79cf0-113">Configure routing and URL paths.</span></span>
> * <span data-ttu-id="79cf0-114">반환 값 지정</span><span class="sxs-lookup"><span data-stu-id="79cf0-114">Specify return values.</span></span>
> * <span data-ttu-id="79cf0-115">Postman을 사용하여 웹 API 호출</span><span class="sxs-lookup"><span data-stu-id="79cf0-115">Call the web API with Postman.</span></span>
> * <span data-ttu-id="79cf0-116">jQuery를 사용하여 웹 API를 호출합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-116">Call the web API with jQuery.</span></span>

<span data-ttu-id="79cf0-117">작업을 완료하면 웹 API는 관계형 데이터베이스에 저장된 "할 일" 항목을 관리할 수 있게 됩니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-117">At the end, you have a web API that can manage "to-do" items stored in a relational database.</span></span>

## <a name="overview"></a><span data-ttu-id="79cf0-118">개요</span><span class="sxs-lookup"><span data-stu-id="79cf0-118">Overview</span></span>

<span data-ttu-id="79cf0-119">이 자습서에서는 다음 API를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-119">This tutorial creates the following API:</span></span>

|<span data-ttu-id="79cf0-120">API</span><span class="sxs-lookup"><span data-stu-id="79cf0-120">API</span></span> | <span data-ttu-id="79cf0-121">설명</span><span class="sxs-lookup"><span data-stu-id="79cf0-121">Description</span></span> | <span data-ttu-id="79cf0-122">요청 본문</span><span class="sxs-lookup"><span data-stu-id="79cf0-122">Request body</span></span> | <span data-ttu-id="79cf0-123">응답 본문</span><span class="sxs-lookup"><span data-stu-id="79cf0-123">Response body</span></span> |
|--- | ---- | ---- | ---- |
|<span data-ttu-id="79cf0-124">GET /api/todo</span><span class="sxs-lookup"><span data-stu-id="79cf0-124">GET /api/todo</span></span> | <span data-ttu-id="79cf0-125">할 일 항목 모두 가져오기</span><span class="sxs-lookup"><span data-stu-id="79cf0-125">Get all to-do items</span></span> | <span data-ttu-id="79cf0-126">없음</span><span class="sxs-lookup"><span data-stu-id="79cf0-126">None</span></span> | <span data-ttu-id="79cf0-127">할 일 항목의 배열</span><span class="sxs-lookup"><span data-stu-id="79cf0-127">Array of to-do items</span></span>|
|<span data-ttu-id="79cf0-128">GET /api/todo/{id}</span><span class="sxs-lookup"><span data-stu-id="79cf0-128">GET /api/todo/{id}</span></span> | <span data-ttu-id="79cf0-129">ID로 항목 가져오기</span><span class="sxs-lookup"><span data-stu-id="79cf0-129">Get an item by ID</span></span> | <span data-ttu-id="79cf0-130">없음</span><span class="sxs-lookup"><span data-stu-id="79cf0-130">None</span></span> | <span data-ttu-id="79cf0-131">할 일 항목</span><span class="sxs-lookup"><span data-stu-id="79cf0-131">To-do item</span></span>|
|<span data-ttu-id="79cf0-132">POST /api/todo</span><span class="sxs-lookup"><span data-stu-id="79cf0-132">POST /api/todo</span></span> | <span data-ttu-id="79cf0-133">새 항목 추가</span><span class="sxs-lookup"><span data-stu-id="79cf0-133">Add a new item</span></span> | <span data-ttu-id="79cf0-134">할 일 항목</span><span class="sxs-lookup"><span data-stu-id="79cf0-134">To-do item</span></span> | <span data-ttu-id="79cf0-135">할 일 항목</span><span class="sxs-lookup"><span data-stu-id="79cf0-135">To-do item</span></span> |
|<span data-ttu-id="79cf0-136">PUT /api/todo/{id}</span><span class="sxs-lookup"><span data-stu-id="79cf0-136">PUT /api/todo/{id}</span></span> | <span data-ttu-id="79cf0-137">기존 항목 업데이트 &nbsp;</span><span class="sxs-lookup"><span data-stu-id="79cf0-137">Update an existing item &nbsp;</span></span> | <span data-ttu-id="79cf0-138">할 일 항목</span><span class="sxs-lookup"><span data-stu-id="79cf0-138">To-do item</span></span> | <span data-ttu-id="79cf0-139">없음</span><span class="sxs-lookup"><span data-stu-id="79cf0-139">None</span></span> |
|<span data-ttu-id="79cf0-140">DELETE /api/todo/{id} &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="79cf0-140">DELETE /api/todo/{id} &nbsp; &nbsp;</span></span> | <span data-ttu-id="79cf0-141">항목 삭제 &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="79cf0-141">Delete an item &nbsp; &nbsp;</span></span> | <span data-ttu-id="79cf0-142">없음</span><span class="sxs-lookup"><span data-stu-id="79cf0-142">None</span></span> | <span data-ttu-id="79cf0-143">없음</span><span class="sxs-lookup"><span data-stu-id="79cf0-143">None</span></span>|

<span data-ttu-id="79cf0-144">다음 다이어그램에서는 앱의 디자인을 보여줍니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-144">The following diagram shows the design of the app.</span></span>

![클라이언트는 왼쪽에 상자로 표시되며 요청을 제출하고 오른쪽에 그린 상자인 애플리케이션에서 응답을 받습니다.](first-web-api/_static/architecture.png)

[!INCLUDE[](~/includes/net-core-prereqs-all-2.2.md)]

## <a name="create-a-web-project"></a><span data-ttu-id="79cf0-149">웹 프로젝트 만들기</span><span class="sxs-lookup"><span data-stu-id="79cf0-149">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="79cf0-150">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="79cf0-150">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="79cf0-151">**파일** 메뉴에서 **새로 만들기** > **프로젝트**를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-151">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="79cf0-152">**ASP.NET Core 웹 애플리케이션** 템플릿을 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-152">Select the **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="79cf0-153">프로젝트 이름을 *TodoApi*로 지정하고 **확인**을 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-153">Name the project *TodoApi* and click **OK**.</span></span>
* <span data-ttu-id="79cf0-154">**새 ASP.NET Core 웹 애플리케이션 - TodoApi** 대화 상자에서 ASP.NET Core 버전을 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-154">In the **New ASP.NET Core Web Application - TodoApi** dialog, choose the ASP.NET Core version.</span></span> <span data-ttu-id="79cf0-155">**API** 템플릿을 선택하고 **확인**을 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-155">Select the **API** template and click **OK**.</span></span> <span data-ttu-id="79cf0-156">**Docker 지원 사용**을 선택하지 **마세요**.</span><span class="sxs-lookup"><span data-stu-id="79cf0-156">Do **not** select **Enable Docker Support**.</span></span>

![VS 새 프로젝트 대화 상자](first-web-api/_static/vs.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="79cf0-158">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="79cf0-158">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="79cf0-159">[통합 터미널](https://code.visualstudio.com/docs/editor/integrated-terminal)을 엽니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-159">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="79cf0-160">디렉터리(`cd`)를 프로젝트 폴더를 포함하는 폴더로 변경합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-160">Change directories (`cd`) to the folder which will contain the project folder.</span></span>
* <span data-ttu-id="79cf0-161">다음 명령을 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-161">Run the following commands:</span></span>

   ```console
   dotnet new webapi -o TodoApi
   code -r TodoApi
   ```

  <span data-ttu-id="79cf0-162">이러한 명령은 새 웹 API 프로젝트를 만들고 새 프로젝트 폴더에서 Visual Studio Code의 새 인스턴스를 엽니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-162">These commands create a new web API project and open a new instance of Visual Studio Code in the new project folder.</span></span>

* <span data-ttu-id="79cf0-163">프로젝트에 필수 자산을 추가하려는지 묻는 대화 상자가 나타나면 **예**를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-163">When a dialog box asks if you want to add required assets to the project, select **Yes**.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="79cf0-164">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="79cf0-164">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="79cf0-165">**파일** > **새 솔루션**을 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-165">Select **File** > **New Solution**.</span></span>

  ![macOS 새 솔루션](first-web-api-mac/_static/sln.png)

* <span data-ttu-id="79cf0-167">**.NET Core App** > **ASP.NET Core Web API** > **다음**을 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-167">Select **.NET Core App** > **ASP.NET Core Web API** > **Next**.</span></span>

  ![macOS 새 프로젝트 대화 상자](first-web-api-mac/_static/1.png)
  
* <span data-ttu-id="79cf0-169">**새 ASP.NET Core Web API 구성** 대화 상자에서 \**.NET Core 2.2*라는 기본 **대상 프레임워크**를 수락합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-169">In the **Configure your new ASP.NET Core Web API** dialog, accept the default **Target Framework** of \**.NET Core 2.2*.</span></span>

* <span data-ttu-id="79cf0-170">**프로젝트 이름**으로 *TodoApi*를 입력한 다음, **만들기**를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-170">Enter *TodoApi* for the **Project Name** and then select **Create**.</span></span>

  ![구성 대화 상자](first-web-api-mac/_static/2.png)

---

### <a name="test-the-api"></a><span data-ttu-id="79cf0-172">API 테스트</span><span class="sxs-lookup"><span data-stu-id="79cf0-172">Test the API</span></span>

<span data-ttu-id="79cf0-173">프로젝트 템플릿은 `values` API를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-173">The project template creates a `values` API.</span></span> <span data-ttu-id="79cf0-174">브라우저에서 `Get` 메서드를 호출하여 앱을 테스트합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-174">Call the `Get` method from a browser to test the app.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="79cf0-175">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="79cf0-175">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="79cf0-176">Ctrl+F5 키를 눌러 앱을 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-176">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="79cf0-177">Visual Studio가 브라우저를 시작하고 `https://localhost:<port>/api/values`로 이동합니다. 여기서 `<port>`는 임의로 선택된 포트 번호입니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-177">Visual Studio launches a browser and navigates to `https://localhost:<port>/api/values`, where `<port>` is a randomly chosen port number.</span></span>

<span data-ttu-id="79cf0-178">IIS Express 인증서를 신뢰해야 하는지 묻는 대화 상자가 표시되면 **예**를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-178">If you get a dialog box that asks if you should trust the IIS Express certificate, select **Yes**.</span></span> <span data-ttu-id="79cf0-179">다음으로, **보안 경고** 대화 상자가 나타나면 **예**를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-179">In the **Security Warning** dialog that appears next, select **Yes**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="79cf0-180">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="79cf0-180">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="79cf0-181">Ctrl+F5 키를 눌러 앱을 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-181">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="79cf0-182">브라우저에서 다음 URL [https://localhost:5001/api/values](https://localhost:5001/api/values)로 이동합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-182">In a browser, go to following URL: [https://localhost:5001/api/values](https://localhost:5001/api/values).</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="79cf0-183">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="79cf0-183">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="79cf0-184">**실행** > **디버깅 시작**을 선택하여 앱을 시작합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-184">Select **Run** > **Start With Debugging** to launch the app.</span></span> <span data-ttu-id="79cf0-185">Mac용 Visual Studio가 브라우저를 시작하고 `https://localhost:<port>`로 이동합니다. 여기서 `<port>`는 임의로 선택된 포트 번호입니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-185">Visual Studio for Mac launches a browser and navigates to `https://localhost:<port>`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="79cf0-186">HTTP 404(찾을 수 없음) 오류가 반환됩니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-186">An HTTP 404 (Not Found) error is returned.</span></span> <span data-ttu-id="79cf0-187">`/api/values`를 URL에 추가합니다(URL을 `https://localhost:<port>/api/values`로 변경).</span><span class="sxs-lookup"><span data-stu-id="79cf0-187">Append `/api/values` to the URL (change the URL to `https://localhost:<port>/api/values`).</span></span>

---

<span data-ttu-id="79cf0-188">다음 JSON이 반환됩니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-188">The following JSON is returned:</span></span>

```json
["value1","value2"]
```

## <a name="add-a-model-class"></a><span data-ttu-id="79cf0-189">모델 클래스 추가</span><span class="sxs-lookup"><span data-stu-id="79cf0-189">Add a model class</span></span>

<span data-ttu-id="79cf0-190">*모델*은 앱에서 관리하는 데이터를 나타내는 일련의 클래스입니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-190">A *model* is a set of classes that represent the data that the app manages.</span></span> <span data-ttu-id="79cf0-191">이 앱에 대한 모델은 단일 `TodoItem` 클래스입니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-191">The model for this app is a single `TodoItem` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="79cf0-192">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="79cf0-192">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="79cf0-193">**솔루션 탐색기**에서 프로젝트를 마우스 오른쪽 단추로 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-193">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="79cf0-194">**추가** > **새 폴더**를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-194">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="79cf0-195">폴더 이름을 *Models*로 지정합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-195">Name the folder *Models*.</span></span>

* <span data-ttu-id="79cf0-196">*Models* 폴더를 마우스 오른쪽 단추로 클릭하고 **추가** > **클래스**를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-196">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="79cf0-197">클래스 이름을 *TodoItem*으로 지정하고 **추가**를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-197">Name the class *TodoItem* and select **Add**.</span></span>

* <span data-ttu-id="79cf0-198">템플릿 코드를 다음 코드로 바꿉니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-198">Replace the template code with the following code:</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="79cf0-199">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="79cf0-199">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="79cf0-200">*Models* 폴더를 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-200">Add a folder named *Models*.</span></span>

* <span data-ttu-id="79cf0-201">다음 코드를 사용하여 *Models* 폴더에 `TodoItem` 클래스를 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-201">Add a `TodoItem` class to the *Models* folder with the following code:</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="79cf0-202">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="79cf0-202">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="79cf0-203">프로젝트를 마우스 오른쪽 단추로 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-203">Right-click the project.</span></span> <span data-ttu-id="79cf0-204">**추가** > **새 폴더**를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-204">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="79cf0-205">폴더 이름을 *Models*로 지정합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-205">Name the folder *Models*.</span></span>

  ![새 폴더](first-web-api-mac/_static/folder.png)

* <span data-ttu-id="79cf0-207">*모델* 폴더를 마우스 오른쪽 단추로 클릭하고 **추가** > **새 파일** > **일반** > **빈 클래스**를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-207">Right-click the *Models* folder, and select **Add** > **New File** > **General** > **Empty Class**.</span></span>

* <span data-ttu-id="79cf0-208">클래스 이름을 *TodoItem*으로 지정한 다음, **새로 만들기**를 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-208">Name the class *TodoItem*, and then click **New**.</span></span>

* <span data-ttu-id="79cf0-209">템플릿 코드를 다음 코드로 바꿉니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-209">Replace the template code with the following code:</span></span>

---

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="79cf0-210">`Id` 속성은 관계형 데이터베이스에서 고유 키로 작동합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-210">The `Id` property functions as the unique key in a relational database.</span></span>

<span data-ttu-id="79cf0-211">모델 클래스는 프로젝트의 어디로든 이동할 수 있지만 규칙에 따라 *Models* 폴더를 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-211">Model classes can go anywhere in the project, but the *Models* folder is used by convention.</span></span>

## <a name="add-a-database-context"></a><span data-ttu-id="79cf0-212">데이터베이스 컨텍스트 추가</span><span class="sxs-lookup"><span data-stu-id="79cf0-212">Add a database context</span></span>

<span data-ttu-id="79cf0-213">*데이터베이스 컨텍스트*는 데이터 모델에 맞게 Entity Framework 기능을 조정하는 주 클래스입니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-213">The *database context* is the main class that coordinates Entity Framework functionality for a data model.</span></span> <span data-ttu-id="79cf0-214">`Microsoft.EntityFrameworkCore.DbContext` 클래스에서 파생시키는 방식으로 이 클래스를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-214">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="79cf0-215">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="79cf0-215">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="79cf0-216">*Models* 폴더를 마우스 오른쪽 단추로 클릭하고 **추가** > **클래스**를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-216">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="79cf0-217">클래스 이름을 *TodoContext*로 지정하고 **추가**를 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-217">Name the class *TodoContext* and click **Add**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="79cf0-218">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="79cf0-218">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="79cf0-219">`TodoContext` 클래스를 *Models* 폴더에 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-219">Add a `TodoContext` class to the *Models* folder.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="79cf0-220">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="79cf0-220">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="79cf0-221">*Models* 폴더에 `TodoContext` 클래스를 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-221">Add a `TodoContext` class in the *Models* folder:</span></span>

---

* <span data-ttu-id="79cf0-222">템플릿 코드를 다음 코드로 바꿉니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-222">Replace the template code with the following code:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoContext.cs)]

## <a name="register-the-database-context"></a><span data-ttu-id="79cf0-223">데이터베이스 컨텍스트 등록</span><span class="sxs-lookup"><span data-stu-id="79cf0-223">Register the database context</span></span>

<span data-ttu-id="79cf0-224">ASP.NET Core에서는 DB 컨텍스트와 같은 서비스를 [DI(종속성 주입)](xref:fundamentals/dependency-injection) 컨테이너에 등록해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-224">In ASP.NET Core, services such as the DB context must be registered with the [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="79cf0-225">컨테이너는 컨트롤러에 서비스를 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-225">The container provides the service to controllers.</span></span>

<span data-ttu-id="79cf0-226">*Startup.cs*를 다음 강조 표시된 코드로 업데이트합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-226">Update *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup1.cs?highlight=5,8,25-26&name=snippet_all)]

<span data-ttu-id="79cf0-227">위의 코드는:</span><span class="sxs-lookup"><span data-stu-id="79cf0-227">The preceding code:</span></span>

* <span data-ttu-id="79cf0-228">사용되지 않는 `using` 선언을 제거합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-228">Removes unused `using` declarations.</span></span>
* <span data-ttu-id="79cf0-229">DI 컨테이너에 데이터베이스 컨텍스트를 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-229">Adds the database context to the DI container.</span></span>
* <span data-ttu-id="79cf0-230">데이터베이스 컨텍스트가 메모리 내 데이터베이스를 사용하도록 지정합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-230">Specifies that the database context will use an in-memory database.</span></span>

## <a name="add-a-controller"></a><span data-ttu-id="79cf0-231">컨트롤러 추가</span><span class="sxs-lookup"><span data-stu-id="79cf0-231">Add a controller</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="79cf0-232">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="79cf0-232">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="79cf0-233">*Controllers* 폴더를 마우스 오른쪽 단추로 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-233">Right-click the *Controllers* folder.</span></span>
* <span data-ttu-id="79cf0-234">**추가** > **새 항목**을 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-234">Select **Add** > **New Item**.</span></span>
* <span data-ttu-id="79cf0-235">**새 항목 추가** 대화 상자에서 **API 컨트롤러 클래스** 템플릿을 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-235">In the **Add New Item** dialog, select the **API Controller Class** template.</span></span>
* <span data-ttu-id="79cf0-236">클래스 이름을 *TodoController*로 지정하고 **추가**를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-236">Name the class *TodoController*, and select **Add**.</span></span>

  ![검색 상자의 컨트롤러 및 웹 API 컨트롤러가 선택된 새 항목 추가 대화 상자](first-web-api/_static/new_controller.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="79cf0-238">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="79cf0-238">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="79cf0-239">*Controllers* 폴더에 `TodoController`라는 클래스를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-239">In the *Controllers* folder, create a class named `TodoController`.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="79cf0-240">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="79cf0-240">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="79cf0-241">*Controllers* 폴더에서 `TodoController` 클래스를 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-241">In the *Controllers* folder, add the class `TodoController`.</span></span>

---

* <span data-ttu-id="79cf0-242">템플릿 코드를 다음 코드로 바꿉니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-242">Replace the template code with the following code:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

<span data-ttu-id="79cf0-243">위의 코드는:</span><span class="sxs-lookup"><span data-stu-id="79cf0-243">The preceding code:</span></span>

* <span data-ttu-id="79cf0-244">메서드 없이 API 컨트롤러 클래스를 정의합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-244">Defines an API controller class without methods.</span></span>
* <span data-ttu-id="79cf0-245">[`[ApiController]`](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) 특성을 사용하여 클래스를 데코레이트합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-245">Decorates the class with the [`[ApiController]`](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute.</span></span> <span data-ttu-id="79cf0-246">이 특성은 컨트롤러가 웹 API 요청에 응답함을 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-246">This attribute indicates that the controller responds to web API requests.</span></span> <span data-ttu-id="79cf0-247">특성을 사용하도록 설정하는 특정 동작에 대한 정보는 [ApiController 특성 주석](xref:web-api/index#annotation-with-apicontroller-attribute)을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="79cf0-247">For information about specific behaviors that the attribute enables, see [Annotation with ApiController attribute](xref:web-api/index#annotation-with-apicontroller-attribute).</span></span>
* <span data-ttu-id="79cf0-248">DI를 사용하여 데이터베이스 컨텍스트(`TodoContext`)를 컨트롤러에 삽입합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-248">Uses DI to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="79cf0-249">컨트롤러의 [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) 메서드 각각에서 데이터베이스 컨텍스트를 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-249">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>
* <span data-ttu-id="79cf0-250">데이터베이스가 비어 있는 경우 데이터베이스에 `Item1`이라는 항목을 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-250">Adds an item named `Item1` to the database if the database is empty.</span></span> <span data-ttu-id="79cf0-251">이 코드는 생성자에 위치하므로 새 HTTP 요청이 발생할 때마다 실행됩니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-251">This code is in the constructor, so it runs every time there's a new HTTP request.</span></span> <span data-ttu-id="79cf0-252">모든 항목을 삭제하면 생성자는 다음에 API가 호출될 경우 `Item1`을 다시 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-252">If you delete all items, the constructor creates `Item1` again the next time an API method is called.</span></span> <span data-ttu-id="79cf0-253">따라서 실제로 작동되는 경우 삭제가 작동하지 않는 것처럼 보일 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-253">So it may look like the deletion didn't work when it actually did work.</span></span>

## <a name="add-get-methods"></a><span data-ttu-id="79cf0-254">GET 메서드 추가</span><span class="sxs-lookup"><span data-stu-id="79cf0-254">Add Get methods</span></span>

<span data-ttu-id="79cf0-255">할 일 항목을 가져오는 API를 제공하려면 다음 메서드를 `TodoController` 클래스에 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-255">To provide an API that retrieves to-do items, add the following methods to the `TodoController` class:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]

<span data-ttu-id="79cf0-256">다음과 같은 메서드는 두 개의 GET 엔드포인트를 구현합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-256">These methods implement two GET endpoints:</span></span>

* `GET /api/todo`
* `GET /api/todo/{id}`

<span data-ttu-id="79cf0-257">브라우저에서 두 개의 엔드포인트를 호출하여 앱을 테스트합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-257">Test the app by calling the two endpoints from a browser.</span></span> <span data-ttu-id="79cf0-258">예:</span><span class="sxs-lookup"><span data-stu-id="79cf0-258">For example:</span></span>

* `https://localhost:<port>/api/todo`
* `https://localhost:<port>/api/todo/1`

<span data-ttu-id="79cf0-259">`GetTodoItems`를 호출하여 다음 HTTP 응답이 생성됩니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-259">The following HTTP response is produced by the call to `GetTodoItems`:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

## <a name="routing-and-url-paths"></a><span data-ttu-id="79cf0-260">라우팅 및 URL 경로</span><span class="sxs-lookup"><span data-stu-id="79cf0-260">Routing and URL paths</span></span>

<span data-ttu-id="79cf0-261">[`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) 특성은 HTTP GET 요청에 응답하는 메서드를 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-261">The [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="79cf0-262">각 방법에 대한 URL 경로는 다음과 같이 구성됩니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-262">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="79cf0-263">컨트롤러의 `Route` 특성에서 템플릿 문자열로 시작합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-263">Start with the template string in the controller's `Route` attribute:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]

* <span data-ttu-id="79cf0-264">`[controller]`를 컨트롤러의 이름으로 바꿉니다. 일반적으로 컨트롤러 클래스 이름에서 "Controller" 접미사를 뺀 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-264">Replace `[controller]` with the name of the controller, which by convention is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="79cf0-265">이 샘플의 경우 컨트롤러 클래스 이름은 **Todo**Controller이므로 컨트롤러 이름은 "todo"입니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-265">For this sample, the controller class name is **Todo**Controller, so the controller name is "todo".</span></span> <span data-ttu-id="79cf0-266">ASP.NET Core [라우팅](xref:mvc/controllers/routing)은 대/소문자를 구분하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-266">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="79cf0-267">`[HttpGet]` 특성에 경로 템플릿(예: `[HttpGet("/products")]`)이 있는 경우 경로에 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-267">If the `[HttpGet]` attribute has a route template (for example, `[HttpGet("/products")]`, append that to the path.</span></span> <span data-ttu-id="79cf0-268">이 샘플은 템플릿을 사용하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-268">This sample doesn't use a template.</span></span> <span data-ttu-id="79cf0-269">자세한 내용은 [Http[동사] 특성을 사용한 특성 라우팅](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes)을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="79cf0-269">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="79cf0-270">다음 `GetTodoItem` 메서드에서 `"{id}"`는 할 일 항목의 고유 식별자에 대한 자리 표시자 변수입니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-270">In the following `GetTodoItem` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="79cf0-271">`GetTodoItem`가 호출되면 URL의 `"{id}"` 값을 `id` 매개 변수의 메서드에 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-271">When `GetTodoItem` is invoked, the value of `"{id}"` in the URL is provided to the method in its`id` parameter.</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

<span data-ttu-id="79cf0-272">`Name = "GetTodo"` 매개 변수는 명명된 경로를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-272">The `Name = "GetTodo"` parameter creates a named route.</span></span> <span data-ttu-id="79cf0-273">앱이 경로 이름을 사용하여 HTTP 링크를 만드는 데 이름을 사용하는 방법을 나중에 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-273">You'll see later how the app can use the name to create an HTTP link using the route name.</span></span>

## <a name="return-values"></a><span data-ttu-id="79cf0-274">반환 값</span><span class="sxs-lookup"><span data-stu-id="79cf0-274">Return values</span></span>

<span data-ttu-id="79cf0-275">`GetTodoItems` 및 `GetTodoItem` 메서드의 반환 형식은 [ActionResult\<T> 형식](xref:web-api/action-return-types#actionresultt-type)입니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-275">The return type of the `GetTodoItems` and `GetTodoItem` methods is [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type).</span></span> <span data-ttu-id="79cf0-276">ASP.NET Core는 자동으로 [JSON](https://www.json.org/)에 개체를 직렬화하고 JSON을 응답 메시지의 본문에 기록합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-276">ASP.NET Core automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="79cf0-277">이 반환 형식의 응답 코드는 200이며 처리되지 않은 예외가 없다고 가정합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-277">The response code for this return type is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="79cf0-278">처리되지 않은 예외는 5xx 오류로 변환됩니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-278">Unhandled exceptions are translated into 5xx errors.</span></span>

<span data-ttu-id="79cf0-279">`ActionResult` 반환 형식은 다양한 HTTP 상태 코드를 나타낼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-279">`ActionResult` return types can represent a wide range of HTTP status codes.</span></span> <span data-ttu-id="79cf0-280">예를 들어 `GetTodoItem`은 두 가지 상태 값을 반환할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-280">For example, `GetTodoItem` can return two different status values:</span></span>

* <span data-ttu-id="79cf0-281">요청된 ID와 일치하는 항목이 없는 경우 메서드가 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) 오류 코드를 반환합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-281">If no item matches the requested ID, the method returns a 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) error code.</span></span>
* <span data-ttu-id="79cf0-282">그렇지 않으면 메서드가 JSON 응답 본문에서 200을 반환합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-282">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="79cf0-283">`item`을 반환하면 HTTP 200 응답이 발생합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-283">Returning `item` results in an HTTP 200 response.</span></span>

## <a name="test-the-gettodoitems-method"></a><span data-ttu-id="79cf0-284">GetTodoItems 메서드 테스트</span><span class="sxs-lookup"><span data-stu-id="79cf0-284">Test the GetTodoItems method</span></span>

<span data-ttu-id="79cf0-285">이 자습서에서는 Postman을 사용하여 웹 API를 테스트합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-285">This tutorial uses Postman to test the web API.</span></span>

* <span data-ttu-id="79cf0-286">[Postman](https://www.getpostman.com/apps) 설치</span><span class="sxs-lookup"><span data-stu-id="79cf0-286">Install [Postman](https://www.getpostman.com/apps)</span></span>
* <span data-ttu-id="79cf0-287">웹앱을 시작합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-287">Start the web app.</span></span>
* <span data-ttu-id="79cf0-288">Postman을 시작합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-288">Start Postman.</span></span>
* <span data-ttu-id="79cf0-289">**SSL 인증서 확인** 사용 안 함</span><span class="sxs-lookup"><span data-stu-id="79cf0-289">Disable **SSL certificate verification**</span></span>
  
  * <span data-ttu-id="79cf0-290">**파일 > 설정**(\**일반* 탭)에서 **SSL 인증서 확인**을 사용하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-290">From  **File > Settings** (\**General* tab), disable **SSL certificate verification**.</span></span>
    > [!WARNING]
    > <span data-ttu-id="79cf0-291">컨트롤러를 테스트한 후에 SSL 인증서 확인을 다시 사용하도록 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-291">Re-enable SSL certificate verification after testing the controller.</span></span>

* <span data-ttu-id="79cf0-292">새 요청을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-292">Create a new request.</span></span>
  * <span data-ttu-id="79cf0-293">HTTP 메서드를 **GET**으로 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-293">Set the HTTP method to **GET**.</span></span>
  * <span data-ttu-id="79cf0-294">요청 URL을 `https://localhost:<port>/api/todo`로 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-294">Set the request URL to `https://localhost:<port>/api/todo`.</span></span> <span data-ttu-id="79cf0-295">예를 들어 `https://localhost:5001/api/todo`과 같은 형식입니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-295">For example, `https://localhost:5001/api/todo`.</span></span>
* <span data-ttu-id="79cf0-296">Postman에서 **두 개의 창 보기**를 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-296">Set **Two pane view** in Postman.</span></span>
* <span data-ttu-id="79cf0-297">**보내기**를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-297">Select **Send**.</span></span>

![Get 요청이 있는 Postman](first-web-api/_static/2pv.png)

## <a name="add-a-create-method"></a><span data-ttu-id="79cf0-299">메서드 만들기 추가</span><span class="sxs-lookup"><span data-stu-id="79cf0-299">Add a Create method</span></span>

<span data-ttu-id="79cf0-300">다음 `PostTodoItem` 메서드를 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-300">Add the following `PostTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="79cf0-301">이전 코드는 [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) 특성으로 표시되는 HTTP POST 메서드입니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-301">The preceding code is an HTTP POST method, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="79cf0-302">이 메서드는 HTTP 요청 본문에서 할 일 항목 값을 가져옵니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-302">The method gets the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="79cf0-303">`CreatedAtAction` 메서드는 다음 작업을 수행합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-303">The `CreatedAtAction` method:</span></span>

* <span data-ttu-id="79cf0-304">201 응답을 반환합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-304">Returns a 201 response.</span></span> <span data-ttu-id="79cf0-305">HTTP 201은 서버에서 새 리소스를 만드는 HTTP POST 메서드의 표준 응답입니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-305">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="79cf0-306">응답에 대한 위치 헤더를 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-306">Adds a Location header to the response.</span></span> <span data-ttu-id="79cf0-307">위치 헤더는 새로 만들어진 할 일 항목의 URI를 지정합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-307">The Location header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="79cf0-308">자세한 내용은 [10.2.2 201 생성됨](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html)을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="79cf0-308">For more information, see [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="79cf0-309">“GetTodo”라는 경로를 사용하여 URL을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-309">Uses the "GetTodo" named route to create the URL.</span></span> <span data-ttu-id="79cf0-310">“GetTodo”라는 경로는 `GetTodoItem`에 정의됩니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-310">The "GetTodo" named route is defined in `GetTodoItem`:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

### <a name="test-the-posttodoitem-method"></a><span data-ttu-id="79cf0-311">PostTodoItem 메서드 테스트</span><span class="sxs-lookup"><span data-stu-id="79cf0-311">Test the PostTodoItem method</span></span>

* <span data-ttu-id="79cf0-312">프로젝트를 빌드합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-312">Build the project.</span></span>
* <span data-ttu-id="79cf0-313">Postman에서 HTTP 메서드를 `POST`로 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-313">In Postman, set the HTTP method to `POST`.</span></span>
* <span data-ttu-id="79cf0-314">**본문** 탭을 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-314">Select the **Body** tab.</span></span>
* <span data-ttu-id="79cf0-315">**원시** 라디오 단추를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-315">Select the **raw** radio button.</span></span>
* <span data-ttu-id="79cf0-316">유형을 **JSON(application/json)** 으로 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-316">Set the type to **JSON (application/json)**.</span></span>
* <span data-ttu-id="79cf0-317">요청 본문에서 할 일 항목에 대한 JSON을 입력합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-317">In the request body enter JSON for a to-do item:</span></span>

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

* <span data-ttu-id="79cf0-318">**보내기**를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-318">Select **Send**.</span></span>

  ![생성 요청이 있는 Postman](first-web-api/_static/create.png)

  <span data-ttu-id="79cf0-320">405 메서드가 허용되지 않음 오류가 발생할 경우 `PostTodoItem` 메서드를 추가한 후에 프로젝트를 컴파일하지 않는 결과가 발생할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-320">If you get a 405 Method Not Allowed error, it's probably the result of not compiling the project after adding the after adding the `PostTodoItem` method.</span></span>

### <a name="test-the-location-header-uri"></a><span data-ttu-id="79cf0-321">위치 헤더 URI 테스트</span><span class="sxs-lookup"><span data-stu-id="79cf0-321">Test the location header URI</span></span>

* <span data-ttu-id="79cf0-322">**응답** 창에서 **헤더** 탭을 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-322">Select the **Headers** tab in the **Response** pane.</span></span>
* <span data-ttu-id="79cf0-323">**위치** 헤더 값을 복사합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-323">Copy the **Location** header value:</span></span>

  ![Postman 콘솔의 헤더 탭](first-web-api/_static/pmc2.png)

* <span data-ttu-id="79cf0-325">메서드를 GET으로 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-325">Set the method to GET.</span></span>
* <span data-ttu-id="79cf0-326">URI(예: `https://localhost:5001/api/Todo/2`) 붙여넣기</span><span class="sxs-lookup"><span data-stu-id="79cf0-326">Paste the URI (for example, `https://localhost:5001/api/Todo/2`)</span></span>
* <span data-ttu-id="79cf0-327">**보내기**를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-327">Select **Send**.</span></span>

## <a name="add-a-puttodoitem-method"></a><span data-ttu-id="79cf0-328">PutTodoItem 메서드 추가</span><span class="sxs-lookup"><span data-stu-id="79cf0-328">Add a PutTodoItem method</span></span>

<span data-ttu-id="79cf0-329">다음 `PutTodoItem` 메서드를 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-329">Add the following `PutTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

<span data-ttu-id="79cf0-330">HTTP PUT을 사용하는 것을 제외하고 `PutTodoItem`는 `PostTodoItem`와 비슷합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-330">`PutTodoItem` is similar to `PostTodoItem`, except it uses HTTP PUT.</span></span> <span data-ttu-id="79cf0-331">응답은 [204(콘텐츠 없음)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html)입니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-331">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="79cf0-332">HTTP 사양에 따라 PUT 요청의 경우 클라이언트는 변경 내용만이 아니라 전체 업데이트된 엔터티를 보내야 합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-332">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the changes.</span></span> <span data-ttu-id="79cf0-333">부분 업데이트를 지원하려면 [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute)를 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-333">To support partial updates, use [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span></span>

### <a name="test-the-puttodoitem-method"></a><span data-ttu-id="79cf0-334">PutTodoItem 메서드 테스트</span><span class="sxs-lookup"><span data-stu-id="79cf0-334">Test the PutTodoItem method</span></span>

<span data-ttu-id="79cf0-335">ID = 1인 할 일 항목을 업데이트하고 해당 이름을 "feed fish"로 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-335">Update the to-do item that has id = 1 and set its name to "feed fish":</span></span>

```json
  {
    "ID":1,
    "name":"feed fish",
    "isComplete":true
  }
```

<span data-ttu-id="79cf0-336">다음 이미지는 Postman 업데이트를 보여줍니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-336">The following image shows the Postman update:</span></span>

![204(콘텐츠 없음) 응답을 보여주는 Postman 콘솔](first-web-api/_static/pmcput.png)

## <a name="add-a-deletetodoitem-method"></a><span data-ttu-id="79cf0-338">DeleteTodoItem 메서드 추가</span><span class="sxs-lookup"><span data-stu-id="79cf0-338">Add a DeleteTodoItem method</span></span>

<span data-ttu-id="79cf0-339">다음 `DeleteTodoItem` 메서드를 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-339">Add the following `DeleteTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="79cf0-340">`DeleteTodoItem` 응답은 [204(콘텐츠 없음)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html)입니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-340">The `DeleteTodoItem` response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

### <a name="test-the-deletetodoitem-method"></a><span data-ttu-id="79cf0-341">DeleteTodoItem 메서드 테스트</span><span class="sxs-lookup"><span data-stu-id="79cf0-341">Test the DeleteTodoItem method</span></span>

<span data-ttu-id="79cf0-342">Postman을 사용하여 할 일 항목을 삭제합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-342">Use Postman to delete a to-do item:</span></span>

* <span data-ttu-id="79cf0-343">메서드를 `DELETE`로 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-343">Set the method to `DELETE`.</span></span>
* <span data-ttu-id="79cf0-344">예를 들어 삭제할 개체의 URI를 `https://localhost:5001/api/todo/1`로 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-344">Set the URI of the object to delete, for example `https://localhost:5001/api/todo/1`</span></span>
* <span data-ttu-id="79cf0-345">**보내기** 선택</span><span class="sxs-lookup"><span data-stu-id="79cf0-345">Select **Send**</span></span>

<span data-ttu-id="79cf0-346">샘플 앱을 사용하면 모든 항목을 삭제할 수 있습니다. 하지만 마지막 항목이 삭제되면 다음에 API를 호출하는 경우 모델 클래스 생성자에서 새로운 항목이 생성됩니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-346">The sample app allows you to delete all the items, but when the last item is deleted, a new one is created by the model class constructor the next time the API is called.</span></span>

## <a name="call-the-api-with-jquery"></a><span data-ttu-id="79cf0-347">jQuery를 사용하여 API 호출</span><span class="sxs-lookup"><span data-stu-id="79cf0-347">Call the API with jQuery</span></span>

<span data-ttu-id="79cf0-348">이 섹션에서는 jQuery를 사용하여 웹 API를 호출하는 HTML 페이지가 추가되었습니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-348">In this section, an HTML page is added that uses jQuery to call the web api.</span></span> <span data-ttu-id="79cf0-349">jQuery는 요청을 시작하고 API 응답의 세부 정보로 페이지를 업데이트합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-349">jQuery initiates the request and updates the page with the details from the API's response.</span></span>

<span data-ttu-id="79cf0-350">[정적 파일](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_)을 제공하고 [기본 파일 매핑을 사용](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_)하도록 프로젝트를 구성합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-350">Configure the app to [serve static files](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [enable default file mapping](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_):</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup.cs?highlight=14-15&name=snippet_configure)]

::: moniker range=">= aspnetcore-2.2"
<span data-ttu-id="79cf0-351">프로젝트 디렉터리에서 *wwwroot* 폴더를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-351">Create a *wwwroot* folder in the project directory.</span></span>
::: moniker-end

<span data-ttu-id="79cf0-352">*index.html*이라는 HTML 파일을 *wwwroot* 디렉터리에 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-352">Add an HTML file named *index.html* to the *wwwroot* directory.</span></span> <span data-ttu-id="79cf0-353">다음 표시로 콘텐츠를 바꿉니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-353">Replace its contents with the following markup:</span></span>

[!code-html[](first-web-api/samples/2.2/TodoApi/wwwroot/index.html)]

<span data-ttu-id="79cf0-354">*site.js*라는 JavaScript 파일을 *wwwroot* 디렉터리에 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-354">Add a JavaScript file named *site.js* to the *wwwroot* directory.</span></span> <span data-ttu-id="79cf0-355">다음 코드로 콘텐츠를 바꿉니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-355">Replace its contents with the following code:</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_SiteJs)]

<span data-ttu-id="79cf0-356">HTML 페이지를 로컬에서 테스트하려면 ASP.NET Core 프로젝트의 시작 설정을 변경해야 할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-356">A change to the ASP.NET Core project's launch settings may be required to test the HTML page locally:</span></span>

* <span data-ttu-id="79cf0-357">*Properties\launchSettings.json*을 엽니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-357">Open *Properties\launchSettings.json*.</span></span>
* <span data-ttu-id="79cf0-358">`launchUrl` 속성을 제거하여 앱이 *index.html*&mdash; 프로젝트의 기본 파일에서 열리도록 합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-358">Remove the `launchUrl` property to force the app to open at *index.html*&mdash;the project's default file.</span></span>

<span data-ttu-id="79cf0-359">여러 가지 방법으로 jQuery를 가져올 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-359">There are several ways to get jQuery.</span></span> <span data-ttu-id="79cf0-360">위의 코드 조각에서 라이브러리는 CDN에서 로드됩니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-360">In the preceding snippet, the library is loaded from a CDN.</span></span>

<span data-ttu-id="79cf0-361">이 샘플은 API의 모든 CRUD 메서드를 호출합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-361">This sample calls all of the CRUD methods of the API.</span></span> <span data-ttu-id="79cf0-362">API 호출에 대한 설명은 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-362">Following are explanations of the calls to the API.</span></span>

### <a name="get-a-list-of-to-do-items"></a><span data-ttu-id="79cf0-363">할 일 항목의 목록 가져오기</span><span class="sxs-lookup"><span data-stu-id="79cf0-363">Get a list of to-do items</span></span>

<span data-ttu-id="79cf0-364">jQuery [ajax](https://api.jquery.com/jquery.ajax/) 함수는 할 일 항목의 배열을 나타내는 JSON을 반환하는 API에 `GET` 요청을 보냅니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-364">The jQuery [ajax](https://api.jquery.com/jquery.ajax/) function sends a `GET` request to the API, which returns JSON representing an array of to-do items.</span></span> <span data-ttu-id="79cf0-365">요청이 성공하면 `success` 콜백 함수가 호출됩니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-365">The `success` callback function is invoked if the request succeeds.</span></span> <span data-ttu-id="79cf0-366">콜백에서 DOM은 할 일 정보로 업데이트됩니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-366">In the callback, the DOM is updated with the to-do information.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_GetData)]

### <a name="add-a-to-do-item"></a><span data-ttu-id="79cf0-367">할 일 항목 추가</span><span class="sxs-lookup"><span data-stu-id="79cf0-367">Add a to-do item</span></span>

<span data-ttu-id="79cf0-368">[ajax](https://api.jquery.com/jquery.ajax/) 함수는 `POST` 요청 본문에서 할 일 항목을 사용하여 요청을 보냅니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-368">The [ajax](https://api.jquery.com/jquery.ajax/) function sends a `POST` request with the to-do item in the request body.</span></span> <span data-ttu-id="79cf0-369">`accepts` 및 `contentType` 옵션은 수신 및 전송되는 미디어 형식을 지정하기 위해 `application/json`으로 설정됩니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-369">The `accepts` and `contentType` options are set to `application/json` to specify the media type being received and sent.</span></span> <span data-ttu-id="79cf0-370">[JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify)를 사용하여 할 일 항목을 JSON으로 변환합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-370">The to-do item is converted to JSON by using [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify).</span></span> <span data-ttu-id="79cf0-371">API가 성공적인 상태 코드를 반환하면 `getData` 함수가 호출되어 HTML 테이블을 업데이트합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-371">When the API returns a successful status code, the `getData` function is invoked to update the HTML table.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AddItem)]

### <a name="update-a-to-do-item"></a><span data-ttu-id="79cf0-372">할 일 항목 업데이트</span><span class="sxs-lookup"><span data-stu-id="79cf0-372">Update a to-do item</span></span>

<span data-ttu-id="79cf0-373">할 일 항목을 업데이트하는 작업은 추가하는 작업과 비슷합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-373">Updating a to-do item is similar to adding one.</span></span> <span data-ttu-id="79cf0-374">`url`은 항목의 고유 식별자를 추가하도록 변경되고 `type`은 `PUT`입니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-374">The `url` changes to add the unique identifier of the item, and the `type` is `PUT`.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxPut)]

### <a name="delete-a-to-do-item"></a><span data-ttu-id="79cf0-375">할 일 항목 삭제</span><span class="sxs-lookup"><span data-stu-id="79cf0-375">Delete a to-do item</span></span>

<span data-ttu-id="79cf0-376">할 일 항목을 삭제하려면 AJAX 호출에서 `type`을 `DELETE`로 설정하고 URL에서 항목의 고유 식별자를 지정하면 됩니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-376">Deleting a to-do item is accomplished by setting the `type` on the AJAX call to `DELETE` and specifying the item's unique identifier in the URL.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxDelete)]

## <a name="additional-resources"></a><span data-ttu-id="79cf0-377">추가 자료</span><span class="sxs-lookup"><span data-stu-id="79cf0-377">Additional resources</span></span>

<span data-ttu-id="79cf0-378">[이 자습서에서 샘플 코드 보기 또는 다운로드](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/samples)</span><span class="sxs-lookup"><span data-stu-id="79cf0-378">[View or download sample code for this tutorial](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/samples).</span></span> <span data-ttu-id="79cf0-379">[다운로드하는 방법](xref:index#how-to-download-a-sample)을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="79cf0-379">See [how to download](xref:index#how-to-download-a-sample).</span></span>

<span data-ttu-id="79cf0-380">자세한 내용은 다음 리소스를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="79cf0-380">For more information, see the following resources:</span></span>

* <xref:web-api/index>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:data/ef-rp/index>
* <xref:mvc/controllers/routing>
* <xref:web-api/action-return-types>
* <xref:host-and-deploy/azure-apps/index>
* <xref:host-and-deploy/index>

## <a name="next-steps"></a><span data-ttu-id="79cf0-381">다음 단계</span><span class="sxs-lookup"><span data-stu-id="79cf0-381">Next steps</span></span>

<span data-ttu-id="79cf0-382">본 자습서에서는 다음 작업에 관한 방법을 학습했습니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-382">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="79cf0-383">웹 API 프로젝트 만들기</span><span class="sxs-lookup"><span data-stu-id="79cf0-383">Create a web api project.</span></span>
> * <span data-ttu-id="79cf0-384">모델 클래스 추가</span><span class="sxs-lookup"><span data-stu-id="79cf0-384">Add a model class.</span></span>
> * <span data-ttu-id="79cf0-385">데이터베이스 컨텍스트 만들기</span><span class="sxs-lookup"><span data-stu-id="79cf0-385">Create the database context.</span></span>
> * <span data-ttu-id="79cf0-386">데이터베이스 컨텍스트 등록</span><span class="sxs-lookup"><span data-stu-id="79cf0-386">Register the database context.</span></span>
> * <span data-ttu-id="79cf0-387">컨트롤러 추가</span><span class="sxs-lookup"><span data-stu-id="79cf0-387">Add a controller.</span></span>
> * <span data-ttu-id="79cf0-388">CRUD 메서드 추가</span><span class="sxs-lookup"><span data-stu-id="79cf0-388">Add CRUD methods.</span></span>
> * <span data-ttu-id="79cf0-389">라우팅 및 URL 경로 구성</span><span class="sxs-lookup"><span data-stu-id="79cf0-389">Configure routing and URL paths.</span></span>
> * <span data-ttu-id="79cf0-390">반환 값 지정</span><span class="sxs-lookup"><span data-stu-id="79cf0-390">Specify return values.</span></span>
> * <span data-ttu-id="79cf0-391">Postman을 사용하여 웹 API 호출</span><span class="sxs-lookup"><span data-stu-id="79cf0-391">Call the web API with Postman.</span></span>
> * <span data-ttu-id="79cf0-392">jQuery를 사용하여 웹 API 호출</span><span class="sxs-lookup"><span data-stu-id="79cf0-392">Call the web api with jQuery.</span></span>

<span data-ttu-id="79cf0-393">API 도움말 페이지를 생성하는 방법을 알아보려면 다음 자습서로 계속 진행합니다.</span><span class="sxs-lookup"><span data-stu-id="79cf0-393">Advance to the next tutorial to learn how to generate API help pages:</span></span>

> [!div class="nextstepaction"]
> <xref:tutorials/get-started-with-swashbuckle>
