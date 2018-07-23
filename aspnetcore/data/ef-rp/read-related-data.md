---
title: ASP.NET Core에서 EF Core를 사용한 Razor 페이지 - 관련 데이터 읽기 - 6/8
author: rick-anderson
description: 이 자습서에서는 관련된 데이터 즉, Entity Framework에서 탐색 속성으로 로드하는 데이터를 읽고 표시합니다.
ms.author: riande
ms.date: 11/05/2017
uid: data/ef-rp/read-related-data
ms.openlocfilehash: bcea6aa6018a937979b8e0aaa2edcdd96da41559
ms.sourcegitcommit: a3675f9704e4e73ecc7cbbbf016a13d2a5c4d725
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/23/2018
ms.locfileid: "39202681"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---read-related-data---6-of-8"></a><span data-ttu-id="272e2-103">ASP.NET Core에서 EF Core를 사용한 Razor 페이지 - 관련 데이터 읽기 - 6/8</span><span class="sxs-lookup"><span data-stu-id="272e2-103">Razor Pages with EF Core in ASP.NET Core - Read Related Data - 6 of 8</span></span>

<span data-ttu-id="272e2-104">작성자: [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog) 및 [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="272e2-104">By [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="272e2-105">이 자습서에서는 관련된 데이터를 읽고 표시합니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-105">In this tutorial, related data is read and displayed.</span></span> <span data-ttu-id="272e2-106">관련된 데이터는 EF Core에서 탐색 속성에 로드하는 데이터입니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-106">Related data is data that EF Core loads into navigation properties.</span></span>

<span data-ttu-id="272e2-107">해결할 수 없는 문제가 발생한 경우 [이 단계에 완성된 앱](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part6-related)을 다운로드합니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-107">If you run into problems you can't solve, download the [completed app for this stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part6-related).</span></span>

<span data-ttu-id="272e2-108">다음 그림은 이 자습서에 대해 완료된 페이지를 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-108">The following illustrations show the completed pages for this tutorial:</span></span>

![과정 인덱스 페이지](read-related-data/_static/courses-index.png)

![강사 인덱스 페이지](read-related-data/_static/instructors-index.png)

## <a name="eager-explicit-and-lazy-loading-of-related-data"></a><span data-ttu-id="272e2-111">관련된 데이터의 즉시, 명시적 및 지연 로드</span><span class="sxs-lookup"><span data-stu-id="272e2-111">Eager, explicit, and lazy Loading of related data</span></span>

<span data-ttu-id="272e2-112">여러 가지 방법으로 EF Core가 관련 데이터를 엔터티의 탐색 속성에 로드할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-112">There are several ways that EF Core can load related data into the navigation properties of an entity:</span></span>

* <span data-ttu-id="272e2-113">[즉시 로드](https://docs.microsoft.com/ef/core/querying/related-data#eager-loading).</span><span class="sxs-lookup"><span data-stu-id="272e2-113">[Eager loading](https://docs.microsoft.com/ef/core/querying/related-data#eager-loading).</span></span> <span data-ttu-id="272e2-114">즉시 로드는 한 형식의 엔터티에 대한 쿼리가 관련 엔터티도 로드하는 경우입니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-114">Eager loading is when a query for one type of entity also loads related entities.</span></span> <span data-ttu-id="272e2-115">엔터티를 읽을 때 관련된 데이터가 검색됩니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-115">When the entity is read, its related data is retrieved.</span></span> <span data-ttu-id="272e2-116">이는 일반적으로 필요한 데이터를 모두 검색하는 단일 조인 쿼리를 발생시킵니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-116">This typically results in a single join query that retrieves all of the data that's needed.</span></span> <span data-ttu-id="272e2-117">EF 코어는 일부 형식의 즉시 로드에 대해 여러 쿼리를 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-117">EF Core will issue multiple queries for some types of eager loading.</span></span> <span data-ttu-id="272e2-118">여러 쿼리를 실행하는 것이 단일 쿼리가 있는 EF6의 일부 쿼리보다 효율적일 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-118">Issuing multiple queries can be more efficient than was the case for some queries in EF6 where there was a single query.</span></span> <span data-ttu-id="272e2-119">즉시 로드는 `Include` 및 `ThenInclude` 메서드로 지정됩니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-119">Eager loading is specified with the `Include` and `ThenInclude` methods.</span></span>

  ![즉시 로드 예제](read-related-data/_static/eager-loading.png)
 
  <span data-ttu-id="272e2-121">즉시 로드는 컬렉션 탐색이 포함된 경우 여러 쿼리를 보냅니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-121">Eager loading sends multiple queries when a collection navigation is included:</span></span>

  * <span data-ttu-id="272e2-122">주 쿼리에 대해 한 개 쿼리</span><span class="sxs-lookup"><span data-stu-id="272e2-122">One query for the main query</span></span> 
  * <span data-ttu-id="272e2-123">로드 트리에서 각 컬렉션 "에지"에 대해 한 개 쿼리</span><span class="sxs-lookup"><span data-stu-id="272e2-123">One query for each collection "edge" in the load tree.</span></span>

* <span data-ttu-id="272e2-124">`Load`로 쿼리 구분: 별도의 쿼리로 데이터를 검색할 수 있으며 EF Core는 탐색 속성을 "수정"합니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-124">Separate queries with `Load`: The data can be retrieved in separate queries, and EF Core "fixes up" the navigation properties.</span></span> <span data-ttu-id="272e2-125">"수정"한다는 것은 EF Core가 탐색 속성을 자동으로 채운다는 것을 의미합니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-125">"fixes up" means that EF Core automatically populates the navigation properties.</span></span> <span data-ttu-id="272e2-126">`Load`로 쿼리를 구분하는 것은 즉시 로드보다 더 명시적인 로드입니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-126">Separate queries with `Load` is more like explict loading than eager loading.</span></span>

  ![별도 쿼리 예제](read-related-data/_static/separate-queries.png)

  <span data-ttu-id="272e2-128">참고: EF Core는 이전에 컨텍스트 인스턴스에 로드된 다른 엔터티로 탐색 속성을 자동으로 수정합니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-128">Note: EF Core automatically fixes up navigation properties to any other entities that were previously loaded into the context instance.</span></span> <span data-ttu-id="272e2-129">탐색 속성에 대한 데이터가 명시적으로 포함되지 *않더라도* 관련 엔터티의 일부 또는 전부가 이전에 로드된 경우에도 속성이 채워질 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-129">Even if the data for a navigation property is *not* explicitly included, the property may still be populated if some or all of the related entities were previously loaded.</span></span>

* <span data-ttu-id="272e2-130">[명시적 로드](https://docs.microsoft.com/ef/core/querying/related-data#explicit-loading).</span><span class="sxs-lookup"><span data-stu-id="272e2-130">[Explicit loading](https://docs.microsoft.com/ef/core/querying/related-data#explicit-loading).</span></span> <span data-ttu-id="272e2-131">엔터티를 처음 읽을 때 관련된 데이터가 검색되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-131">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="272e2-132">필요할 때 관련된 데이터를 검색하기 위한 코드를 작성해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-132">Code must be written to retrieve the related data when it's needed.</span></span> <span data-ttu-id="272e2-133">별도 쿼리가 있는 명시적 로드의 경우 여러 쿼리가 DB로 전송됩니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-133">Explicit loading with separate queries results in multiple queries sent to the DB.</span></span> <span data-ttu-id="272e2-134">명시적 로드에서 코드는 로드될 탐색 속성을 지정합니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-134">With explicit loading, the code specifies the navigation properties to be loaded.</span></span> <span data-ttu-id="272e2-135">`Load` 메서드를 사용하여 명시적 로드를 수행합니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-135">Use the `Load` method to do explicit loading.</span></span> <span data-ttu-id="272e2-136">예:</span><span class="sxs-lookup"><span data-stu-id="272e2-136">For example:</span></span>

  ![명시적 로드 예제](read-related-data/_static/explicit-loading.png)

* <span data-ttu-id="272e2-138">[지연 로드](https://docs.microsoft.com/ef/core/querying/related-data#lazy-loading).</span><span class="sxs-lookup"><span data-stu-id="272e2-138">[Lazy loading](https://docs.microsoft.com/ef/core/querying/related-data#lazy-loading).</span></span> <span data-ttu-id="272e2-139">[EF Core가 현재 지연 로드를 지원하지 않습니다](https://github.com/aspnet/EntityFrameworkCore/issues/3797).</span><span class="sxs-lookup"><span data-stu-id="272e2-139">[EF Core doesn't currently support lazy loading](https://github.com/aspnet/EntityFrameworkCore/issues/3797).</span></span> <span data-ttu-id="272e2-140">엔터티를 처음 읽을 때 관련된 데이터가 검색되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-140">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="272e2-141">탐색 속성에 처음으로 액세스하려고 할 때 해당 탐색 속성에 필요한 데이터가 자동으로 검색됩니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-141">The first time a navigation property is accessed, the data required for that navigation property is automatically retrieved.</span></span> <span data-ttu-id="272e2-142">탐색 속성에 처음으로 액세스할 때마다 쿼리가 DB에 전송됩니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-142">A query is sent to the DB each time a navigation property is accessed for the first time.</span></span>

* <span data-ttu-id="272e2-143">`Select` 연산자는 필요한 관련된 데이터만 로드합니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-143">The `Select` operator loads only the related data needed.</span></span>

## <a name="create-a-courses-page-that-displays-department-name"></a><span data-ttu-id="272e2-144">부서 이름을 표시하는 과정 페이지 만들기</span><span class="sxs-lookup"><span data-stu-id="272e2-144">Create a Courses page that displays department name</span></span>

<span data-ttu-id="272e2-145">과정 엔터티는 `Department` 엔터티가 포함된 탐색 속성을 포함합니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-145">The Course entity includes a navigation property that contains the `Department` entity.</span></span> <span data-ttu-id="272e2-146">`Department` 엔터티는 과정이 할당된 부서를 포함합니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-146">The `Department` entity contains the department that the course is assigned to.</span></span>

<span data-ttu-id="272e2-147">과정 목록에 할당된 부서 이름을 표시하려면</span><span class="sxs-lookup"><span data-stu-id="272e2-147">To display the name of the assigned department in a list of courses:</span></span>

* <span data-ttu-id="272e2-148">`Department` 엔터티에서 `Name` 속성을 가져옵니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-148">Get the `Name` property from the `Department` entity.</span></span>
* <span data-ttu-id="272e2-149">`Department` 엔터티는 `Course.Department` 탐색 속성에서 가져옵니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-149">The `Department` entity comes from the `Course.Department` navigation property.</span></span>

![ourse.Department](read-related-data/_static/dep-crs.png)

<a name="scaffold"></a>
### <a name="scaffold-the-course-model"></a><span data-ttu-id="272e2-151">과정 모델 스캐폴드</span><span class="sxs-lookup"><span data-stu-id="272e2-151">Scaffold the Course model</span></span>

* <span data-ttu-id="272e2-152">Visual Studio를 끝냅니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-152">Exit Visual Studio.</span></span>
* <span data-ttu-id="272e2-153">프로젝트 디렉터리(*Program.cs*, *Startup.cs* 및 *.csproj* 파일이 포함된 디렉터리)에서 명령 창을 엽니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-153">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="272e2-154">다음 명령을 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-154">Run the following command:</span></span>

  ```console
  dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design --version 2.1.0
  dotnet aspnet-codegenerator razorpage -m Course -dc SchoolContext -udl -outDir Pages\Courses --referenceScriptLibraries
  ```

<span data-ttu-id="272e2-155">위의 명령은 `Course` 모델을 스캐폴드합니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-155">The preceding command scaffolds the `Course` model.</span></span> <span data-ttu-id="272e2-156">Visual Studio에서 프로젝트를 엽니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-156">Open the project in Visual Studio.</span></span>

<span data-ttu-id="272e2-157">*Pages/Courses/Index.cshtml.cs*를 열고 `OnGetAsync` 메서드를 검사합니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-157">Open *Pages/Courses/Index.cshtml.cs* and examine the `OnGetAsync` method.</span></span> <span data-ttu-id="272e2-158">스캐폴딩 엔진은 `Department` 탐색 속성에 대한 즉시 로드를 지정했습니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-158">The scaffolding engine specified eager loading for the `Department` navigation property.</span></span> <span data-ttu-id="272e2-159">`Include` 메서드가 즉시 로드를 지정합니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-159">The `Include` method specifies eager loading.</span></span>

<span data-ttu-id="272e2-160">앱을 실행하고 **과정** 링크를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-160">Run the app and select the **Courses** link.</span></span> <span data-ttu-id="272e2-161">Department 열에 도움이 되지 않는 `DepartmentID`가 표시됩니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-161">The department column displays the `DepartmentID`, which isn't useful.</span></span>

<span data-ttu-id="272e2-162">`OnGetAsync` 메서드를 다음 코드로 업데이트합니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-162">Update the `OnGetAsync` method with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod)]

<span data-ttu-id="272e2-163">위의 코드는 `AsNoTracking`을 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-163">The preceding code adds `AsNoTracking`.</span></span> <span data-ttu-id="272e2-164">반환된 엔터티는 추적되지 않으므로 `AsNoTracking`이 성능을 개선합니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-164">`AsNoTracking` improves performance because the entities returned are not tracked.</span></span> <span data-ttu-id="272e2-165">현재 컨텍스트에서 업데이트되지 않으므로 엔터티가 추적되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-165">The entities are not tracked because they're not updated in the current context.</span></span>

<span data-ttu-id="272e2-166">*Pages/Courses/Index.cshtml*을 다음 강조 표시된 내용으로 업데이트합니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-166">Update *Pages/Courses/Index.cshtml* with the following highlighted markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Courses/Index.cshtml?highlight=4,7,15-17,34-36,44)]

<span data-ttu-id="272e2-167">스캐폴드 코드에 다음 변경 내용을 적용했습니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-167">The following changes have been made to the scaffolded code:</span></span>

* <span data-ttu-id="272e2-168">제목을 Index(인덱스)에서 Courses(과정)로 변경했습니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-168">Changed the heading from Index to Courses.</span></span>
* <span data-ttu-id="272e2-169">`CourseID` 속성 값을 보여 주는 **Number** 열을 추가했습니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-169">Added a **Number** column that shows the `CourseID` property value.</span></span> <span data-ttu-id="272e2-170">일반적으로 최종 사용자에게 의미가 없으므로 기본적으로 기본 키는 스캐폴드되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-170">By default, primary keys aren't scaffolded because normally they're meaningless to end users.</span></span> <span data-ttu-id="272e2-171">그러나 이 경우 기본 키는 의미가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-171">However, in this case the primary key is meaningful.</span></span>
* <span data-ttu-id="272e2-172">부서 이름을 표시하도록 **Department** 열을 변경했습니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-172">Changed the **Department** column to display the department name.</span></span> <span data-ttu-id="272e2-173">코드는 `Department` 탐색 속성으로 로드되는 `Department` 엔터티의 `Name` 속성을 표시합니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-173">The code displays the `Name` property of the `Department` entity that's loaded into the `Department` navigation property:</span></span>

  ```html
  @Html.DisplayFor(modelItem => item.Department.Name)
  ```

<span data-ttu-id="272e2-174">앱을 실행하고 **Courses(과정)** 탭을 선택하여 부서 이름이 있는 목록을 봅니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-174">Run the app and select the **Courses** tab to see the list with department names.</span></span>

![과정 인덱스 페이지](read-related-data/_static/courses-index.png)

<a name="select"></a>
### <a name="loading-related-data-with-select"></a><span data-ttu-id="272e2-176">Select로 관련된 데이터 로드</span><span class="sxs-lookup"><span data-stu-id="272e2-176">Loading related data with Select</span></span>

<span data-ttu-id="272e2-177">`OnGetAsync` 메서드는 `Include` 메서드로 관련된 데이터를 로드합니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-177">The `OnGetAsync` method loads related data with the `Include` method:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

<span data-ttu-id="272e2-178">`Select` 연산자는 필요한 관련된 데이터만 로드합니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-178">The `Select` operator loads only the related data needed.</span></span> <span data-ttu-id="272e2-179">`Department.Name`과 같은 단일 항목에서는 SQL INNER JOIN을 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-179">For single items, like the `Department.Name` it uses a SQL INNER JOIN.</span></span> <span data-ttu-id="272e2-180">컬렉션의 경우 다른 데이터베이스 액세스를 사용하지만 컬렉션의 `Include` 연산자도 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-180">For collections, it uses another database access, but so does the `Include` operator on collections.</span></span>

<span data-ttu-id="272e2-181">다음 코드는 `Select` 메서드로 관련된 데이터를 로드합니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-181">The following code loads related data with the `Select` method:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

<span data-ttu-id="272e2-182">`CourseViewModel`:</span><span class="sxs-lookup"><span data-stu-id="272e2-182">The `CourseViewModel`:</span></span>

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/CourseViewModel.cs?name=snippet)]

<span data-ttu-id="272e2-183">전체 예제는 [IndexSelect.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml) 및 [IndexSelect.cshtml.cs](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs)를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="272e2-183">See [IndexSelect.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml) and [IndexSelect.cshtml.cs](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs) for a complete example.</span></span>

## <a name="create-an-instructors-page-that-shows-courses-and-enrollments"></a><span data-ttu-id="272e2-184">과정 및 등록을 보여 주는 강사 페이지 만들기</span><span class="sxs-lookup"><span data-stu-id="272e2-184">Create an Instructors page that shows Courses and Enrollments</span></span>

<span data-ttu-id="272e2-185">이 섹션에서는 강사 페이지가 생성됩니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-185">In this section, the Instructors page is created.</span></span>

<a name="IP"></a>
<span data-ttu-id="272e2-186">![강사 인덱스 페이지](read-related-data/_static/instructors-index.png)</span><span class="sxs-lookup"><span data-stu-id="272e2-186">![Instructors Index page](read-related-data/_static/instructors-index.png)</span></span>

<span data-ttu-id="272e2-187">이 페이지는 다음과 같은 방법으로 관련된 데이터를 읽고 표시합니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-187">This page reads and displays related data in the following ways:</span></span>

* <span data-ttu-id="272e2-188">강사 목록은 `OfficeAssignment` 엔터티에서 관련된 데이터를 표시합니다(이전 이미지에서 Office).</span><span class="sxs-lookup"><span data-stu-id="272e2-188">The list of instructors displays related data from the `OfficeAssignment` entity (Office in the preceding image).</span></span> <span data-ttu-id="272e2-189">`Instructor` 및 `OfficeAssignment` 엔터티는 일대영 또는 일 관계에 있습니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-189">The `Instructor` and `OfficeAssignment` entities are in a one-to-zero-or-one relationship.</span></span> <span data-ttu-id="272e2-190">즉시 로드는 `OfficeAssignment` 엔터티에 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-190">Eager loading is used for the `OfficeAssignment` entities.</span></span> <span data-ttu-id="272e2-191">즉시 로드는 일반적으로 관련된 데이터를 표시해야 할 때 더 효율적입니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-191">Eager loading is typically more efficient when the related data needs to be displayed.</span></span> <span data-ttu-id="272e2-192">이 경우 강사를 위한 사무실 할당이 표시됩니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-192">In this case, office assignments for the instructors are displayed.</span></span>
* <span data-ttu-id="272e2-193">사용자가 강사(이전 이미지에서 Harui)를 선택하는 경우 관련된 `Course` 엔터티가 표시됩니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-193">When the user selects an instructor (Harui in the preceding image), related `Course` entities are displayed.</span></span> <span data-ttu-id="272e2-194">`Instructor` 및 `Course` 엔터티는 다대다 관계에 있습니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-194">The `Instructor` and `Course` entities are in a many-to-many relationship.</span></span> <span data-ttu-id="272e2-195">`Course` 엔터티 및 관련 `Department` 엔터티에 대해 즉시 로드가 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-195">Eager loading is used for the `Course` entities and their related `Department` entities.</span></span> <span data-ttu-id="272e2-196">이 경우 선택한 강사에 대한 과정만 필요하므로 별도 쿼리가 더 효율적일 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-196">In this case, separate queries might be more efficient because only courses for the selected instructor are needed.</span></span> <span data-ttu-id="272e2-197">이 예제에서는 탐색 속성에 있는 엔터티에서 탐색 속성에 대한 즉시 로드를 사용하는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-197">This example shows how to use eager loading for navigation properties in entities that are in navigation properties.</span></span>
* <span data-ttu-id="272e2-198">사용자가 과정(이전 이미지에서 Chemistry)을 선택하면 `Enrollments` 엔터티의 관련된 데이터가 표시됩니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-198">When the user selects a course (Chemistry in the preceding image), related data from the `Enrollments` entity is displayed.</span></span> <span data-ttu-id="272e2-199">이전 이미지에서는 학생 이름 및 학점이 표시됩니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-199">In the preceding image, student name and grade are displayed.</span></span> <span data-ttu-id="272e2-200">`Course` 및 `Enrollment` 엔터티는 일대다 관계에 있습니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-200">The `Course` and `Enrollment` entities are in a one-to-many relationship.</span></span>

### <a name="create-a-view-model-for-the-instructor-index-view"></a><span data-ttu-id="272e2-201">강사 인덱스 뷰에 대한 뷰 모델 만들기</span><span class="sxs-lookup"><span data-stu-id="272e2-201">Create a view model for the Instructor Index view</span></span>

<span data-ttu-id="272e2-202">강사 페이지는 서로 다른 세 테이블의 데이터를 표시합니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-202">The instructors page shows data from three different tables.</span></span> <span data-ttu-id="272e2-203">세 개 테이블을 나타내는 세 개 엔터티가 포함된 뷰 모델이 생성됩니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-203">A view model is created that includes the three entities representing the three tables.</span></span>

<span data-ttu-id="272e2-204">다음 코드로 *SchoolViewModels* 폴더에서 *InstructorIndexData.cs*를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-204">In the *SchoolViewModels* folder, create *InstructorIndexData.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/InstructorIndexData.cs)]

### <a name="scaffold-the-instructor-model"></a><span data-ttu-id="272e2-205">강사 모델 스캐폴드</span><span class="sxs-lookup"><span data-stu-id="272e2-205">Scaffold the Instructor model</span></span>

* <span data-ttu-id="272e2-206">Visual Studio를 끝냅니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-206">Exit Visual Studio.</span></span>
* <span data-ttu-id="272e2-207">프로젝트 디렉터리(*Program.cs*, *Startup.cs* 및 *.csproj* 파일이 포함된 디렉터리)에서 명령 창을 엽니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-207">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="272e2-208">다음 명령을 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-208">Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Instructor -dc SchoolContext -udl -outDir Pages\Instructors --referenceScriptLibraries
  ```

<span data-ttu-id="272e2-209">위의 명령은 `Instructor` 모델을 스캐폴드합니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-209">The preceding command scaffolds the `Instructor` model.</span></span> <span data-ttu-id="272e2-210">Visual Studio에서 프로젝트를 엽니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-210">Open the project in Visual Studio.</span></span>

<span data-ttu-id="272e2-211">프로젝트를 빌드합니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-211">Build the project.</span></span> <span data-ttu-id="272e2-212">빌드에서 오류를 생성합니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-212">The build generates errors.</span></span>

<span data-ttu-id="272e2-213">`_context.Instructor`를 `_context.Instructors`로 전역으로 변경합니다(즉, “s”를 `Instructor`에 추가).</span><span class="sxs-lookup"><span data-stu-id="272e2-213">Globally change `_context.Instructor` to `_context.Instructors` (that is, add an "s" to `Instructor`).</span></span> <span data-ttu-id="272e2-214">7개 항목이 발견되어 업데이트됩니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-214">7 occurrences are found and updated.</span></span>

<span data-ttu-id="272e2-215">앱을 실행하고 강사 페이지로 이동합니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-215">Run the app and navigate to the instructors page.</span></span>

<span data-ttu-id="272e2-216">*Pages/Instructors/Index.cshtml.cs*를 다음 코드로 바꿉니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-216">Replace *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_all&highlight=2,18-99)]

<span data-ttu-id="272e2-217">`OnGetAsync` 메서드는 선택한 강사의 ID에 대해 경로 데이터(선택 사항)를 받아들입니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-217">The `OnGetAsync` method accepts optional route data for the ID of the selected instructor.</span></span>

<span data-ttu-id="272e2-218">*Pages/Instructors/Index.cshtml.cs* 파일에서 쿼리를 검사합니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-218">Examine the query in the *Pages/Instructors/Index.cshtml.cs* file:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_ThenInclude)]

<span data-ttu-id="272e2-219">쿼리에는 다음 두 include가 포함됩니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-219">The query has two includes:</span></span>

* <span data-ttu-id="272e2-220">`OfficeAssignment`: [강사 뷰](#IP)에 표시됩니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-220">`OfficeAssignment`: Displayed in the [instructors view](#IP).</span></span>
* <span data-ttu-id="272e2-221">`CourseAssignments`: 가르친 과정을 가져옵니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-221">`CourseAssignments`: Which brings in the courses taught.</span></span>


### <a name="update-the-instructors-index-page"></a><span data-ttu-id="272e2-222">강사 인덱스 페이지 업데이트</span><span class="sxs-lookup"><span data-stu-id="272e2-222">Update the instructors Index page</span></span>

<span data-ttu-id="272e2-223">*Pages/Instructors/Index.cshtml*을 다음 표시로 업데이트합니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-223">Update *Pages/Instructors/Index.cshtml* with the following markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=1-65&highlight=1,5,8,16-21,25-32,43-57)]

<span data-ttu-id="272e2-224">위의 표시로 다음이 변경됩니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-224">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="272e2-225">`page` 지시문을 `@page`에서 `@page "{id:int?}"`로 업데이트합니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-225">Updates the `page` directive from `@page` to `@page "{id:int?}"`.</span></span> <span data-ttu-id="272e2-226">`"{id:int?}"`는 경로 템플릿입니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-226">`"{id:int?}"` is a route template.</span></span> <span data-ttu-id="272e2-227">경로 템플릿은 데이터를 라우팅할 URL에서 정수 쿼리 문자열을 변경합니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-227">The route template changes integer query strings in the URL to route data.</span></span> <span data-ttu-id="272e2-228">예를 들어, `@page` 지시문만 있는 강사에 대해 **Select** 링크를 클릭하면 다음과 같은 URL이 생성됩니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-228">For example, clicking on the **Select** link for an instructor with only the `@page` directive produces a URL like the following:</span></span>

    `http://localhost:1234/Instructors?id=2`

    <span data-ttu-id="272e2-229">page 지시문이 `@page "{id:int?}"`이면, 이전 URL은 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-229">When the page directive is `@page "{id:int?}"`, the previous URL is:</span></span>

    `http://localhost:1234/Instructors/2`

* <span data-ttu-id="272e2-230">페이지 제목은 **Instructors(강사)** 입니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-230">Page title is **Instructors**.</span></span>
* <span data-ttu-id="272e2-231">`item.OfficeAssignment`가 Null이 아닌 경우에만 `item.OfficeAssignment.Location`을 표시하는 **Office** 열을 추가했습니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-231">Added an **Office** column that displays `item.OfficeAssignment.Location` only if `item.OfficeAssignment` isn't null.</span></span> <span data-ttu-id="272e2-232">이는 일대영 또는 일 관계이기 때문에 관련된 OfficeAssignment 엔터티가 있을 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-232">Because this is a one-to-zero-or-one relationship, there might not be a related OfficeAssignment entity.</span></span>

  ```html
  @if (item.OfficeAssignment != null)
  {
      @item.OfficeAssignment.Location
  }
  ```

* <span data-ttu-id="272e2-233">각 강사가 가르치는 과정을 표시하는 **Courses** 열을 추가했습니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-233">Added a **Courses** column that displays courses taught by each instructor.</span></span> <span data-ttu-id="272e2-234">이 razor 구문에 대한 자세한 내용은 [`@:`를 사용하여 명시적 줄 전환](xref:mvc/views/razor#explicit-line-transition-with-)을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="272e2-234">See [Explicit Line Transition with `@:`](xref:mvc/views/razor#explicit-line-transition-with-) for more about this razor syntax.</span></span>

* <span data-ttu-id="272e2-235">선택된 강사의 `tr` 요소에 `class="success"`를 동적으로 추가하는 코드를 추가했습니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-235">Added code that dynamically adds `class="success"` to the `tr` element of the selected instructor.</span></span> <span data-ttu-id="272e2-236">부트스트랩 클래스를 사용하여 선택된 행에 대한 배경색을 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-236">This sets a background color for the selected row using a Bootstrap class.</span></span>

  ```html
  string selectedRow = "";
  if (item.CourseID == Model.CourseID)
  {
      selectedRow = "success";
  }
  <tr class="@selectedRow">
  ```

* <span data-ttu-id="272e2-237">**Select**로 레이블 지정된 새 하이퍼링크를 추가했습니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-237">Added a new hyperlink labeled **Select**.</span></span> <span data-ttu-id="272e2-238">이 링크는 선택한 강사의 ID를 `Index` 메서드에 보내고 배경색을 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-238">This link sends the selected instructor's ID to the `Index` method and sets a background color.</span></span>

  ```html
  <a asp-action="Index" asp-route-id="@item.ID">Select</a> |
  ```

<span data-ttu-id="272e2-239">앱을 실행하고 **강사** 탭을 선택합니다. 페이지에 관련된 `OfficeAssignment` 엔터티의 `Location`(사무실)이 표시됩니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-239">Run the app and select the **Instructors** tab. The page displays the `Location` (office) from the related `OfficeAssignment` entity.</span></span> <span data-ttu-id="272e2-240">OfficeAssignment가 Null이면 빈 테이블 셀이 표시됩니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-240">If OfficeAssignment\` is null, an empty table cell is displayed.</span></span>

![선택한 항목이 없는 강사 인덱스 페이지](read-related-data/_static/instructors-index-no-selection.png)

<span data-ttu-id="272e2-242">**Select** 링크를 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-242">Click on the **Select** link.</span></span> <span data-ttu-id="272e2-243">행 스타일이 변경됩니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-243">The row style changes.</span></span>

### <a name="add-courses-taught-by-selected-instructor"></a><span data-ttu-id="272e2-244">선택한 강사가 가르친 과정 추가</span><span class="sxs-lookup"><span data-stu-id="272e2-244">Add courses taught by selected instructor</span></span>

<span data-ttu-id="272e2-245">*Pages/Instructors/Index.cshtml.cs*에서 `OnGetAsync` 메서드를 다음 코드로 업데이트합니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-245">Update the `OnGetAsync` method in *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_OnGetAsync&highlight=1,8,16-999)]

<span data-ttu-id="272e2-246">`public int CourseID { get; set; }` 추가</span><span class="sxs-lookup"><span data-stu-id="272e2-246">Add `public int CourseID { get; set; }`</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_1&highlight=12)]

<span data-ttu-id="272e2-247">업데이트된 쿼리를 검토합니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-247">Examine the updated query:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ThenInclude)]

<span data-ttu-id="272e2-248">이전 쿼리는 `Department` 엔터티를 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-248">The preceding query adds the `Department` entities.</span></span>

<span data-ttu-id="272e2-249">다음 코드는 강사가 선택되었을 때 실행됩니다(`id != null`).</span><span class="sxs-lookup"><span data-stu-id="272e2-249">The following code executes when an instructor is selected (`id != null`).</span></span> <span data-ttu-id="272e2-250">선택한 강사는 뷰 모델의 강사 목록에서 검색됩니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-250">The selected instructor is retrieved from the list of instructors in the view model.</span></span> <span data-ttu-id="272e2-251">뷰 모델의 `Courses` 속성은 해당 강사의 `CourseAssignments` 탐색 속성에서 `Course` 엔터티로 로드됩니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-251">The view model's `Courses` property is loaded with the `Course` entities from that instructor's `CourseAssignments` navigation property.</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ID)]

<span data-ttu-id="272e2-252">`Where` 메서드는 컬렉션도 반환합니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-252">The `Where` method returns a collection.</span></span> <span data-ttu-id="272e2-253">앞의 `Where` 메서드에서 단일 `Instructor` 엔터티만 반환됩니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-253">In the preceding `Where` method, only a single `Instructor` entity is returned.</span></span> <span data-ttu-id="272e2-254">`Single` 메서드는 컬렉션을 단일 `Instructor` 엔터티로 변환합니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-254">The `Single` method converts the collection into a single `Instructor` entity.</span></span> <span data-ttu-id="272e2-255">`Instructor` 엔터티는 `CourseAssignments` 속성에 대한 액세스를 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-255">The `Instructor` entity provides access to the `CourseAssignments` property.</span></span> <span data-ttu-id="272e2-256">`CourseAssignments`는 관련 `Course` 엔터티에 대한 액세스를 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-256">`CourseAssignments` provides access to the related `Course` entities.</span></span>

![강사 및 과정 간 관계 m:M](complex-data-model/_static/courseassignment.png)

<span data-ttu-id="272e2-258">`Single` 메서드는 컬렉션에 한 개 항목만 있을 때 컬렉션에서 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-258">The `Single` method is used on a collection when the collection has only one item.</span></span> <span data-ttu-id="272e2-259">`Single` 메서드는 컬렉션이 비어 있거나 둘 이상의 항목이 있는 경우 예외를 throw합니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-259">The `Single` method throws an exception if the collection is empty or if there's more than one item.</span></span> <span data-ttu-id="272e2-260">대안은 `SingleOrDefault`입니다. 컬렉션이 비어 있는 경우 기본값을 반환합니다(이 경우 Null).</span><span class="sxs-lookup"><span data-stu-id="272e2-260">An alternative is `SingleOrDefault`, which returns a default value (null in this case) if the collection is empty.</span></span> <span data-ttu-id="272e2-261">비어 있는 컬렉션에 `SingleOrDefault` 사용</span><span class="sxs-lookup"><span data-stu-id="272e2-261">Using `SingleOrDefault` on an empty collection:</span></span>

* <span data-ttu-id="272e2-262">예외가 발생합니다(null 참조에서 `Courses` 속성을 찾으려고 시도하므로).</span><span class="sxs-lookup"><span data-stu-id="272e2-262">Results in an exception (from trying to find a `Courses` property on a null reference).</span></span>
* <span data-ttu-id="272e2-263">예외 메시지로는 문제의 원인을 명확하게 알기 어렵습니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-263">The exception message would less clearly indicate the cause of the problem.</span></span>

<span data-ttu-id="272e2-264">다음 코드는 과정을 선택할 때 뷰 모델의 `Enrollments` 속성을 채웁니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-264">The following code populates the view model's `Enrollments` property when a course is selected:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_courseID)]

<span data-ttu-id="272e2-265">다음 태그를 *Pages/Instructors/Index.cshtml* Razor Page 끝에 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-265">Add the following markup to the end of the *Pages/Instructors/Index.cshtml* Razor Page:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=60-102&highlight=7-999)]

<span data-ttu-id="272e2-266">위의 표시는 강사가 선택된 경우 강사와 관련된 과정의 목록을 표시합니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-266">The preceding markup displays a list of courses related to an instructor when an instructor is selected.</span></span>

<span data-ttu-id="272e2-267">앱을 테스트합니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-267">Test the app.</span></span> <span data-ttu-id="272e2-268">강사 페이지에서 **Select** 링크를 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-268">Click on a **Select** link on the instructors page.</span></span>

![강사가 선택된 강사 인덱스 페이지](read-related-data/_static/instructors-index-instructor-selected.png)

### <a name="show-student-data"></a><span data-ttu-id="272e2-270">학생 데이터 표시</span><span class="sxs-lookup"><span data-stu-id="272e2-270">Show student data</span></span>

<span data-ttu-id="272e2-271">이 섹션에서는 선택한 과정에 대한 학생 데이터를 표시하도록 앱이 업데이트됩니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-271">In this section, the app is updated to show the student data for a selected course.</span></span>

<span data-ttu-id="272e2-272">*Pages/Instructors/Index.cshtml.cs*에서 `OnGetAsync` 메서드의 쿼리를 다음 코드로 업데이트합니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-272">Update the query in the `OnGetAsync` method in *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

<span data-ttu-id="272e2-273">*Pages/Instructors/Index.cshtml*을 업데이트합니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-273">Update *Pages/Instructors/Index.cshtml*.</span></span> <span data-ttu-id="272e2-274">다음 표시를 파일 끝에 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-274">Add the following markup to the end of the file:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=103-)]

<span data-ttu-id="272e2-275">이전 표시는 선택한 과정을 등록한 학생의 목록을 표시합니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-275">The preceding markup displays a list of the students who are enrolled in the selected course.</span></span>

<span data-ttu-id="272e2-276">페이지를 새로 고치고 강사를 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-276">Refresh the page and select an instructor.</span></span> <span data-ttu-id="272e2-277">과정을 선택하여 등록된 학생 및 해당 등급의 목록을 봅니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-277">Select a course to see the list of enrolled students and their grades.</span></span>

![강사 및 과정이 선택된 강사 인덱스 페이지](read-related-data/_static/instructors-index.png)

## <a name="using-single"></a><span data-ttu-id="272e2-279">Single 사용</span><span class="sxs-lookup"><span data-stu-id="272e2-279">Using Single</span></span>

<span data-ttu-id="272e2-280">`Single` 메서드는 `Where` 메서드를 별도로 호출하는 대신 `Where` 조건을 전달할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-280">The `Single` method can pass in the `Where` condition instead of calling the `Where` method separately:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/IndexSingle.cshtml.cs?name=snippet_single&highlight=21-22,30-31)]

<span data-ttu-id="272e2-281">위의 `Single` 접근 방식은 `Where`를 사용하는 것보다 장점을 제공하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-281">The preceding `Single` approach provides no benefits over using `Where`.</span></span> <span data-ttu-id="272e2-282">일부 개발자는 `Single` 방식을 선호합니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-282">Some developers prefer the `Single` approach style.</span></span>

## <a name="explicit-loading"></a><span data-ttu-id="272e2-283">명시적 로드</span><span class="sxs-lookup"><span data-stu-id="272e2-283">Explicit loading</span></span>

<span data-ttu-id="272e2-284">현재 코드는 `Enrollments` 및 `Students`에 대한 즉시 로드를 지정합니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-284">The current code specifies eager loading for `Enrollments` and `Students`:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

<span data-ttu-id="272e2-285">사용자가 과정에 등록된 내용을 거의 보지 않는다고 가정해 보겠습니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-285">Suppose users rarely want to see enrollments in a course.</span></span> <span data-ttu-id="272e2-286">이 경우 최적화는 요청된 경우에만 등록 데이터를 로드합니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-286">In that case, an optimization would be to only load the enrollment data if it's requested.</span></span> <span data-ttu-id="272e2-287">이 섹션에서는 `Enrollments` 및 `Students`의 명시적 로드를 사용하도록 `OnGetAsync`가 업데이트됩니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-287">In this section, the `OnGetAsync` is updated to use explicit loading of `Enrollments` and `Students`.</span></span>

<span data-ttu-id="272e2-288">`OnGetAsync`를 다음 코드로 업데이트합니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-288">Update the `OnGetAsync` with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/IndexXp.cshtml.cs?name=snippet_OnGetAsync&highlight=9-13,29-35)]

<span data-ttu-id="272e2-289">위의 코드는 등록 및 학생 데이터에 대한 *ThenInclude* 메서드 호출을 삭제합니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-289">The preceding code drops the *ThenInclude* method calls for enrollment and student data.</span></span> <span data-ttu-id="272e2-290">과정을 선택하면 강조 표시된 코드가 다음을 검색합니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-290">If a course is selected, the highlighted code retrieves:</span></span>

* <span data-ttu-id="272e2-291">선택한 과정에 대한 `Enrollment` 엔터티</span><span class="sxs-lookup"><span data-stu-id="272e2-291">The `Enrollment` entities for the selected course.</span></span>
* <span data-ttu-id="272e2-292">각 `Enrollment`에 대한 `Student` 엔터티</span><span class="sxs-lookup"><span data-stu-id="272e2-292">The `Student` entities for each `Enrollment`.</span></span>

<span data-ttu-id="272e2-293">위의 코드에서는 `.AsNoTracking()`을 주석 처리했습니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-293">Notice the preceding code comments out `.AsNoTracking()`.</span></span> <span data-ttu-id="272e2-294">탐색 속성은 추적된 엔터티에 대해서만 명시적으로 로드할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-294">Navigation properties can only be explicitly loaded for tracked entities.</span></span>

<span data-ttu-id="272e2-295">앱을 테스트합니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-295">Test the app.</span></span> <span data-ttu-id="272e2-296">사용자 관점에서 앱은 이전 버전과 동일하게 동작합니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-296">From a users perspective, the app behaves identically to the previous version.</span></span>

<span data-ttu-id="272e2-297">다음 자습서에서는 관련된 데이터를 업데이트하는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="272e2-297">The next tutorial shows how to update related data.</span></span>

>[!div class="step-by-step"]
><span data-ttu-id="272e2-298">[이전](xref:data/ef-rp/complex-data-model)
>[다음](xref:data/ef-rp/update-related-data)</span><span class="sxs-lookup"><span data-stu-id="272e2-298">[Previous](xref:data/ef-rp/complex-data-model)
[Next](xref:data/ef-rp/update-related-data)</span></span>
