---
title: ASP.NET Core에서 SQL Server LocalDB 사용
author: rick-anderson
description: 간단한 ASP.NET Core MVC 앱에서 SQL Server LocalDB를 사용하는 방법에 대해 배웁니다.
manager: wpickett
ms.author: riande
ms.date: 03/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app/working-with-sql
ms.openlocfilehash: 3f69657cb21e163bdf00fb1faea98889046e9b45
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/06/2018
---
# <a name="work-with-sql-server-localdb-in-aspnet-core"></a>ASP.NET Core에서 SQL Server LocalDB 사용

작성자: [Rick Anderson](https://twitter.com/RickAndMSFT)

`MvcMovieContext` 개체는 데이터베이스에 연결하고 데이터베이스 레코드에 `Movie` 개체를 매핑하는 작업을 처리합니다. 데이터베이스 컨텍스트는 *Startup.cs* 파일의 `ConfigureServices` 메서드에서 [종속성 주입](xref:fundamentals/dependency-injection) 컨테이너에 등록됩니다.

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=ConfigureServices&highlight=6-7)]

ASP.NET Core [구성](xref:fundamentals/configuration/index) 시스템은 `ConnectionString`을 읽습니다. 로컬 개발의 경우 *appsettings.json* 파일에서 연결 문자열을 가져옵니다.

[!code-json[](start-mvc/sample/MvcMovie/appsettings.json?highlight=2&range=8-10)]

테스트 또는 프로덕션 서버에 앱을 배포할 때 환경 변수 또는 다른 방법을 사용하여 실제 SQL Server에 연결 문자열을 설정할 수 있습니다. 자세한 내용은 [구성](xref:fundamentals/configuration/index)을 참조하세요.

## <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

LocalDB는 프로그램 개발용으로 대상이 지정된 SQL Server Express 데이터베이스 엔진의 간단 버전입니다. LocalDB는 요청 시 시작하고 사용자 모드에서 실행되므로 복잡한 구성이 없습니다. 기본적으로 LocalDB 데이터베이스는 *C:/사용자/\<user\>* 디렉터리에 "\*.mdf" 파일을 만듭니다.

* **보기** 메뉴에서 SSOX(**SQL Server 개체 탐색기**)를 엽니다.

  ![보기 메뉴](working-with-sql/_static/ssox.png)

* 마우스 오른쪽 단추로 `Movie` 테이블 **> 디자이너 보기**를 클릭합니다.

  ![동영상 테이블에서 열린 바로 가기 메뉴](working-with-sql/_static/design.png)

  ![디자이너에 열린 동영상 테이블](working-with-sql/_static/dv.png)

`ID` 옆의 키 아이콘을 확인합니다. 기본적으로 EF는 `ID`라는 속성을 기본 키로 만듭니다.

* 마우스 오른쪽 단추로 `Movie` 테이블 **> 데이터 보기**를 클릭합니다.

  ![동영상 테이블에서 열린 바로 가기 메뉴](working-with-sql/_static/ssox2.png)

  ![테이블 데이터를 보여 주는 열린 동영상 테이블](working-with-sql/_static/vd22.png)

## <a name="seed-the-database"></a>데이터베이스 시드

*Models* 폴더에 `SeedData`라는 새 클래스를 만듭니다. 생성된 코드를 다음으로 바꿉니다.

[!code-csharp[](start-mvc/sample/MvcMovie/Models/SeedData.cs?name=snippet_1)]

DB에 동영상이 있는 경우 시드 이니셜라이저가 반환되고 동영상이 추가되지 않습니다.

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```

<a name="si"></a>
### <a name="add-the-seed-initializer"></a>시드 이니셜라이저 추가

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)
*Program.cs* 파일에서 `Main` 메서드에 시드 이니셜라이저를 추가합니다.

[!code-csharp[](start-mvc/sample/MvcMovie/Program.cs?highlight=6,14-32)]

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)
*Startup.cs* 파일에서 `Configure` 메서드의 끝에 시드 이니셜라이저를 추가합니다.

[!code-csharp[](start-mvc/sample/MvcMovie/Startup.cs?highlight=9&name=snippet_seed)]

* * *
앱 테스트

* DB의 모든 레코드를 삭제합니다. 브라우저 또는 SSOX에서 삭제 링크를 사용하여 이를 수행할 수 있습니다.
* 시드 메서드가 실행되도록 앱에서 초기화를 적용하도록 합니다(`Startup` 클래스에서 메서드 호출). 초기화를 적용하려면 IIS Express를 중지하고 다시 시작해야 합니다. 다음 접근 방식 중 하나를 사용하여 이를 수행할 수 있습니다.

  * 알림 영역에서 IIS Express 시스템 트레이 아이콘을 마우스 오른쪽 단추로 클릭하고 **종료** 또는 **사이트 중지**를 누릅니다.

    ![IIS Express 시스템 트레이 아이콘](working-with-sql/_static/iisExIcon.png)

    ![바로 가기 메뉴](working-with-sql/_static/stopIIS.png)

    * 비디버그 모드에서 VS를 실행했던 경우 F5 키를 눌러 디버그 모드에서 실행되도록 합니다.
    * 디버그 모드에서 VS를 실행했던 경우 디버거를 중지하고 F5 키를 누릅니다.

앱에서 시드된 데이터를 보여 줍니다.

![동영상 데이터를 표시하는 Microsoft Edge에서 열린 MVC 동영상 응용 프로그램](working-with-sql/_static/m55.png)

> [!div class="step-by-step"]
> [이전](adding-model.md)
> [다음](controller-methods-views.md)  
