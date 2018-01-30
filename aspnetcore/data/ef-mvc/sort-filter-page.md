---
title: "ASP.NET Core MVC EF 코어-정렬, 필터, 페이징-3 / 10 인"
author: tdykstra
description: "이 자습서에서는 정렬, 필터링 및 페이징을 ASP.NET 코어 및 Entity Framework 코어를 사용 하 여 페이지 기능을 추가 합니다."
ms.author: tdykstra
ms.date: 03/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-mvc/sort-filter-page
ms.openlocfilehash: feb4a50c9e5602064e7d493b6991485949903f47
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/30/2018
---
# <a name="sorting-filtering-paging-and-grouping---ef-core-with-aspnet-core-mvc-tutorial-3-of-10"></a><span data-ttu-id="5d1c8-103">정렬, 필터링, 페이징 및 그룹화-EF 코어 ASP.NET Core MVC 자습서 (3 / 10)</span><span class="sxs-lookup"><span data-stu-id="5d1c8-103">Sorting, filtering, paging, and grouping - EF Core with ASP.NET Core MVC tutorial (3 of 10)</span></span>

<span data-ttu-id="5d1c8-104">여 [Tom Dykstra](https://github.com/tdykstra) 및 [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="5d1c8-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="5d1c8-105">Contoso 대학 샘플 웹 응용 프로그램에는 Entity Framework Core 및 Visual Studio를 사용 하 여 ASP.NET Core MVC 웹 응용 프로그램을 만드는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-105">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="5d1c8-106">자습서 시리즈에 대 한 정보를 참조 하십시오. [시리즈의 첫 번째 자습서](intro.md)합니다.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-106">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="5d1c8-107">이전 자습서 인 학생 엔터티에 대 한 기본적인 CRUD 작업에 대 한 웹 페이지의 집합을 구현합니다.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-107">In the previous tutorial, you implemented a set of web pages for basic CRUD operations for Student entities.</span></span> <span data-ttu-id="5d1c8-108">이 자습서에서는 정렬, 필터링 및 페이징 기능 학생 인덱스 페이지에 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-108">In this tutorial you'll add sorting, filtering, and paging functionality to the Students Index page.</span></span> <span data-ttu-id="5d1c8-109">단순 그룹화를 수행 하는 페이지를 만들 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-109">You'll also create a page that does simple grouping.</span></span>

<span data-ttu-id="5d1c8-110">다음은 페이지 모양은 완료 되 면입니다.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-110">The following illustration shows what the page will look like when you're done.</span></span> <span data-ttu-id="5d1c8-111">열 머리글은 해당 열으로 정렬 하기 위해 클릭할 수 있는 링크입니다.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-111">The column headings are links that the user can click to sort by that column.</span></span> <span data-ttu-id="5d1c8-112">열 머리글을 반복 해 서 클릭 하면 오름차순 및 내림차순 정렬 순서 사이 토글합니다.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-112">Clicking a column heading repeatedly toggles between ascending and descending sort order.</span></span>

![학생 인덱스 페이지](sort-filter-page/_static/paging.png)

## <a name="add-column-sort-links-to-the-students-index-page"></a><span data-ttu-id="5d1c8-114">학생 인덱스 페이지에 열 정렬 링크 추가</span><span class="sxs-lookup"><span data-stu-id="5d1c8-114">Add Column Sort Links to the Students Index Page</span></span>

<span data-ttu-id="5d1c8-115">학생 인덱스 페이지에 정렬를 추가 하려면 변경 된 `Index` 학생 컨트롤러의 메서드 코드 학생 인덱스 뷰를 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-115">To add sorting to the Student Index page, you'll change the `Index` method of the Students controller and add code to the Student Index view.</span></span>

### <a name="add-sorting-functionality-to-the-index-method"></a><span data-ttu-id="5d1c8-116">인덱스 메서드에 정렬 기능 추가</span><span class="sxs-lookup"><span data-stu-id="5d1c8-116">Add sorting Functionality to the Index method</span></span>

<span data-ttu-id="5d1c8-117">*StudentsController.cs*, 대체 된 `Index` 메서드를 다음 코드로:</span><span class="sxs-lookup"><span data-stu-id="5d1c8-117">In *StudentsController.cs*, replace the `Index` method with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortOnly)]

<span data-ttu-id="5d1c8-118">이 코드는 수신 된 `sortOrder` URL에 쿼리 문자열에서 매개 변수입니다.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-118">This code receives a `sortOrder` parameter from the query string in the URL.</span></span> <span data-ttu-id="5d1c8-119">쿼리 문자열 값은 작업 메서드에 매개 변수로 ASP.NET Core MVC에서 제공 됩니다.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-119">The query string value is provided by ASP.NET Core MVC as a parameter to the action method.</span></span> <span data-ttu-id="5d1c8-120">매개 변수는 "Name" 또는 "날짜" 밑줄과 문자열 "desc" 내림차순을 지정 하 여 필요에 따라 문자열로 됩니다.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-120">The parameter will be a string that's either "Name" or "Date", optionally followed by an underscore and the string "desc" to specify descending order.</span></span> <span data-ttu-id="5d1c8-121">기본 정렬 순서는 오름차순입니다.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-121">The default sort order is ascending.</span></span>

<span data-ttu-id="5d1c8-122">인덱스 페이지를 요청 하 고, 처음으로 쿼리 문자열이 없는 합니다.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-122">The first time the Index page is requested, there's no query string.</span></span> <span data-ttu-id="5d1c8-123">학생의 fall 통해 사례에서 설정한의 기본 설정인 성을 기준으로 오름차순에 표시 되는 `switch` 문.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-123">The students are displayed in ascending order by last name, which is the default as established by the fall-through case in the `switch` statement.</span></span> <span data-ttu-id="5d1c8-124">적절 한 열 머리글 하이퍼링크를 클릭 하면 `sortOrder` 쿼리 문자열에 값을 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-124">When the user clicks a column heading hyperlink, the appropriate `sortOrder` value is provided in the query string.</span></span>

<span data-ttu-id="5d1c8-125">두 `ViewData` 요소 (NameSortParm 및 DateSortParm)를 사용 하는 보기를 열 머리글 하이퍼링크 적절 한 쿼리 문자열 값으로 구성 합니다.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-125">The two `ViewData` elements (NameSortParm and DateSortParm) are used by the view to configure the column heading hyperlinks with the appropriate query string values.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortOnly&highlight=3-4)]

<span data-ttu-id="5d1c8-126">이들은 삼항 문입니다.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-126">These are ternary statements.</span></span> <span data-ttu-id="5d1c8-127">첫 번째 지정 되는 경우는 `sortOrder` 매개 변수는 null 또는 비어 있는 경우 NameSortParm로 설정 해야 "name_desc", 그렇지 않으면 빈 문자열로 설정 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-127">The first one specifies that if the `sortOrder` parameter is null or empty, NameSortParm should be set to "name_desc"; otherwise, it should be set to an empty string.</span></span> <span data-ttu-id="5d1c8-128">열 머리글 하이퍼링크를 다음과 같이 설정 하려면 보기를 사용 하는이 두 개의 문:</span><span class="sxs-lookup"><span data-stu-id="5d1c8-128">These two statements enable the view to set the column heading hyperlinks as follows:</span></span>

|  <span data-ttu-id="5d1c8-129">현재 정렬 순서</span><span class="sxs-lookup"><span data-stu-id="5d1c8-129">Current sort order</span></span>  | <span data-ttu-id="5d1c8-130">마지막 이름 하이퍼링크</span><span class="sxs-lookup"><span data-stu-id="5d1c8-130">Last Name Hyperlink</span></span> | <span data-ttu-id="5d1c8-131">날짜 하이퍼링크</span><span class="sxs-lookup"><span data-stu-id="5d1c8-131">Date Hyperlink</span></span> |
|:--------------------:|:-------------------:|:--------------:|
| <span data-ttu-id="5d1c8-132">이름 오름차순 마지막</span><span class="sxs-lookup"><span data-stu-id="5d1c8-132">Last Name ascending</span></span>  | <span data-ttu-id="5d1c8-133">descending</span><span class="sxs-lookup"><span data-stu-id="5d1c8-133">descending</span></span>          | <span data-ttu-id="5d1c8-134">ascending</span><span class="sxs-lookup"><span data-stu-id="5d1c8-134">ascending</span></span>      |
| <span data-ttu-id="5d1c8-135">이름 내림차순 마지막</span><span class="sxs-lookup"><span data-stu-id="5d1c8-135">Last Name descending</span></span> | <span data-ttu-id="5d1c8-136">ascending</span><span class="sxs-lookup"><span data-stu-id="5d1c8-136">ascending</span></span>           | <span data-ttu-id="5d1c8-137">ascending</span><span class="sxs-lookup"><span data-stu-id="5d1c8-137">ascending</span></span>      |
| <span data-ttu-id="5d1c8-138">오름차순 날짜</span><span class="sxs-lookup"><span data-stu-id="5d1c8-138">Date ascending</span></span>       | <span data-ttu-id="5d1c8-139">ascending</span><span class="sxs-lookup"><span data-stu-id="5d1c8-139">ascending</span></span>           | <span data-ttu-id="5d1c8-140">descending</span><span class="sxs-lookup"><span data-stu-id="5d1c8-140">descending</span></span>     |
| <span data-ttu-id="5d1c8-141">내림차순 날짜</span><span class="sxs-lookup"><span data-stu-id="5d1c8-141">Date descending</span></span>      | <span data-ttu-id="5d1c8-142">ascending</span><span class="sxs-lookup"><span data-stu-id="5d1c8-142">ascending</span></span>           | <span data-ttu-id="5d1c8-143">ascending</span><span class="sxs-lookup"><span data-stu-id="5d1c8-143">ascending</span></span>      |

<span data-ttu-id="5d1c8-144">메서드가 LINQ to Entities 사용 하 여 기준으로 정렬 하려면 열을 지정 합니다.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-144">The method uses LINQ to Entities to specify the column to sort by.</span></span> <span data-ttu-id="5d1c8-145">코드 만듭니다는 `IQueryable` switch 문의 하기 전에 변수 switch 문과 호출에서 수정는 `ToListAsync` 후 메서드는 `switch` 문.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-145">The code creates an `IQueryable` variable before the switch statement, modifies it in the switch statement, and calls the `ToListAsync` method after the `switch` statement.</span></span> <span data-ttu-id="5d1c8-146">생성 및 수정할 `IQueryable` 변수 쿼리가 없는 데이터베이스에 전송 됩니다.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-146">When you create and modify `IQueryable` variables, no query is sent to the database.</span></span> <span data-ttu-id="5d1c8-147">변환 될 때까지 쿼리가 실행 되지 않습니다는 `IQueryable` 개체와 같은 메서드를 호출 하 여 컬렉션에 `ToListAsync`합니다.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-147">The query isn't executed until you convert the `IQueryable` object into a collection by calling a method such as `ToListAsync`.</span></span> <span data-ttu-id="5d1c8-148">따라서이 코드 발생 되어야 실행 하는 단일 쿼리는 `return View` 문.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-148">Therefore, this code results in a single query that's not executed until the `return View` statement.</span></span>

<span data-ttu-id="5d1c8-149">이 코드 열 다 수 포함 된 자세한 정보를 가져올 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-149">This code could get verbose with a large number of columns.</span></span> <span data-ttu-id="5d1c8-150">[이 시리즈의 마지막 자습서](advanced.md#dynamic-linq) 의 이름을 전달할 수 있도록 하는 코드를 작성 하는 방법을 보여 줍니다.는 `OrderBy` 문자열 변수에 열입니다.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-150">[The last tutorial in this series](advanced.md#dynamic-linq) shows how to write code that lets you pass the name of the `OrderBy` column in a string variable.</span></span>

### <a name="add-column-heading-hyperlinks-to-the-student-index-view"></a><span data-ttu-id="5d1c8-151">열 머리글 하이퍼링크 학생 인덱스 뷰를 추가</span><span class="sxs-lookup"><span data-stu-id="5d1c8-151">Add column heading hyperlinks to the Student Index view</span></span>

<span data-ttu-id="5d1c8-152">코드 *Views/Students/Index.cshtml*, 열 머리글 하이퍼링크를 추가 하려면 다음 코드와 함께 합니다.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-152">Replace the code in *Views/Students/Index.cshtml*, with the following code to add column heading hyperlinks.</span></span> <span data-ttu-id="5d1c8-153">변경 된 줄이 강조 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-153">The changed lines are highlighted.</span></span>

[!code-html[](intro/samples/cu/Views/Students/Index2.cshtml?highlight=16,22)]

<span data-ttu-id="5d1c8-154">이 코드의 정보를 사용 하 여 `ViewData` 적절 한 쿼리 된 하이퍼링크를 설정 하는 속성 문자열 값입니다.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-154">This code uses the information in `ViewData` properties to set up hyperlinks with the appropriate query string values.</span></span>

<span data-ttu-id="5d1c8-155">응용 프로그램 실행을 선택는 **학생** 탭을 클릭는 **성** 및 **등록 날짜** 확인 해당 정렬 하려면 열 머리글 작동 합니다.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-155">Run the app, select the **Students** tab, and click the **Last Name** and **Enrollment Date** column headings to verify that sorting works.</span></span>

![이름 순서로 학생 인덱스 페이지](sort-filter-page/_static/name-order.png)

## <a name="add-a-search-box-to-the-students-index-page"></a><span data-ttu-id="5d1c8-157">검색 상자 학생 인덱스 페이지에 추가</span><span class="sxs-lookup"><span data-stu-id="5d1c8-157">Add a Search Box to the Students Index page</span></span>

<span data-ttu-id="5d1c8-158">학생 인덱스 페이지에 필터링을 추가 하려면 추가 텍스트 상자 및 전송 단추가 보기로 및에 해당 변경는 `Index` 메서드.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-158">To add filtering to the Students Index page, you'll add a text box and a submit button to the view and make corresponding changes in the `Index` method.</span></span> <span data-ttu-id="5d1c8-159">입력란 이름 및 성 필드에서 검색할 문자열을 입력할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-159">The text box will let you enter a string to search for in the first name and last name fields.</span></span>

### <a name="add-filtering-functionality-to-the-index-method"></a><span data-ttu-id="5d1c8-160">Index 메서드에 필터링 기능 추가</span><span class="sxs-lookup"><span data-stu-id="5d1c8-160">Add filtering functionality to the Index method</span></span>

<span data-ttu-id="5d1c8-161">*StudentsController.cs*, 대체는 `Index` 메서드를 다음 코드로 (변경 내용을 강조 표시).</span><span class="sxs-lookup"><span data-stu-id="5d1c8-161">In *StudentsController.cs*, replace the `Index` method with the following code (the changes are highlighted).</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortFilter&highlight=1,5,9-13)]

<span data-ttu-id="5d1c8-162">사용자가 추가한는 `searchString` 매개 변수는 `Index` 메서드.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-162">You've added a `searchString` parameter to the `Index` method.</span></span> <span data-ttu-id="5d1c8-163">검색 문자열 값이 인덱스 뷰를 추가 하는 텍스트 상자가에서 수신 합니다.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-163">The search string value is received from a text box that you'll add to the Index view.</span></span> <span data-ttu-id="5d1c8-164">또한를 추가 했으므로 LINQ 명령문 where 해당 이름 또는 성을 검색 문자열을 포함 하는 학생만을 선택 하는 절.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-164">You've also added to the LINQ statement a where clause that selects only students whose first name or last name contains the search string.</span></span> <span data-ttu-id="5d1c8-165">Where를 추가 하는 문에 절이 검색 하려면 값이 있는 경우에 실행 됩니다.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-165">The statement that adds the where clause is executed only if there's a value to search for.</span></span>

> [!NOTE]
> <span data-ttu-id="5d1c8-166">호출 하는 여기에서 `Where` 메서드는 `IQueryable` 개체 및 필터는 서버에서 처리 됩니다.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-166">Here you are calling the `Where` method on an `IQueryable` object, and the filter will be processed on the server.</span></span> <span data-ttu-id="5d1c8-167">일부 시나리오에서는 있습니다를 호출할 수는 `Where` 메모리 내 컬렉션에 대 한 확장 메서드이기 메서드.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-167">In some scenarios you might be calling the `Where` method as an extension method on an in-memory collection.</span></span> <span data-ttu-id="5d1c8-168">(에 대 한 참조를 변경 한다고 가정 예를 들어 `_context.Students` 는 EF의 즉 대신 `DbSet` 반환 하는 저장소 메서드를 참조 하는 `IEnumerable` 컬렉션입니다.) 결과 일반적으로 동일 하지만 경우에 따라 다를 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-168">(For example, suppose you change the reference to `_context.Students` so that instead of an EF `DbSet` it references a repository method that returns an `IEnumerable` collection.) The result would normally be the same but in some cases may be different.</span></span>
>
><span data-ttu-id="5d1c8-169">예를 들어의.NET Framework 구현은 `Contains` 메서드는 기본적으로 대/소문자 구분 비교를 수행 하지만 SQL Server에서 SQL Server 인스턴스의 데이터 정렬 설정에 따라 결정 됩니다.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-169">For example, the .NET Framework implementation of the `Contains` method performs a case-sensitive comparison by default, but in SQL Server this is determined by the collation setting of the SQL Server instance.</span></span> <span data-ttu-id="5d1c8-170">해당 설정은 기본적으로 대/소문자 구분 됩니다.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-170">That setting defaults to case-insensitive.</span></span> <span data-ttu-id="5d1c8-171">호출할 수는 `ToUpper` 명시적으로 대/소문자 구분 테스트를 만드는 메서드와 알림이: *여기서 (s = > s.LastName.ToUpper() 합니다. Contains(searchString.ToUpper())*합니다.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-171">You could call the `ToUpper` method to make the test explicitly case-insensitive:  *Where(s => s.LastName.ToUpper().Contains(searchString.ToUpper())*.</span></span> <span data-ttu-id="5d1c8-172">결과 그대로 반환 하는 리포지토리를 사용 하도록 나중에 코드를 변경 하는 경우 않게 하는 `IEnumerable` 컬렉션 대신 한 `IQueryable` 개체입니다.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-172">That would ensure that results stay the same if you change the code later to use a repository which returns   an `IEnumerable` collection instead of an `IQueryable` object.</span></span> <span data-ttu-id="5d1c8-173">(호출 하는 경우는 `Contains` 에서 메서드는 `IEnumerable` .NET Framework 구현은 가져올 컬렉션을; 호출 하는 경우 그에 `IQueryable` 개체, 데이터베이스 공급자 구현을 가져옵니다.) 그러나이 솔루션에 대 한 성능 저하가 됩니다.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-173">(When you call the `Contains` method on an `IEnumerable` collection, you get the .NET Framework implementation; when you call it on an `IQueryable` object, you get the database provider implementation.) However, there's a performance penalty for this solution.</span></span> <span data-ttu-id="5d1c8-174">`ToUpper` 코드 TSQL SELECT 문의 WHERE 절에 함수를 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-174">The `ToUpper` code would put a function in the WHERE clause of the TSQL SELECT statement.</span></span> <span data-ttu-id="5d1c8-175">인덱스를 사용 하 여 최적화 프로그램이 수 없게 됩니다.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-175">That would prevent the optimizer from using an index.</span></span> <span data-ttu-id="5d1c8-176">않는 것이 좋습니다은 대/소문자 SQL이 설치 되어 대부분, 있다고 가정은 `ToUpper` 대/소문자 구분 데이터 저장소로 마이그레이션하기 전 까지는 코드입니다.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-176">Given that SQL is mostly installed as case-insensitive, it's best to avoid the `ToUpper` code until you migrate to a case-sensitive data store.</span></span>

### <a name="add-a-search-box-to-the-student-index-view"></a><span data-ttu-id="5d1c8-177">검색 상자 학생 인덱스 뷰를 추가</span><span class="sxs-lookup"><span data-stu-id="5d1c8-177">Add a Search Box to the Student Index View</span></span>

<span data-ttu-id="5d1c8-178">*Views/Student/Index.cshtml*, 캡션, 텍스트 상자를 만들려면 테이블 태그를 열기 전에 강조 표시 된 코드를 추가 및 **검색** 단추입니다.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-178">In *Views/Student/Index.cshtml*, add the highlighted code immediately before the opening table tag in order to create a caption, a text box, and a **Search** button.</span></span>

[!code-html[](intro/samples/cu/Views/Students/Index3.cshtml?range=9-23&highlight=5-13)]

<span data-ttu-id="5d1c8-179">이 코드에서는 사용는 `<form>` [태그 도우미](xref:mvc/views/tag-helpers/intro) 검색 텍스트 상자 및 단추를 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-179">This code uses the `<form>` [tag helper](xref:mvc/views/tag-helpers/intro) to add the search text box and button.</span></span> <span data-ttu-id="5d1c8-180">기본적으로는 `<form>` 태그 도우미 즉, 매개 변수가 URL 아니라 HTTP 메시지 본문에 쿼리 문자열로 전달 하는 POST로 양식 데이터를 전송 합니다.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-180">By default, the `<form>` tag helper submits form data with a POST, which means that parameters are passed in the HTTP message body and not in the URL as query strings.</span></span> <span data-ttu-id="5d1c8-181">HTTP GET을 지정 하면 양식 데이터 변수로 전달 됩니다 URL에 쿼리 문자열, URL에 책갈피를 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-181">When you specify HTTP GET, the form data is passed in the URL as query strings, which enables users to bookmark the URL.</span></span> <span data-ttu-id="5d1c8-182">동작 하지 않는 업데이트 될 때 전달 되는 사용 해야 하는 W3C 지침 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-182">The W3C guidelines recommend that you should use GET when the action doesn't result in an update.</span></span>

<span data-ttu-id="5d1c8-183">응용 프로그램을 실행, 선택는 **학생** 탭 검색 문자열을 입력 하 고 필터링이 작동 하는지 확인 하는 검색을 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-183">Run the app, select the **Students** tab, enter a search string, and click Search to verify that filtering is working.</span></span>

![필터링 된 인덱스 페이지 학생](sort-filter-page/_static/filtering.png)

<span data-ttu-id="5d1c8-185">검색 문자열은 URL에 포함 되어 있는지 확인 합니다.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-185">Notice that the URL contains the search string.</span></span>

```html
http://localhost:5813/Students?SearchString=an
```

<span data-ttu-id="5d1c8-186">이 페이지에 책갈피는 책갈피를 사용 하는 경우 필터링된 된 목록을 봅니다.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-186">If you bookmark this page, you'll get the filtered list when you use the bookmark.</span></span> <span data-ttu-id="5d1c8-187">추가 `method="get"` 에 `form` 태그는 원인을 쿼리 문자열을 생성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-187">Adding `method="get"` to the `form` tag is what caused the query string to be generated.</span></span>

<span data-ttu-id="5d1c8-188">이 단계에서 열 머리글 정렬 링크를 클릭 하면 손실 됩니다에 입력 된 필터 값은 **검색** 상자입니다.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-188">At this stage, if you click a column heading sort link you'll lose the filter value that you entered in the **Search** box.</span></span> <span data-ttu-id="5d1c8-189">다음 섹션에서 수정 하 게 합니다.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-189">You'll fix that in the next section.</span></span>

## <a name="add-paging-functionality-to-the-students-index-page"></a><span data-ttu-id="5d1c8-190">학생 인덱스 페이지에 페이징 기능을 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-190">Add paging functionality to the Students Index page</span></span>

<span data-ttu-id="5d1c8-191">페이징에 학생 인덱스 페이지를 추가 하려면 만들어는 `PaginatedList` 클래스를 사용 하 `Skip` 및 `Take` 문은 항상 테이블의 모든 행을 검색 하는 대신 서버에서 데이터를 필터링 합니다.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-191">To add paging to the Students Index page, you'll create a `PaginatedList` class that uses `Skip` and `Take` statements to filter data on the server instead of always retrieving all rows of the table.</span></span> <span data-ttu-id="5d1c8-192">다음에 추가로 변경할 수는 `Index` 메서드 페이징 단추를 추가 하 고는 `Index` 보기.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-192">Then you'll make additional changes in the `Index` method and add paging buttons to the `Index` view.</span></span> <span data-ttu-id="5d1c8-193">다음 그림에는 페이징 단추가 나와 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-193">The following illustration shows the paging buttons.</span></span>

![학생 인덱스 페이징 링크가 있는 페이지](sort-filter-page/_static/paging.png)

<span data-ttu-id="5d1c8-195">프로젝트 폴더에서 만들 `PaginatedList.cs`, 다음 템플릿 코드를 다음 코드로 바꿉니다.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-195">In the project folder, create `PaginatedList.cs`, and then replace the template code with the following code.</span></span>

[!code-csharp[Main](intro/samples/cu/PaginatedList.cs)]

<span data-ttu-id="5d1c8-196">`CreateAsync` 이 코드에서 메서드는 페이지 크기 및 페이지 번호를 사용 하 고 적절 한 적용 `Skip` 및 `Take` 문을 `IQueryable`합니다.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-196">The `CreateAsync` method in this code takes page size and page number and applies the appropriate `Skip` and `Take` statements to the `IQueryable`.</span></span> <span data-ttu-id="5d1c8-197">때 `ToListAsync` 라고 하는 `IQueryable`, 요청된 된 페이지를 포함 하는 목록을 반환 합니다.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-197">When `ToListAsync` is called on the `IQueryable`, it will return a List containing only the requested page.</span></span> <span data-ttu-id="5d1c8-198">속성 `HasPreviousPage` 및 `HasNextPage` 활성화 하거나 비활성화 하는 데 사용 될 **이전** 및 **다음** 단추 페이징 합니다.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-198">The properties `HasPreviousPage` and `HasNextPage` can be used to enable or disable **Previous** and **Next** paging buttons.</span></span>

<span data-ttu-id="5d1c8-199">A `CreateAsync` 메서드는 생성자를 대신 만드는 데 사용 되는 `PaginatedList<T>` 개체 생성자는 비동기 코드를 실행할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-199">A `CreateAsync` method is used instead of a constructor to create the `PaginatedList<T>` object because constructors can't run asynchronous code.</span></span>

## <a name="add-paging-functionality-to-the-index-method"></a><span data-ttu-id="5d1c8-200">인덱스 메서드에 페이징 기능을 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-200">Add paging functionality to the Index method</span></span>

<span data-ttu-id="5d1c8-201">*StudentsController.cs*, 대체 된 `Index` 메서드를 다음 코드로 합니다.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-201">In *StudentsController.cs*, replace the `Index` method with the following code.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortFilterPage&highlight=1-5,7,11-18,45-46)]

<span data-ttu-id="5d1c8-202">이 코드 페이지 번호 매개 변수, 현재 정렬 순서 매개 변수, 및 현재 필터 매개 변수 메서드 시그니처를 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-202">This code adds a page number parameter, a current sort order parameter, and a current filter parameter to the method signature.</span></span>

```csharp
public async Task<IActionResult> Index(
    string sortOrder,
    string currentFilter,
    string searchString,
    int? page)
```

<span data-ttu-id="5d1c8-203">처음으로 페이지 표시 되거나, 사용자 하지 않은 한 페이징 또는 정렬 링크를 클릭 한 경우 모든 매개 변수가 null이 됩니다.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-203">The first time the page is displayed, or if the user hasn't clicked a paging or sorting link, all the parameters will be null.</span></span>  <span data-ttu-id="5d1c8-204">페이징 링크를 클릭 하는 경우 페이지 변수 표시할 페이지 번호를 포함 됩니다.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-204">If a paging link is clicked, the page variable will contain the page number to display.</span></span>

<span data-ttu-id="5d1c8-205">`ViewData` CurrentSort 라는 요소를이 페이징 하는 동안 동일한 정렬 순서를 유지 하기 위해 페이징 파일에 대 한 링크에 포함 되어야 하기 때문에 현재 정렬 순서를 사용 하 여 뷰를 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-205">The `ViewData` element named CurrentSort provides the view with the current sort order, because this must be included in the paging links in order to keep the sort order the same while paging.</span></span>

<span data-ttu-id="5d1c8-206">`ViewData` 현재 필터 라는 요소를 현재 필터 문자열을 사용 하 여 뷰를 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-206">The `ViewData` element named CurrentFilter provides the view with the current filter string.</span></span> <span data-ttu-id="5d1c8-207">이 값, 페이징 하는 동안 필터 설정을 유지 하기 위해 페이징 링크에 포함 되어야 하며 페이지 다시 표시 하는 경우 텍스트 상자에 복원 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-207">This value must be included in the paging links in order to maintain the filter settings during paging, and it must be restored to the text box when the page is redisplayed.</span></span>

<span data-ttu-id="5d1c8-208">검색 문자열 페이징 하는 동안 변경 되 면 표시할 다른 데이터를 새 필터 발생할 수 있기 때문 1로 다시 설정 되는 페이지에 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-208">If the search string is changed during paging, the page has to be reset to 1, because the new filter can result in different data to display.</span></span> <span data-ttu-id="5d1c8-209">검색 문자열 값이 텍스트 상자에 입력 하 고 전송 단추를 누를 때 변경 됩니다.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-209">The search string is changed when a value is entered in the text box and the Submit button is pressed.</span></span> <span data-ttu-id="5d1c8-210">이 경우에 `searchString` null이 아닌 매개 변수입니다.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-210">In that case, the `searchString` parameter isn't null.</span></span>

```csharp
if (searchString != null)
{
    page = 1;
}
else
{
    searchString = currentFilter;
}
```

<span data-ttu-id="5d1c8-211">끝에는 `Index` 메서드는 `PaginatedList.CreateAsync` 메서드 학생 페이징을 지원 되는 컬렉션 형식에는 단일 페이지로 학생 쿼리를 변환 합니다.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-211">At the end of the `Index` method, the `PaginatedList.CreateAsync` method converts the student query to a single page of students in a collection type that supports paging.</span></span> <span data-ttu-id="5d1c8-212">그러면 학생 들의 해당 단일 페이지 보기에 전달 됩니다.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-212">That single page of students is then passed to the view.</span></span>

```csharp
return View(await PaginatedList<Student>.CreateAsync(students.AsNoTracking(), page ?? 1, pageSize));
```

<span data-ttu-id="5d1c8-213">`PaginatedList.CreateAsync` 메서드는 페이지 번호를 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-213">The `PaginatedList.CreateAsync` method takes a page number.</span></span> <span data-ttu-id="5d1c8-214">두 물음표 null 병합 연산자를 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-214">The two question marks represent the null-coalescing operator.</span></span> <span data-ttu-id="5d1c8-215">Null 병합 연산자는 null 허용 형식에 대 한 기본값을 정의 식 `(page ?? 1)` 의 값을 반환 하는 수단 `page` 값을 가진 또는 이면 1을 반환 하는 경우 `page` null입니다.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-215">The null-coalescing operator defines a default value for a nullable type; the expression `(page ?? 1)` means return the value of `page` if it has a value, or return 1 if `page` is null.</span></span>

## <a name="add-paging-links-to-the-student-index-view"></a><span data-ttu-id="5d1c8-216">학생 인덱스 뷰를 페이징 링크 추가</span><span class="sxs-lookup"><span data-stu-id="5d1c8-216">Add paging links to the Student Index view</span></span>

<span data-ttu-id="5d1c8-217">*Views/Students/Index.cshtml*, 기존 코드를 다음 코드로 바꿉니다.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-217">In *Views/Students/Index.cshtml*, replace the existing code with the following code.</span></span> <span data-ttu-id="5d1c8-218">변경 내용은 강조 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-218">The changes are highlighted.</span></span>

[!code-html[](intro/samples/cu/Views/Students/Index.cshtml?highlight=1,27,30,33,61-79)]

<span data-ttu-id="5d1c8-219">`@model` 페이지 맨 위에 있는 문은 보기 이제 가져옴을 지정는 `PaginatedList<T>` 개체가 아니라는 `List<T>` 개체입니다.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-219">The `@model` statement at the top of the page specifies that the view now gets a `PaginatedList<T>` object instead of a `List<T>` object.</span></span>

<span data-ttu-id="5d1c8-220">열 머리글 링크 필터 결과 내에서 사용자를 정렬할 수 있도록 컨트롤러에 현재 검색 문자열을 전달 하는 쿼리 문자열을 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-220">The column header links use the query string to pass the current search string to the controller so that the user can sort within filter results:</span></span>

```html
<a asp-action="Index" asp-route-sortOrder="@ViewData["DateSortParm"]" asp-route-currentFilter ="@ViewData["CurrentFilter"]">Enrollment Date</a>
```

<span data-ttu-id="5d1c8-221">태그 도우미에서 페이징 단추가 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-221">The paging buttons are displayed by tag helpers:</span></span>

```html
<a asp-action="Index"
   asp-route-sortOrder="@ViewData["CurrentSort"]"
   asp-route-page="@(Model.PageIndex - 1)"
   asp-route-currentFilter="@ViewData["CurrentFilter"]"
   class="btn btn-default @prevDisabled">
   Previous
</a>
```

<span data-ttu-id="5d1c8-222">응용 프로그램을 실행 하 고 학생 페이지로 이동 합니다.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-222">Run the app and go to the Students page.</span></span>

![학생 인덱스 페이징 링크가 있는 페이지](sort-filter-page/_static/paging.png)

<span data-ttu-id="5d1c8-224">페이징 작동 하는지 확인 하려면 서로 다른 정렬 순서에서 페이징 링크를 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-224">Click the paging links in different sort orders to make sure paging works.</span></span> <span data-ttu-id="5d1c8-225">검색 문자열을 입력 하 고 페이징 페이징도 제대로 작동 하는지 확인 된 정렬 및 필터링을 다시 시도 하십시오.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-225">Then enter a search string and try paging again to verify that paging also works correctly with sorting and filtering.</span></span>

## <a name="create-an-about-page-that-shows-student-statistics"></a><span data-ttu-id="5d1c8-226">학생 통계를 보여 주는 정보 페이지 만들기</span><span class="sxs-lookup"><span data-stu-id="5d1c8-226">Create an About page that shows Student statistics</span></span>

<span data-ttu-id="5d1c8-227">Contoso 대학 웹 사이트에 대 한 **에 대 한** 페이지에서 각 등록 날짜에 대해 등록 한 학생 수를 표시 합니다.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-227">For the Contoso University website's **About** page, you'll display how many students have enrolled for each enrollment date.</span></span> <span data-ttu-id="5d1c8-228">이 그룹에 그룹화 및 간단한 계산 해야합니다.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-228">This requires grouping and simple calculations on the groups.</span></span> <span data-ttu-id="5d1c8-229">이렇게 하려면 다음을 수행 합니다.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-229">To accomplish this, you'll do the following:</span></span>

* <span data-ttu-id="5d1c8-230">보기를 전달 해야 하는 데이터에 대 한 보기 모델 클래스를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-230">Create a view model class for the data that you need to pass to the view.</span></span>

* <span data-ttu-id="5d1c8-231">Home 컨트롤러에서 정보 메서드를 수정 합니다.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-231">Modify the About method in the Home controller.</span></span>

* <span data-ttu-id="5d1c8-232">정보 보기를 수정 합니다.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-232">Modify the About view.</span></span>

### <a name="create-the-view-model"></a><span data-ttu-id="5d1c8-233">뷰 모델 만들기</span><span class="sxs-lookup"><span data-stu-id="5d1c8-233">Create the view model</span></span>

<span data-ttu-id="5d1c8-234">만들기는 *SchoolViewModels* 폴더에는 *모델* 폴더입니다.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-234">Create a *SchoolViewModels* folder in the *Models* folder.</span></span>

<span data-ttu-id="5d1c8-235">새 폴더에 클래스 파일 추가 *EnrollmentDateGroup.cs* 템플릿 코드를 다음 코드로 바꿉니다.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-235">In the new folder, add a class file *EnrollmentDateGroup.cs* and replace the template code with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/EnrollmentDateGroup.cs)]

### <a name="modify-the-home-controller"></a><span data-ttu-id="5d1c8-236">Home 컨트롤러를 수정 합니다.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-236">Modify the Home Controller</span></span>

<span data-ttu-id="5d1c8-237">*HomeController.cs*, 다음 추가 파일 맨 위에 있는 문을 사용 하 여:</span><span class="sxs-lookup"><span data-stu-id="5d1c8-237">In *HomeController.cs*, add the following using statements at the top of the file:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/HomeController.cs?name=snippet_Usings1)]

<span data-ttu-id="5d1c8-238">여는 중괄호는 클래스에 대 한 직후 데이터베이스 컨텍스트에 대 한 클래스 변수를 추가 하 고에서 ASP.NET Core DI 컨텍스트의 인스턴스를 가져옵니다.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-238">Add a class variable for the database context immediately after the opening curly brace for the class, and get an instance of the context from ASP.NET Core DI:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/HomeController.cs?name=snippet_AddContext&highlight=3,5,7)]

<span data-ttu-id="5d1c8-239">`About` 메서드를 다음 코드로 바꿉니다.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-239">Replace the `About` method with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/HomeController.cs?name=snippet_UseDbSet)]

<span data-ttu-id="5d1c8-240">LINQ 명령문 학생 엔터티 등록 날짜별으로 그룹화 각 그룹에 있는 엔터티 수를 계산 하 고 컬렉션의 결과 저장할지 `EnrollmentDateGroup` 모델 개체를 보고 합니다.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-240">The LINQ statement groups the student entities by enrollment date, calculates the number of entities in each group, and stores the results in a collection of `EnrollmentDateGroup` view model objects.</span></span>
> [!NOTE] 
> <span data-ttu-id="5d1c8-241">Entity Framework Core 1.0 버전의 클라이언트에는 전체 결과 집합이 반환 됩니다 및 그룹화 클라이언트에서 수행 됩니다.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-241">In the 1.0 version of Entity Framework Core, the entire result set is returned to the client, and grouping is done on the client.</span></span> <span data-ttu-id="5d1c8-242">일부 시나리오에서 성능 문제가 발생할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-242">In some scenarios this could create performance problems.</span></span> <span data-ttu-id="5d1c8-243">필요한 경우 원시 SQL을 사용 하 여 서버에서 그룹화 작업을 수행 하 고 프로덕션 볼륨 데이터의 성능을 테스트 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-243">Be sure to test performance with production volumes of data, and if necessary use raw SQL to do the grouping on the server.</span></span> <span data-ttu-id="5d1c8-244">원시 SQL을 사용 하는 방법에 대 한 정보를 참조 하십시오. [이 시리즈의 마지막 자습서](advanced.md)합니다.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-244">For information about how to use raw SQL, see [the last tutorial in this series](advanced.md).</span></span>

### <a name="modify-the-about-view"></a><span data-ttu-id="5d1c8-245">수정 된 보기에 대 한</span><span class="sxs-lookup"><span data-stu-id="5d1c8-245">Modify the About View</span></span>

<span data-ttu-id="5d1c8-246">코드는 *Views/Home/About.cshtml* 를 다음 코드로 파일:</span><span class="sxs-lookup"><span data-stu-id="5d1c8-246">Replace the code in the *Views/Home/About.cshtml* file with the following code:</span></span>

[!code-html[](intro/samples/cu/Views/Home/About.cshtml)]

<span data-ttu-id="5d1c8-247">응용 프로그램을 실행 하 고 정보 페이지로 이동 합니다.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-247">Run the app and go to the About page.</span></span> <span data-ttu-id="5d1c8-248">각 등록 날짜에 대 한 학생 수는 테이블에 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-248">The count of students for each enrollment date is displayed in a table.</span></span>

![페이지 정보](sort-filter-page/_static/about.png)

## <a name="summary"></a><span data-ttu-id="5d1c8-250">요약</span><span class="sxs-lookup"><span data-stu-id="5d1c8-250">Summary</span></span>

<span data-ttu-id="5d1c8-251">이 자습서에서는 정렬, 필터링, 페이징, 및 그룹화를 수행 하는 방법을 살펴보았습니다.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-251">In this tutorial, you've seen how to perform sorting, filtering, paging, and grouping.</span></span> <span data-ttu-id="5d1c8-252">다음 자습서에서 마이그레이션을 사용 하 여 데이터 모델 변경 내용을 처리 하는 방법을 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="5d1c8-252">In the next tutorial, you'll learn how to handle data model changes by using migrations.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="5d1c8-253">[이전](crud.md)
[다음](migrations.md)</span><span class="sxs-lookup"><span data-stu-id="5d1c8-253">[Previous](crud.md)
[Next](migrations.md)</span></span>  
