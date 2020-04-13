---
title: '자습서: ASP.NET Core를 사용하여 웹 API 만들기'
author: rick-anderson
description: ASP.NET Core를 사용하여 웹 API를 빌드하는 방법을 알아봅니다.
ms.author: riande
ms.custom: mvc
ms.date: 2/25/2020
uid: tutorials/first-web-api
ms.openlocfilehash: 7418e962076fae3ebdbb25381838757b09046578
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80417655"
---
# <a name="tutorial-create-a-web-api-with-aspnet-core"></a><span data-ttu-id="bdbde-103">자습서: ASP.NET Core를 사용하여 웹 API 만들기</span><span class="sxs-lookup"><span data-stu-id="bdbde-103">Tutorial: Create a web API with ASP.NET Core</span></span>

<span data-ttu-id="bdbde-104">작성자: [Rick Anderson](https://twitter.com/RickAndMSFT), [Kirk Larkin](https://twitter.com/serpent5), [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="bdbde-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Kirk Larkin](https://twitter.com/serpent5), and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="bdbde-105">이 자습서에서는 ASP.NET Core를 사용하여 웹 API를 빌드하는 작업의 기본 사항을 설명합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-105">This tutorial teaches the basics of building a web API with ASP.NET Core.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="bdbde-106">이 자습서에서는 다음과 같은 작업을 수행하는 방법을 살펴봅니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-106">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="bdbde-107">웹 API 프로젝트를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-107">Create a web API project.</span></span>
> * <span data-ttu-id="bdbde-108">모델 클래스와 데이터베이스 컨텍스트를 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-108">Add a model class and a database context.</span></span>
> * <span data-ttu-id="bdbde-109">CRUD 메서드로 컨트롤러를 스캐폴드합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-109">Scaffold a controller with CRUD methods.</span></span>
> * <span data-ttu-id="bdbde-110">라우팅, URL 경로 및 반환 값을 구성합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-110">Configure routing, URL paths, and return values.</span></span>
> * <span data-ttu-id="bdbde-111">Postman을 사용하여 웹 API를 호출합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-111">Call the web API with Postman.</span></span>

<span data-ttu-id="bdbde-112">과정을 마치면 웹 API를 통해 데이터베이스에 저장된 “할 일” 항목을 관리할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-112">At the end, you have a web API that can manage "to-do" items stored in a database.</span></span>

## <a name="overview"></a><span data-ttu-id="bdbde-113">개요</span><span class="sxs-lookup"><span data-stu-id="bdbde-113">Overview</span></span>

<span data-ttu-id="bdbde-114">이 자습서에서는 다음 API를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-114">This tutorial creates the following API:</span></span>

|<span data-ttu-id="bdbde-115">API</span><span class="sxs-lookup"><span data-stu-id="bdbde-115">API</span></span> | <span data-ttu-id="bdbde-116">설명</span><span class="sxs-lookup"><span data-stu-id="bdbde-116">Description</span></span> | <span data-ttu-id="bdbde-117">요청 본문</span><span class="sxs-lookup"><span data-stu-id="bdbde-117">Request body</span></span> | <span data-ttu-id="bdbde-118">응답 본문</span><span class="sxs-lookup"><span data-stu-id="bdbde-118">Response body</span></span> |
|--- | ---- | ---- | ---- |
|<span data-ttu-id="bdbde-119">GET /api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="bdbde-119">GET /api/TodoItems</span></span> | <span data-ttu-id="bdbde-120">할 일 항목 모두 가져오기</span><span class="sxs-lookup"><span data-stu-id="bdbde-120">Get all to-do items</span></span> | <span data-ttu-id="bdbde-121">없음</span><span class="sxs-lookup"><span data-stu-id="bdbde-121">None</span></span> | <span data-ttu-id="bdbde-122">할 일 항목의 배열</span><span class="sxs-lookup"><span data-stu-id="bdbde-122">Array of to-do items</span></span>|
|<span data-ttu-id="bdbde-123">GET /api/TodoItems/{id}</span><span class="sxs-lookup"><span data-stu-id="bdbde-123">GET /api/TodoItems/{id}</span></span> | <span data-ttu-id="bdbde-124">ID로 항목 가져오기</span><span class="sxs-lookup"><span data-stu-id="bdbde-124">Get an item by ID</span></span> | <span data-ttu-id="bdbde-125">없음</span><span class="sxs-lookup"><span data-stu-id="bdbde-125">None</span></span> | <span data-ttu-id="bdbde-126">할 일 항목</span><span class="sxs-lookup"><span data-stu-id="bdbde-126">To-do item</span></span>|
|<span data-ttu-id="bdbde-127">POST /api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="bdbde-127">POST /api/TodoItems</span></span> | <span data-ttu-id="bdbde-128">새 항목 추가</span><span class="sxs-lookup"><span data-stu-id="bdbde-128">Add a new item</span></span> | <span data-ttu-id="bdbde-129">할 일 항목</span><span class="sxs-lookup"><span data-stu-id="bdbde-129">To-do item</span></span> | <span data-ttu-id="bdbde-130">할 일 항목</span><span class="sxs-lookup"><span data-stu-id="bdbde-130">To-do item</span></span> |
|<span data-ttu-id="bdbde-131">PUT /api/TodoItems/{id}</span><span class="sxs-lookup"><span data-stu-id="bdbde-131">PUT /api/TodoItems/{id}</span></span> | <span data-ttu-id="bdbde-132">기존 항목 업데이트 &nbsp;</span><span class="sxs-lookup"><span data-stu-id="bdbde-132">Update an existing item &nbsp;</span></span> | <span data-ttu-id="bdbde-133">할 일 항목</span><span class="sxs-lookup"><span data-stu-id="bdbde-133">To-do item</span></span> | <span data-ttu-id="bdbde-134">없음</span><span class="sxs-lookup"><span data-stu-id="bdbde-134">None</span></span> |
|<span data-ttu-id="bdbde-135">DELETE /api/TodoItems/{id} &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="bdbde-135">DELETE /api/TodoItems/{id} &nbsp; &nbsp;</span></span> | <span data-ttu-id="bdbde-136">항목 삭제 &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="bdbde-136">Delete an item &nbsp; &nbsp;</span></span> | <span data-ttu-id="bdbde-137">없음</span><span class="sxs-lookup"><span data-stu-id="bdbde-137">None</span></span> | <span data-ttu-id="bdbde-138">없음</span><span class="sxs-lookup"><span data-stu-id="bdbde-138">None</span></span>|

<span data-ttu-id="bdbde-139">다음 다이어그램에서는 앱의 디자인을 보여줍니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-139">The following diagram shows the design of the app.</span></span>

![클라이언트는 왼쪽에 상자로 표시됩니다.](first-web-api/_static/architecture.png)

## <a name="prerequisites"></a><span data-ttu-id="bdbde-145">사전 요구 사항</span><span class="sxs-lookup"><span data-stu-id="bdbde-145">Prerequisites</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="bdbde-146">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bdbde-146">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.1.md)]

# <a name="visual-studio-code"></a>[<span data-ttu-id="bdbde-147">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="bdbde-147">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.1.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="bdbde-148">Mac용 Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bdbde-148">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.1.md)]

---

## <a name="create-a-web-project"></a><span data-ttu-id="bdbde-149">웹 프로젝트 만들기</span><span class="sxs-lookup"><span data-stu-id="bdbde-149">Create a web project</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="bdbde-150">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bdbde-150">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="bdbde-151">**파일 메뉴**에서 **새로 만들기** > **프로젝트**를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-151">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="bdbde-152">**ASP.NET Core 웹 애플리케이션** 템플릿을 선택하고 **다음**을 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-152">Select the **ASP.NET Core Web Application** template and click **Next**.</span></span>
* <span data-ttu-id="bdbde-153">프로젝트 이름을 *TodoApi*로 지정하고 **만들기**를 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-153">Name the project *TodoApi* and click **Create**.</span></span>
* <span data-ttu-id="bdbde-154">**새 ASP.NET Core 웹 애플리케이션 만들기** 대화 상자에서 **.NET Core** 및 **ASP.NET Core 3.1**이 선택되었는지 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-154">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 3.1** are selected.</span></span> <span data-ttu-id="bdbde-155">**API** 템플릿을 선택하고 **만들기**를 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-155">Select the **API** template and click **Create**.</span></span>

![VS 새 프로젝트 대화 상자](first-web-api/_static/vs3.png)

# <a name="visual-studio-code"></a>[<span data-ttu-id="bdbde-157">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="bdbde-157">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="bdbde-158">[통합 터미널](https://code.visualstudio.com/docs/editor/integrated-terminal)을 엽니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-158">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="bdbde-159">디렉터리(`cd`)를 프로젝트 폴더를 포함하는 폴더로 변경합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-159">Change directories (`cd`) to the folder that will contain the project folder.</span></span>
* <span data-ttu-id="bdbde-160">다음 명령을 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-160">Run the following commands:</span></span>

   ```dotnetcli
   dotnet new webapi -o TodoApi
   cd TodoApi
   dotnet add package Microsoft.EntityFrameworkCore.SqlServer
   dotnet add package Microsoft.EntityFrameworkCore.InMemory
   code -r ../TodoApi
   ```

* <span data-ttu-id="bdbde-161">프로젝트에 필수 자산을 추가하려는지 묻는 대화 상자가 나타나면 **예**를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-161">When a dialog box asks if you want to add required assets to the project, select **Yes**.</span></span>

  <span data-ttu-id="bdbde-162">이전 명령은</span><span class="sxs-lookup"><span data-stu-id="bdbde-162">The preceding commands:</span></span>

  * <span data-ttu-id="bdbde-163">새 웹 API 프로젝트를 만들고 Visual Studio Code에서 엽니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-163">Creates a new web API project and opens it in Visual Studio Code.</span></span>
  * <span data-ttu-id="bdbde-164">다음 섹션에서 필요한 NuGet 패키지를 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-164">Adds the NuGet packages which are required in the next section.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="bdbde-165">Mac용 Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bdbde-165">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="bdbde-166">**파일** > **새 솔루션**을 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-166">Select **File** > **New Solution**.</span></span>

  ![macOS 새 솔루션](first-web-api-mac/_static/sln.png)

* <span data-ttu-id="bdbde-168">**.NET Core** > **앱** > **API** > **다음**을 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-168">Select **.NET Core** > **App** > **API** > **Next**.</span></span>

  ![macOS 새 프로젝트 대화 상자](first-web-api-mac/_static/1.png)
  
* <span data-ttu-id="bdbde-170">**새 ASP.NET Core Web API 구성** 대화 상자에서 \* *.NET Core 3.1*의 **대상 프레임워크**를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-170">In the **Configure your new ASP.NET Core Web API** dialog, select **Target Framework** of \**.NET Core 3.1*.</span></span>

* <span data-ttu-id="bdbde-171">**프로젝트 이름**으로 *TodoApi*를 입력한 다음, **만들기**를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-171">Enter *TodoApi* for the **Project Name** and then select **Create**.</span></span>

  ![구성 대화 상자](first-web-api-mac/_static/2.png)

[!INCLUDE[](~/includes/mac-terminal-access.md)]

<span data-ttu-id="bdbde-173">프로젝트 폴더에서 명령 터미널을 열고 다음 명령을 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-173">Open a command terminal in the project folder and run the following commands:</span></span>

   ```dotnetcli
   dotnet add package Microsoft.EntityFrameworkCore.SqlServer
   dotnet add package Microsoft.EntityFrameworkCore.InMemory
   ```

---

### <a name="test-the-api"></a><span data-ttu-id="bdbde-174">API 테스트</span><span class="sxs-lookup"><span data-stu-id="bdbde-174">Test the API</span></span>

<span data-ttu-id="bdbde-175">프로젝트 템플릿은 `WeatherForecast` API를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-175">The project template creates a `WeatherForecast` API.</span></span> <span data-ttu-id="bdbde-176">브라우저에서 `Get` 메서드를 호출하여 앱을 테스트합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-176">Call the `Get` method from a browser to test the app.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="bdbde-177">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bdbde-177">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="bdbde-178">Ctrl+F5 키를 눌러 앱을 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-178">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="bdbde-179">Visual Studio가 브라우저를 시작하고 `https://localhost:<port>/WeatherForecast`로 이동합니다. 여기서 `<port>`는 임의로 선택된 포트 번호입니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-179">Visual Studio launches a browser and navigates to `https://localhost:<port>/WeatherForecast`, where `<port>` is a randomly chosen port number.</span></span>

<span data-ttu-id="bdbde-180">IIS Express 인증서를 신뢰해야 하는지 묻는 대화 상자가 표시되면 **예**를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-180">If you get a dialog box that asks if you should trust the IIS Express certificate, select **Yes**.</span></span> <span data-ttu-id="bdbde-181">다음으로, **보안 경고** 대화 상자가 나타나면 **예**를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-181">In the **Security Warning** dialog that appears next, select **Yes**.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="bdbde-182">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="bdbde-182">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="bdbde-183">Ctrl+F5 키를 눌러 앱을 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-183">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="bdbde-184">브라우저에서 `https://localhost:5001/WeatherForecast` URL로 이동합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-184">In a browser, go to following URL: `https://localhost:5001/WeatherForecast`.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="bdbde-185">Mac용 Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bdbde-185">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="bdbde-186">**실행** > **디버깅 시작**을 선택하여 앱을 시작합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-186">Select **Run** > **Start Debugging** to launch the app.</span></span> <span data-ttu-id="bdbde-187">Mac용 Visual Studio가 브라우저를 시작하고 `https://localhost:<port>`로 이동합니다. 여기서 `<port>`는 임의로 선택된 포트 번호입니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-187">Visual Studio for Mac launches a browser and navigates to `https://localhost:<port>`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="bdbde-188">HTTP 404(찾을 수 없음) 오류가 반환됩니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-188">An HTTP 404 (Not Found) error is returned.</span></span> <span data-ttu-id="bdbde-189">`/WeatherForecast`를 URL에 추가합니다(URL을 `https://localhost:<port>/WeatherForecast`로 변경).</span><span class="sxs-lookup"><span data-stu-id="bdbde-189">Append `/WeatherForecast` to the URL (change the URL to `https://localhost:<port>/WeatherForecast`).</span></span>

---

<span data-ttu-id="bdbde-190">다음과 비슷한 JSON이 반환됩니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-190">JSON similar to the following is returned:</span></span>

```json
[
    {
        "date": "2019-07-16T19:04:05.7257911-06:00",
        "temperatureC": 52,
        "temperatureF": 125,
        "summary": "Mild"
    },
    {
        "date": "2019-07-17T19:04:05.7258461-06:00",
        "temperatureC": 36,
        "temperatureF": 96,
        "summary": "Warm"
    },
    {
        "date": "2019-07-18T19:04:05.7258467-06:00",
        "temperatureC": 39,
        "temperatureF": 102,
        "summary": "Cool"
    },
    {
        "date": "2019-07-19T19:04:05.7258471-06:00",
        "temperatureC": 10,
        "temperatureF": 49,
        "summary": "Bracing"
    },
    {
        "date": "2019-07-20T19:04:05.7258474-06:00",
        "temperatureC": -1,
        "temperatureF": 31,
        "summary": "Chilly"
    }
]
```

## <a name="add-a-model-class"></a><span data-ttu-id="bdbde-191">모델 클래스 추가</span><span class="sxs-lookup"><span data-stu-id="bdbde-191">Add a model class</span></span>

<span data-ttu-id="bdbde-192">*모델*은 앱에서 관리하는 데이터를 나타내는 일련의 클래스입니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-192">A *model* is a set of classes that represent the data that the app manages.</span></span> <span data-ttu-id="bdbde-193">이 앱에 대한 모델은 단일 `TodoItem` 클래스입니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-193">The model for this app is a single `TodoItem` class.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="bdbde-194">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bdbde-194">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="bdbde-195">**솔루션 탐색기**에서 프로젝트를 마우스 오른쪽 단추로 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-195">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="bdbde-196">**추가** > **새 폴더**를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-196">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="bdbde-197">폴더 이름을 *Models*로 지정합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-197">Name the folder *Models*.</span></span>

* <span data-ttu-id="bdbde-198">*Models* 폴더를 마우스 오른쪽 단추로 클릭하고 **추가** > **클래스**를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-198">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="bdbde-199">클래스 이름을 *TodoItem*으로 지정하고 **추가**를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-199">Name the class *TodoItem* and select **Add**.</span></span>

* <span data-ttu-id="bdbde-200">템플릿 코드를 다음 코드로 바꿉니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-200">Replace the template code with the following code:</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="bdbde-201">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="bdbde-201">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="bdbde-202">*Models* 폴더를 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-202">Add a folder named *Models*.</span></span>

* <span data-ttu-id="bdbde-203">다음 코드를 사용하여 *Models* 폴더에 `TodoItem` 클래스를 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-203">Add a `TodoItem` class to the *Models* folder with the following code:</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="bdbde-204">Mac용 Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bdbde-204">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="bdbde-205">프로젝트를 마우스 오른쪽 단추로 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-205">Right-click the project.</span></span> <span data-ttu-id="bdbde-206">**추가** > **새 폴더**를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-206">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="bdbde-207">폴더 이름을 *Models*로 지정합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-207">Name the folder *Models*.</span></span>

  ![새 폴더](first-web-api-mac/_static/folder.png)

* <span data-ttu-id="bdbde-209">*Models* 폴더를 마우스 오른쪽 단추로 클릭하고 **추가** > **새 파일** > **일반** > **빈 클래스**를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-209">Right-click the *Models* folder, and select **Add** > **New File** > **General** > **Empty Class**.</span></span>

* <span data-ttu-id="bdbde-210">클래스 이름을 *TodoItem*으로 지정한 다음, **새로 만들기**를 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-210">Name the class *TodoItem*, and then click **New**.</span></span>

* <span data-ttu-id="bdbde-211">템플릿 코드를 다음 코드로 바꿉니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-211">Replace the template code with the following code:</span></span>

---

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Models/TodoItem.cs?name=snippet)]

<span data-ttu-id="bdbde-212">`Id` 속성은 관계형 데이터베이스에서 고유 키로 작동합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-212">The `Id` property functions as the unique key in a relational database.</span></span>

<span data-ttu-id="bdbde-213">모델 클래스는 프로젝트의 어디로든 이동할 수 있지만 규칙에 따라 *Models* 폴더를 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-213">Model classes can go anywhere in the project, but the *Models* folder is used by convention.</span></span>

## <a name="add-a-database-context"></a><span data-ttu-id="bdbde-214">데이터베이스 컨텍스트 추가</span><span class="sxs-lookup"><span data-stu-id="bdbde-214">Add a database context</span></span>

<span data-ttu-id="bdbde-215">*데이터베이스 컨텍스트*는 데이터 모델에 맞게 Entity Framework 기능을 조정하는 주 클래스입니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-215">The *database context* is the main class that coordinates Entity Framework functionality for a data model.</span></span> <span data-ttu-id="bdbde-216">`Microsoft.EntityFrameworkCore.DbContext` 클래스에서 파생시키는 방식으로 이 클래스를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-216">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="bdbde-217">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bdbde-217">Visual Studio</span></span>](#tab/visual-studio)

### <a name="add-microsoftentityframeworkcoresqlserver"></a><span data-ttu-id="bdbde-218">Microsoft.EntityFrameworkCore.SqlServer 추가</span><span class="sxs-lookup"><span data-stu-id="bdbde-218">Add Microsoft.EntityFrameworkCore.SqlServer</span></span>

* <span data-ttu-id="bdbde-219">**도구** 메뉴에서 **NuGet 패키지 관리자 > 솔루션용 NuGet 패키지 관리**를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-219">From the **Tools** menu, select **NuGet Package Manager > Manage NuGet Packages for Solution**.</span></span>
* <span data-ttu-id="bdbde-220">**찾아보기** 탭을 선택한 다음 검색 상자에 **Microsoft.EntityFrameworkCore.SqlServer**를 입력합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-220">Select the **Browse** tab, and then enter **Microsoft.EntityFrameworkCore.SqlServer** in the search box.</span></span>
* <span data-ttu-id="bdbde-221">왼쪽 창에서 **Microsoft.EntityFrameworkCore.SqlServer**를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-221">Select **Microsoft.EntityFrameworkCore.SqlServer** in the left pane.</span></span>
* <span data-ttu-id="bdbde-222">오른쪽 창에서 **프로젝트** 확인란을 선택하고 **설치**를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-222">Select the **Project** check box in the right pane and then select **Install**.</span></span>
* <span data-ttu-id="bdbde-223">위 지침에 따라 `Microsoft.EntityFrameworkCore.InMemory` NuGet 패키지를 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-223">Use the preceding instructions to add the `Microsoft.EntityFrameworkCore.InMemory` NuGet package.</span></span>

![NuGet 패키지 관리자](first-web-api/_static/vs3NuGet.png)

## <a name="add-the-todocontext-database-context"></a><span data-ttu-id="bdbde-225">TodoContext 데이터베이스 컨텍스트 추가</span><span class="sxs-lookup"><span data-stu-id="bdbde-225">Add the TodoContext database context</span></span>

* <span data-ttu-id="bdbde-226">*Models* 폴더를 마우스 오른쪽 단추로 클릭하고 **추가** > **클래스**를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-226">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="bdbde-227">클래스 이름을 *TodoContext*로 지정하고 **추가**를 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-227">Name the class *TodoContext* and click **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mac"></a>[<span data-ttu-id="bdbde-228">Visual Studio Code / Mac용 Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bdbde-228">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="bdbde-229">`TodoContext` 클래스를 *Models* 폴더에 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-229">Add a `TodoContext` class to the *Models* folder.</span></span>

---

* <span data-ttu-id="bdbde-230">다음 코드를 입력합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-230">Enter the following code:</span></span>

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Models/TodoContext.cs)]

## <a name="register-the-database-context"></a><span data-ttu-id="bdbde-231">데이터베이스 컨텍스트 등록</span><span class="sxs-lookup"><span data-stu-id="bdbde-231">Register the database context</span></span>

<span data-ttu-id="bdbde-232">ASP.NET Core에서는 DB 컨텍스트와 같은 서비스를 [DI(종속성 주입)](xref:fundamentals/dependency-injection) 컨테이너에 등록해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-232">In ASP.NET Core, services such as the DB context must be registered with the [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="bdbde-233">컨테이너는 컨트롤러에 서비스를 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-233">The container provides the service to controllers.</span></span>

<span data-ttu-id="bdbde-234">*Startup.cs*를 다음 강조 표시된 코드로 업데이트합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-234">Update *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Startup.cs?highlight=7-8,23-24&name=snippet_all)]

<span data-ttu-id="bdbde-235">위의 코드는</span><span class="sxs-lookup"><span data-stu-id="bdbde-235">The preceding code:</span></span>

* <span data-ttu-id="bdbde-236">사용되지 않는 `using` 선언을 제거합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-236">Removes unused `using` declarations.</span></span>
* <span data-ttu-id="bdbde-237">DI 컨테이너에 데이터베이스 컨텍스트를 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-237">Adds the database context to the DI container.</span></span>
* <span data-ttu-id="bdbde-238">데이터베이스 컨텍스트가 메모리 내 데이터베이스를 사용하도록 지정합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-238">Specifies that the database context will use an in-memory database.</span></span>

## <a name="scaffold-a-controller"></a><span data-ttu-id="bdbde-239">컨트롤러 스캐폴드</span><span class="sxs-lookup"><span data-stu-id="bdbde-239">Scaffold a controller</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="bdbde-240">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bdbde-240">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="bdbde-241">*Controllers* 폴더를 마우스 오른쪽 단추로 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-241">Right-click the *Controllers* folder.</span></span>
* <span data-ttu-id="bdbde-242">**추가** > **스캐폴드 항목 새로 만들기**를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-242">Select **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="bdbde-243">**Entity Framework를 사용하며 동작이 포함된 API 컨트롤러**를 선택하고 **추가**를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-243">Select **API Controller with actions, using Entity Framework**, and then select **Add**.</span></span>
* <span data-ttu-id="bdbde-244">**Entity Framework를 사용하며 동작이 포함된 API 컨트롤러 추가** 대화 상자에서:</span><span class="sxs-lookup"><span data-stu-id="bdbde-244">In the **Add API Controller with actions, using Entity Framework** dialog:</span></span>

  * <span data-ttu-id="bdbde-245">**모델 클래스**에서 **TodoItem (TodoApi.Models)** 을 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-245">Select **TodoItem (TodoApi.Models)** in the **Model class**.</span></span>
  * <span data-ttu-id="bdbde-246">**데이터 컨텍스트 클래스**에서 **TodoContext (TodoApi.Models)** 를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-246">Select **TodoContext (TodoApi.Models)** in the **Data context class**.</span></span>
  * <span data-ttu-id="bdbde-247">**추가**를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-247">Select **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mac"></a>[<span data-ttu-id="bdbde-248">Visual Studio Code / Mac용 Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bdbde-248">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="bdbde-249">다음 명령을 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-249">Run the following commands:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design
dotnet tool install --global dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator controller -name TodoItemsController -async -api -m TodoItem -dc TodoContext -outDir Controllers
```

<span data-ttu-id="bdbde-250">이전 명령은</span><span class="sxs-lookup"><span data-stu-id="bdbde-250">The preceding commands:</span></span>

* <span data-ttu-id="bdbde-251">스캐폴딩에 필요한 NuGet 패키지를 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-251">Add NuGet packages required for scaffolding.</span></span>
* <span data-ttu-id="bdbde-252">스캐폴딩 엔진(`dotnet-aspnet-codegenerator`)을 설치합니다</span><span class="sxs-lookup"><span data-stu-id="bdbde-252">Installs the scaffolding engine (`dotnet-aspnet-codegenerator`).</span></span>
* <span data-ttu-id="bdbde-253">`TodoItemsController`를 스캐폴드합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-253">Scaffolds the `TodoItemsController`.</span></span>

---

<span data-ttu-id="bdbde-254">생성된 코드는:</span><span class="sxs-lookup"><span data-stu-id="bdbde-254">The generated code:</span></span>

* <span data-ttu-id="bdbde-255">[`[ApiController]`](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) 특성으로 클래스를 표시합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-255">Marks the class with the [`[ApiController]`](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute.</span></span> <span data-ttu-id="bdbde-256">이 특성은 컨트롤러가 웹 API 요청에 응답함을 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-256">This attribute indicates that the controller responds to web API requests.</span></span> <span data-ttu-id="bdbde-257">특성을 사용하도록 설정하는 특정 동작에 대한 정보는 <xref:web-api/index>를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="bdbde-257">For information about specific behaviors that the attribute enables, see <xref:web-api/index>.</span></span>
* <span data-ttu-id="bdbde-258">DI를 사용하여 데이터베이스 컨텍스트(`TodoContext`)를 컨트롤러에 삽입합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-258">Uses DI to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="bdbde-259">컨트롤러의 각 [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) 메서드에서 해당 데이터베이스 컨텍스트를 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-259">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>

<span data-ttu-id="bdbde-260">ASP.NET Core 템플릿과 관련해서 다음 사항을 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-260">The ASP.NET Core templates for:</span></span>

* <span data-ttu-id="bdbde-261">뷰가 있는 컨트롤러용 ASP.NET Core 템플릿의 경우, 경로 템플릿에 `[action]`이 포함됩니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-261">Controllers with views include `[action]` in the route template.</span></span>
* <span data-ttu-id="bdbde-262">API 컨트롤러용 ASP.NET Core 템플릿의 경우, 경로 템플릿에 `[action]`이 포함되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-262">API controllers don't include `[action]` in the route template.</span></span>

<span data-ttu-id="bdbde-263">`[action]` 토큰이 경로 템플릿에 없는 경우 경로에서 [작업](xref:mvc/controllers/routing#action) 이름이 제외됩니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-263">When the `[action]` token isn't in the route template, the [action](xref:mvc/controllers/routing#action) name is excluded from the route.</span></span> <span data-ttu-id="bdbde-264">즉, 일치하는 경로에서 작업과 연결된 메서드 이름이 사용되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-264">That is, the action's associated method name isn't used in the matching route.</span></span>

## <a name="examine-the-posttodoitem-create-method"></a><span data-ttu-id="bdbde-265">PostTodoItem 만들기 메서드 검사</span><span class="sxs-lookup"><span data-stu-id="bdbde-265">Examine the PostTodoItem create method</span></span>

<span data-ttu-id="bdbde-266">`PostTodoItem`의 return 문이 [nameof](/dotnet/csharp/language-reference/operators/nameof) 연산자를 사용하도록 바꿉니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-266">Replace the return statement in the `PostTodoItem` to use the [nameof](/dotnet/csharp/language-reference/operators/nameof) operator:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Create)]

<span data-ttu-id="bdbde-267">위의 코드는 [`[HttpPost]`](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) 특성으로 표시되는 HTTP POST 메서드입니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-267">The preceding code is an HTTP POST method, as indicated by the [`[HttpPost]`](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="bdbde-268">이 메서드는 HTTP 요청 본문에서 할 일 항목 값을 가져옵니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-268">The method gets the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="bdbde-269"><xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*> 메서드는 다음 작업을 수행합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-269">The <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*> method:</span></span>

* <span data-ttu-id="bdbde-270">성공 시 HTTP 201 상태 코드를 반환합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-270">Returns an HTTP 201 status code if successful.</span></span> <span data-ttu-id="bdbde-271">HTTP 201은 서버에서 새 리소스를 만드는 HTTP POST 메서드의 표준 응답입니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-271">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="bdbde-272">응답에 대한 [위치](https://developer.mozilla.org/docs/Web/HTTP/Headers/Location) 헤더를 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-272">Adds a [Location](https://developer.mozilla.org/docs/Web/HTTP/Headers/Location) header to the response.</span></span> <span data-ttu-id="bdbde-273">`Location` 헤더는 새로 만들어진 할 일 항목의 [URI](https://developer.mozilla.org/docs/Glossary/URI)를 지정합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-273">The `Location` header specifies the [URI](https://developer.mozilla.org/docs/Glossary/URI) of the newly created to-do item.</span></span> <span data-ttu-id="bdbde-274">자세한 내용은 [10.2.2 201 생성됨](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html)을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="bdbde-274">For more information, see [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="bdbde-275">`Location` 헤더의 URI를 만들려면 `GetTodoItem` 작업을 참조합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-275">References the `GetTodoItem` action to create the `Location` header's URI.</span></span> <span data-ttu-id="bdbde-276">C# `nameof` 키워드는 `CreatedAtAction` 호출에서 작업 이름의 하드 코딩을 방지하는 데 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-276">The C# `nameof` keyword is used to avoid hard-coding the action name in the `CreatedAtAction` call.</span></span>

### <a name="install-postman"></a><span data-ttu-id="bdbde-277">Postman을 설치합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-277">Install Postman</span></span>

<span data-ttu-id="bdbde-278">이 자습서에서는 Postman을 사용하여 웹 API를 테스트합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-278">This tutorial uses Postman to test the web API.</span></span>

* <span data-ttu-id="bdbde-279">[Postman](https://www.getpostman.com/downloads/)을 설치합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-279">Install [Postman](https://www.getpostman.com/downloads/)</span></span>
* <span data-ttu-id="bdbde-280">웹앱을 시작합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-280">Start the web app.</span></span>
* <span data-ttu-id="bdbde-281">Postman을 시작합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-281">Start Postman.</span></span>
* <span data-ttu-id="bdbde-282">**SSL 인증서 확인**을 사용하지 않도록 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-282">Disable **SSL certificate verification**</span></span>
  * <span data-ttu-id="bdbde-283">**파일** > **설정**(**일반** 탭)에서 **SSL 인증서 확인**을 사용하지 않도록 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-283">From **File** > **Settings** (**General** tab), disable **SSL certificate verification**.</span></span>
    > [!WARNING]
    > <span data-ttu-id="bdbde-284">컨트롤러를 테스트한 후에 SSL 인증서 확인을 다시 사용하도록 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-284">Re-enable SSL certificate verification after testing the controller.</span></span>

<a name="post"></a>

### <a name="test-posttodoitem-with-postman"></a><span data-ttu-id="bdbde-285">Postman을 사용하여 PostTodoItem 테스트</span><span class="sxs-lookup"><span data-stu-id="bdbde-285">Test PostTodoItem with Postman</span></span>

* <span data-ttu-id="bdbde-286">새 요청을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-286">Create a new request.</span></span>
* <span data-ttu-id="bdbde-287">HTTP 메서드를 `POST`로 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-287">Set the HTTP method to `POST`.</span></span>
* <span data-ttu-id="bdbde-288">**본문** 탭을 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-288">Select the **Body** tab.</span></span>
* <span data-ttu-id="bdbde-289">**원시** 라디오 단추를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-289">Select the **raw** radio button.</span></span>
* <span data-ttu-id="bdbde-290">유형을 **JSON(application/json)** 으로 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-290">Set the type to **JSON (application/json)**.</span></span>
* <span data-ttu-id="bdbde-291">요청 본문에서 할 일 항목에 대한 JSON을 입력합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-291">In the request body enter JSON for a to-do item:</span></span>

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

* <span data-ttu-id="bdbde-292">**보내기**를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-292">Select **Send**.</span></span>

  ![생성 요청이 있는 Postman](first-web-api/_static/3/create.png)

### <a name="test-the-location-header-uri"></a><span data-ttu-id="bdbde-294">위치 헤더 URI 테스트</span><span class="sxs-lookup"><span data-stu-id="bdbde-294">Test the location header URI</span></span>

* <span data-ttu-id="bdbde-295">**응답** 창에서 **헤더** 탭을 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-295">Select the **Headers** tab in the **Response** pane.</span></span>
* <span data-ttu-id="bdbde-296">**위치** 헤더 값을 복사합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-296">Copy the **Location** header value:</span></span>

  ![Postman 콘솔의 헤더 탭](first-web-api/_static/3/create.png)

* <span data-ttu-id="bdbde-298">메서드를 GET으로 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-298">Set the method to GET.</span></span>
* <span data-ttu-id="bdbde-299">URI(예: `https://localhost:5001/api/TodoItems/1`)를 붙여넣습니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-299">Paste the URI (for example, `https://localhost:5001/api/TodoItems/1`).</span></span>
* <span data-ttu-id="bdbde-300">**보내기**를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-300">Select **Send**.</span></span>

## <a name="examine-the-get-methods"></a><span data-ttu-id="bdbde-301">GET 메서드 검사</span><span class="sxs-lookup"><span data-stu-id="bdbde-301">Examine the GET methods</span></span>

<span data-ttu-id="bdbde-302">다음과 같은 메서드는 두 개의 GET 엔드포인트를 구현합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-302">These methods implement two GET endpoints:</span></span>

* `GET /api/TodoItems`
* `GET /api/TodoItems/{id}`

<span data-ttu-id="bdbde-303">브라우저 또는 Postman에서 두 개의 엔드포인트를 호출하여 앱을 테스트합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-303">Test the app by calling the two endpoints from a browser or Postman.</span></span> <span data-ttu-id="bdbde-304">예를 들어:</span><span class="sxs-lookup"><span data-stu-id="bdbde-304">For example:</span></span>

* `https://localhost:5001/api/TodoItems`
* `https://localhost:5001/api/TodoItems/1`

<span data-ttu-id="bdbde-305">`GetTodoItems`를 호출하면 다음과 비슷한 응답이 생성됩니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-305">A response similar to the following is produced by the call to `GetTodoItems`:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

### <a name="test-get-with-postman"></a><span data-ttu-id="bdbde-306">Postman을 사용하여 Get 테스트</span><span class="sxs-lookup"><span data-stu-id="bdbde-306">Test Get with Postman</span></span>

* <span data-ttu-id="bdbde-307">새 요청을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-307">Create a new request.</span></span>
* <span data-ttu-id="bdbde-308">HTTP 메서드를 **GET**으로 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-308">Set the HTTP method to **GET**.</span></span>
* <span data-ttu-id="bdbde-309">요청 URL을 `https://localhost:<port>/api/TodoItems`로 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-309">Set the request URL to `https://localhost:<port>/api/TodoItems`.</span></span> <span data-ttu-id="bdbde-310">예: `https://localhost:5001/api/TodoItems`.</span><span class="sxs-lookup"><span data-stu-id="bdbde-310">For example, `https://localhost:5001/api/TodoItems`.</span></span>
* <span data-ttu-id="bdbde-311">Postman에서 **두 개의 창 보기**를 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-311">Set **Two pane view** in Postman.</span></span>
* <span data-ttu-id="bdbde-312">**보내기**를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-312">Select **Send**.</span></span>

<span data-ttu-id="bdbde-313">이 앱은 메모리 내 데이터베이스를 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-313">This app uses an in-memory database.</span></span> <span data-ttu-id="bdbde-314">앱이 중지된 후 시작되면 이전 GET 요청이 데이터를 반환하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-314">If the app is stopped and started, the preceding GET request will not return any data.</span></span> <span data-ttu-id="bdbde-315">데이터가 반환되지 않으면 앱에 데이터를 [POST](#post)합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-315">If no data is returned, [POST](#post) data to the app.</span></span>

## <a name="routing-and-url-paths"></a><span data-ttu-id="bdbde-316">라우팅 및 URL 경로</span><span class="sxs-lookup"><span data-stu-id="bdbde-316">Routing and URL paths</span></span>

<span data-ttu-id="bdbde-317">[`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) 특성은 HTTP GET 요청에 응답하는 메서드를 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-317">The [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="bdbde-318">각 방법에 대한 URL 경로는 다음과 같이 구성됩니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-318">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="bdbde-319">컨트롤러의 `Route` 특성에서 템플릿 문자열로 시작합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-319">Start with the template string in the controller's `Route` attribute:</span></span>

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=TodoController&highlight=1)]

* <span data-ttu-id="bdbde-320">`[controller]`를 컨트롤러의 이름으로 바꿉니다. 일반적으로 컨트롤러 클래스 이름에서 "Controller" 접미사를 뺀 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-320">Replace `[controller]` with the name of the controller, which by convention is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="bdbde-321">이 샘플의 경우 컨트롤러 클래스 이름은 **TodoItems**Controller이므로 컨트롤러 이름은 “TodoItems”입니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-321">For this sample, the controller class name is **TodoItems**Controller, so the controller name is "TodoItems".</span></span> <span data-ttu-id="bdbde-322">ASP.NET Core [라우팅](xref:mvc/controllers/routing)은 대/소문자를 구분하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-322">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="bdbde-323">`[HttpGet]` 특성에 경로 템플릿(예: `[HttpGet("products")]`)이 있는 경우 경로에 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-323">If the `[HttpGet]` attribute has a route template (for example, `[HttpGet("products")]`), append that to the path.</span></span> <span data-ttu-id="bdbde-324">이 샘플은 템플릿을 사용하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-324">This sample doesn't use a template.</span></span> <span data-ttu-id="bdbde-325">자세한 내용은 [Http[동사] 특성을 사용한 특성 라우팅](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes)을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="bdbde-325">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="bdbde-326">다음 `GetTodoItem` 메서드에서 `"{id}"`는 할 일 항목의 고유 식별자에 대한 자리 표시자 변수입니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-326">In the following `GetTodoItem` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="bdbde-327">`GetTodoItem`이 호출되면 URL의 `"{id}"` 값을 `id` 매개 변수의 메서드에 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-327">When `GetTodoItem` is invoked, the value of `"{id}"` in the URL is provided to the method in its `id` parameter.</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_GetByID&highlight=1-2)]

## <a name="return-values"></a><span data-ttu-id="bdbde-328">반환 값</span><span class="sxs-lookup"><span data-stu-id="bdbde-328">Return values</span></span>

<span data-ttu-id="bdbde-329">`GetTodoItems` 및 `GetTodoItem` 메서드의 반환 형식은 [ActionResult\<T> 형식](xref:web-api/action-return-types#actionresultt-type)입니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-329">The return type of the `GetTodoItems` and `GetTodoItem` methods is [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type).</span></span> <span data-ttu-id="bdbde-330">ASP.NET Core는 자동으로 [JSON](https://www.json.org/)에 개체를 직렬화하고 JSON을 응답 메시지의 본문에 기록합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-330">ASP.NET Core automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="bdbde-331">이 반환 형식의 응답 코드는 200이며 처리되지 않은 예외가 없다고 가정합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-331">The response code for this return type is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="bdbde-332">처리되지 않은 예외는 5xx 오류로 변환됩니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-332">Unhandled exceptions are translated into 5xx errors.</span></span>

<span data-ttu-id="bdbde-333">`ActionResult` 반환 형식은 다양한 HTTP 상태 코드를 나타낼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-333">`ActionResult` return types can represent a wide range of HTTP status codes.</span></span> <span data-ttu-id="bdbde-334">예를 들어 `GetTodoItem`은 두 가지 상태 값을 반환할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-334">For example, `GetTodoItem` can return two different status values:</span></span>

* <span data-ttu-id="bdbde-335">요청된 ID와 일치하는 항목이 없는 경우 메서드가 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) 오류 코드를 반환합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-335">If no item matches the requested ID, the method returns a 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) error code.</span></span>
* <span data-ttu-id="bdbde-336">그렇지 않으면 메서드가 JSON 응답 본문에서 200을 반환합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-336">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="bdbde-337">`item`을 반환하면 HTTP 200 응답이 발생합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-337">Returning `item` results in an HTTP 200 response.</span></span>

## <a name="the-puttodoitem-method"></a><span data-ttu-id="bdbde-338">PutTodoItem 메서드</span><span class="sxs-lookup"><span data-stu-id="bdbde-338">The PutTodoItem method</span></span>

<span data-ttu-id="bdbde-339">다음과 같이 `PutTodoItem` 메서드를 검사합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-339">Examine the `PutTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Update)]

<span data-ttu-id="bdbde-340">HTTP PUT을 사용하는 것을 제외하고 `PutTodoItem`는 `PostTodoItem`와 비슷합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-340">`PutTodoItem` is similar to `PostTodoItem`, except it uses HTTP PUT.</span></span> <span data-ttu-id="bdbde-341">응답은 [204(콘텐츠 없음)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html)입니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-341">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="bdbde-342">HTTP 사양에 따라 PUT 요청의 경우 클라이언트는 변경 내용만이 아니라 전체 업데이트된 엔터티를 보내야 합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-342">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the changes.</span></span> <span data-ttu-id="bdbde-343">부분 업데이트를 지원하려면 [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute)를 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-343">To support partial updates, use [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span></span>

<span data-ttu-id="bdbde-344">`PutTodoItem`을 호출하는 중 오류가 발생하면 `GET`을 호출하여 데이터베이스에 항목이 있는지 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-344">If you get an error calling `PutTodoItem`, call `GET` to ensure there's an item in the database.</span></span>

### <a name="test-the-puttodoitem-method"></a><span data-ttu-id="bdbde-345">PutTodoItem 메서드 테스트</span><span class="sxs-lookup"><span data-stu-id="bdbde-345">Test the PutTodoItem method</span></span>

<span data-ttu-id="bdbde-346">이 샘플은 앱이 시작될 때마다 초기화되어야 하는 메모리 내 데이터베이스를 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-346">This sample uses an in-memory database that must be initialized each time the app is started.</span></span> <span data-ttu-id="bdbde-347">PUT 호출을 실행하기 전에 데이터베이스에 항목이 있어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-347">There must be an item in the database before you make a PUT call.</span></span> <span data-ttu-id="bdbde-348">GET을 호출하여 PUT 호출을 실행하기 전에 데이터베이스에 항목이 있는지 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-348">Call GET to insure there's an item in the database before making a PUT call.</span></span>

<span data-ttu-id="bdbde-349">ID = 1인 할 일 항목을 업데이트하고 해당 이름을 “feed fish”로 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-349">Update the to-do item that has ID = 1 and set its name to "feed fish":</span></span>

```json
  {
    "ID":1,
    "name":"feed fish",
    "isComplete":true
  }
```

<span data-ttu-id="bdbde-350">다음 이미지는 Postman 업데이트를 보여줍니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-350">The following image shows the Postman update:</span></span>

![204(콘텐츠 없음) 응답을 보여주는 Postman 콘솔](first-web-api/_static/3/pmcput.png)

## <a name="the-deletetodoitem-method"></a><span data-ttu-id="bdbde-352">DeleteTodoItem 메서드</span><span class="sxs-lookup"><span data-stu-id="bdbde-352">The DeleteTodoItem method</span></span>

<span data-ttu-id="bdbde-353">다음과 같이 `DeleteTodoItem` 메서드를 검사합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-353">Examine the `DeleteTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Delete)]

### <a name="test-the-deletetodoitem-method"></a><span data-ttu-id="bdbde-354">DeleteTodoItem 메서드 테스트</span><span class="sxs-lookup"><span data-stu-id="bdbde-354">Test the DeleteTodoItem method</span></span>

<span data-ttu-id="bdbde-355">Postman을 사용하여 할 일 항목을 삭제합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-355">Use Postman to delete a to-do item:</span></span>

* <span data-ttu-id="bdbde-356">메서드를 `DELETE`로 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-356">Set the method to `DELETE`.</span></span>
* <span data-ttu-id="bdbde-357">예를 들어 삭제할 개체의 URI를 `https://localhost:5001/api/TodoItems/1`로 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-357">Set the URI of the object to delete (for example `https://localhost:5001/api/TodoItems/1`).</span></span>
* <span data-ttu-id="bdbde-358">**보내기**를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-358">Select **Send**.</span></span>

<a name="over-post"></a>

## <a name="prevent-over-posting"></a><span data-ttu-id="bdbde-359">과도한 게시 방지</span><span class="sxs-lookup"><span data-stu-id="bdbde-359">Prevent over-posting</span></span>

<span data-ttu-id="bdbde-360">현재 샘플 앱은 전체 `TodoItem` 개체를 공개합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-360">Currently the sample app exposes the entire `TodoItem` object.</span></span> <span data-ttu-id="bdbde-361">일반적으로 프로덕션 앱은 모델의 하위 집합을 사용하여 입력 및 반환되는 데이터를 제한합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-361">Productions apps typically limit the data that's input and returned using a subset of the model.</span></span> <span data-ttu-id="bdbde-362">이 동작에는 여러 가지 이유가 있으며, 보안이 주요 이유 중 하나입니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-362">There are multiple reasons behind this and security is a major one.</span></span> <span data-ttu-id="bdbde-363">일반적으로 모델의 하위 집합을 DTO(데이터 전송 개체), 입력 모델 또는 뷰 모델이라고 합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-363">The subset of a model is usually referred to as a Data Transfer Object (DTO), input model, or view model.</span></span> <span data-ttu-id="bdbde-364">이 문서에서는 **DTO**를 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-364">**DTO** is used in this article.</span></span>

<span data-ttu-id="bdbde-365">DTO는 다음과 같은 용도로 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-365">A DTO may be used to:</span></span>

* <span data-ttu-id="bdbde-366">과도한 게시를 방지합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-366">Prevent over-posting.</span></span>
* <span data-ttu-id="bdbde-367">클라이언트에 표시되지 않아야 하는 속성을 숨깁니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-367">Hide properties that clients are not supposed to view.</span></span>
* <span data-ttu-id="bdbde-368">페이로드 크기를 줄이기 위해 일부 속성을 생략합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-368">Omit some properties in order to reduce payload size.</span></span>
* <span data-ttu-id="bdbde-369">중첩된 개체를 포함하는 개체 그래프를 평면화합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-369">Flatten object graphs that contain nested objects.</span></span> <span data-ttu-id="bdbde-370">클라이언트에는 평면화된 개체 그래프가 더 편리할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-370">Flattened object graphs can be more convenient for clients.</span></span>

<span data-ttu-id="bdbde-371">DTO 방법을 설명하려면 비밀 필드를 포함하도록 `TodoItem` 클래스를 업데이트합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-371">To demonstrate the DTO approach, update the `TodoItem` class to include a secret field:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApiDTO/Models/TodoItem.cs?name=snippet&highlight=6)]

<span data-ttu-id="bdbde-372">이 앱에서는 숨겨진 필드를 숨겨야 하지만, 관리 앱은 숨겨진 필드를 공개할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-372">The secret field needs to be hidden from this app, but an administrative app could choose to expose it.</span></span>

<span data-ttu-id="bdbde-373">비밀 필드를 게시하고 가져올 수 있는지 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-373">Verify you can post and get the secret field.</span></span>

<span data-ttu-id="bdbde-374">DTO 모델을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-374">Create a DTO model:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApiDTO/Models/TodoItemDTO.cs?name=snippet)]

<span data-ttu-id="bdbde-375">`TodoItemDTO`를 사용하도록 `TodoItemsController`를 업데이트합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-375">Update the `TodoItemsController` to use `TodoItemDTO`:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApiDTO/Controllers/TodoItemsController.cs?name=snippet)]

<span data-ttu-id="bdbde-376">비밀 필드를 게시하거나 가져올 수 없음을 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-376">Verify you can't post or get the secret field.</span></span>

## <a name="call-the-web-api-with-javascript"></a><span data-ttu-id="bdbde-377">JavaScript를 사용하여 웹 API 호출</span><span class="sxs-lookup"><span data-stu-id="bdbde-377">Call the web API with JavaScript</span></span>

<span data-ttu-id="bdbde-378">[자습서: JavaScript로 ASP.NET Core 웹 API 호출하기](xref:tutorials/web-api-javascript)를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="bdbde-378">See [Tutorial: Call an ASP.NET Core web API with JavaScript](xref:tutorials/web-api-javascript).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="bdbde-379">이 자습서에서는 다음과 같은 작업을 수행하는 방법을 살펴봅니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-379">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="bdbde-380">웹 API 프로젝트를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-380">Create a web API project.</span></span>
> * <span data-ttu-id="bdbde-381">모델 클래스와 데이터베이스 컨텍스트를 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-381">Add a model class and a database context.</span></span>
> * <span data-ttu-id="bdbde-382">컨트롤러 추가</span><span class="sxs-lookup"><span data-stu-id="bdbde-382">Add a controller.</span></span>
> * <span data-ttu-id="bdbde-383">CRUD 메서드 추가</span><span class="sxs-lookup"><span data-stu-id="bdbde-383">Add CRUD methods.</span></span>
> * <span data-ttu-id="bdbde-384">라우팅 및 URL 경로 구성</span><span class="sxs-lookup"><span data-stu-id="bdbde-384">Configure routing and URL paths.</span></span>
> * <span data-ttu-id="bdbde-385">반환 값 지정</span><span class="sxs-lookup"><span data-stu-id="bdbde-385">Specify return values.</span></span>
> * <span data-ttu-id="bdbde-386">Postman을 사용하여 웹 API를 호출합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-386">Call the web API with Postman.</span></span>
> * <span data-ttu-id="bdbde-387">JavaScript를 사용하여 웹 API를 호출합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-387">Call the web API with JavaScript.</span></span>

<span data-ttu-id="bdbde-388">작업을 완료하면 웹 API는 관계형 데이터베이스에 저장된 "할 일" 항목을 관리할 수 있게 됩니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-388">At the end, you have a web API that can manage "to-do" items stored in a relational database.</span></span>

## <a name="overview"></a><span data-ttu-id="bdbde-389">개요</span><span class="sxs-lookup"><span data-stu-id="bdbde-389">Overview</span></span>

<span data-ttu-id="bdbde-390">이 자습서에서는 다음 API를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-390">This tutorial creates the following API:</span></span>

|<span data-ttu-id="bdbde-391">API</span><span class="sxs-lookup"><span data-stu-id="bdbde-391">API</span></span> | <span data-ttu-id="bdbde-392">설명</span><span class="sxs-lookup"><span data-stu-id="bdbde-392">Description</span></span> | <span data-ttu-id="bdbde-393">요청 본문</span><span class="sxs-lookup"><span data-stu-id="bdbde-393">Request body</span></span> | <span data-ttu-id="bdbde-394">응답 본문</span><span class="sxs-lookup"><span data-stu-id="bdbde-394">Response body</span></span> |
|--- | ---- | ---- | ---- |
|<span data-ttu-id="bdbde-395">GET /api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="bdbde-395">GET /api/TodoItems</span></span> | <span data-ttu-id="bdbde-396">할 일 항목 모두 가져오기</span><span class="sxs-lookup"><span data-stu-id="bdbde-396">Get all to-do items</span></span> | <span data-ttu-id="bdbde-397">없음</span><span class="sxs-lookup"><span data-stu-id="bdbde-397">None</span></span> | <span data-ttu-id="bdbde-398">할 일 항목의 배열</span><span class="sxs-lookup"><span data-stu-id="bdbde-398">Array of to-do items</span></span>|
|<span data-ttu-id="bdbde-399">GET /api/TodoItems/{id}</span><span class="sxs-lookup"><span data-stu-id="bdbde-399">GET /api/TodoItems/{id}</span></span> | <span data-ttu-id="bdbde-400">ID로 항목 가져오기</span><span class="sxs-lookup"><span data-stu-id="bdbde-400">Get an item by ID</span></span> | <span data-ttu-id="bdbde-401">없음</span><span class="sxs-lookup"><span data-stu-id="bdbde-401">None</span></span> | <span data-ttu-id="bdbde-402">할 일 항목</span><span class="sxs-lookup"><span data-stu-id="bdbde-402">To-do item</span></span>|
|<span data-ttu-id="bdbde-403">POST /api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="bdbde-403">POST /api/TodoItems</span></span> | <span data-ttu-id="bdbde-404">새 항목 추가</span><span class="sxs-lookup"><span data-stu-id="bdbde-404">Add a new item</span></span> | <span data-ttu-id="bdbde-405">할 일 항목</span><span class="sxs-lookup"><span data-stu-id="bdbde-405">To-do item</span></span> | <span data-ttu-id="bdbde-406">할 일 항목</span><span class="sxs-lookup"><span data-stu-id="bdbde-406">To-do item</span></span> |
|<span data-ttu-id="bdbde-407">PUT /api/TodoItems/{id}</span><span class="sxs-lookup"><span data-stu-id="bdbde-407">PUT /api/TodoItems/{id}</span></span> | <span data-ttu-id="bdbde-408">기존 항목 업데이트 &nbsp;</span><span class="sxs-lookup"><span data-stu-id="bdbde-408">Update an existing item &nbsp;</span></span> | <span data-ttu-id="bdbde-409">할 일 항목</span><span class="sxs-lookup"><span data-stu-id="bdbde-409">To-do item</span></span> | <span data-ttu-id="bdbde-410">없음</span><span class="sxs-lookup"><span data-stu-id="bdbde-410">None</span></span> |
|<span data-ttu-id="bdbde-411">DELETE /api/TodoItems/{id} &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="bdbde-411">DELETE /api/TodoItems/{id} &nbsp; &nbsp;</span></span> | <span data-ttu-id="bdbde-412">항목 삭제 &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="bdbde-412">Delete an item &nbsp; &nbsp;</span></span> | <span data-ttu-id="bdbde-413">없음</span><span class="sxs-lookup"><span data-stu-id="bdbde-413">None</span></span> | <span data-ttu-id="bdbde-414">없음</span><span class="sxs-lookup"><span data-stu-id="bdbde-414">None</span></span>|

<span data-ttu-id="bdbde-415">다음 다이어그램에서는 앱의 디자인을 보여줍니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-415">The following diagram shows the design of the app.</span></span>

![클라이언트는 왼쪽에 상자로 표시됩니다.](first-web-api/_static/architecture.png)

## <a name="prerequisites"></a><span data-ttu-id="bdbde-421">사전 요구 사항</span><span class="sxs-lookup"><span data-stu-id="bdbde-421">Prerequisites</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="bdbde-422">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bdbde-422">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-code"></a>[<span data-ttu-id="bdbde-423">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="bdbde-423">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="bdbde-424">Mac용 Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bdbde-424">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

## <a name="create-a-web-project"></a><span data-ttu-id="bdbde-425">웹 프로젝트 만들기</span><span class="sxs-lookup"><span data-stu-id="bdbde-425">Create a web project</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="bdbde-426">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bdbde-426">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="bdbde-427">**파일 메뉴**에서 **새로 만들기** > **프로젝트**를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-427">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="bdbde-428">**ASP.NET Core 웹 애플리케이션** 템플릿을 선택하고 **다음**을 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-428">Select the **ASP.NET Core Web Application** template and click **Next**.</span></span>
* <span data-ttu-id="bdbde-429">프로젝트 이름을 *TodoApi*로 지정하고 **만들기**를 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-429">Name the project *TodoApi* and click **Create**.</span></span>
* <span data-ttu-id="bdbde-430">**새 ASP.NET Core 웹 애플리케이션 만들기** 대화 상자에서 **.NET Core** 및 **ASP.NET Core 2.2**가 선택되었는지 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-430">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 2.2** are selected.</span></span> <span data-ttu-id="bdbde-431">**API** 템플릿을 선택하고 **만들기**를 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-431">Select the **API** template and click **Create**.</span></span> <span data-ttu-id="bdbde-432">**Docker 지원 사용**을 선택하지 **마세요**.</span><span class="sxs-lookup"><span data-stu-id="bdbde-432">**Don't** select **Enable Docker Support**.</span></span>

![VS 새 프로젝트 대화 상자](first-web-api/_static/vs.png)

# <a name="visual-studio-code"></a>[<span data-ttu-id="bdbde-434">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="bdbde-434">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="bdbde-435">[통합 터미널](https://code.visualstudio.com/docs/editor/integrated-terminal)을 엽니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-435">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="bdbde-436">디렉터리(`cd`)를 프로젝트 폴더를 포함하는 폴더로 변경합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-436">Change directories (`cd`) to the folder that will contain the project folder.</span></span>
* <span data-ttu-id="bdbde-437">다음 명령을 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-437">Run the following commands:</span></span>

   ```dotnetcli
   dotnet new webapi -o TodoApi
   code -r TodoApi
   ```

  <span data-ttu-id="bdbde-438">이러한 명령은 새 웹 API 프로젝트를 만들고 새 프로젝트 폴더에서 Visual Studio Code의 새 인스턴스를 엽니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-438">These commands create a new web API project and open a new instance of Visual Studio Code in the new project folder.</span></span>

* <span data-ttu-id="bdbde-439">프로젝트에 필수 자산을 추가하려는지 묻는 대화 상자가 나타나면 **예**를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-439">When a dialog box asks if you want to add required assets to the project, select **Yes**.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="bdbde-440">Mac용 Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bdbde-440">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="bdbde-441">**파일** > **새 솔루션**을 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-441">Select **File** > **New Solution**.</span></span>

  ![macOS 새 솔루션](first-web-api-mac/_static/sln.png)

* <span data-ttu-id="bdbde-443">**.NET Core** > **앱** > **API** > **다음**을 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-443">Select **.NET Core** > **App** > **API** > **Next**.</span></span>

  ![macOS 새 프로젝트 대화 상자](first-web-api-mac/_static/1.png)
  
* <span data-ttu-id="bdbde-445">**새 ASP.NET Core Web API 구성** 대화 상자에서 \* *.NET Core 2.2*라는 기본 **대상 프레임워크**를 수락합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-445">In the **Configure your new ASP.NET Core Web API** dialog, accept the default **Target Framework** of \**.NET Core 2.2*.</span></span>

* <span data-ttu-id="bdbde-446">**프로젝트 이름**으로 *TodoApi*를 입력한 다음, **만들기**를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-446">Enter *TodoApi* for the **Project Name** and then select **Create**.</span></span>

  ![구성 대화 상자](first-web-api-mac/_static/2.png)

---

### <a name="test-the-api"></a><span data-ttu-id="bdbde-448">API 테스트</span><span class="sxs-lookup"><span data-stu-id="bdbde-448">Test the API</span></span>

<span data-ttu-id="bdbde-449">프로젝트 템플릿은 `values` API를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-449">The project template creates a `values` API.</span></span> <span data-ttu-id="bdbde-450">브라우저에서 `Get` 메서드를 호출하여 앱을 테스트합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-450">Call the `Get` method from a browser to test the app.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="bdbde-451">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bdbde-451">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="bdbde-452">Ctrl+F5 키를 눌러 앱을 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-452">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="bdbde-453">Visual Studio가 브라우저를 시작하고 `https://localhost:<port>/api/values`로 이동합니다. 여기서 `<port>`는 임의로 선택된 포트 번호입니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-453">Visual Studio launches a browser and navigates to `https://localhost:<port>/api/values`, where `<port>` is a randomly chosen port number.</span></span>

<span data-ttu-id="bdbde-454">IIS Express 인증서를 신뢰해야 하는지 묻는 대화 상자가 표시되면 **예**를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-454">If you get a dialog box that asks if you should trust the IIS Express certificate, select **Yes**.</span></span> <span data-ttu-id="bdbde-455">다음으로, **보안 경고** 대화 상자가 나타나면 **예**를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-455">In the **Security Warning** dialog that appears next, select **Yes**.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="bdbde-456">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="bdbde-456">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="bdbde-457">Ctrl+F5 키를 눌러 앱을 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-457">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="bdbde-458">브라우저에서 `https://localhost:5001/api/values` URL로 이동합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-458">In a browser, go to following URL: `https://localhost:5001/api/values`.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="bdbde-459">Mac용 Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bdbde-459">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="bdbde-460">**실행** > **디버깅 시작**을 선택하여 앱을 시작합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-460">Select **Run** > **Start Debugging** to launch the app.</span></span> <span data-ttu-id="bdbde-461">Mac용 Visual Studio가 브라우저를 시작하고 `https://localhost:<port>`로 이동합니다. 여기서 `<port>`는 임의로 선택된 포트 번호입니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-461">Visual Studio for Mac launches a browser and navigates to `https://localhost:<port>`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="bdbde-462">HTTP 404(찾을 수 없음) 오류가 반환됩니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-462">An HTTP 404 (Not Found) error is returned.</span></span> <span data-ttu-id="bdbde-463">`/api/values`를 URL에 추가합니다(URL을 `https://localhost:<port>/api/values`로 변경).</span><span class="sxs-lookup"><span data-stu-id="bdbde-463">Append `/api/values` to the URL (change the URL to `https://localhost:<port>/api/values`).</span></span>

---

<span data-ttu-id="bdbde-464">다음 JSON이 반환됩니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-464">The following JSON is returned:</span></span>

```json
["value1","value2"]
```

## <a name="add-a-model-class"></a><span data-ttu-id="bdbde-465">모델 클래스 추가</span><span class="sxs-lookup"><span data-stu-id="bdbde-465">Add a model class</span></span>

<span data-ttu-id="bdbde-466">*모델*은 앱에서 관리하는 데이터를 나타내는 일련의 클래스입니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-466">A *model* is a set of classes that represent the data that the app manages.</span></span> <span data-ttu-id="bdbde-467">이 앱에 대한 모델은 단일 `TodoItem` 클래스입니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-467">The model for this app is a single `TodoItem` class.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="bdbde-468">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bdbde-468">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="bdbde-469">**솔루션 탐색기**에서 프로젝트를 마우스 오른쪽 단추로 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-469">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="bdbde-470">**추가** > **새 폴더**를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-470">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="bdbde-471">폴더 이름을 *Models*로 지정합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-471">Name the folder *Models*.</span></span>

* <span data-ttu-id="bdbde-472">*Models* 폴더를 마우스 오른쪽 단추로 클릭하고 **추가** > **클래스**를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-472">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="bdbde-473">클래스 이름을 *TodoItem*으로 지정하고 **추가**를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-473">Name the class *TodoItem* and select **Add**.</span></span>

* <span data-ttu-id="bdbde-474">템플릿 코드를 다음 코드로 바꿉니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-474">Replace the template code with the following code:</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="bdbde-475">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="bdbde-475">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="bdbde-476">*Models* 폴더를 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-476">Add a folder named *Models*.</span></span>

* <span data-ttu-id="bdbde-477">다음 코드를 사용하여 *Models* 폴더에 `TodoItem` 클래스를 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-477">Add a `TodoItem` class to the *Models* folder with the following code:</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="bdbde-478">Mac용 Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bdbde-478">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="bdbde-479">프로젝트를 마우스 오른쪽 단추로 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-479">Right-click the project.</span></span> <span data-ttu-id="bdbde-480">**추가** > **새 폴더**를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-480">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="bdbde-481">폴더 이름을 *Models*로 지정합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-481">Name the folder *Models*.</span></span>

  ![새 폴더](first-web-api-mac/_static/folder.png)

* <span data-ttu-id="bdbde-483">*Models* 폴더를 마우스 오른쪽 단추로 클릭하고 **추가** > **새 파일** > **일반** > **빈 클래스**를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-483">Right-click the *Models* folder, and select **Add** > **New File** > **General** > **Empty Class**.</span></span>

* <span data-ttu-id="bdbde-484">클래스 이름을 *TodoItem*으로 지정한 다음, **새로 만들기**를 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-484">Name the class *TodoItem*, and then click **New**.</span></span>

* <span data-ttu-id="bdbde-485">템플릿 코드를 다음 코드로 바꿉니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-485">Replace the template code with the following code:</span></span>

---

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="bdbde-486">`Id` 속성은 관계형 데이터베이스에서 고유 키로 작동합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-486">The `Id` property functions as the unique key in a relational database.</span></span>

<span data-ttu-id="bdbde-487">모델 클래스는 프로젝트의 어디로든 이동할 수 있지만 규칙에 따라 *Models* 폴더를 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-487">Model classes can go anywhere in the project, but the *Models* folder is used by convention.</span></span>

## <a name="add-a-database-context"></a><span data-ttu-id="bdbde-488">데이터베이스 컨텍스트 추가</span><span class="sxs-lookup"><span data-stu-id="bdbde-488">Add a database context</span></span>

<span data-ttu-id="bdbde-489">*데이터베이스 컨텍스트*는 데이터 모델에 맞게 Entity Framework 기능을 조정하는 주 클래스입니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-489">The *database context* is the main class that coordinates Entity Framework functionality for a data model.</span></span> <span data-ttu-id="bdbde-490">`Microsoft.EntityFrameworkCore.DbContext` 클래스에서 파생시키는 방식으로 이 클래스를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-490">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="bdbde-491">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bdbde-491">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="bdbde-492">*Models* 폴더를 마우스 오른쪽 단추로 클릭하고 **추가** > **클래스**를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-492">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="bdbde-493">클래스 이름을 *TodoContext*로 지정하고 **추가**를 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-493">Name the class *TodoContext* and click **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mac"></a>[<span data-ttu-id="bdbde-494">Visual Studio Code / Mac용 Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bdbde-494">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="bdbde-495">`TodoContext` 클래스를 *Models* 폴더에 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-495">Add a `TodoContext` class to the *Models* folder.</span></span>

---

* <span data-ttu-id="bdbde-496">템플릿 코드를 다음 코드로 바꿉니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-496">Replace the template code with the following code:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoContext.cs)]

## <a name="register-the-database-context"></a><span data-ttu-id="bdbde-497">데이터베이스 컨텍스트 등록</span><span class="sxs-lookup"><span data-stu-id="bdbde-497">Register the database context</span></span>

<span data-ttu-id="bdbde-498">ASP.NET Core에서는 DB 컨텍스트와 같은 서비스를 [DI(종속성 주입)](xref:fundamentals/dependency-injection) 컨테이너에 등록해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-498">In ASP.NET Core, services such as the DB context must be registered with the [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="bdbde-499">컨테이너는 컨트롤러에 서비스를 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-499">The container provides the service to controllers.</span></span>

<span data-ttu-id="bdbde-500">*Startup.cs*를 다음 강조 표시된 코드로 업데이트합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-500">Update *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup1.cs?highlight=5,8,25-26&name=snippet_all)]

<span data-ttu-id="bdbde-501">위의 코드는</span><span class="sxs-lookup"><span data-stu-id="bdbde-501">The preceding code:</span></span>

* <span data-ttu-id="bdbde-502">사용되지 않는 `using` 선언을 제거합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-502">Removes unused `using` declarations.</span></span>
* <span data-ttu-id="bdbde-503">DI 컨테이너에 데이터베이스 컨텍스트를 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-503">Adds the database context to the DI container.</span></span>
* <span data-ttu-id="bdbde-504">데이터베이스 컨텍스트가 메모리 내 데이터베이스를 사용하도록 지정합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-504">Specifies that the database context will use an in-memory database.</span></span>

## <a name="add-a-controller"></a><span data-ttu-id="bdbde-505">컨트롤러 추가</span><span class="sxs-lookup"><span data-stu-id="bdbde-505">Add a controller</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="bdbde-506">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bdbde-506">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="bdbde-507">*Controllers* 폴더를 마우스 오른쪽 단추로 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-507">Right-click the *Controllers* folder.</span></span>
* <span data-ttu-id="bdbde-508">**추가** > **새 항목**을 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-508">Select **Add** > **New Item**.</span></span>
* <span data-ttu-id="bdbde-509">**새 항목 추가** 대화 상자에서 **API 컨트롤러 클래스** 템플릿을 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-509">In the **Add New Item** dialog, select the **API Controller Class** template.</span></span>
* <span data-ttu-id="bdbde-510">클래스 이름을 *TodoController*로 지정하고 **추가**를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-510">Name the class *TodoController*, and select **Add**.</span></span>

  ![검색 상자의 컨트롤러 및 웹 API 컨트롤러가 선택된 새 항목 추가 대화 상자](first-web-api/_static/new_controller.png)

# <a name="visual-studio-code--visual-studio-for-mac"></a>[<span data-ttu-id="bdbde-512">Visual Studio Code / Mac용 Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bdbde-512">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="bdbde-513">*Controllers* 폴더에 `TodoController`라는 클래스를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-513">In the *Controllers* folder, create a class named `TodoController`.</span></span>

---

* <span data-ttu-id="bdbde-514">템플릿 코드를 다음 코드로 바꿉니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-514">Replace the template code with the following code:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

<span data-ttu-id="bdbde-515">위의 코드는</span><span class="sxs-lookup"><span data-stu-id="bdbde-515">The preceding code:</span></span>

* <span data-ttu-id="bdbde-516">메서드 없이 API 컨트롤러 클래스를 정의합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-516">Defines an API controller class without methods.</span></span>
* <span data-ttu-id="bdbde-517">[`[ApiController]`](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) 특성으로 클래스를 표시합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-517">Marks the class with the [`[ApiController]`](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute.</span></span> <span data-ttu-id="bdbde-518">이 특성은 컨트롤러가 웹 API 요청에 응답함을 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-518">This attribute indicates that the controller responds to web API requests.</span></span> <span data-ttu-id="bdbde-519">특성을 사용하도록 설정하는 특정 동작에 대한 정보는 <xref:web-api/index>를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="bdbde-519">For information about specific behaviors that the attribute enables, see <xref:web-api/index>.</span></span>
* <span data-ttu-id="bdbde-520">DI를 사용하여 데이터베이스 컨텍스트(`TodoContext`)를 컨트롤러에 삽입합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-520">Uses DI to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="bdbde-521">컨트롤러의 각 [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) 메서드에서 해당 데이터베이스 컨텍스트를 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-521">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>
* <span data-ttu-id="bdbde-522">데이터베이스가 비어 있는 경우 데이터베이스에 `Item1`이라는 항목을 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-522">Adds an item named `Item1` to the database if the database is empty.</span></span> <span data-ttu-id="bdbde-523">이 코드는 생성자에 위치하므로 새 HTTP 요청이 발생할 때마다 실행됩니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-523">This code is in the constructor, so it runs every time there's a new HTTP request.</span></span> <span data-ttu-id="bdbde-524">모든 항목을 삭제하면 생성자는 다음에 API가 호출될 경우 `Item1`을 다시 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-524">If you delete all items, the constructor creates `Item1` again the next time an API method is called.</span></span> <span data-ttu-id="bdbde-525">따라서 실제로 작동되는 경우 삭제가 작동하지 않는 것처럼 보일 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-525">So it may look like the deletion didn't work when it actually did work.</span></span>

## <a name="add-get-methods"></a><span data-ttu-id="bdbde-526">GET 메서드 추가</span><span class="sxs-lookup"><span data-stu-id="bdbde-526">Add Get methods</span></span>

<span data-ttu-id="bdbde-527">할 일 항목을 가져오는 API를 제공하려면 다음 메서드를 `TodoController` 클래스에 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-527">To provide an API that retrieves to-do items, add the following methods to the `TodoController` class:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]

<span data-ttu-id="bdbde-528">다음과 같은 메서드는 두 개의 GET 엔드포인트를 구현합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-528">These methods implement two GET endpoints:</span></span>

* `GET /api/todo`
* `GET /api/todo/{id}`

<span data-ttu-id="bdbde-529">앱이 계속 실행되고 있으면 중지합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-529">Stop the app if it's still running.</span></span> <span data-ttu-id="bdbde-530">그런 다음, 다시 실행하여 최신 변경 내용을 포함합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-530">Then run it again to include the latest changes.</span></span>

<span data-ttu-id="bdbde-531">브라우저에서 두 개의 엔드포인트를 호출하여 앱을 테스트합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-531">Test the app by calling the two endpoints from a browser.</span></span> <span data-ttu-id="bdbde-532">예를 들어:</span><span class="sxs-lookup"><span data-stu-id="bdbde-532">For example:</span></span>

* `https://localhost:<port>/api/todo`
* `https://localhost:<port>/api/todo/1`

<span data-ttu-id="bdbde-533">`GetTodoItems`를 호출하여 다음 HTTP 응답이 생성됩니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-533">The following HTTP response is produced by the call to `GetTodoItems`:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

## <a name="routing-and-url-paths"></a><span data-ttu-id="bdbde-534">라우팅 및 URL 경로</span><span class="sxs-lookup"><span data-stu-id="bdbde-534">Routing and URL paths</span></span>

<span data-ttu-id="bdbde-535">[`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) 특성은 HTTP GET 요청에 응답하는 메서드를 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-535">The [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="bdbde-536">각 방법에 대한 URL 경로는 다음과 같이 구성됩니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-536">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="bdbde-537">컨트롤러의 `Route` 특성에서 템플릿 문자열로 시작합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-537">Start with the template string in the controller's `Route` attribute:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]

* <span data-ttu-id="bdbde-538">`[controller]`를 컨트롤러의 이름으로 바꿉니다. 일반적으로 컨트롤러 클래스 이름에서 "Controller" 접미사를 뺀 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-538">Replace `[controller]` with the name of the controller, which by convention is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="bdbde-539">이 샘플의 경우 컨트롤러 클래스 이름은 **Todo**Controller이므로 컨트롤러 이름은 "todo"입니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-539">For this sample, the controller class name is **Todo**Controller, so the controller name is "todo".</span></span> <span data-ttu-id="bdbde-540">ASP.NET Core [라우팅](xref:mvc/controllers/routing)은 대/소문자를 구분하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-540">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="bdbde-541">`[HttpGet]` 특성에 경로 템플릿(예: `[HttpGet("products")]`)이 있는 경우 경로에 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-541">If the `[HttpGet]` attribute has a route template (for example, `[HttpGet("products")]`), append that to the path.</span></span> <span data-ttu-id="bdbde-542">이 샘플은 템플릿을 사용하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-542">This sample doesn't use a template.</span></span> <span data-ttu-id="bdbde-543">자세한 내용은 [Http[동사] 특성을 사용한 특성 라우팅](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes)을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="bdbde-543">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="bdbde-544">다음 `GetTodoItem` 메서드에서 `"{id}"`는 할 일 항목의 고유 식별자에 대한 자리 표시자 변수입니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-544">In the following `GetTodoItem` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="bdbde-545">`GetTodoItem`가 호출되면 URL의 `"{id}"` 값을 `id` 매개 변수의 메서드에 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-545">When `GetTodoItem` is invoked, the value of `"{id}"` in the URL is provided to the method in its`id` parameter.</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

## <a name="return-values"></a><span data-ttu-id="bdbde-546">반환 값</span><span class="sxs-lookup"><span data-stu-id="bdbde-546">Return values</span></span>

<span data-ttu-id="bdbde-547">`GetTodoItems` 및 `GetTodoItem` 메서드의 반환 형식은 [ActionResult\<T> 형식](xref:web-api/action-return-types#actionresultt-type)입니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-547">The return type of the `GetTodoItems` and `GetTodoItem` methods is [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type).</span></span> <span data-ttu-id="bdbde-548">ASP.NET Core는 자동으로 [JSON](https://www.json.org/)에 개체를 직렬화하고 JSON을 응답 메시지의 본문에 기록합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-548">ASP.NET Core automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="bdbde-549">이 반환 형식의 응답 코드는 200이며 처리되지 않은 예외가 없다고 가정합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-549">The response code for this return type is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="bdbde-550">처리되지 않은 예외는 5xx 오류로 변환됩니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-550">Unhandled exceptions are translated into 5xx errors.</span></span>

<span data-ttu-id="bdbde-551">`ActionResult` 반환 형식은 다양한 HTTP 상태 코드를 나타낼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-551">`ActionResult` return types can represent a wide range of HTTP status codes.</span></span> <span data-ttu-id="bdbde-552">예를 들어 `GetTodoItem`은 두 가지 상태 값을 반환할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-552">For example, `GetTodoItem` can return two different status values:</span></span>

* <span data-ttu-id="bdbde-553">요청된 ID와 일치하는 항목이 없는 경우 메서드가 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) 오류 코드를 반환합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-553">If no item matches the requested ID, the method returns a 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) error code.</span></span>
* <span data-ttu-id="bdbde-554">그렇지 않으면 메서드가 JSON 응답 본문에서 200을 반환합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-554">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="bdbde-555">`item`을 반환하면 HTTP 200 응답이 발생합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-555">Returning `item` results in an HTTP 200 response.</span></span>

## <a name="test-the-gettodoitems-method"></a><span data-ttu-id="bdbde-556">GetTodoItems 메서드 테스트</span><span class="sxs-lookup"><span data-stu-id="bdbde-556">Test the GetTodoItems method</span></span>

<span data-ttu-id="bdbde-557">이 자습서에서는 Postman을 사용하여 웹 API를 테스트합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-557">This tutorial uses Postman to test the web API.</span></span>

* <span data-ttu-id="bdbde-558">[Postman](https://www.getpostman.com/downloads/)을 설치합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-558">Install [Postman](https://www.getpostman.com/downloads/).</span></span>
* <span data-ttu-id="bdbde-559">웹앱을 시작합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-559">Start the web app.</span></span>
* <span data-ttu-id="bdbde-560">Postman을 시작합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-560">Start Postman.</span></span>
* <span data-ttu-id="bdbde-561">**SSL 인증서 확인**을 사용하지 않도록 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-561">Disable **SSL certificate verification**.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="bdbde-562">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bdbde-562">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="bdbde-563">**파일** > **설정**(**일반** 탭)에서 **SSL 인증서 확인**을 사용하지 않도록 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-563">From **File** > **Settings** (**General** tab), disable **SSL certificate verification**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mac"></a>[<span data-ttu-id="bdbde-564">Visual Studio Code / Mac용 Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bdbde-564">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="bdbde-565">**Postman** > **기본 설정**(**일반** 탭)에서 **SSL 인증서 확인**을 사용하지 않게 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-565">From **Postman** > **Preferences** (**General** tab), disable **SSL certificate verification**.</span></span> <span data-ttu-id="bdbde-566">또는 렌치를 선택하고 **설정**을 선택한 다음 SSL 인증서 확인을 사용하지 않게 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-566">Alternatively, select the wrench and select **Settings**, then disable the SSL certificate verification.</span></span>

---
  
> [!WARNING]
> <span data-ttu-id="bdbde-567">컨트롤러를 테스트한 후에 SSL 인증서 확인을 다시 사용하도록 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-567">Re-enable SSL certificate verification after testing the controller.</span></span>

* <span data-ttu-id="bdbde-568">새 요청을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-568">Create a new request.</span></span>
  * <span data-ttu-id="bdbde-569">HTTP 메서드를 **GET**으로 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-569">Set the HTTP method to **GET**.</span></span>
  * <span data-ttu-id="bdbde-570">요청 URL을 `https://localhost:<port>/api/todo`로 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-570">Set the request URL to `https://localhost:<port>/api/todo`.</span></span> <span data-ttu-id="bdbde-571">예: `https://localhost:5001/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="bdbde-571">For example, `https://localhost:5001/api/todo`.</span></span>
* <span data-ttu-id="bdbde-572">Postman에서 **두 개의 창 보기**를 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-572">Set **Two pane view** in Postman.</span></span>
* <span data-ttu-id="bdbde-573">**보내기**를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-573">Select **Send**.</span></span>

![Get 요청이 있는 Postman](first-web-api/_static/2pv.png)

## <a name="add-a-create-method"></a><span data-ttu-id="bdbde-575">메서드 만들기 추가</span><span class="sxs-lookup"><span data-stu-id="bdbde-575">Add a Create method</span></span>

<span data-ttu-id="bdbde-576">*Controllers/TodoController.cs* 내부에 다음 `PostTodoItem` 메서드를 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-576">Add the following `PostTodoItem` method inside of *Controllers/TodoController.cs*:</span></span> 

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="bdbde-577">위의 코드는 [`[HttpPost]`](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) 특성으로 표시되는 HTTP POST 메서드입니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-577">The preceding code is an HTTP POST method, as indicated by the [`[HttpPost]`](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="bdbde-578">이 메서드는 HTTP 요청 본문에서 할 일 항목 값을 가져옵니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-578">The method gets the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="bdbde-579">`CreatedAtAction` 메서드는 다음 작업을 수행합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-579">The `CreatedAtAction` method:</span></span>

* <span data-ttu-id="bdbde-580">성공 시 HTTP 201 상태 코드를 반환합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-580">Returns an HTTP 201 status code, if successful.</span></span> <span data-ttu-id="bdbde-581">HTTP 201은 서버에서 새 리소스를 만드는 HTTP POST 메서드의 표준 응답입니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-581">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="bdbde-582">`Location` 헤더를 응답에 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-582">Adds a `Location` header to the response.</span></span> <span data-ttu-id="bdbde-583">`Location` 헤더는 새로 만들어진 할 일 항목의 URI를 지정합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-583">The `Location` header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="bdbde-584">자세한 내용은 [10.2.2 201 생성됨](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html)을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="bdbde-584">For more information, see [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="bdbde-585">`Location` 헤더의 URI를 만들려면 `GetTodoItem` 작업을 참조합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-585">References the `GetTodoItem` action to create the `Location` header's URI.</span></span> <span data-ttu-id="bdbde-586">C# `nameof` 키워드는 `CreatedAtAction` 호출에서 작업 이름의 하드 코딩을 방지하는 데 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-586">The C# `nameof` keyword is used to avoid hard-coding the action name in the `CreatedAtAction` call.</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

### <a name="test-the-posttodoitem-method"></a><span data-ttu-id="bdbde-587">PostTodoItem 메서드 테스트</span><span class="sxs-lookup"><span data-stu-id="bdbde-587">Test the PostTodoItem method</span></span>

* <span data-ttu-id="bdbde-588">프로젝트를 빌드합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-588">Build the project.</span></span>
* <span data-ttu-id="bdbde-589">Postman에서 HTTP 메서드를 `POST`로 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-589">In Postman, set the HTTP method to `POST`.</span></span>
* <span data-ttu-id="bdbde-590">**본문** 탭을 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-590">Select the **Body** tab.</span></span>
* <span data-ttu-id="bdbde-591">**원시** 라디오 단추를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-591">Select the **raw** radio button.</span></span>
* <span data-ttu-id="bdbde-592">유형을 **JSON(application/json)** 으로 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-592">Set the type to **JSON (application/json)**.</span></span>
* <span data-ttu-id="bdbde-593">요청 본문에서 할 일 항목에 대한 JSON을 입력합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-593">In the request body enter JSON for a to-do item:</span></span>

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

* <span data-ttu-id="bdbde-594">**보내기**를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-594">Select **Send**.</span></span>

  ![생성 요청이 있는 Postman](first-web-api/_static/create.png)

  <span data-ttu-id="bdbde-596">405 메서드가 허용되지 않음 오류가 발생할 경우 `PostTodoItem` 메서드를 추가한 후에 프로젝트를 컴파일하지 않아서 발생한 결과일 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-596">If you get a 405 Method Not Allowed error, it's probably the result of not compiling the project after adding the `PostTodoItem` method.</span></span>

### <a name="test-the-location-header-uri"></a><span data-ttu-id="bdbde-597">위치 헤더 URI 테스트</span><span class="sxs-lookup"><span data-stu-id="bdbde-597">Test the location header URI</span></span>

* <span data-ttu-id="bdbde-598">**응답** 창에서 **헤더** 탭을 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-598">Select the **Headers** tab in the **Response** pane.</span></span>
* <span data-ttu-id="bdbde-599">**위치** 헤더 값을 복사합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-599">Copy the **Location** header value:</span></span>

  ![Postman 콘솔의 헤더 탭](first-web-api/_static/pmc2.png)

* <span data-ttu-id="bdbde-601">메서드를 GET으로 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-601">Set the method to GET.</span></span>
* <span data-ttu-id="bdbde-602">URI(예: `https://localhost:5001/api/Todo/2`)를 붙여넣습니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-602">Paste the URI (for example, `https://localhost:5001/api/Todo/2`).</span></span>
* <span data-ttu-id="bdbde-603">**보내기**를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-603">Select **Send**.</span></span>

## <a name="add-a-puttodoitem-method"></a><span data-ttu-id="bdbde-604">PutTodoItem 메서드 추가</span><span class="sxs-lookup"><span data-stu-id="bdbde-604">Add a PutTodoItem method</span></span>

<span data-ttu-id="bdbde-605">다음 `PutTodoItem` 메서드를 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-605">Add the following `PutTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

<span data-ttu-id="bdbde-606">HTTP PUT을 사용하는 것을 제외하고 `PutTodoItem`는 `PostTodoItem`와 비슷합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-606">`PutTodoItem` is similar to `PostTodoItem`, except it uses HTTP PUT.</span></span> <span data-ttu-id="bdbde-607">응답은 [204(콘텐츠 없음)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html)입니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-607">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="bdbde-608">HTTP 사양에 따라 PUT 요청의 경우 클라이언트는 변경 내용만이 아니라 전체 업데이트된 엔터티를 보내야 합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-608">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the changes.</span></span> <span data-ttu-id="bdbde-609">부분 업데이트를 지원하려면 [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute)를 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-609">To support partial updates, use [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span></span>

<span data-ttu-id="bdbde-610">`PutTodoItem`을 호출하는 중 오류가 발생하면 `GET`을 호출하여 데이터베이스에 항목이 있는지 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-610">If you get an error calling `PutTodoItem`, call `GET` to ensure there's an item in the database.</span></span>

### <a name="test-the-puttodoitem-method"></a><span data-ttu-id="bdbde-611">PutTodoItem 메서드 테스트</span><span class="sxs-lookup"><span data-stu-id="bdbde-611">Test the PutTodoItem method</span></span>

<span data-ttu-id="bdbde-612">이 샘플은 앱이 시작될 때마다 초기화되어야 하는 메모리 내 데이터베이스를 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-612">This sample uses an in-memory database that must be initialized each time the app is started.</span></span> <span data-ttu-id="bdbde-613">PUT 호출을 실행하기 전에 데이터베이스에 항목이 있어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-613">There must be an item in the database before you make a PUT call.</span></span> <span data-ttu-id="bdbde-614">GET을 호출하여 PUT 호출을 실행하기 전에 데이터베이스에 항목이 있는지 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-614">Call GET to insure there's an item in the database before making a PUT call.</span></span>

<span data-ttu-id="bdbde-615">ID = 1인 할 일 항목을 업데이트하고 해당 이름을 "feed fish"로 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-615">Update the to-do item that has id = 1 and set its name to "feed fish":</span></span>

```json
  {
    "ID":1,
    "name":"feed fish",
    "isComplete":true
  }
```

<span data-ttu-id="bdbde-616">다음 이미지는 Postman 업데이트를 보여줍니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-616">The following image shows the Postman update:</span></span>

![204(콘텐츠 없음) 응답을 보여주는 Postman 콘솔](first-web-api/_static/pmcput.png)

## <a name="add-a-deletetodoitem-method"></a><span data-ttu-id="bdbde-618">DeleteTodoItem 메서드 추가</span><span class="sxs-lookup"><span data-stu-id="bdbde-618">Add a DeleteTodoItem method</span></span>

<span data-ttu-id="bdbde-619">다음 `DeleteTodoItem` 메서드를 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-619">Add the following `DeleteTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="bdbde-620">`DeleteTodoItem` 응답은 [204(콘텐츠 없음)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html)입니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-620">The `DeleteTodoItem` response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

### <a name="test-the-deletetodoitem-method"></a><span data-ttu-id="bdbde-621">DeleteTodoItem 메서드 테스트</span><span class="sxs-lookup"><span data-stu-id="bdbde-621">Test the DeleteTodoItem method</span></span>

<span data-ttu-id="bdbde-622">Postman을 사용하여 할 일 항목을 삭제합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-622">Use Postman to delete a to-do item:</span></span>

* <span data-ttu-id="bdbde-623">메서드를 `DELETE`로 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-623">Set the method to `DELETE`.</span></span>
* <span data-ttu-id="bdbde-624">삭제할 개체의 URI를 설정합니다(예: `https://localhost:5001/api/todo/1`).</span><span class="sxs-lookup"><span data-stu-id="bdbde-624">Set the URI of the object to delete (for example, `https://localhost:5001/api/todo/1`).</span></span>
* <span data-ttu-id="bdbde-625">**보내기**를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-625">Select **Send**.</span></span>

<span data-ttu-id="bdbde-626">샘플 앱을 사용하면 모든 항목을 삭제할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-626">The sample app allows you to delete all the items.</span></span> <span data-ttu-id="bdbde-627">하지만 마지막 항목이 삭제되면 다음에 API를 호출하는 경우 모델 클래스 생성자에서 새로운 항목이 생성됩니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-627">However, when the last item is deleted, a new one is created by the model class constructor the next time the API is called.</span></span>

## <a name="call-the-web-api-with-javascript"></a><span data-ttu-id="bdbde-628">JavaScript를 사용하여 웹 API 호출</span><span class="sxs-lookup"><span data-stu-id="bdbde-628">Call the web API with JavaScript</span></span>

<span data-ttu-id="bdbde-629">이 섹션에는 JavaScript를 사용하여 웹 API를 호출하는 HTML 페이지가 추가되었습니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-629">In this section, an HTML page is added that uses JavaScript to call the web API.</span></span> <span data-ttu-id="bdbde-630">jQuery가 요청을 시작합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-630">jQuery initiates the request.</span></span> <span data-ttu-id="bdbde-631">JavaScript는 웹 API 응답의 세부 정보를 토대로 페이지를 업데이트합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-631">JavaScript updates the page with the details from the web API's response.</span></span>

<span data-ttu-id="bdbde-632">다음 강조 표시된 코드로 *Startup.cs*를 업데이트하여 앱이 [정적 파일을 제공](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_)하고 [기본 파일 매핑을 사용](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_)하도록 구성합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-632">Configure the app to [serve static files](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [enable default file mapping](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) by updating *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup.cs?highlight=14-15&name=snippet_configure)]

<span data-ttu-id="bdbde-633">프로젝트 디렉터리에서 *wwwroot* 폴더를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-633">Create a *wwwroot* folder in the project directory.</span></span>

<span data-ttu-id="bdbde-634">*index.html*이라는 HTML 파일을 *wwwroot* 디렉터리에 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-634">Add an HTML file named *index.html* to the *wwwroot* directory.</span></span> <span data-ttu-id="bdbde-635">다음 표시로 콘텐츠를 바꿉니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-635">Replace its contents with the following markup:</span></span>

[!code-html[](first-web-api/samples/2.2/TodoApi/wwwroot/index.html)]

<span data-ttu-id="bdbde-636">*site.js*라는 JavaScript 파일을 *wwwroot* 디렉터리에 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-636">Add a JavaScript file named *site.js* to the *wwwroot* directory.</span></span> <span data-ttu-id="bdbde-637">다음 코드로 콘텐츠를 바꿉니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-637">Replace its contents with the following code:</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_SiteJs)]

<span data-ttu-id="bdbde-638">HTML 페이지를 로컬에서 테스트하려면 ASP.NET Core 프로젝트의 시작 설정을 변경해야 할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-638">A change to the ASP.NET Core project's launch settings may be required to test the HTML page locally:</span></span>

* <span data-ttu-id="bdbde-639">*Properties\launchSettings.json*을 엽니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-639">Open *Properties\launchSettings.json*.</span></span>
* <span data-ttu-id="bdbde-640">`launchUrl` 속성을 제거하여 앱이 *index.html*&mdash; 프로젝트의 기본 파일에서 열리도록 합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-640">Remove the `launchUrl` property to force the app to open at *index.html*&mdash;the project's default file.</span></span>

<span data-ttu-id="bdbde-641">이 샘플은 웹 API의 CRUD 메서드를 모두 호출합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-641">This sample calls all of the CRUD methods of the web API.</span></span> <span data-ttu-id="bdbde-642">API 호출에 대한 설명은 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-642">Following are explanations of the calls to the API.</span></span>

### <a name="get-a-list-of-to-do-items"></a><span data-ttu-id="bdbde-643">할 일 항목의 목록 가져오기</span><span class="sxs-lookup"><span data-stu-id="bdbde-643">Get a list of to-do items</span></span>

<span data-ttu-id="bdbde-644">jQuery는 할 일 항목의 배열을 나타내는 JSON을 반환하는 웹 API에 HTTP GET 요청을 보냅니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-644">jQuery sends an HTTP GET request to the web API, which returns JSON representing an array of to-do items.</span></span> <span data-ttu-id="bdbde-645">요청이 성공하면 `success` 콜백 함수가 호출됩니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-645">The `success` callback function is invoked if the request succeeds.</span></span> <span data-ttu-id="bdbde-646">콜백에서 DOM은 할 일 정보로 업데이트됩니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-646">In the callback, the DOM is updated with the to-do information.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_GetData)]

### <a name="add-a-to-do-item"></a><span data-ttu-id="bdbde-647">할 일 항목 추가</span><span class="sxs-lookup"><span data-stu-id="bdbde-647">Add a to-do item</span></span>

<span data-ttu-id="bdbde-648">jQuery는 요청 본문에 있는 할 일 항목을 사용하여 HTTP POST 요청을 보냅니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-648">jQuery sends an HTTP POST request with the to-do item in the request body.</span></span> <span data-ttu-id="bdbde-649">`accepts` 및 `contentType` 옵션은 수신 및 전송되는 미디어 형식을 지정하기 위해 `application/json`으로 설정됩니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-649">The `accepts` and `contentType` options are set to `application/json` to specify the media type being received and sent.</span></span> <span data-ttu-id="bdbde-650">[JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify)를 사용하여 할 일 항목을 JSON으로 변환합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-650">The to-do item is converted to JSON by using [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify).</span></span> <span data-ttu-id="bdbde-651">API가 성공적인 상태 코드를 반환하면 `getData` 함수가 호출되어 HTML 테이블을 업데이트합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-651">When the API returns a successful status code, the `getData` function is invoked to update the HTML table.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AddItem)]

### <a name="update-a-to-do-item"></a><span data-ttu-id="bdbde-652">할 일 항목 업데이트</span><span class="sxs-lookup"><span data-stu-id="bdbde-652">Update a to-do item</span></span>

<span data-ttu-id="bdbde-653">할 일 항목을 업데이트하는 작업은 추가하는 작업과 비슷합니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-653">Updating a to-do item is similar to adding one.</span></span> <span data-ttu-id="bdbde-654">`url`은 항목의 고유 식별자를 추가하도록 변경되고 `type`은 `PUT`입니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-654">The `url` changes to add the unique identifier of the item, and the `type` is `PUT`.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxPut)]

### <a name="delete-a-to-do-item"></a><span data-ttu-id="bdbde-655">할 일 항목 삭제</span><span class="sxs-lookup"><span data-stu-id="bdbde-655">Delete a to-do item</span></span>

<span data-ttu-id="bdbde-656">할 일 항목을 삭제하려면 AJAX 호출에서 `type`을 `DELETE`로 설정하고 URL에서 항목의 고유 식별자를 지정하면 됩니다.</span><span class="sxs-lookup"><span data-stu-id="bdbde-656">Deleting a to-do item is accomplished by setting the `type` on the AJAX call to `DELETE` and specifying the item's unique identifier in the URL.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxDelete)]

::: moniker-end

<a name="auth"></a>

## <a name="add-authentication-support-to-a-web-api"></a><span data-ttu-id="bdbde-657">웹 API에 인증 지원 추가</span><span class="sxs-lookup"><span data-stu-id="bdbde-657">Add authentication support to a web API</span></span>

[!INCLUDE[](~/includes/IdentityServer4.md)]

## <a name="additional-resources"></a><span data-ttu-id="bdbde-658">추가 자료</span><span class="sxs-lookup"><span data-stu-id="bdbde-658">Additional resources</span></span>

<span data-ttu-id="bdbde-659">[이 자습서에서 샘플 코드 보기 또는 다운로드](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-web-api/samples)</span><span class="sxs-lookup"><span data-stu-id="bdbde-659">[View or download sample code for this tutorial](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-web-api/samples).</span></span> <span data-ttu-id="bdbde-660">[다운로드하는 방법](xref:index#how-to-download-a-sample)을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="bdbde-660">See [how to download](xref:index#how-to-download-a-sample).</span></span>

<span data-ttu-id="bdbde-661">자세한 내용은 다음 자료를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="bdbde-661">For more information, see the following resources:</span></span>

* <xref:web-api/index>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:data/ef-rp/intro>
* <xref:mvc/controllers/routing>
* <xref:web-api/action-return-types>
* <xref:host-and-deploy/azure-apps/index>
* <xref:host-and-deploy/index>
* [<span data-ttu-id="bdbde-662">이 자습서의 YouTube 버전</span><span class="sxs-lookup"><span data-stu-id="bdbde-662">YouTube version of this tutorial</span></span>](https://www.youtube.com/watch?v=TTkhEyGBfAk)
