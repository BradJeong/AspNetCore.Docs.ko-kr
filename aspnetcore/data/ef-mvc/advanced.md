---
title: "ASP.NET Core MVC EF 코어-고급-10 / 10"
author: tdykstra
description: "이 자습서에서는 고급의 Entity Framework Core를 사용 하는 ASP.NET 웹 응용 프로그램 개발 기능 수행 하는 경우 고려해 야 하는 데 유용 하는 여러 항목을 소개 합니다."
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/advanced
ms.openlocfilehash: 4ee12cae0220825c81bd8b178dea3ac777f97bb6
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/24/2018
---
# <a name="advanced-topics---ef-core-with-aspnet-core-mvc-tutorial-10-of-10"></a><span data-ttu-id="9b49f-103">고급 항목-EF 코어 ASP.NET Core MVC 자습서 (10 / 10)</span><span class="sxs-lookup"><span data-stu-id="9b49f-103">Advanced topics - EF Core with ASP.NET Core MVC tutorial (10 of 10)</span></span>

<span data-ttu-id="9b49f-104">여 [Tom Dykstra](https://github.com/tdykstra) 및 [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="9b49f-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="9b49f-105">Contoso 대학 샘플 웹 응용 프로그램에는 Entity Framework Core 및 Visual Studio를 사용 하 여 ASP.NET Core MVC 웹 응용 프로그램을 만드는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="9b49f-105">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="9b49f-106">자습서 시리즈에 대 한 정보를 참조 하십시오. [시리즈의 첫 번째 자습서](intro.md)합니다.</span><span class="sxs-lookup"><span data-stu-id="9b49f-106">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="9b49f-107">이전 자습서에서 구현한 계층당 하나의 테이블 상속 합니다.</span><span class="sxs-lookup"><span data-stu-id="9b49f-107">In the previous tutorial, you implemented table-per-hierarchy inheritance.</span></span> <span data-ttu-id="9b49f-108">이 자습서에서는 고급의 Entity Framework Core를 사용 하는 ASP.NET Core 웹 응용 프로그램 개발 기능 수행 하는 경우 고려해 야 하는 데 유용 하는 여러 항목을 소개 합니다.</span><span class="sxs-lookup"><span data-stu-id="9b49f-108">This tutorial introduces several topics that are useful to be aware of when you go beyond the basics of developing ASP.NET Core web applications that use Entity Framework Core.</span></span>

## <a name="raw-sql-queries"></a><span data-ttu-id="9b49f-109">원시 SQL 쿼리</span><span class="sxs-lookup"><span data-stu-id="9b49f-109">Raw SQL Queries</span></span>

<span data-ttu-id="9b49f-110">Entity Framework 사용의 장점 중 하나를 방지할 수 제한 된 데이터를 저장 하는 특정 방법에 가깝게 너무 코드입니다.</span><span class="sxs-lookup"><span data-stu-id="9b49f-110">One of the advantages of using the Entity Framework is that it avoids tying your code too closely to a particular method of storing data.</span></span> <span data-ttu-id="9b49f-111">생성 하 여 SQL 쿼리 및 명령, 직접 작성 하지 않아도 하므로 수행 합니다.</span><span class="sxs-lookup"><span data-stu-id="9b49f-111">It does this by generating SQL queries and commands for you, which also frees you from having to write them yourself.</span></span> <span data-ttu-id="9b49f-112">하지만 사용자가 직접 만든 특정 SQL 쿼리를 실행 해야 할 때 예외적인 경우도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="9b49f-112">But there are exceptional scenarios when you need to run specific SQL queries that you have manually created.</span></span> <span data-ttu-id="9b49f-113">이러한 시나리오에는 엔터티 프레임 워크 코드의 첫 번째 API SQL 명령을 데이터베이스에 직접 전달할 수 있도록 하는 메서드를 포함 합니다.</span><span class="sxs-lookup"><span data-stu-id="9b49f-113">For these scenarios, the Entity Framework Code First API includes methods that enable you to pass SQL commands directly to the database.</span></span> <span data-ttu-id="9b49f-114">EF Core 1.0에서 다음 옵션을 선택할:</span><span class="sxs-lookup"><span data-stu-id="9b49f-114">You have the following options in EF Core 1.0:</span></span>

* <span data-ttu-id="9b49f-115">사용 하 여 `DbSet.FromSql` 엔터티 형식을 반환 하는 쿼리에 대 한 메서드.</span><span class="sxs-lookup"><span data-stu-id="9b49f-115">Use the `DbSet.FromSql` method for queries that return entity types.</span></span> <span data-ttu-id="9b49f-116">반환 된 개체에 필요한 형식 이어야 합니다는 `DbSet` 개체 및 이러한 하는 자동으로 데이터베이스 컨텍스트에서 추적 하지 않는 한 있습니다 [추적 해제](crud.md#no-tracking-queries)합니다.</span><span class="sxs-lookup"><span data-stu-id="9b49f-116">The returned objects must be of the type expected by the `DbSet` object, and they're automatically tracked by the database context unless you [turn tracking off](crud.md#no-tracking-queries).</span></span>

* <span data-ttu-id="9b49f-117">사용 하 여 `Database.ExecuteSqlCommand` 쿼리가 아닌 명령에 대 한 합니다.</span><span class="sxs-lookup"><span data-stu-id="9b49f-117">Use the `Database.ExecuteSqlCommand` for non-query commands.</span></span>

<span data-ttu-id="9b49f-118">엔터티 지원 하지 않는 형식을 반환 하는 쿼리를 실행 해야 하는 경우 ADO.NET EF에서 제공 하는 데이터베이스 연결으로 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="9b49f-118">If you need to run a query that returns types that aren't entities, you can use ADO.NET with the database connection provided by EF.</span></span> <span data-ttu-id="9b49f-119">반환 된 데이터 엔터티 형식을 검색 하려면이 메서드를 사용 하는 경우에 데이터베이스 컨텍스트에 의해 추적 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="9b49f-119">The returned data isn't tracked by the database context, even if you use this method to retrieve entity types.</span></span>

<span data-ttu-id="9b49f-120">항상 true 인 웹 응용 프로그램에서 SQL 명령을 실행 하는 경우, 사이트 SQL 삽입 공격 으로부터 보호 하기 위해 예방 조치 수행 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="9b49f-120">As is always true when you execute SQL commands in a web application, you must take precautions to protect your site against SQL injection attacks.</span></span> <span data-ttu-id="9b49f-121">작업을 수행 하는 한 가지 방법은 웹 페이지를 제출한 문자열 SQL 명령으로 해석할 수 없는 되도록 매개 변수가 있는 쿼리를 사용 하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="9b49f-121">One way to do that is to use parameterized queries to make sure that strings submitted by a web page can't be interpreted as SQL commands.</span></span> <span data-ttu-id="9b49f-122">이 자습서에서는 사용자 입력을 쿼리에 통합 하는 경우 매개 변수가 있는 쿼리를 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="9b49f-122">In this tutorial you'll use parameterized queries when integrating user input into a query.</span></span>

## <a name="call-a-query-that-returns-entities"></a><span data-ttu-id="9b49f-123">엔터티를 반환 하는 쿼리를 호출 합니다.</span><span class="sxs-lookup"><span data-stu-id="9b49f-123">Call a query that returns entities</span></span>

<span data-ttu-id="9b49f-124">`DbSet<TEntity>` 클래스 형식의 엔터티를 반환 하는 쿼리를 실행 하는 데 사용할 수 있는 메서드를 제공 `TEntity`합니다.</span><span class="sxs-lookup"><span data-stu-id="9b49f-124">The `DbSet<TEntity>` class provides a method that you can use to execute a query that returns an entity of type `TEntity`.</span></span> <span data-ttu-id="9b49f-125">이 방법을 보려면 있습니다의 코드를 변경 합니다는 `Details` 부서 컨트롤러의 메서드.</span><span class="sxs-lookup"><span data-stu-id="9b49f-125">To see how this works you'll change the code in the `Details` method of the Department controller.</span></span>

<span data-ttu-id="9b49f-126">*DepartmentsController.cs*에 `Details` 메서드를 사용 하는 부서를 검색 하는 코드를 대체 한 `FromSql` 다음 강조 표시 된 코드에 나와 있는 것 처럼 메서드 호출:</span><span class="sxs-lookup"><span data-stu-id="9b49f-126">In *DepartmentsController.cs*, in the `Details` method, replace the code that retrieves a department with a `FromSql` method call, as shown in the following highlighted code:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_RawSQL&highlight=8,9,10,13)]

<span data-ttu-id="9b49f-127">새 코드는 올바르게 작동 하는지 확인 하려면 선택은 **부서** 탭 한 다음 **세부 정보** 부서 중 하나에 대 한 합니다.</span><span class="sxs-lookup"><span data-stu-id="9b49f-127">To verify that the new code works correctly, select the **Departments** tab and then **Details** for one of the departments.</span></span>

![부서 세부 정보](advanced/_static/department-details.png)

## <a name="call-a-query-that-returns-other-types"></a><span data-ttu-id="9b49f-129">다른 형식을 반환 하는 쿼리를 호출 합니다.</span><span class="sxs-lookup"><span data-stu-id="9b49f-129">Call a query that returns other types</span></span>

<span data-ttu-id="9b49f-130">각 등록 날짜에 대 한 학생 수를 보여 주는 정보 페이지에 대 한 학생 통계 표에서 이전 만들었습니다.</span><span class="sxs-lookup"><span data-stu-id="9b49f-130">Earlier you created a student statistics grid for the About page that showed the number of students for each enrollment date.</span></span> <span data-ttu-id="9b49f-131">학생 엔터티 집합에서 데이터를 가져왔습니다 (`_context.Students`) LINQ의 목록으로 결과 프로젝션 하는 데 사용 하 고 `EnrollmentDateGroup` 모델 개체를 보고 합니다.</span><span class="sxs-lookup"><span data-stu-id="9b49f-131">You got the data from the Students entity set (`_context.Students`) and used LINQ to project the results into a list of `EnrollmentDateGroup` view model objects.</span></span> <span data-ttu-id="9b49f-132">LINQ를 사용 하지 않고 자체 sql 문을 작성 하려는 가정 합니다.</span><span class="sxs-lookup"><span data-stu-id="9b49f-132">Suppose you want to write the SQL itself rather than using LINQ.</span></span> <span data-ttu-id="9b49f-133">SQL 쿼리를 실행 해야 작업을 수행 하는 이외의 형식을 반환 엔터티 개체입니다.</span><span class="sxs-lookup"><span data-stu-id="9b49f-133">To do that you need to run a SQL query that returns something other than entity objects.</span></span> <span data-ttu-id="9b49f-134">EF Core 1.0에서 작업을 수행 하는 한 가지 방법은 ADO.NET 코드를 작성 하 고 EF에서 데이터베이스 연결을 가져옵니다.</span><span class="sxs-lookup"><span data-stu-id="9b49f-134">In EF Core 1.0, one way to do that is write ADO.NET code and get the database connection from EF.</span></span>

<span data-ttu-id="9b49f-135">*HomeController.cs*, 대체 된 `About` 메서드를 다음 코드로:</span><span class="sxs-lookup"><span data-stu-id="9b49f-135">In *HomeController.cs*, replace the `About` method with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/HomeController.cs?name=snippet_UseRawSQL&highlight=3-32)]

<span data-ttu-id="9b49f-136">사용 하 여 추가 문:</span><span class="sxs-lookup"><span data-stu-id="9b49f-136">Add a using statement:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/HomeController.cs?name=snippet_Usings2)]

<span data-ttu-id="9b49f-137">응용 프로그램을 실행 하 고 정보 페이지로 이동 합니다.</span><span class="sxs-lookup"><span data-stu-id="9b49f-137">Run the app and go to the About page.</span></span> <span data-ttu-id="9b49f-138">이전과 동일한 데이터를 표시 합니다.</span><span class="sxs-lookup"><span data-stu-id="9b49f-138">It displays the same data it did before.</span></span>

![페이지 정보](advanced/_static/about.png)

## <a name="call-an-update-query"></a><span data-ttu-id="9b49f-140">업데이트 쿼리를 호출 합니다.</span><span class="sxs-lookup"><span data-stu-id="9b49f-140">Call an update query</span></span>

<span data-ttu-id="9b49f-141">Contoso 대학 관리자가 모든 과정에 대 한 크레딧의 수를 변경 하는 등 데이터베이스의 전역 변경을 수행 하려는 가정 합니다.</span><span class="sxs-lookup"><span data-stu-id="9b49f-141">Suppose Contoso University administrators want to perform global changes in the database, such as changing the number of credits for every course.</span></span> <span data-ttu-id="9b49f-142">대학교 많은 수의 과목 있으면 수 없기 엔터티로 모두 검색 하 고 개별적으로 변경 하 여 효율적입니다.</span><span class="sxs-lookup"><span data-stu-id="9b49f-142">If the university has a large number of courses, it would be inefficient to retrieve them all as entities and change them individually.</span></span> <span data-ttu-id="9b49f-143">이 섹션의 사용자는 모든 과정에 대 한 크레딧의 수를 변경 하는 인수를 지정할 수 있게 하는 웹 페이지를 구현 하 고 SQL UPDATE 문을 실행 하 여 변경할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="9b49f-143">In this section you'll implement a web page that enables the user to specify a factor by which to change the number of credits for all courses, and you'll make the change by executing a SQL UPDATE statement.</span></span> <span data-ttu-id="9b49f-144">웹 페이지는 다음 그림과 같이 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="9b49f-144">The web page will look like the following illustration:</span></span>

![업데이트 과정 크레딧 페이지](advanced/_static/update-credits.png)

<span data-ttu-id="9b49f-146">*CoursesContoller.cs*, HttpGet 및 HttpPost UpdateCourseCredits 메서드 추가:</span><span class="sxs-lookup"><span data-stu-id="9b49f-146">In *CoursesContoller.cs*, add UpdateCourseCredits methods for HttpGet and HttpPost:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_UpdateGet)]

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_UpdatePost)]

<span data-ttu-id="9b49f-147">컨트롤러 HttpGet 요청을 처리할 때 아무 것도 반환에 `ViewData["RowsAffected"]`, 앞의 그림에 나와 있는 것 처럼 뷰의 빈 텍스트 상자 및 전송 단추를 표시 합니다.</span><span class="sxs-lookup"><span data-stu-id="9b49f-147">When the controller processes an HttpGet request, nothing is returned in `ViewData["RowsAffected"]`, and the view displays an empty text box and a submit button, as shown in the preceding illustration.</span></span>

<span data-ttu-id="9b49f-148">경우는 **업데이트** 단추를 클릭 하면 HttpPost 메서드를 호출 하 고 승수에 텍스트 상자에 입력 된 값입니다.</span><span class="sxs-lookup"><span data-stu-id="9b49f-148">When the **Update** button is clicked, the HttpPost method is called, and multiplier has the value entered in the text box.</span></span> <span data-ttu-id="9b49f-149">다음 코드를 실행 과정을 업데이트 하 고 보기에 영향을 받는 행 수를 반환 하는 SQL `ViewData`합니다.</span><span class="sxs-lookup"><span data-stu-id="9b49f-149">The code then executes the SQL that updates courses and returns the number of affected rows to the view in `ViewData`.</span></span> <span data-ttu-id="9b49f-150">뷰를 가져옵니다는 `RowsAffected` 값을 업데이트 된 행의 수를 표시 합니다.</span><span class="sxs-lookup"><span data-stu-id="9b49f-150">When the view gets a `RowsAffected` value, it displays the number of rows updated.</span></span>

<span data-ttu-id="9b49f-151">**솔루션 탐색기**를 마우스 오른쪽 단추로 클릭는 *뷰/Courses* 폴더를 마우스 클릭 한 다음 **추가 > 새 항목**합니다.</span><span class="sxs-lookup"><span data-stu-id="9b49f-151">In **Solution Explorer**, right-click the *Views/Courses* folder, and then click **Add > New Item**.</span></span>

<span data-ttu-id="9b49f-152">에 **새 항목 추가** 대화 상자에서 클릭 **ASP.NET** 아래 **설치 됨** 왼쪽된 창에서 클릭 **MVC 뷰 페이지**, 새뷰이름을지정하고 *UpdateCourseCredits.cshtml*합니다.</span><span class="sxs-lookup"><span data-stu-id="9b49f-152">In the **Add New Item** dialog, click **ASP.NET** under **Installed** in the left pane, click **MVC View Page**, and name the new view *UpdateCourseCredits.cshtml*.</span></span>

<span data-ttu-id="9b49f-153">*Views/Courses/UpdateCourseCredits.cshtml*, 템플릿 코드를 다음 코드로 바꿉니다.</span><span class="sxs-lookup"><span data-stu-id="9b49f-153">In *Views/Courses/UpdateCourseCredits.cshtml*, replace the template code with the following code:</span></span>

[!code-html[Main](intro/samples/cu/Views/Courses/UpdateCourseCredits.cshtml)]

<span data-ttu-id="9b49f-154">실행의 `UpdateCourseCredits` 메서드를 선택 하 여는 **Courses** 탭에서 다음 추가 "/ UpdateCourseCredits" 브라우저의 주소 표시줄에 URL의 끝에 (예: `http://localhost:5813/Courses/UpdateCourseCredits`).</span><span class="sxs-lookup"><span data-stu-id="9b49f-154">Run the `UpdateCourseCredits` method by selecting the **Courses** tab, then adding "/UpdateCourseCredits" to the end of the URL in the browser's address bar (for example: `http://localhost:5813/Courses/UpdateCourseCredits`).</span></span> <span data-ttu-id="9b49f-155">텍스트 상자에 숫자를 입력 합니다.</span><span class="sxs-lookup"><span data-stu-id="9b49f-155">Enter a number in the text box:</span></span>

![업데이트 과정 크레딧 페이지](advanced/_static/update-credits.png)

<span data-ttu-id="9b49f-157">**업데이트**를 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="9b49f-157">Click **Update**.</span></span> <span data-ttu-id="9b49f-158">영향을 받는 행 수가 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="9b49f-158">You see the number of rows affected:</span></span>

![업데이트 과정 크레딧 영향을 받는 행을 페이지](advanced/_static/update-credits-rows-affected.png)

<span data-ttu-id="9b49f-160">클릭 **목록으로 돌아가기** 크레딧의 수정 된 번호로 과정의 목록을 볼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="9b49f-160">Click **Back to List** to see the list of courses with the revised number of credits.</span></span>

<span data-ttu-id="9b49f-161">Note 프로덕션 코드를 업데이트 하는 항상 결과 유효한 데이터를 확인 합니다.</span><span class="sxs-lookup"><span data-stu-id="9b49f-161">Note that production code would ensure that updates always result in valid data.</span></span> <span data-ttu-id="9b49f-162">여기에 표시 된 간소화 된 코드 수 크레딧 충분히 결과적으로 5 보다 큰 수를 곱합니다.</span><span class="sxs-lookup"><span data-stu-id="9b49f-162">The simplified code shown here could multiply the number of credits enough to result in numbers greater than 5.</span></span> <span data-ttu-id="9b49f-163">(의 `Credits` 속성에는 `[Range(0, 5)]` 특성입니다.) 업데이트 쿼리 작동 하지만 잘못 된 데이터 크레딧 수는 5 개 가정 하는 시스템의 다른 부분에서 예기치 않은 결과가 발생할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="9b49f-163">(The `Credits` property has a `[Range(0, 5)]` attribute.) The update query would work but the invalid data could cause unexpected results in other parts of the system that assume the number of credits is 5 or less.</span></span>

<span data-ttu-id="9b49f-164">원시 SQL 쿼리 하는 방법에 대 한 자세한 내용은 참조 [원시 SQL 쿼리](https://docs.microsoft.com/ef/core/querying/raw-sql)합니다.</span><span class="sxs-lookup"><span data-stu-id="9b49f-164">For more information about raw SQL queries, see [Raw SQL Queries](https://docs.microsoft.com/ef/core/querying/raw-sql).</span></span>

## <a name="examine-sql-sent-to-the-database"></a><span data-ttu-id="9b49f-165">SQL 데이터베이스에 전송 검사</span><span class="sxs-lookup"><span data-stu-id="9b49f-165">Examine SQL sent to the database</span></span>

<span data-ttu-id="9b49f-166">데이터베이스에 전송 되는 실제 SQL 쿼리를 볼 수 많은 도움이 됩니다.</span><span class="sxs-lookup"><span data-stu-id="9b49f-166">Sometimes it's helpful to be able to see the actual SQL queries that are sent to the database.</span></span> <span data-ttu-id="9b49f-167">ASP.NET Core에 대 한 기본 제공 로깅 기능 쿼리 및 업데이트에 대 한 SQL을 포함 하는 로그를 쓰지 EF 코어에서 자동으로 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="9b49f-167">Built-in logging functionality for ASP.NET Core is automatically used by EF Core to write logs that contain the SQL for queries and updates.</span></span> <span data-ttu-id="9b49f-168">이 섹션에서는 SQL 로깅의 몇 가지 예를 볼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="9b49f-168">In this section you'll see some examples of SQL logging.</span></span>

<span data-ttu-id="9b49f-169">열기 *StudentsController.cs* 및는 `Details` 에 중단점을 설정 하는 메서드는 `if (student == null)` 문.</span><span class="sxs-lookup"><span data-stu-id="9b49f-169">Open *StudentsController.cs* and in the `Details` method set a breakpoint on the `if (student == null)` statement.</span></span>

<span data-ttu-id="9b49f-170">디버그 모드에서 앱을 실행 하 고 학생에 대 한 세부 정보 페이지로 이동 합니다.</span><span class="sxs-lookup"><span data-stu-id="9b49f-170">Run the app in debug mode, and go to the Details page for a student.</span></span>

<span data-ttu-id="9b49f-171">이동 하는 **출력** 디버그 표시 창 출력을 보고, 쿼리를 참조 하십시오.</span><span class="sxs-lookup"><span data-stu-id="9b49f-171">Go to the **Output** window showing debug output, and you see the query:</span></span>

```
Microsoft.EntityFrameworkCore.Database.Command:Information: Executed DbCommand (56ms) [Parameters=[@__id_0='?'], CommandType='Text', CommandTimeout='30']
SELECT TOP(2) [s].[ID], [s].[Discriminator], [s].[FirstName], [s].[LastName], [s].[EnrollmentDate]
FROM [Person] AS [s]
WHERE ([s].[Discriminator] = N'Student') AND ([s].[ID] = @__id_0)
ORDER BY [s].[ID]
Microsoft.EntityFrameworkCore.Database.Command:Information: Executed DbCommand (122ms) [Parameters=[@__id_0='?'], CommandType='Text', CommandTimeout='30']
SELECT [s.Enrollments].[EnrollmentID], [s.Enrollments].[CourseID], [s.Enrollments].[Grade], [s.Enrollments].[StudentID], [e.Course].[CourseID], [e.Course].[Credits], [e.Course].[DepartmentID], [e.Course].[Title]
FROM [Enrollment] AS [s.Enrollments]
INNER JOIN [Course] AS [e.Course] ON [s.Enrollments].[CourseID] = [e.Course].[CourseID]
INNER JOIN (
    SELECT TOP(1) [s0].[ID]
    FROM [Person] AS [s0]
    WHERE ([s0].[Discriminator] = N'Student') AND ([s0].[ID] = @__id_0)
    ORDER BY [s0].[ID]
) AS [t] ON [s.Enrollments].[StudentID] = [t].[ID]
ORDER BY [t].[ID]
```

<span data-ttu-id="9b49f-172">알 수 여기 놀라운 수 있는: 최대 2 개의 행을 선택 하는 SQL (`TOP(2)`) Person 테이블 로부터 합니다.</span><span class="sxs-lookup"><span data-stu-id="9b49f-172">You'll notice something here that might surprise you: the SQL selects up to 2 rows (`TOP(2)`) from the Person table.</span></span> <span data-ttu-id="9b49f-173">`SingleOrDefaultAsync` 메서드는 서버에서 1 개 행으로 확인 되지 않았습니다.</span><span class="sxs-lookup"><span data-stu-id="9b49f-173">The `SingleOrDefaultAsync` method doesn't resolve to 1 row on the server.</span></span> <span data-ttu-id="9b49f-174">그 이유는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="9b49f-174">Here's why:</span></span>

* <span data-ttu-id="9b49f-175">쿼리에서 여러 행을 반환 하는 경우이 메서드는 null을 반환 합니다.</span><span class="sxs-lookup"><span data-stu-id="9b49f-175">If the query would return multiple rows, the method returns null.</span></span>
* <span data-ttu-id="9b49f-176">쿼리에서 여러 행을 반환 여부를 확인 하려면 EF이 2 개 이상 반환 하는 경우를 확인 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="9b49f-176">To determine whether the query would return multiple rows, EF has to check if it returns at least 2.</span></span>

<span data-ttu-id="9b49f-177">디버그 모드를 사용 하에 로깅 출력을 보려면 중단점에서 중지 없는 참고는 **출력** 창.</span><span class="sxs-lookup"><span data-stu-id="9b49f-177">Note that you don't have to use debug mode and stop at a breakpoint to get logging output in the **Output** window.</span></span> <span data-ttu-id="9b49f-178">결과 확인 하려는 시점 로깅을 중지할 수 편리한 방법 뿐입니다.</span><span class="sxs-lookup"><span data-stu-id="9b49f-178">It's just a convenient way to stop the logging at the point you want to look at the output.</span></span> <span data-ttu-id="9b49f-179">로깅 계속 하는 경우 그럴 필요가, 하 한에 관심이 파트 찾을 돌아가기 스크롤해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="9b49f-179">If you don't do that, logging continues and you have to scroll back to find the parts you're interested in.</span></span>

## <a name="repository-and-unit-of-work-patterns"></a><span data-ttu-id="9b49f-180">작업 패턴의 단위 및 저장소</span><span class="sxs-lookup"><span data-stu-id="9b49f-180">Repository and unit of work patterns</span></span>

<span data-ttu-id="9b49f-181">대부분의 개발자는 Entity Framework와 함께 사용할 수 있는 코드 주위에서 래퍼로 저장소와 단위 작업 패턴을 구현 하는 코드를 작성 합니다.</span><span class="sxs-lookup"><span data-stu-id="9b49f-181">Many developers write code to implement the repository and unit of work patterns as a wrapper around code that works with the Entity Framework.</span></span> <span data-ttu-id="9b49f-182">이러한 패턴은 데이터 액세스 계층 및 응용 프로그램의 비즈니스 논리 계층 간에 추상화 계층을 만드는 데 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="9b49f-182">These patterns are intended to create an abstraction layer between the data access layer and the business logic layer of an application.</span></span> <span data-ttu-id="9b49f-183">이러한 패턴을 구현 하는 데이터 저장소의 변경 내용 으로부터 응용 프로그램을 분리 하는 데 도움이 및 자동화 된 단위 테스트 또는 테스트 기반 개발 (TDD)으로 기여할 수입니다.</span><span class="sxs-lookup"><span data-stu-id="9b49f-183">Implementing these patterns can help insulate your application from changes in the data store and can facilitate automated unit testing or test-driven development (TDD).</span></span> <span data-ttu-id="9b49f-184">그러나 이러한 패턴을 구현 하는 추가 코드를 작성은 항상 같은 여러 가지 이유로 EF를 사용 하는 응용 프로그램에 적합 합니다.</span><span class="sxs-lookup"><span data-stu-id="9b49f-184">However, writing additional code to implement these patterns isn't always the best choice for applications that use EF, for several reasons:</span></span>

* <span data-ttu-id="9b49f-185">EF 컨텍스트 클래스 자체 데이터 저장소 관련 코드에서 코드를 분리 합니다.</span><span class="sxs-lookup"><span data-stu-id="9b49f-185">The EF context class itself insulates your code from data-store-specific code.</span></span>

* <span data-ttu-id="9b49f-186">EF 컨텍스트 클래스 EF를 사용 하 여 수행 하는 데이터베이스에 대 한 작업 단위가 클래스 업데이트 작동할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="9b49f-186">The EF context class can act as a unit-of-work class for database updates that you do using EF.</span></span>

* <span data-ttu-id="9b49f-187">EF는 리포지토리에 코드를 작성 하지 않고도 TDD를 구현 하기 위한 기능이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="9b49f-187">EF includes features for implementing TDD without writing repository code.</span></span>

<span data-ttu-id="9b49f-188">저장소와 단위 작업 패턴을 구현 하는 방법에 대 한 정보를 참조 하십시오. [이 자습서 시리즈의 Entity Framework 5 버전](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application)합니다.</span><span class="sxs-lookup"><span data-stu-id="9b49f-188">For information about how to implement the repository and unit of work patterns, see [the Entity Framework 5 version of this tutorial series](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application).</span></span>

<span data-ttu-id="9b49f-189">Entity Framework Core 테스트에 사용할 수 있는 메모리 내 데이터베이스 공급자를 구현 합니다.</span><span class="sxs-lookup"><span data-stu-id="9b49f-189">Entity Framework Core implements an in-memory database provider that can be used for testing.</span></span> <span data-ttu-id="9b49f-190">자세한 내용은 참조 [with InMemory 테스트](https://docs.microsoft.com/ef/core/miscellaneous/testing/in-memory)합니다.</span><span class="sxs-lookup"><span data-stu-id="9b49f-190">For more information, see [Testing with InMemory](https://docs.microsoft.com/ef/core/miscellaneous/testing/in-memory).</span></span>

## <a name="automatic-change-detection"></a><span data-ttu-id="9b49f-191">자동 변경 내용 검색</span><span class="sxs-lookup"><span data-stu-id="9b49f-191">Automatic change detection</span></span>

<span data-ttu-id="9b49f-192">Entity Framework 엔터티의 현재 값과 원래 값을 비교 하 여 엔터티의 변경 된 방식을 (및 따라서로 업데이트를 데이터베이스에 전송 해야 할)를 결정 합니다.</span><span class="sxs-lookup"><span data-stu-id="9b49f-192">The Entity Framework determines how an entity has changed (and therefore which updates need to be sent to the database) by comparing the current values of an entity with the original values.</span></span> <span data-ttu-id="9b49f-193">엔터티 쿼리 하거나 연결 하는 경우 원래 값 저장 됩니다.</span><span class="sxs-lookup"><span data-stu-id="9b49f-193">The original values are stored when the entity is queried or attached.</span></span> <span data-ttu-id="9b49f-194">자동 변경 내용 검색 발생 하는 메서드 중 일부는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="9b49f-194">Some of the methods that cause automatic change detection are the following:</span></span>

* <span data-ttu-id="9b49f-195">DbContext.SaveChanges</span><span class="sxs-lookup"><span data-stu-id="9b49f-195">DbContext.SaveChanges</span></span>

* <span data-ttu-id="9b49f-196">DbContext.Entry</span><span class="sxs-lookup"><span data-stu-id="9b49f-196">DbContext.Entry</span></span>

* <span data-ttu-id="9b49f-197">ChangeTracker.Entries</span><span class="sxs-lookup"><span data-stu-id="9b49f-197">ChangeTracker.Entries</span></span>

<span data-ttu-id="9b49f-198">많은 엔터티를 추적 하 고 있는 경우 루프에서 여러 번 다음이 방법 중 하나 호출 하면 변경 내용 자동 감지를 사용 하 여 일시적으로 해제 하 여 성능 향상을 발생할 수 있습니다는 `ChangeTracker.AutoDetectChangesEnabled` 속성입니다.</span><span class="sxs-lookup"><span data-stu-id="9b49f-198">If you're tracking a large number of entities and you call one of these methods many times in a loop, you might get significant performance improvements by temporarily turning off automatic change detection using the `ChangeTracker.AutoDetectChangesEnabled` property.</span></span> <span data-ttu-id="9b49f-199">예:</span><span class="sxs-lookup"><span data-stu-id="9b49f-199">For example:</span></span>

```csharp
_context.ChangeTracker.AutoDetectChangesEnabled = false;
```

## <a name="entity-framework-core-source-code-and-development-plans"></a><span data-ttu-id="9b49f-200">Entity Framework Core 소스 코드 및 개발 계획</span><span class="sxs-lookup"><span data-stu-id="9b49f-200">Entity Framework Core source code and development plans</span></span>

<span data-ttu-id="9b49f-201">에 Entity Framework Core 소스를 [https://github.com/aspnet/EntityFrameworkCore](https://github.com/aspnet/EntityFrameworkCore)합니다.</span><span class="sxs-lookup"><span data-stu-id="9b49f-201">The Entity Framework Core source is at [https://github.com/aspnet/EntityFrameworkCore](https://github.com/aspnet/EntityFrameworkCore).</span></span> <span data-ttu-id="9b49f-202">야간 빌드, 문제 추적, 기능 사양, 디자인 회의 노트, EF 핵심 리포지토리 포함 및 [이후 개발에 대 한 로드맵](https://github.com/aspnet/EntityFrameworkCore/wiki/Roadmap)합니다.</span><span class="sxs-lookup"><span data-stu-id="9b49f-202">The EF Core repository contains nightly builds, issue tracking, feature specs, design meeting notes, and the [the roadmap for future development](https://github.com/aspnet/EntityFrameworkCore/wiki/Roadmap).</span></span> <span data-ttu-id="9b49f-203">파일 또는 기여 버그를 찾을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="9b49f-203">You can file or find bugs, and contribute.</span></span>

<span data-ttu-id="9b49f-204">소스 코드를 연 하지만 Entity Framework 코어는 Microsoft 제품으로 완전히 지원 됩니다.</span><span class="sxs-lookup"><span data-stu-id="9b49f-204">Although the source code is open, Entity Framework Core is fully supported as a Microsoft product.</span></span> <span data-ttu-id="9b49f-205">Microsoft Entity Framework 팀 기여 허용 되는 제어를 유지 하 고 각 릴리스의 품질을 보장할 수 있는 모든 코드 변경 내용을 테스트 합니다.</span><span class="sxs-lookup"><span data-stu-id="9b49f-205">The Microsoft Entity Framework team keeps control over which contributions are accepted and tests all code changes to ensure the quality of each release.</span></span>

## <a name="reverse-engineer-from-existing-database"></a><span data-ttu-id="9b49f-206">기존 데이터베이스 리버스 엔지니어링</span><span class="sxs-lookup"><span data-stu-id="9b49f-206">Reverse engineer from existing database</span></span>

<span data-ttu-id="9b49f-207">리버스 엔지니어링 하는 기존 데이터베이스에서 엔터티 클래스를 포함 하 여 데이터 모델을 사용 하 여는 [스 캐 폴드 dbcontext](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell#scaffold-dbcontext) 명령입니다.</span><span class="sxs-lookup"><span data-stu-id="9b49f-207">To reverse engineer a data model including entity classes from an existing database, use the [scaffold-dbcontext](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell#scaffold-dbcontext) command.</span></span> <span data-ttu-id="9b49f-208">참조는 [-시작 자습서](https://docs.microsoft.com/ef/core/get-started/aspnetcore/existing-db)합니다.</span><span class="sxs-lookup"><span data-stu-id="9b49f-208">See the [getting-started tutorial](https://docs.microsoft.com/ef/core/get-started/aspnetcore/existing-db).</span></span>

<a id="dynamic-linq"></a>
## <a name="use-dynamic-linq-to-simplify-sort-selection-code"></a><span data-ttu-id="9b49f-209">동적 LINQ를 사용 하 여 정렬 선택 코드를 단순화 합니다.</span><span class="sxs-lookup"><span data-stu-id="9b49f-209">Use dynamic LINQ to simplify sort selection code</span></span>

<span data-ttu-id="9b49f-210">[이 시리즈의 세 번째 자습서](sort-filter-page.md) 열 이름은 하드 코딩 하 여 LINQ 코드를 작성 하는 방법을 보여 줍니다는 `switch` 문.</span><span class="sxs-lookup"><span data-stu-id="9b49f-210">The [third tutorial in this series](sort-filter-page.md) shows how to write LINQ code by hard-coding column names in a `switch` statement.</span></span> <span data-ttu-id="9b49f-211">중에서 선택 하려면 두 개의 열이 정상적으로 작동 하지만 코드 자세한 정보를 가져올 수 많은 열이 있는 경우.</span><span class="sxs-lookup"><span data-stu-id="9b49f-211">With two columns to choose from, this works fine, but if you have many columns the code could get verbose.</span></span> <span data-ttu-id="9b49f-212">이러한 문제를 해결 하기 위해 사용할 수 있습니다는 `EF.Property` 메서드를 문자열로 속성의 이름을 지정 합니다.</span><span class="sxs-lookup"><span data-stu-id="9b49f-212">To solve that problem, you can use the `EF.Property` method to specify the name of the property as a string.</span></span> <span data-ttu-id="9b49f-213">이 방법은 실행 하려면 대체는 `Index` 에서 메서드는 `StudentsController` 를 다음 코드로 합니다.</span><span class="sxs-lookup"><span data-stu-id="9b49f-213">To try out this approach, replace the `Index` method in the `StudentsController` with the following code.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_DynamicLinq)]

## <a name="next-steps"></a><span data-ttu-id="9b49f-214">다음 단계</span><span class="sxs-lookup"><span data-stu-id="9b49f-214">Next steps</span></span>

<span data-ttu-id="9b49f-215">이 일련의 ASP.NET MVC 응용 프로그램에서 Entity Framework Core를 사용 하는 자습서를 완료 했습니다.</span><span class="sxs-lookup"><span data-stu-id="9b49f-215">This completes this series of tutorials on using the Entity Framework Core in an ASP.NET MVC application.</span></span>

<span data-ttu-id="9b49f-216">EF 코어에 대 한 자세한 내용은 참조는 [Entity Framework Core 설명서](https://docs.microsoft.com/ef/core)합니다.</span><span class="sxs-lookup"><span data-stu-id="9b49f-216">For more information about EF Core, see the [Entity Framework Core documentation](https://docs.microsoft.com/ef/core).</span></span> <span data-ttu-id="9b49f-217">책 제공 됩니다.: [동작에서 Entity Framework Core](https://www.manning.com/books/entity-framework-core-in-action)합니다.</span><span class="sxs-lookup"><span data-stu-id="9b49f-217">A book is also available: [Entity Framework Core in Action](https://www.manning.com/books/entity-framework-core-in-action).</span></span>

<span data-ttu-id="9b49f-218">웹 앱을 배포 하는 방법에 대 한 정보를 참조 하십시오. [호스트 하 고 배포](xref:host-and-deploy/index)합니다.</span><span class="sxs-lookup"><span data-stu-id="9b49f-218">For information on how to deploy a web app, see [Host and deploy](xref:host-and-deploy/index).</span></span>

<span data-ttu-id="9b49f-219">인증 및 권한, 같은 ASP.NET Core MVC와 관련 된 다른 항목에 대 한 정보에 대 한 참조는 [ASP.NET Core 설명서](https://docs.microsoft.com/aspnet/core/)합니다.</span><span class="sxs-lookup"><span data-stu-id="9b49f-219">For information about other topics related to ASP.NET Core MVC, such as authentication and authorization, see the [ASP.NET Core documentation](https://docs.microsoft.com/aspnet/core/).</span></span>

## <a name="acknowledgments"></a><span data-ttu-id="9b49f-220">승인</span><span class="sxs-lookup"><span data-stu-id="9b49f-220">Acknowledgments</span></span>

<span data-ttu-id="9b49f-221">Tom Dykstra 및 Rick Anderson (twitter @RickAndMSFT)이이 자습서를 작성 합니다.</span><span class="sxs-lookup"><span data-stu-id="9b49f-221">Tom Dykstra and Rick Anderson (twitter @RickAndMSFT) wrote this tutorial.</span></span> <span data-ttu-id="9b49f-222">Rowan, 디에고 Vega 및 Entity Framework 팀의 다른 멤버가 코드 검토와 보조 및 자습서에 대 한 코드 작성 했다면 하는 동안 발생 하는 문제를 디버그 하는 데 도움을 준 합니다.</span><span class="sxs-lookup"><span data-stu-id="9b49f-222">Rowan Miller, Diego Vega, and other members of the Entity Framework team assisted with code reviews and helped debug issues that arose while we were writing code for the tutorials.</span></span>

## <a name="common-errors"></a><span data-ttu-id="9b49f-223">일반적인 오류</span><span class="sxs-lookup"><span data-stu-id="9b49f-223">Common errors</span></span>  

### <a name="contosouniversitydll-used-by-another-process"></a><span data-ttu-id="9b49f-224">다른 프로세스에서 사용 하는 ContosoUniversity.dll</span><span class="sxs-lookup"><span data-stu-id="9b49f-224">ContosoUniversity.dll used by another process</span></span>

<span data-ttu-id="9b49f-225">오류 메시지:</span><span class="sxs-lookup"><span data-stu-id="9b49f-225">Error message:</span></span>

> <span data-ttu-id="9b49f-226">열 수 없습니다 '... bin\Debug\netcoreapp1.0\ContosoUniversity.dll' 쓰기용-' 프로세스는 파일에 액세스할 수 없습니다 '... \bin\Debug\netcoreapp1.0\ContosoUniversity.dll' 다른 프로세스에서 사용 되 고 있으므로 합니다.</span><span class="sxs-lookup"><span data-stu-id="9b49f-226">Cannot open '...bin\Debug\netcoreapp1.0\ContosoUniversity.dll' for writing -- 'The process cannot access the file '...\bin\Debug\netcoreapp1.0\ContosoUniversity.dll' because it is being used by another process.</span></span>

<span data-ttu-id="9b49f-227">해결책:</span><span class="sxs-lookup"><span data-stu-id="9b49f-227">Solution:</span></span>

<span data-ttu-id="9b49f-228">IIS express 사이트를 중지 합니다.</span><span class="sxs-lookup"><span data-stu-id="9b49f-228">Stop the site in IIS Express.</span></span> <span data-ttu-id="9b49f-229">로 이동는 Windows 시스템 트레이 찾기 IIS Express 및 해당 아이콘을 마우스 오른쪽 단추로 클릭, Contoso 대학 사이트를 클릭 한 다음 **중지 사이트**합니다.</span><span class="sxs-lookup"><span data-stu-id="9b49f-229">Go to the Windows System Tray, find IIS Express and right-click its icon, select the Contoso University site, and then click **Stop Site**.</span></span>

### <a name="migration-scaffolded-with-no-code-in-up-and-down-methods"></a><span data-ttu-id="9b49f-230">메서드 위쪽 및 아래쪽에서 코드가 없는 스 캐 폴드 된 마이그레이션</span><span class="sxs-lookup"><span data-stu-id="9b49f-230">Migration scaffolded with no code in Up and Down methods</span></span>

<span data-ttu-id="9b49f-231">가능한 원인:</span><span class="sxs-lookup"><span data-stu-id="9b49f-231">Possible cause:</span></span>

<span data-ttu-id="9b49f-232">자동으로 EF CLI 명령을 닫고 코드 파일을 저장 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="9b49f-232">The EF CLI commands don't automatically close and save code files.</span></span> <span data-ttu-id="9b49f-233">실행할 때 변경 하지 않은 경우는 `migrations add` 명령, EF 변경 내용을 찾을 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="9b49f-233">If you have unsaved changes when you run the `migrations add` command, EF won't find your changes.</span></span>

<span data-ttu-id="9b49f-234">해결책:</span><span class="sxs-lookup"><span data-stu-id="9b49f-234">Solution:</span></span>

<span data-ttu-id="9b49f-235">실행의 `migrations remove` 명령 코드 변경 내용을 저장 하 고 다시 실행의 `migrations add` 명령입니다.</span><span class="sxs-lookup"><span data-stu-id="9b49f-235">Run the `migrations remove` command, save your code changes and rerun the `migrations add` command.</span></span>

### <a name="errors-while-running-database-update"></a><span data-ttu-id="9b49f-236">실행 중인 데이터베이스를 업데이트 하는 동안 오류</span><span class="sxs-lookup"><span data-stu-id="9b49f-236">Errors while running database update</span></span>

<span data-ttu-id="9b49f-237">기존 데이터를 포함 하는 데이터베이스의 스키마를 변경 하는 경우 다른 오류를 얻을 수는 것이 불가능 합니다.</span><span class="sxs-lookup"><span data-stu-id="9b49f-237">It's possible to get other errors when making schema changes in a database that has existing data.</span></span> <span data-ttu-id="9b49f-238">마이그레이션 오류를 해결할 수 없으면 발생 하는 경우 연결 문자열에 데이터베이스 이름을 변경 하거나 데이터베이스를 삭제 합니다.</span><span class="sxs-lookup"><span data-stu-id="9b49f-238">If you get migration errors you can't resolve, you can either change the database name in the connection string or delete the database.</span></span> <span data-ttu-id="9b49f-239">새 데이터베이스와 함께 마이그레이션할 데이터가 더 않으며 update-database 명령을 작업이 오류 없이 완료 가능성이 훨씬 더 높습니다.</span><span class="sxs-lookup"><span data-stu-id="9b49f-239">With a new database, there's no data to migrate, and the update-database command is much more likely to complete without errors.</span></span>

<span data-ttu-id="9b49f-240">가장 간단한 방법은 데이터베이스의 이름을 바꾸려면 *appsettings.json*합니다.</span><span class="sxs-lookup"><span data-stu-id="9b49f-240">The simplest approach is to rename the database in *appsettings.json*.</span></span> <span data-ttu-id="9b49f-241">다음에 실행할 때 `database update`, 새 데이터베이스가 생성 됩니다.</span><span class="sxs-lookup"><span data-stu-id="9b49f-241">The next time you run `database update`, a new database will be created.</span></span>

<span data-ttu-id="9b49f-242">SSOX에 데이터베이스를 삭제 하려면 데이터베이스를 마우스 오른쪽 단추로 클릭 합니다 **삭제**, 한 다음는 **데이터베이스 삭제** 대화 상자 선택 **기존 연결 닫기** 클릭 **정상**합니다.</span><span class="sxs-lookup"><span data-stu-id="9b49f-242">To delete a database in SSOX, right-click the database, click **Delete**, and then in the **Delete Database** dialog box select **Close existing connections** and click **OK**.</span></span>

<span data-ttu-id="9b49f-243">실행 CLI를 사용 하 여 데이터베이스를 삭제 하는 `database drop` CLI 명령을:</span><span class="sxs-lookup"><span data-stu-id="9b49f-243">To delete a database by using the CLI, run the `database drop` CLI command:</span></span>

```console
dotnet ef database drop
```

### <a name="error-locating-sql-server-instance"></a><span data-ttu-id="9b49f-244">오류 찾기 SQL Server 인스턴스</span><span class="sxs-lookup"><span data-stu-id="9b49f-244">Error locating SQL Server instance</span></span>

<span data-ttu-id="9b49f-245">오류 메시지:</span><span class="sxs-lookup"><span data-stu-id="9b49f-245">Error Message:</span></span>

> <span data-ttu-id="9b49f-246">SQL Server에 연결을 설정 하는 동안 네트워크 관련 또는 인스턴스 관련 오류가 발생 했습니다.</span><span class="sxs-lookup"><span data-stu-id="9b49f-246">A network-related or instance-specific error occurred while establishing a connection to SQL Server.</span></span> <span data-ttu-id="9b49f-247">서버를 찾을 수 없거나 액세스할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="9b49f-247">The server was not found or was not accessible.</span></span> <span data-ttu-id="9b49f-248">인스턴스 이름이 올바르고 SQL Server가 원격 연결을 허용하도록 구성되어 있는지 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="9b49f-248">Verify that the instance name is correct and that SQL Server is configured to allow remote connections.</span></span> <span data-ttu-id="9b49f-249">(공급자: SQL 네트워크 인터페이스, 오류: 26-서버/인스턴스 지정 된 찾기 오류)</span><span class="sxs-lookup"><span data-stu-id="9b49f-249">(provider: SQL Network Interfaces, error: 26 - Error Locating Server/Instance Specified)</span></span>

<span data-ttu-id="9b49f-250">해결책:</span><span class="sxs-lookup"><span data-stu-id="9b49f-250">Solution:</span></span>

<span data-ttu-id="9b49f-251">연결 문자열을 확인 합니다.</span><span class="sxs-lookup"><span data-stu-id="9b49f-251">Check the connection string.</span></span> <span data-ttu-id="9b49f-252">데이터베이스 파일을 수동으로 삭제 한 경우 새 데이터베이스를 다시 시작 하도록 생성 문자열에 데이터베이스의 이름을 변경 합니다.</span><span class="sxs-lookup"><span data-stu-id="9b49f-252">If you have manually deleted the database file, change the name of the database in the construction string to start over with a new database.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="9b49f-253">이전</span><span class="sxs-lookup"><span data-stu-id="9b49f-253">Previous</span></span>](inheritance.md)
