---
title: ASP.NET Core 및 MongoDB를 사용하여 웹 API 빌드
author: prkhandelwal
description: 이 자습서에서는 MongoDB NoSQL 데이터베이스를 사용하여 ASP.NET Core 웹 API를 빌드하는 방법을 보여 줍니다.
ms.author: scaddie
ms.custom: mvc, seodec18
ms.date: 01/23/2019
uid: tutorials/first-mongo-app
ms.openlocfilehash: 6375ae618816671bd9c64f038603747c64cdce56
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/24/2019
ms.locfileid: "54835598"
---
# <a name="create-a-web-api-with-aspnet-core-and-mongodb"></a><span data-ttu-id="4978d-103">ASP.NET Core 및 MongoDB를 사용하여 웹 API 만들기</span><span class="sxs-lookup"><span data-stu-id="4978d-103">Create a web API with ASP.NET Core and MongoDB</span></span>

<span data-ttu-id="4978d-104">작성자: [Pratik Khandelwal](https://twitter.com/K2Prk) 및 [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="4978d-104">By [Pratik Khandelwal](https://twitter.com/K2Prk) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="4978d-105">이 자습서에서는 [MongoDB](https://www.mongodb.com/what-is-mongodb) NoSQL 데이터베이스에 대해 CRUD(만들기, 읽기, 업데이트 및 삭제) 작업을 수행하는 웹 API를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="4978d-105">This tutorial creates a web API that performs Create, Read, Update, and Delete (CRUD) operations on a [MongoDB](https://www.mongodb.com/what-is-mongodb) NoSQL database.</span></span>

<span data-ttu-id="4978d-106">이 자습서에서는 다음과 같은 작업을 수행하는 방법을 살펴봅니다.</span><span class="sxs-lookup"><span data-stu-id="4978d-106">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="4978d-107">MongoDB 구성</span><span class="sxs-lookup"><span data-stu-id="4978d-107">Configure MongoDB</span></span>
> * <span data-ttu-id="4978d-108">MongoDB 데이터베이스 만들기</span><span class="sxs-lookup"><span data-stu-id="4978d-108">Create a MongoDB database</span></span>
> * <span data-ttu-id="4978d-109">MongoDB 컬렉션 및 스키마 정의</span><span class="sxs-lookup"><span data-stu-id="4978d-109">Define a MongoDB collection and schema</span></span>
> * <span data-ttu-id="4978d-110">웹 API에서 MongoDB CRUD 작업 수행</span><span class="sxs-lookup"><span data-stu-id="4978d-110">Perform MongoDB CRUD operations from a web API</span></span>

<span data-ttu-id="4978d-111">[예제 코드 살펴보기 및 다운로드](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-mongo-app/sample) ([다운로드 방법](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="4978d-111">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-mongo-app/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4978d-112">전제 조건</span><span class="sxs-lookup"><span data-stu-id="4978d-112">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4978d-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4978d-113">Visual Studio</span></span>](#tab/visual-studio)

* [<span data-ttu-id="4978d-114">.NET Core SDK 2.2 이상</span><span class="sxs-lookup"><span data-stu-id="4978d-114">.NET Core SDK 2.2 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="4978d-115">[Visual Studio 2017 버전 15.9 이상](https://www.visualstudio.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)(**ASP.NET 및 웹 개발** 워크로드 필요)</span><span class="sxs-lookup"><span data-stu-id="4978d-115">[Visual Studio 2017 version 15.9 or later](https://www.visualstudio.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="4978d-116">MongoDB</span><span class="sxs-lookup"><span data-stu-id="4978d-116">MongoDB</span></span>](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-windows/)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="4978d-117">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="4978d-117">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="4978d-118">.NET Core SDK 2.2 이상</span><span class="sxs-lookup"><span data-stu-id="4978d-118">.NET Core SDK 2.2 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="4978d-119">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="4978d-119">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="4978d-120">Visual Studio Code용 C#</span><span class="sxs-lookup"><span data-stu-id="4978d-120">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [<span data-ttu-id="4978d-121">MongoDB</span><span class="sxs-lookup"><span data-stu-id="4978d-121">MongoDB</span></span>](https://docs.mongodb.com/manual/administration/install-community/)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="4978d-122">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="4978d-122">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* [<span data-ttu-id="4978d-123">.NET Core SDK 2.2 이상</span><span class="sxs-lookup"><span data-stu-id="4978d-123">.NET Core SDK 2.2 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="4978d-124">Mac용 Visual Studio 버전 7.7 이상</span><span class="sxs-lookup"><span data-stu-id="4978d-124">Visual Studio for Mac version 7.7 or later</span></span>](https://www.visualstudio.com/downloads/)
* [<span data-ttu-id="4978d-125">MongoDB</span><span class="sxs-lookup"><span data-stu-id="4978d-125">MongoDB</span></span>](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-os-x/)

---

## <a name="configure-mongodb"></a><span data-ttu-id="4978d-126">MongoDB 구성</span><span class="sxs-lookup"><span data-stu-id="4978d-126">Configure MongoDB</span></span>

<span data-ttu-id="4978d-127">Windows를 사용하는 경우 MongoDB는 기본적으로*C:\\Program Files\\MongoDB*에 설치됩니다.</span><span class="sxs-lookup"><span data-stu-id="4978d-127">If using Windows, MongoDB is installed at *C:\\Program Files\\MongoDB* by default.</span></span> <span data-ttu-id="4978d-128">*C:\\Program Files\\MongoDB\\Server\\\<version_number>\\bin*을 `Path` 환경 변수에 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="4978d-128">Add *C:\\Program Files\\MongoDB\\Server\\\<version_number>\\bin* to the `Path` environment variable.</span></span> <span data-ttu-id="4978d-129">이렇게 변경하면 개발 머신의 어디에서나 MongoDB에 액세스할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4978d-129">This change enables MongoDB access from anywhere on your development machine.</span></span>

<span data-ttu-id="4978d-130">다음 단계에서 mongo 셸을 사용하여 데이터베이스 및 컬렉션을 만들고 문서를 저장합니다.</span><span class="sxs-lookup"><span data-stu-id="4978d-130">Use the mongo Shell in the following steps to create a database, make collections, and store documents.</span></span> <span data-ttu-id="4978d-131">mongo 셸 명령에 대한 자세한 내용은 [mongo 셸 작업](https://docs.mongodb.com/manual/mongo/#working-with-the-mongo-shell)을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="4978d-131">For more information on mongo Shell commands, see [Working with the mongo Shell](https://docs.mongodb.com/manual/mongo/#working-with-the-mongo-shell).</span></span>

1. <span data-ttu-id="4978d-132">개발 머신에서 데이터를 저장할 디렉터리를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="4978d-132">Choose a directory on your development machine for storing the data.</span></span> <span data-ttu-id="4978d-133">예를 들어 Windows의 경우 *C:\\BooksData*를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="4978d-133">For example, *C:\\BooksData* on Windows.</span></span> <span data-ttu-id="4978d-134">디렉터리가 없을 경우 새로 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="4978d-134">Create the directory if it doesn't exist.</span></span> <span data-ttu-id="4978d-135">mongo 셸은 새 디렉터리를 만들지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="4978d-135">The mongo Shell doesn't create new directories.</span></span>
1. <span data-ttu-id="4978d-136">명령 셸을 엽니다.</span><span class="sxs-lookup"><span data-stu-id="4978d-136">Open a command shell.</span></span> <span data-ttu-id="4978d-137">다음 명령을 실행하여 기본 포트 27017에서 MongoDB에 연결합니다.</span><span class="sxs-lookup"><span data-stu-id="4978d-137">Run the following command to connect to MongoDB on default port 27017.</span></span> <span data-ttu-id="4978d-138">`<data_directory_path>`를 이전 단계에서 선택한 디렉터리로 바꿔야 합니다.</span><span class="sxs-lookup"><span data-stu-id="4978d-138">Remember to replace `<data_directory_path>` with the directory you chose in the previous step.</span></span>

    ```console
    mongod --dbpath <data_directory_path>
    ```

1. <span data-ttu-id="4978d-139">다른 명령 셸 인스턴스를 엽니다.</span><span class="sxs-lookup"><span data-stu-id="4978d-139">Open another command shell instance.</span></span> <span data-ttu-id="4978d-140">다음 명령을 실행하여 기본 테스트 데이터베이스에 연결합니다.</span><span class="sxs-lookup"><span data-stu-id="4978d-140">Connect to the default test database by running the following command:</span></span>

    ```console
    mongo
    ```

1. <span data-ttu-id="4978d-141">명령 셸에서 다음을 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="4978d-141">Run the following in a command shell:</span></span>

    ```console
    use BookstoreDb
    ```

    <span data-ttu-id="4978d-142">아직 존재하지 않는 경우 *BookstoreDb*라는 데이터베이스가 생성됩니다.</span><span class="sxs-lookup"><span data-stu-id="4978d-142">If it doesn't already exist, a database named *BookstoreDb* is created.</span></span> <span data-ttu-id="4978d-143">데이터베이스가 있는 경우 트랜잭션을 위해 해당 연결이 열립니다.</span><span class="sxs-lookup"><span data-stu-id="4978d-143">If the database does exist, its connection is opened for transactions.</span></span>

1. <span data-ttu-id="4978d-144">다음 명령을 사용하여 `Books` 컬렉션을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="4978d-144">Create a `Books` collection using following command:</span></span>

    ```console
    db.createCollection('Books')
    ```

    <span data-ttu-id="4978d-145">다음 결과가 표시됩니다.</span><span class="sxs-lookup"><span data-stu-id="4978d-145">The following result is displayed:</span></span>

    ```console
    { "ok" : 1 }
    ```

1. <span data-ttu-id="4978d-146">다음 명령을 사용하여 `Books` 컬렉션에 대한 스키마를 정의하고 두 개의 문서를 삽입합니다.</span><span class="sxs-lookup"><span data-stu-id="4978d-146">Define a schema for the `Books` collection and insert two documents using the following command:</span></span>

    ```console
    db.Books.insertMany([{'Name':'Design Patterns','Price':54.93,'Category':'Computers','Author':'Ralph Johnson'}, {'Name':'Clean Code','Price':43.15,'Category':'Computers','Author':'Robert C. Martin'}])
    ```

    <span data-ttu-id="4978d-147">다음 결과가 표시됩니다.</span><span class="sxs-lookup"><span data-stu-id="4978d-147">The following result is displayed:</span></span>

    ```console
    {
      "acknowledged" : true,
      "insertedIds" : [
        ObjectId("5bfd996f7b8e48dc15ff215d"),
        ObjectId("5bfd996f7b8e48dc15ff215e")
      ]
    }
    ```

1. <span data-ttu-id="4978d-148">다음 명령을 사용하여 데이터베이스의 문서를 봅니다.</span><span class="sxs-lookup"><span data-stu-id="4978d-148">View the documents in the database using the following command:</span></span>

    ```console
    db.Books.find({}).pretty()
    ```

    <span data-ttu-id="4978d-149">다음 결과가 표시됩니다.</span><span class="sxs-lookup"><span data-stu-id="4978d-149">The following result is displayed:</span></span>

    ```console
    {
      "_id" : ObjectId("5bfd996f7b8e48dc15ff215d"),
      "Name" : "Design Patterns",
      "Price" : 54.93,
      "Category" : "Computers",
      "Author" : "Ralph Johnson"
    }
    {
      "_id" : ObjectId("5bfd996f7b8e48dc15ff215e"),
      "Name" : "Clean Code",
      "Price" : 43.15,
      "Category" : "Computers",
      "Author" : "Robert C. Martin"
    }
    ```

    <span data-ttu-id="4978d-150">스키마가 각 문서에 대해 자동 생성된 `ObjectId` 형식의 `_id` 속성을 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="4978d-150">The schema adds an autogenerated `_id` property of type `ObjectId` for each document.</span></span>

<span data-ttu-id="4978d-151">데이터베이스가 준비되었습니다.</span><span class="sxs-lookup"><span data-stu-id="4978d-151">The database is ready.</span></span> <span data-ttu-id="4978d-152">이제 ASP.NET Core 웹 API를 만들기 시작할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4978d-152">You can start creating the ASP.NET Core web API.</span></span>

## <a name="create-the-aspnet-core-web-api-project"></a><span data-ttu-id="4978d-153">ASP.NET Core 웹 API 프로젝트 만들기</span><span class="sxs-lookup"><span data-stu-id="4978d-153">Create the ASP.NET Core web API project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4978d-154">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4978d-154">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="4978d-155">**파일** > **새로 만들기** > **프로젝트**로 이동합니다.</span><span class="sxs-lookup"><span data-stu-id="4978d-155">Go to **File** > **New** > **Project**.</span></span>
1. <span data-ttu-id="4978d-156">**ASP.NET Core 웹 애플리케이션**을 선택하고 프로젝트 이름을 *BooksApi*로 지정한 다음, **확인**을 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="4978d-156">Select **ASP.NET Core Web Application**, name the project *BooksApi*, and click **OK**.</span></span>
1. <span data-ttu-id="4978d-157">**.NET Core** 대상 프레임워크 및 **ASP.NET Core 2.1**을 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="4978d-157">Select the **.NET Core** target framework and **ASP.NET Core 2.1**.</span></span> <span data-ttu-id="4978d-158">**API** 프로젝트 템플릿을 선택하고 **확인**을 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="4978d-158">Select the **API** project template, and click **OK**:</span></span>
1. <span data-ttu-id="4978d-159">**패키지 관리자 콘솔** 창에서 프로젝트 루트로 이동합니다.</span><span class="sxs-lookup"><span data-stu-id="4978d-159">In the **Package Manager Console** window, navigate to the project root.</span></span> <span data-ttu-id="4978d-160">다음 명령을 실행하여 MongoDB용 .NET 드라이버를 설치합니다.</span><span class="sxs-lookup"><span data-stu-id="4978d-160">Run the following command to install the .NET driver for MongoDB:</span></span>

    ```powershell
    Install-Package MongoDB.Driver -Version 2.7.2
    ```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="4978d-161">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="4978d-161">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="4978d-162">명령 셸에서 다음 명령을 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="4978d-162">Run the following commands in a command shell:</span></span>

    ```console
    dotnet new webapi -o BooksApi
    code BooksApi
    ```

    <span data-ttu-id="4978d-163">.NET Core를 대상으로 하는 새로운 ASP.NET Core 웹 API 프로젝트가 Visual Studio Code에서 생성되어 열립니다.</span><span class="sxs-lookup"><span data-stu-id="4978d-163">A new ASP.NET Core web API project targeting .NET Core is generated and opened in Visual Studio Code.</span></span>

1. <span data-ttu-id="4978d-164">‘빌드 및 디버그에 필요한 자산이 ‘BooksApi’에서 누락되었습니다. 추가하시겠습니까?’ 알림이 표시되면 **예**를 클릭합니다. </span><span class="sxs-lookup"><span data-stu-id="4978d-164">Click **Yes** when the *Required assets to build and debug are missing from 'BooksApi'. Add them?* notification appears.</span></span>
1. <span data-ttu-id="4978d-165">**통합 터미널**을 열고 프로젝트 루트로 이동합니다.</span><span class="sxs-lookup"><span data-stu-id="4978d-165">Open **Integrated Terminal** and navigate to the project root.</span></span> <span data-ttu-id="4978d-166">다음 명령을 실행하여 MongoDB용 .NET 드라이버를 설치합니다.</span><span class="sxs-lookup"><span data-stu-id="4978d-166">Run the following command to install the .NET driver for MongoDB:</span></span>

    ```console
    dotnet add BooksApi.csproj package MongoDB.Driver -v 2.7.2
    ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="4978d-167">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="4978d-167">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

1. <span data-ttu-id="4978d-168">**파일** > **새 솔루션** > **.NET Core** > **앱**으로 이동합니다.</span><span class="sxs-lookup"><span data-stu-id="4978d-168">Go to **File** > **New Solution** > **.NET Core** > **App**.</span></span>
1. <span data-ttu-id="4978d-169">**ASP.NET Core Web API** C# 프로젝트 템플릿을 선택하고 **다음**을 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="4978d-169">Select the **ASP.NET Core Web API** C# project template, and click **Next**.</span></span>
1. <span data-ttu-id="4978d-170">**대상 프레임워크** 드롭다운 목록에서 **.NET Core 2.2**를 선택하고 **다음**을 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="4978d-170">Select **.NET Core 2.2** from the **Target Framework** drop-down list, and click **Next**.</span></span>
1. <span data-ttu-id="4978d-171">**프로젝트 이름**으로 *BooksApi*를 입력한 다음, **만들기**를 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="4978d-171">Enter *BooksApi* for the **Project Name**, and click **Create**.</span></span>
1. <span data-ttu-id="4978d-172">**솔루션** 패드에서 프로젝트의 **종속성** 노드를 마우스 오른쪽 단추로 클릭하고 **패키지 추가**를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="4978d-172">In the **Solution** pad, right-click the project's **Dependencies** node and select **Add Packages**.</span></span>
1. <span data-ttu-id="4978d-173">검색 상자에 *MongoDB.Driver*를 입력하고 *MongoDB.Driver* 패키지를 선택한 다음, **패키지 추가**를 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="4978d-173">Enter *MongoDB.Driver* in the search box, select the *MongoDB.Driver* package, and click **Add Package**.</span></span>
1. <span data-ttu-id="4978d-174">**라이선스 승인** 대화 상자에서 **동의** 단추를 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="4978d-174">Click the **Accept** button in the **License Acceptance** dialog.</span></span>

---

## <a name="add-a-model"></a><span data-ttu-id="4978d-175">모델 추가</span><span class="sxs-lookup"><span data-stu-id="4978d-175">Add a model</span></span>

1. <span data-ttu-id="4978d-176">프로젝트 루트에 *Models* 디렉터리를 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="4978d-176">Add a *Models* directory to the project root.</span></span>
1. <span data-ttu-id="4978d-177">다음 코드를 사용하여 *Models* 디렉터리에 `Book` 클래스를 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="4978d-177">Add a `Book` class to the *Models* directory with the following code:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Models/Book.cs)]

<span data-ttu-id="4978d-178">앞의 클래스에서 `Id` 속성은</span><span class="sxs-lookup"><span data-stu-id="4978d-178">In the preceding class, the `Id` property:</span></span>

* <span data-ttu-id="4978d-179">CLR(공용 언어 런타임) 개체를 MongoDB 컬렉션에 매핑하는 데 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="4978d-179">Is required for mapping the Common Language Runtime (CLR) object to the MongoDB collection.</span></span>
* <span data-ttu-id="4978d-180">이 속성을 문서의 기본 키로 지정하려면 `[BsonId]`로 주석이 추가됩니다.</span><span class="sxs-lookup"><span data-stu-id="4978d-180">Is annotated with `[BsonId]` to designate this property as the document's primary key.</span></span>
* <span data-ttu-id="4978d-181">`ObjectId` 대신 `string` 형식으로 매개 변수를 전달할 수 있도록 `[BsonRepresentation(BsonType.ObjectId)]`로 주석이 추가됩니다.</span><span class="sxs-lookup"><span data-stu-id="4978d-181">Is annotated with `[BsonRepresentation(BsonType.ObjectId)]` to allow passing the parameter as type `string` instead of `ObjectId`.</span></span> <span data-ttu-id="4978d-182">Mongo는 `string`에서 `ObjectId`로 변환을 처리합니다.</span><span class="sxs-lookup"><span data-stu-id="4978d-182">Mongo handles the conversion from `string` to `ObjectId`.</span></span>

<span data-ttu-id="4978d-183">클래스의 기타 속성은 `[BsonElement]` 특성으로 주석이 추가됩니다.</span><span class="sxs-lookup"><span data-stu-id="4978d-183">Other properties in the class are annotated with the `[BsonElement]` attribute.</span></span> <span data-ttu-id="4978d-184">특성 값은 MongoDB 컬렉션의 속성 이름을 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="4978d-184">The attribute's value represents the property name in the MongoDB collection.</span></span>

## <a name="add-a-crud-operations-class"></a><span data-ttu-id="4978d-185">CRUD 작업 클래스 추가</span><span class="sxs-lookup"><span data-stu-id="4978d-185">Add a CRUD operations class</span></span>

1. <span data-ttu-id="4978d-186">프로젝트 루트에 *Services* 디렉터리를 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="4978d-186">Add a *Services* directory to the project root.</span></span>
1. <span data-ttu-id="4978d-187">다음 코드를 사용하여 *Services* 디렉터리에 `BookService` 클래스를 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="4978d-187">Add a `BookService` class to the *Services* directory with the following code:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Services/BookService.cs?name=snippet_BookServiceClass)]

1. <span data-ttu-id="4978d-188">*appsettings.json*에 MongoDB 연결 문자열을 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="4978d-188">Add the MongoDB connection string to *appsettings.json*:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/appsettings.json?highlight=2-4)]

    <span data-ttu-id="4978d-189">앞의 `BookstoreDb` 속성은 `BookService` 클래스 생성자에서 액세스됩니다.</span><span class="sxs-lookup"><span data-stu-id="4978d-189">The preceding `BookstoreDb` property is accessed in the `BookService` class constructor.</span></span>

1. <span data-ttu-id="4978d-190">`Startup.ConfigureServices`에서 `BookService` 클래스를 종속성 삽입 시스템에 등록합니다.</span><span class="sxs-lookup"><span data-stu-id="4978d-190">In `Startup.ConfigureServices`, register the `BookService` class with the Dependency Injection system:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Startup.cs?name=snippet_ConfigureServices&highlight=3)]

    <span data-ttu-id="4978d-191">앞의 서비스 등록은 사용 클래스에서 생성자 삽입을 지원하는 데 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="4978d-191">The preceding service registration is necessary to support constructor injection in consuming classes.</span></span>

<span data-ttu-id="4978d-192">`BookService` 클래스는 다음 `MongoDB.Driver` 멤버를 사용하여 데이터베이스에 대해 CRUD 작업을 수행합니다.</span><span class="sxs-lookup"><span data-stu-id="4978d-192">The `BookService` class uses the following `MongoDB.Driver` members to perform CRUD operations against the database:</span></span>

* <span data-ttu-id="4978d-193">`MongoClient` &ndash; 데이터베이스 작업을 수행하기 위한 서버 인스턴스를 읽습니다.</span><span class="sxs-lookup"><span data-stu-id="4978d-193">`MongoClient` &ndash; Reads the server instance for performing database operations.</span></span> <span data-ttu-id="4978d-194">이 클래스의 생성자에 MongoDB 연결 문자열이 제공됩니다.</span><span class="sxs-lookup"><span data-stu-id="4978d-194">The constructor of this class is provided the MongoDB connection string:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Services/BookService.cs?name=snippet_BookServiceConstructor&highlight=3)]

* <span data-ttu-id="4978d-195">`IMongoDatabase` &ndash; 작업 수행을 위한 Mongo 데이터베이스를 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="4978d-195">`IMongoDatabase` &ndash; Represents the Mongo database for performing operations.</span></span> <span data-ttu-id="4978d-196">이 자습서에서는 인터페이스의 일반 `GetCollection<T>(collection)` 메서드를 사용하여 특정 컬렉션의 데이터에 액세스합니다.</span><span class="sxs-lookup"><span data-stu-id="4978d-196">This tutorial uses the generic `GetCollection<T>(collection)` method on the interface to gain access to data in a specific collection.</span></span> <span data-ttu-id="4978d-197">이 메서드를 호출한 후 컬렉션에 대해 CRUD 작업을 수행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4978d-197">CRUD operations can be performed against the collection after this method is called.</span></span> <span data-ttu-id="4978d-198">`GetCollection<T>(collection)` 메서드 호출에서 다음을 수행합니다.</span><span class="sxs-lookup"><span data-stu-id="4978d-198">In the `GetCollection<T>(collection)` method call:</span></span>
  * <span data-ttu-id="4978d-199">`collection`은 컬렉션 이름을 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="4978d-199">`collection` represents the collection name.</span></span>
  * <span data-ttu-id="4978d-200">`T`는 컬렉션에 저장된 CLR 개체 형식을 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="4978d-200">`T` represents the CLR object type stored in the collection.</span></span>

<span data-ttu-id="4978d-201">`GetCollection<T>(collection)`은 컬렉션을 나타내는 `MongoCollection` 개체를 반환합니다.</span><span class="sxs-lookup"><span data-stu-id="4978d-201">`GetCollection<T>(collection)` returns a `MongoCollection` object representing the collection.</span></span> <span data-ttu-id="4978d-202">이 자습서에서는 컬렉션에 대해 다음 메서드를 호출합니다.</span><span class="sxs-lookup"><span data-stu-id="4978d-202">In this tutorial, the following methods are invoked on the collection:</span></span>

* <span data-ttu-id="4978d-203">`Find<T>` &ndash; 제공된 검색 조건과 일치하는 컬렉션의 모든 문서를 반환합니다.</span><span class="sxs-lookup"><span data-stu-id="4978d-203">`Find<T>` &ndash; Returns all documents in the collection matching the provided search criteria.</span></span>
* <span data-ttu-id="4978d-204">`InsertOne` &ndash; 제공된 개체를 컬렉션에 새 문서로 삽입합니다.</span><span class="sxs-lookup"><span data-stu-id="4978d-204">`InsertOne` &ndash; Inserts the provided object as a new document in the collection.</span></span>
* <span data-ttu-id="4978d-205">`ReplaceOne` &ndash; 제공된 검색 조건과 일치하는 단일 문서를 제공된 개체로 바꿉니다.</span><span class="sxs-lookup"><span data-stu-id="4978d-205">`ReplaceOne` &ndash; Replaces the single document matching the provided search criteria with the provided object.</span></span>
* <span data-ttu-id="4978d-206">`DeleteOne` &ndash; 제공된 검색 조건과 일치하는 단일 문서를 삭제합니다.</span><span class="sxs-lookup"><span data-stu-id="4978d-206">`DeleteOne` &ndash; Deletes a single document matching the provided search criteria.</span></span>

## <a name="add-a-controller"></a><span data-ttu-id="4978d-207">컨트롤러 추가</span><span class="sxs-lookup"><span data-stu-id="4978d-207">Add a controller</span></span>

1. <span data-ttu-id="4978d-208">다음 코드를 사용하여 *Controllers* 디렉터리에 `BooksController` 클래스를 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="4978d-208">Add a `BooksController` class to the *Controllers* directory with the following code:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Controllers/BooksController.cs)]

    <span data-ttu-id="4978d-209">앞의 웹 API 컨트롤러는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="4978d-209">The preceding web API controller:</span></span>

    * <span data-ttu-id="4978d-210">`BookService` 클래스를 사용하여 CRUD 작업을 수행합니다.</span><span class="sxs-lookup"><span data-stu-id="4978d-210">Uses the `BookService` class to perform CRUD operations.</span></span>
    * <span data-ttu-id="4978d-211">GET, POST, PUT 및 DELETE HTTP 요청을 지원하는 작업 메서드를 포함합니다.</span><span class="sxs-lookup"><span data-stu-id="4978d-211">Contains action methods to support GET, POST, PUT, and DELETE HTTP requests.</span></span>
1. <span data-ttu-id="4978d-212">앱을 빌드하고 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="4978d-212">Build and run the app.</span></span>
1. <span data-ttu-id="4978d-213">브라우저에서 `http://localhost:<port>/api/books`로 이동합니다.</span><span class="sxs-lookup"><span data-stu-id="4978d-213">Navigate to `http://localhost:<port>/api/books` in your browser.</span></span> <span data-ttu-id="4978d-214">다음 JSON 응답이 표시됩니다.</span><span class="sxs-lookup"><span data-stu-id="4978d-214">The following JSON response is displayed:</span></span>

    ```json
    [
      {
        "id":"5bfd996f7b8e48dc15ff215d",
        "bookName":"Design Patterns",
        "price":54.93,
        "category":"Computers",
        "author":"Ralph Johnson"
      },
      {
        "id":"5bfd996f7b8e48dc15ff215e",
        "bookName":"Clean Code",
        "price":43.15,
        "category":"Computers",
        "author":"Robert C. Martin"
      }
    ]
    ```

## <a name="next-steps"></a><span data-ttu-id="4978d-215">다음 단계</span><span class="sxs-lookup"><span data-stu-id="4978d-215">Next steps</span></span>

<span data-ttu-id="4978d-216">ASP.NET Core 웹 API 빌드 방법에 대한 자세한 내용은 다음 리소스를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="4978d-216">For more information on building ASP.NET Core web APIs, see the following resources:</span></span>

* <xref:web-api/index>
* <xref:web-api/action-return-types>
