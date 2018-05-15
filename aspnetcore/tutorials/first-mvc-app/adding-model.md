---
title: ASP.NET Core MVC 앱에 모델 추가
author: rick-anderson
description: 간단한 ASP.NET Core 앱에 모델을 추가합니다.
manager: wpickett
ms.author: riande
ms.date: 12/8/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app/adding-model
ms.openlocfilehash: 4204d4e2d474db51692d42751a9f82373e9f0c0d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/06/2018
---
# <a name="add-a-model-to-an-aspnet-core-mvc-app"></a><span data-ttu-id="7522e-103">ASP.NET Core MVC 앱에 모델 추가</span><span class="sxs-lookup"><span data-stu-id="7522e-103">Add a model to an ASP.NET Core MVC app</span></span>

[!INCLUDE [adding-model](../../includes/mvc-intro/adding-model1.md)]

<span data-ttu-id="7522e-104">참고: ASP.NET Core 2.0 템플릿은 *Models* 폴더를 포함합니다.</span><span class="sxs-lookup"><span data-stu-id="7522e-104">Note: The ASP.NET Core 2.0 templates contain the *Models* folder.</span></span>

<span data-ttu-id="7522e-105">*Models* 폴더> **추가** > **클래스**를 마우스 오른쪽 단추로 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="7522e-105">Right-click the *Models* folder > **Add** > **Class**.</span></span> <span data-ttu-id="7522e-106">클래스 이름을 **Movie**로 지정하고 다음 속성을 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="7522e-106">Name the class **Movie** and add the following properties:</span></span>

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieNoEF.cs?name=snippet_1)]

<span data-ttu-id="7522e-107">`ID` 필드는 데이터베이스에서 기본 키 대신 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="7522e-107">The `ID` field is required by the database for the primary key.</span></span> 

<span data-ttu-id="7522e-108">프로젝트를 빌드하여 오류가 없는지 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="7522e-108">Build the project to verify you don't have any errors.</span></span> <span data-ttu-id="7522e-109">이제 **M**VC 앱에 **모**델이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="7522e-109">You now have a **M**odel in your **M**VC app.</span></span>

## <a name="scaffolding-a-controller"></a><span data-ttu-id="7522e-110">컨트롤러 스캐폴딩</span><span class="sxs-lookup"><span data-stu-id="7522e-110">Scaffolding a controller</span></span>

<span data-ttu-id="7522e-111">**솔루션 탐색기**에서 *Controllers* 폴더를 마우스 오른쪽 단추로 클릭하고 **추가 > 컨트롤러**를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="7522e-111">In **Solution Explorer**, right-click the *Controllers* folder **> Add > Controller**.</span></span>

![위의 단계 보기](adding-model/_static/add_controller.png)

<span data-ttu-id="7522e-113">**MVC 종속성 추가** 대화 상자가 나타나는 경우:</span><span class="sxs-lookup"><span data-stu-id="7522e-113">If the **Add MVC Dependencies** dialog appears:</span></span>

* <span data-ttu-id="7522e-114">[Visual Studio를 최신 버전으로 업데이트합니다](https://www.visualstudio.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="7522e-114">[Update Visual Studio to the latest version](https://www.visualstudio.com/downloads/).</span></span> <span data-ttu-id="7522e-115">Visual Studio 15.5 이전 버전이 이 대화 상자를 표시합니다.</span><span class="sxs-lookup"><span data-stu-id="7522e-115">Visual Studio versions prior to 15.5 show this dialog.</span></span>
* <span data-ttu-id="7522e-116">업데이트할 수 없는 경우 **추가**를 선택한 다음 컨트롤러 추가 단계를 다시 따릅니다.</span><span class="sxs-lookup"><span data-stu-id="7522e-116">If you can't update, select **ADD**, and then follow the add controller steps again.</span></span>

<span data-ttu-id="7522e-117">**스캐폴드 추가** 대화 상자에서 **보기 포함 MVC 컨트롤러, Entity Framework > 추가 사용**을 누릅니다.</span><span class="sxs-lookup"><span data-stu-id="7522e-117">In the **Add Scaffold** dialog, tap **MVC Controller with views, using Entity Framework > Add**.</span></span>

![스캐폴드 추가 대화 상자](adding-model/_static/add_scaffold2.png)

<span data-ttu-id="7522e-119">**컨트롤러 추가** 대화 상자를 입력합니다.</span><span class="sxs-lookup"><span data-stu-id="7522e-119">Complete the **Add Controller** dialog:</span></span>

* <span data-ttu-id="7522e-120">**모델 클래스:** *Movie (MvcMovie.Models)*</span><span class="sxs-lookup"><span data-stu-id="7522e-120">**Model class:** *Movie (MvcMovie.Models)*</span></span>
* <span data-ttu-id="7522e-121">**데이터 컨텍스트 클래스**: **+** 아이콘을 선택하고 기본값 **MvcMovie.Models.MvcMovieContext**를 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="7522e-121">**Data context class:** Select the **+** icon and add the default **MvcMovie.Models.MvcMovieContext**</span></span>

![데이터 컨텍스트 추가](adding-model/_static/dc.png)

* <span data-ttu-id="7522e-123">**보기:** 각 옵션의 기본값 선택 유지</span><span class="sxs-lookup"><span data-stu-id="7522e-123">**Views:** Keep the default of each option checked</span></span>
* <span data-ttu-id="7522e-124">**컨트롤러 이름:** 기본값 *MoviesController* 유지</span><span class="sxs-lookup"><span data-stu-id="7522e-124">**Controller name:** Keep the default *MoviesController*</span></span>
* <span data-ttu-id="7522e-125">**추가** 누르기</span><span class="sxs-lookup"><span data-stu-id="7522e-125">Tap **Add**</span></span>

![컨트롤러 추가 대화 상자](adding-model/_static/add_controller2.png)

<span data-ttu-id="7522e-127">Visual Studio에서 다음을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="7522e-127">Visual Studio creates:</span></span>

* <span data-ttu-id="7522e-128">Entity Framework Core [데이터베이스 컨텍스트 클래스](xref:data/ef-mvc/intro#create-the-database-context)(*Data/MvcMovieContext.cs*)</span><span class="sxs-lookup"><span data-stu-id="7522e-128">An Entity Framework Core [database context class](xref:data/ef-mvc/intro#create-the-database-context) (*Data/MvcMovieContext.cs*)</span></span>
* <span data-ttu-id="7522e-129">동영상 컨트롤러(*Controllers/MoviesController.cs*)</span><span class="sxs-lookup"><span data-stu-id="7522e-129">A movies controller (*Controllers/MoviesController.cs*)</span></span>
* <span data-ttu-id="7522e-130">만들기, 삭제, 세부 정보, 편집 및 인덱스 페이지에 대한 Razor 뷰 파일(<em>Views/Movies/&ast;.cshtml</em>)</span><span class="sxs-lookup"><span data-stu-id="7522e-130">Razor view files for Create, Delete, Details, Edit, and Index pages (<em>Views/Movies/&ast;.cshtml</em>)</span></span>

<span data-ttu-id="7522e-131">[CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete)(만들기, 읽기, 업데이트 및 삭제) 작업 메서드와 뷰 및 데이터베이스 컨텍스트의 자동 생성을 *스캐폴딩*이라고 합니다.</span><span class="sxs-lookup"><span data-stu-id="7522e-131">The automatic creation of the database context and [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (create, read, update, and delete) action methods and views is known as *scaffolding*.</span></span> <span data-ttu-id="7522e-132">동영상 데이터베이스를 관리할 수 있는 완벽하게 작동하는 웹 응용 프로그램이 곧 제공됩니다.</span><span class="sxs-lookup"><span data-stu-id="7522e-132">You'll soon have a fully functional web application that lets you manage a movie database.</span></span>

<span data-ttu-id="7522e-133">응용 프로그램을 실행하고 **Mvc Movie** 링크를 클릭하면 다음과 유사한 오류가 표시됩니다.</span><span class="sxs-lookup"><span data-stu-id="7522e-133">If you run the app and click on the **Mvc Movie** link, you get an error similar to the following:</span></span>

```
An unhandled exception occurred while processing the request.

SqlException: Cannot open database "MvcMovieContext-<GUID removed>" requested by the login. The login failed.
Login failed for user 'Rick'.

System.Data.SqlClient.SqlInternalConnectionTds..ctor(DbConnectionPoolIdentity identity, SqlConnectionString 
```

<span data-ttu-id="7522e-134">데이터베이스를 만들어야 하며 여기에 EF Core [마이그레이션](xref:data/ef-mvc/migrations) 기능을 사용할 것입니다.</span><span class="sxs-lookup"><span data-stu-id="7522e-134">You need to create the database, and you'll use the EF Core [Migrations](xref:data/ef-mvc/migrations) feature to do that.</span></span> <span data-ttu-id="7522e-135">마이그레이션을 통해 데이터 모델과 일치하는 데이터베이스를 만들고 데이터 모델 변경 시 데이터베이스 스키마를 업데이트할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="7522e-135">Migrations lets you create a database that matches your data model and update the database schema when your data model changes.</span></span>

## <a name="add-ef-tooling-and-perform-initial-migration"></a><span data-ttu-id="7522e-136">EF 도구 추가 및 초기 마이그레이션 수행</span><span class="sxs-lookup"><span data-stu-id="7522e-136">Add EF tooling and perform initial migration</span></span>

<span data-ttu-id="7522e-137">이 섹션에서는 PMC(패키지 관리자 콘솔)를 사용하여 다음을 수행합니다.</span><span class="sxs-lookup"><span data-stu-id="7522e-137">In this section you'll use the Package Manager Console (PMC) to:</span></span>

* <span data-ttu-id="7522e-138">Entity Framework Core 도구 패키지 추가</span><span class="sxs-lookup"><span data-stu-id="7522e-138">Add the Entity Framework Core Tools package.</span></span> <span data-ttu-id="7522e-139">이 패키지는 마이그레이션을 추가하고 데이터베이스를 업데이트하는 데 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="7522e-139">This package is required to add migrations and update the database.</span></span>
* <span data-ttu-id="7522e-140">초기 마이그레이션을 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="7522e-140">Add an initial migration.</span></span>
* <span data-ttu-id="7522e-141">초기 마이그레이션을 사용하여 데이터베이스를 업데이트합니다.</span><span class="sxs-lookup"><span data-stu-id="7522e-141">Update the database with the initial migration.</span></span>

<span data-ttu-id="7522e-142">**도구** 메뉴에서 **NuGet 패키지 관리자 > 패키지 관리자 콘솔**을 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="7522e-142">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>

<!-- following image shared with uid: tutorials/razor-pages/model -->
  ![PMC 메뉴](adding-model/_static/pmc.png)

<span data-ttu-id="7522e-144">PMC에서 다음 명령을 입력합니다.</span><span class="sxs-lookup"><span data-stu-id="7522e-144">In the PMC, enter the following commands:</span></span>

``` PMC
Install-Package Microsoft.EntityFrameworkCore.Tools
Add-Migration Initial
Update-Database
```

<span data-ttu-id="7522e-145">**참고:** `Install-Package` 명령에서 오류가 발생할 경우 NuGet Package Manager를 열고 `Microsoft.EntityFrameworkCore.Tools` 패키지를 검색하세요.</span><span class="sxs-lookup"><span data-stu-id="7522e-145">**Note:** If you receive an error with the `Install-Package` command, open NuGet Package Manager and search for the `Microsoft.EntityFrameworkCore.Tools` package.</span></span> <span data-ttu-id="7522e-146">그러면 패키지를 설치하거나 패키지가 이미 설치되어 있는지 확인할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="7522e-146">This allows you to install the package or check if it's already installed.</span></span> <span data-ttu-id="7522e-147">또는 PMC 문제가 있는 경우 [CLI 방식](#cli)을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="7522e-147">Alternatively, see the [CLI approach](#cli) if you have problems with the PMC.</span></span>

<span data-ttu-id="7522e-148">`Add-Migration` 명령은 초기 데이터베이스 스키마를 만드는 코드를 생성합니다.</span><span class="sxs-lookup"><span data-stu-id="7522e-148">The `Add-Migration` command creates code to create the initial database schema.</span></span> <span data-ttu-id="7522e-149">스키마는 `DbContext`에 지정된 모델을 기반으로 합니다(*Data/MvcMovieContext.cs* 파일).</span><span class="sxs-lookup"><span data-stu-id="7522e-149">The schema is based on the model specified in the `DbContext`(In the *Data/MvcMovieContext.cs* file).</span></span> <span data-ttu-id="7522e-150">`Initial` 인수는 마이그레이션 이름을 지정하는 데 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="7522e-150">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="7522e-151">모든 이름을 사용할 수 있지만 일반적으로 마이그레이션을 설명하는 이름을 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="7522e-151">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="7522e-152">자세한 내용은 [마이그레이션 소개](xref:data/ef-mvc/migrations#introduction-to-migrations)를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="7522e-152">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="7522e-153">`Update-Database` 명령은 *Migrations/\<time-stamp>_Initial.cs* 파일에서 데이터베이스를 만드는 `Up` 메서드를 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="7522e-153">The `Update-Database` command runs the `Up` method in the *Migrations/\<time-stamp>_Initial.cs* file, which creates the database.</span></span>

<a name="cli"></a><span data-ttu-id="7522e-154">PMC 대신 명령줄 인터페이스(CLI)를 사용하여 앞의 단계를 수행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="7522e-154">You can perform the preceeding steps using the command-line interface (CLI) rather than the PMC:</span></span>

* <span data-ttu-id="7522e-155">*.csproj* 파일에 [EF Core 도구](xref:data/ef-mvc/migrations#entity-framework-core-nuget-packages-for-migrations)를 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="7522e-155">Add [EF Core tooling](xref:data/ef-mvc/migrations#entity-framework-core-nuget-packages-for-migrations) to the *.csproj* file.</span></span>
* <span data-ttu-id="7522e-156">콘솔(프로젝트 디렉터리)에서 다음 명령을 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="7522e-156">Run the following commands from the console (in the project directory):</span></span>

  ```console
  dotnet ef migrations add Initial
  dotnet ef database update
  ```     
  
  <span data-ttu-id="7522e-157">앱을 실행하고 오류가 발생하는 경우:</span><span class="sxs-lookup"><span data-stu-id="7522e-157">If you run the app and get the error:</span></span>
  
  ```text
  SqlException: Cannot open database "Movie" requested by the login.
  The login failed.
  Login failed for user 'user name'.
  ```

<span data-ttu-id="7522e-158">` dotnet ef database update`를 실행하지 않았을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="7522e-158">You probably have not run ` dotnet ef database update`.</span></span>
  
[!INCLUDE [adding-model](../../includes/mvc-intro/adding-model3.md)]

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=ConfigureServices&highlight=6-7)]

[!INCLUDE [adding-model](../../includes/mvc-intro/adding-model4.md)]

![ID, 가격, 출시일, 제목에 대해 사용 가능한 속성을 열거하는 모델 항목의 Intellisense 상황별 메뉴](adding-model/_static/ints.png)

## <a name="additional-resources"></a><span data-ttu-id="7522e-160">추가 자료</span><span class="sxs-lookup"><span data-stu-id="7522e-160">Additional resources</span></span>

* [<span data-ttu-id="7522e-161">태그 도우미</span><span class="sxs-lookup"><span data-stu-id="7522e-161">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="7522e-162">전역화 및 지역화</span><span class="sxs-lookup"><span data-stu-id="7522e-162">Globalization and localization</span></span>](xref:fundamentals/localization)

> [!div class="step-by-step"]
> <span data-ttu-id="7522e-163">[이전 뷰 추가](adding-view.md)
> [다음 SQL 사용](working-with-sql.md)</span><span class="sxs-lookup"><span data-stu-id="7522e-163">[Previous Adding a View](adding-view.md)
[Next Working with SQL](working-with-sql.md)</span></span>  
