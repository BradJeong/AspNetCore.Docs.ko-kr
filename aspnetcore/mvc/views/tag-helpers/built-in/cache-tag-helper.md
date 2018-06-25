---
title: ASP.NET Core MVC의 캐시 태그 도우미
author: pkellner
description: 캐시 태그 도우미 사용 방법 소개
ms.author: riande
ms.date: 02/14/2017
uid: mvc/views/tag-helpers/builtin-th/cache-tag-helper
ms.openlocfilehash: 969716e21211513053f52049368a0a7190ffba47
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36276554"
---
# <a name="cache-tag-helper-in-aspnet-core-mvc"></a>ASP.NET Core MVC의 캐시 태그 도우미

작성자: [Peter Kellner](http://peterkellner.net) 

캐시 태그 도우미는 콘텐츠를 내부 ASP.NET Core 캐시 공급자에 캐시하여 ASP.NET Core 앱 성능을 획기적으로 개선하는 기능을 제공합니다.

Razor 보기 엔진은 기본 `expires-after`를 20분으로 설정합니다.

다음 Razor 태그는 날짜/시간을 캐시합니다.

```cshtml
<cache>@DateTime.Now</cache>
```

`CacheTagHelper`를 포함하는 페이지에 대한 첫 번째 요청은 현재 날짜/시간을 표시합니다. 추가 요청은 캐시가 만료될 때까지(기본값 20분) 또는 메모리 압력에 의해 제거될 때까지 캐시된 값을 표시합니다.

다음 특성을 사용하여 캐시 기간을 설정할 수 있습니다.

## <a name="cache-tag-helper-attributes"></a>캐시 태그 도우미 특성

- - -

### <a name="enabled"></a>사용    


| 특성 유형    | 유효한 값      |
|----------------   |----------------   |
| boolean           | "true"(기본값)  |
|                   | "false"   |


캐시 태그 도우미로 묶인 콘텐츠가 캐시되었는지 확인합니다. 기본값은 `true`입니다.  `false`로 설정되면 이 캐시 태그 도우미는 렌더링된 출력에 캐싱 효과를 적용하지 않습니다.

예제:

```cshtml
<cache enabled="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-on"></a>expires-on 

| 특성 유형 |           예제 값            |
|----------------|------------------------------------|
| DateTimeOffset | "@new DateTime(2025,1,29,17,02,0)" |

절대 만료 날짜를 설정합니다. 다음 예제는 2025년 1월 29일 오후 5시 2분이 될 때까지 캐시 태그 도우미의 콘텐츠를 캐시합니다.

예제:

```cshtml
<cache expires-on="@new DateTime(2025,1,29,17,02,0)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-after"></a>expires-after

| 특성 유형 |        예제 값         |
|----------------|------------------------------|
|    TimeSpan    | "@TimeSpan.FromSeconds(120)" |

콘텐츠를 캐시할 첫 번째 요청 시간부터 시간 길이를 설정합니다. 

예제:

```cshtml
<cache expires-after="@TimeSpan.FromSeconds(120)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-sliding"></a>expires-sliding

| 특성 유형 |        예제 값        |
|----------------|-----------------------------|
|    TimeSpan    | "@TimeSpan.FromSeconds(60)" |

캐시 항목이 액세스되지 않는 경우 캐시 항목을 제거해야 하는 시간을 설정합니다.

예제:

```cshtml
<cache expires-sliding="@TimeSpan.FromSeconds(60)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-header"></a>vary-by-header

| 특성 유형    | 예제 값                |
|----------------   |----------------               |
| 문자열            | "User-Agent"                  |
|                   | "User-Agent,content-encoding" |

값이 변경되면 캐시 새로 고침을 트리거하는 단일 헤더 값 또는 쉼표로 구분된 헤더 값 목록을 허용합니다. 다음 예제에서는 헤더 값 `User-Agent`를 모니터링합니다. 이 예제는 웹 서버에 제공된 모든 `User-Agent`에 대한 콘텐츠를 캐시합니다.

예제:

```cshtml
<cache vary-by-header="User-Agent">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-query"></a>vary-by-query

| 특성 유형    | 예제 값                |
|----------------   |----------------               |
| 문자열            | "Make"                |
|                   | "Make,Model" |

헤더 값이 변경되면 캐시 새로 고침을 트리거하는 단일 헤더 값 또는 쉼표로 구분된 헤더 값 목록을 허용합니다. 다음 예제에서는 `Make` 및 `Model` 값을 살펴봅니다.

예제:

```cshtml
<cache vary-by-query="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-route"></a>vary-by-route

| 특성 유형    | 예제 값                |
|----------------   |----------------               |
| 문자열            | "Make"                |
|                   | "Make,Model" |

경로 데이터 매개 변수 값이 변경되면 캐시 새로 고침을 트리거하는 단일 헤더 값 또는 쉼표로 구분된 헤더 값 목록을 허용합니다. 예제:

*Startup.cs* 

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{Make?}/{Model?}");
```

*Index.cshtml*

```cshtml
<cache vary-by-route="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-cookie"></a>vary-by-cookie

| 특성 유형    | 예제 값                |
|----------------   |----------------               |
| 문자열            | ".AspNetCore.Identity.Application"                |
|                   | ".AspNetCore.Identity.Application,HairColor" |

헤더 값이 변경되면 캐시 새로 고침을 트리거하는 단일 헤더 값 또는 쉼표로 구분된 헤더 값 목록을 허용합니다. 다음 예제에서는 ASP.NET ID와 연결된 쿠키를 살펴봅니다. 사용자가 인증되면 캐시 새로 고침을 트리거하는 요청 쿠키를 설정해야 합니다.

예제:

```cshtml
<cache vary-by-cookie=".AspNetCore.Identity.Application">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-user"></a>vary-by-user

| 특성 유형    | 예제 값                |
|----------------   |----------------               |
| 부울             | "true"                  |
|                     | "false"(기본값) |

로그인한 사용자(또는 컨텍스트 보안 주체)가 변경되면 캐시를 다시 설정해야 하는지 여부를 지정합니다. 현재 사용자를 요청 컨텍스트 보안 주체라고도 하며 `@User.Identity.Name`을 참조하여 Razor 보기에서 볼 수 있습니다.

다음 예제는 현재 로그인한 사용자를 살펴봅니다.  

예제:

```cshtml
<cache vary-by-user="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

이 특성을 사용하면 로그인 및 로그아웃 주기 동안 콘텐츠가 캐시에 유지됩니다.  `vary-by-user="true"`를 사용하면 로그인 및 로그아웃 작업에서 인증된 사용자에 대한 캐시를 무효화합니다.  로그인 시 새로운 고유 쿠키 값이 생성되므로 캐시가 무효화됩니다. 쿠키가 없거나 만료된 경우 캐시는 익명 상태로 유지됩니다. 즉, 로그인한 사용자가 없으면 캐시가 유지됩니다.

- - -

### <a name="vary-by"></a>vary-by

| 특성 유형 | 예제 값 |
|----------------|----------------|
|     문자열     |    "@Model"    |

캐시되는 데이터의 사용자 지정을 허용합니다. 특성의 문자열 값에서 참조하는 개체가 변경되면 캐시 태그 도우미의 콘텐츠가 업데이트됩니다. 종종 모델 값의 문자열 연결이 이 특성에 할당됩니다.  연결된 값을 업데이트하면 캐시가 무효화된다는 의미입니다.

다음 예제에서는 보기를 렌더링하는 컨트롤러 메서드가 두 경로 매개 변수 `myParam1` 및 `myParam2`의 정수 값을 합산하고, 그 값을 단일 모델 속성으로 반환합니다. 이 합계가 변경되면 캐시 태그 도우미의 콘텐츠가 다시 렌더링 및 캐시됩니다.  

예제:

작업:

```csharp
public IActionResult Index(string myParam1,string myParam2,string myParam3)
{
    int num1;
    int num2;
    int.TryParse(myParam1, out num1);
    int.TryParse(myParam2, out num2);
    return View(viewName, num1 + num2);
}
```

*Index.cshtml*

```cshtml
<cache vary-by="@Model"">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="priority"></a>priority

| 특성 유형    | 예제 값                |
|----------------   |----------------               |
| CacheItemPriority  | "High"                   |
|                    | "Low" |
|                    | "NeverRemove" |
|                    | "Normal" |

기본 제공 캐시 공급자에게 캐시 제거 지침을 제공합니다. 웹 서버는 메모리가 부족할 경우 가장 먼저 `Low` 캐시 항목을 제거합니다.

예제:

```cshtml
<cache priority="High">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

`priority` 특성은 특정 수준의 캐시 보존을 보장하지 않습니다. `CacheItemPriority`는 권장 사항일 뿐입니다. 이 특성을 `NeverRemove`로 설정해도 캐시가 항상 보존된다는 보장은 없습니다. 자세한 내용은 [추가 리소스](#additional-resources)를 참조하세요.

캐시 태그 도우미는 [메모리 캐시 서비스](xref:performance/caching/memory)에 따라 달라집니다. 서비스가 추가되지 않은 경우 캐시 태그 도우미가 서비스를 추가합니다.

## <a name="additional-resources"></a>추가 자료

* [메모리 내 캐시](xref:performance/caching/memory)
* [ID 소개](xref:security/authentication/identity)
