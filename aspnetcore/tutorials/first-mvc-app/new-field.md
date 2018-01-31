---
title: "새 필드 추가"
author: rick-anderson
description: 
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app/new-field
ms.openlocfilehash: 046e16b1a9581edb2be9a2315cf7f2677684747b
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/19/2018
---
# <a name="adding-a-new-field"></a>새 필드 추가

작성자: [Rick Anderson](https://twitter.com/RickAndMSFT)

이 섹션에서는 [Entity Framework](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) Code First 마이그레이션을 사용하여 모델에 새 필드를 추가하고 해당 변경 내용을 데이터베이스로 마이그레이션합니다.

EF Code First를 사용하여 자동으로 데이터베이스를 만들 때 Code First는 데이터베이스에 테이블을 추가하여 데이터베이스의 스키마가 생성된 모델 클래스와 동기화되어 있는지 여부를 추적합니다. 동기화되어 있지 않은 경우 EF에서 예외를 throw합니다. 이렇게 하면 더 쉽게 일관성이 없는 데이터베이스/코드 문제를 찾을 수 있습니다.

## <a name="adding-a-rating-property-to-the-movie-model"></a>동영상 모델에 등급 속성 추가

*Models/Movie.cs* 파일을 열고 `Rating` 속성을 추가합니다.

[!code-csharp[Main](start-mvc/sample/MvcMovie/Models/MovieDateRating.cs?highlight=11&range=7-18)]

앱을 빌드합니다(Ctrl+Shift+B).

`Movie` 클래스에 새 필드를 추가했으므로 이 새 속성이 포함되도록 바인딩 허용 목록도 업데이트해야 합니다. *MoviesController.cs*에서 `Rating` 속성을 포함하도록 `Create` 및 `Edit` 동작 메서드에 대해 `[Bind]` 속성을 업데이트합니다.

```csharp
[Bind("ID,Title,ReleaseDate,Genre,Price,Rating")]
   ```

브라우저 보기에서 새 `Rating` 속성을 표시, 작성 및 편집하기 위해 보기 템플릿도 업데이트해야 합니다.

*/Views/Movies/Index.cshtml* 파일을 편집하고 `Rating` 필드를 추가합니다.

[!code-HTML[Main](start-mvc/sample/MvcMovie/Views/Movies/IndexGenreRating.cshtml?highlight=17,39&range=24-64)]

`Rating` 필드로 */Views/Movies/Create.cshtml*을 업데이트합니다. 이전 "양식 그룹"을 복사/붙여넣기하고 intelliSense에서 필드를 업데이트하도록 할 수 있습니다. IntelliSense는 [태그 도우미](xref:mvc/views/tag-helpers/intro)와 함께 작동합니다. 참고: Visual Studio 2017의 RTM 버전에서 Razor intelliSense에 대해 [Razor 언어 서비스](https://marketplace.visualstudio.com/items?itemName=ms-madsk.RazorLanguageServices)를 설치해야 합니다. 그러면 다음 릴리스에서 수정됩니다.

![개발자가 보기의 두 번째 레이블 요소에서 asp-for의 특성 값에 대해 문자 R을 입력했습니다. 목록에서 자동으로 강조 표시되는 Rating을 포함하여 사용 가능한 필드를 보여 주는 Intellisense 바로 가기 메뉴가 표시되었습니다. 개발자가 필드를 클릭하거나 키보드에서 Enter 키를 누르면 값은 Rating으로 설정됩니다.](new-field/_static/cr.png)

앱은 새 필드를 포함하도록 DB를 업데이트할 때까지 작동하지 않습니다. 지금 실행하는 경우 다음 `SqlException`이 발생합니다.

`SqlException: Invalid column name 'Rating'.`

업데이트된 Movie 모델 클래스는 기존 데이터베이스의 Movie 테이블의 스키마와 다르기 때문에 이 오류가 표시됩니다. (데이터베이스 테이블에 Rating 열이 없습니다.)

오류를 해결하는 몇 가지 방법이 있습니다.

1. Entity Framework에서 새 모델 클래스 스키마에 따라 데이터베이스를 자동으로 삭제하고 다시 만들도록 합니다. 이 방법은 테스트 데이터베이스에서 활발한 개발을 수행할 때 개발 주기의 초기 단계에서 매우 편리하며 신속하게 모델 및 데이터베이스 스키마를 함께 개발할 수 있습니다. 그러나 단점은 데이터베이스에서 기존 데이터를 손실한다는 것입니다. 따라서 프로덕션 데이터베이스에서 이 방법을 사용하지 않으려 합니다. 테스트 데이터로 데이터베이스를 자동으로 시드하는 데 이니셜라이저를 사용하는 것은 종종 응용 프로그램을 개발하는 효율적인 방법입니다.

2. 모델 클래스와 일치하도록 기존 데이터베이스의 스키마를 명시적으로 수정합니다. 이 방법의 장점은 데이터를 유지한다는 점입니다. 이러한 변경을 수동으로 수행하거나 데이터베이스 변경 스크립트를 만들어 수행할 수 있습니다.

3. Code First 마이그레이션을 사용하여 데이터베이스 스키마를 업데이트합니다.

이 자습서의 경우 Code First 마이그레이션을 사용합니다.

새 열에 대해 값을 제공하도록 `SeedData` 클래스를 업데이트합니다. 샘플 변경은 아래에 표시되지만 각 `new Movie`에 대해 이 변경을 수행하려고 합니다.

[!code-csharp[Main](start-mvc/sample/MvcMovie/Models/SeedDataRating.cs?name=snippet1&highlight=6)]

솔루션을 빌드합니다.

**도구** 메뉴에서 **NuGet 패키지 관리자 > 패키지 관리자 콘솔**을 선택합니다.

  ![PMC 메뉴](adding-model/_static/pmc.png)

PMC에서 다음 명령을 입력합니다.

```powershell
Add-Migration Rating
Update-Database
```

`Add-Migration` 명령은 마이그레이션 프레임워크에서 현재 `Movie` DB 스키마로 현재 `Movie` 모델을 검사하고 DB를 새 모델로 마이그레이션하는 데 필요한 코드를 만들도록 합니다. "Rating" 이름은 임의로 지정되며 마이그레이션 파일의 이름을 지정하는 데 사용됩니다. 마이그레이션 파일에 의미 있는 이름을 사용하는 것이 좋습니다.

DB의 모든 레코드를 삭제하는 경우 이니셜라이저에서 DB를 시드하고 `Rating` 필드를 포함합니다. 브라우저 또는 SSOX에서 삭제 링크를 사용하여 이를 수행할 수 있습니다.

앱을 실행하고 `Rating` 필드를 사용하여 동영상을 만들고/편집/표시할 수 있는지 확인합니다. 또한 `Edit`, `Details` 및 `Delete` 보기 템플릿에 `Rating` 필드를 추가해야 합니다.

>[!div class="step-by-step"]
[이전](search.md)
[다음](validation.md)  
