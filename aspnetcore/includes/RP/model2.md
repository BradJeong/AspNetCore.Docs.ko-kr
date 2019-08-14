<a name="dc"></a>

### <a name="add-a-database-context-class"></a>데이터베이스 컨텍스트 클래스 추가

RazorPagesMovie 프로젝트에서 *Data*라는 새 폴더를 만듭니다. 다음 `RazorPagesMovieContext` 클래스를 *Data* 폴더에 추가:

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

이전 코드에서는 엔터티 집합에 대한 `DbSet` 속성을 만듭니다. Entity Framework 용어에서 엔터티 집합은 일반적으로 데이터베이스 테이블에 해당하고 엔터티는 테이블의 행에 해당합니다.

<a name="cs"></a>

### <a name="add-a-database-connection-string"></a>데이터베이스 연결 문자열 추가

다음 강조된 코드에 표시된 대로 연결 문자열을 *appsettings.json* 파일에 추가:

::: moniker range=">= aspnetcore-3.0"

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/appsettings_SQLite.json?highlight=10-12)]

### <a name="add-nuget-packages-and-ef-tools"></a>NuGet 패키지 및 EF 도구 추가

RazorPagesMovie 프로젝트의 터미널을 엽니다.  디자인/레이아웃 표시줄에서 프로젝트 이름을 마우스 오른쪽 단추로 클릭하고 터미널에서 **도구 > 열기**로 이동합니다. 터미널에서 다음 .NET Core CLI 명령을 실행합니다.

```console
dotnet tool install --global dotnet-ef --version 3.0.0-*
dotnet add package Microsoft.EntityFrameworkCore.SQLite --version 3.0.0-*
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design --version 3.0.0-*
dotnet add package Microsoft.EntityFrameworkCore.Design --version 3.0.0-*
dotnet add package Microsoft.EntityFrameworkCore.SqlServer --version 3.0.0-*
```

위의 명령은 .NET CLI 및 여러 패키지의 Entity Framework Core 도구를 프로젝트에 추가합니다. 스캐폴딩에는 `Microsoft.VisualStudio.Web.CodeGeneration.Design` 패키지가 필요합니다.

<a name="reg"></a>

### <a name="register-the-database-context"></a>데이터베이스 컨텍스트 등록

*Startup.cs* 맨 위에 다음 `using` 문을 추가합니다.

```csharp
using RazorPagesMovie.Models;
using Microsoft.EntityFrameworkCore;
```

데이터베이스 컨텍스트를 `Startup.ConfigureServices`의 [종속성 주입](xref:fundamentals/dependency-injection) 컨테이너에 등록합니다.

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-9)]

### <a name="add-required-nuget-packages"></a>필요한 NuGet 패키지 추가

다음 .NET Core CLI 명령을 실행하여 SQLite 및 CodeGeneration.Design을 프로젝트에 추가합니다.

```console
dotnet add package Microsoft.EntityFrameworkCore.SQLite
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design

```

스캐폴딩에는 `Microsoft.VisualStudio.Web.CodeGeneration.Design` 패키지가 필요합니다.

<a name="reg"></a>

### <a name="register-the-database-context"></a>데이터베이스 컨텍스트 등록

*Startup.cs* 맨 위에 다음 `using` 문을 추가합니다.

```csharp
using RazorPagesMovie.Models;
using Microsoft.EntityFrameworkCore;
```

데이터베이스 컨텍스트를 `Startup.ConfigureServices`의 [종속성 주입](xref:fundamentals/dependency-injection) 컨테이너에 등록합니다.

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

오류에 대한 검사 방법으로 프로젝트를 빌드합니다.
::: moniker-end
