<a name="dc"></a>

### <a name="add-a-database-context-class"></a><span data-ttu-id="15a92-101">데이터베이스 컨텍스트 클래스 추가</span><span class="sxs-lookup"><span data-stu-id="15a92-101">Add a database context class</span></span>

<span data-ttu-id="15a92-102">RazorPagesMovie 프로젝트에서 *Data*라는 새 폴더를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="15a92-102">In the RazorPagesMovie project, create a new folder called *Data*.</span></span> <span data-ttu-id="15a92-103">다음 `RazorPagesMovieContext` 클래스를 *Data* 폴더에 추가:</span><span class="sxs-lookup"><span data-stu-id="15a92-103">Add the following `RazorPagesMovieContext` class to the *Data* folder:</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="15a92-104">이전 코드에서는 엔터티 집합에 대한 `DbSet` 속성을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="15a92-104">The preceding code creates a `DbSet` property for the entity set.</span></span> <span data-ttu-id="15a92-105">Entity Framework 용어에서 엔터티 집합은 일반적으로 데이터베이스 테이블에 해당하고 엔터티는 테이블의 행에 해당합니다.</span><span class="sxs-lookup"><span data-stu-id="15a92-105">In Entity Framework terminology, an entity set typically corresponds to a database table, and an entity corresponds to a row in the table.</span></span> <span data-ttu-id="15a92-106">이후 단계에서 종속성이 추가될 때까지 코드는 컴파일되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="15a92-106">The code won't compile until dependencies are added in a later step.</span></span>

<a name="cs"></a>

### <a name="add-a-database-connection-string"></a><span data-ttu-id="15a92-107">데이터베이스 연결 문자열 추가</span><span class="sxs-lookup"><span data-stu-id="15a92-107">Add a database connection string</span></span>

<span data-ttu-id="15a92-108">다음 강조된 코드에 표시된 대로 연결 문자열을 *appsettings.json* 파일에 추가:</span><span class="sxs-lookup"><span data-stu-id="15a92-108">Add a connection string to the *appsettings.json* file as shown in the following highlighted code:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/appsettings_SQLite.json?highlight=10-12)]

### <a name="add-nuget-packages-and-ef-tools"></a><span data-ttu-id="15a92-109">NuGet 패키지 및 EF 도구 추가</span><span class="sxs-lookup"><span data-stu-id="15a92-109">Add NuGet packages and EF tools</span></span>

[!INCLUDE[](~/includes/add-EF-NuGet-SQLite-CLI.md)]

<a name="reg"></a>

### <a name="register-the-database-context"></a><span data-ttu-id="15a92-110">데이터베이스 컨텍스트 등록</span><span class="sxs-lookup"><span data-stu-id="15a92-110">Register the database context</span></span>

<span data-ttu-id="15a92-111">*Startup.cs* 맨 위에 다음 `using` 문을 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="15a92-111">Add the following `using` statements at the top of *Startup.cs*:</span></span>

```csharp
using RazorPagesMovie.Data;
using Microsoft.EntityFrameworkCore;
```

<span data-ttu-id="15a92-112">데이터베이스 컨텍스트를 `Startup.ConfigureServices`의 [종속성 주입](xref:fundamentals/dependency-injection) 컨테이너에 등록합니다.</span><span class="sxs-lookup"><span data-stu-id="15a92-112">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-9)]

### <a name="add-required-nuget-packages"></a><span data-ttu-id="15a92-113">필요한 NuGet 패키지 추가</span><span class="sxs-lookup"><span data-stu-id="15a92-113">Add required NuGet packages</span></span>

<span data-ttu-id="15a92-114">다음 .NET Core CLI 명령을 실행하여 SQLite 및 CodeGeneration.Design을 프로젝트에 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="15a92-114">Run the following .NET Core CLI command to add SQLite and CodeGeneration.Design to the project:</span></span>

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.SQLite
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design
```

<span data-ttu-id="15a92-115">스캐폴딩에는 `Microsoft.VisualStudio.Web.CodeGeneration.Design` 패키지가 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="15a92-115">The `Microsoft.VisualStudio.Web.CodeGeneration.Design` package is required for scaffolding.</span></span>

<a name="reg"></a>

### <a name="register-the-database-context"></a><span data-ttu-id="15a92-116">데이터베이스 컨텍스트 등록</span><span class="sxs-lookup"><span data-stu-id="15a92-116">Register the database context</span></span>

<span data-ttu-id="15a92-117">*Startup.cs* 맨 위에 다음 `using` 문을 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="15a92-117">Add the following `using` statements at the top of *Startup.cs*:</span></span>

```csharp
using RazorPagesMovie.Models;
using Microsoft.EntityFrameworkCore;
```

<span data-ttu-id="15a92-118">데이터베이스 컨텍스트를 `Startup.ConfigureServices`의 [종속성 주입](xref:fundamentals/dependency-injection) 컨테이너에 등록합니다.</span><span class="sxs-lookup"><span data-stu-id="15a92-118">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

<span data-ttu-id="15a92-119">오류에 대한 검사 방법으로 프로젝트를 빌드합니다.</span><span class="sxs-lookup"><span data-stu-id="15a92-119">Build the project as a check for errors.</span></span>

::: moniker-end
