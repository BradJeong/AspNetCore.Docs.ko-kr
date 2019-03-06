---
title: ASP.NET Core에서 razor 페이지 권한 부여 규칙
author: guardrex
description: 사용자 권한 부여 및 익명 사용자가 페이지 또는 페이지의 폴더에 액세스 하도록 허용 하는 규칙을 사용 하 여 페이지에 대 한 액세스를 제어 하는 방법에 알아봅니다.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/03/2019
uid: security/authorization/razor-pages-authorization
ms.openlocfilehash: 040d33eba7eaf7a3aece2eedcdef7343e52972af
ms.sourcegitcommit: 036d4b03fd86ca5bb378198e29ecf2704257f7b2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/05/2019
ms.locfileid: "57345505"
---
# <a name="razor-pages-authorization-conventions-in-aspnet-core"></a>ASP.NET Core에서 razor 페이지 권한 부여 규칙

[Luke Latham](https://github.com/guardrex)으로

Razor 페이지 앱에 대 한 액세스를 제어 하는 한 가지 방법은 시작 시 권한 부여 규칙을 사용 하는 것입니다. 이러한 규칙을 사용 하면 사용자 권한을 부여 하 고 익명 사용자가 개별 페이지 또는 페이지의 폴더에 액세스할 수 있습니다. 자동으로이 항목에서 설명 하는 규칙이 적용 [권한 부여 필터](xref:mvc/controllers/filters#authorization-filters) 액세스 제어 합니다.

[예제 코드 살펴보기 및 다운로드](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([다운로드 방법](xref:index#how-to-download-a-sample))

샘플 앱에서는 [ASP.NET Core Id 없이 쿠키 인증](xref:security/authentication/cookie)합니다. 개념 및이 항목에 표시 하는 예제 ASP.NET Core Id를 사용 하는 앱에 동일 하 게 적용 됩니다. ASP.NET Core Id를 사용 하려면의 지침에 따라 <xref:security/authentication/identity>합니다.

## <a name="require-authorization-to-access-a-page"></a>페이지에 액세스 권한 부여 필요

사용 합니다 <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*> 규칙을 통해 <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> 추가할는 <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> 지정된 된 경로에서 페이지:

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,4)]

지정된 된 경로 경로인 뷰 엔진에만 슬래시를 포함 하 고 확장 없이 Razor 페이지 루트 상대 경로입니다.

지정 하는 [권한 부여 정책](xref:security/authorization/policies)를 사용 하 여는 [AuthorizePage 오버 로드](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*):

```csharp
options.Conventions.AuthorizePage("/Contact", "AtLeast21");
```

> [!NOTE]
> <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> 사용 하 여 페이지 모델 클래스에 적용할 수는 `[Authorize]` 필터 특성입니다. 자세한 내용은 [권한 부여 필터 특성](xref:razor-pages/filter#authorize-filter-attribute)합니다.

## <a name="require-authorization-to-access-a-folder-of-pages"></a>페이지 폴더 액세스 권한 부여 필요

사용 된 <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*> 규칙을 통해 <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> 추가할는 <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> 지정된 된 경로에 폴더에 있는 페이지의 모든:

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,5)]

지정된 된 경로 경로인 뷰 엔진, Razor 페이지 루트 상대 경로입니다.

지정 하는 [권한 부여 정책](xref:security/authorization/policies)를 사용 하 여는 [AuthorizeFolder 오버 로드](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*):

```csharp
options.Conventions.AuthorizeFolder("/Private", "AtLeast21");
```

## <a name="require-authorization-to-access-an-area-page"></a>영역 페이지에 액세스할 수 있는 인증 필요

사용 된 <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*> 규칙을 통해 <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> 추가할는 <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> 지정 된 경로의 영역 페이지에:

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts");
```

페이지 이름은 지정된 된 영역에 대 한 페이지 루트 디렉터리에 상대적인 확장명 없이 파일의 경로입니다. 예를 들어 파일의 페이지 이름을 *Areas/Identity/Pages/Manage/Accounts.cshtml* 됩니다 */관리/계정*합니다.

지정 하는 [권한 부여 정책](xref:security/authorization/policies)를 사용 하 여는 [AuthorizeAreaPage 오버 로드](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*):

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts", "AtLeast21");
```

## <a name="require-authorization-to-access-a-folder-of-areas"></a>영역의 폴더 액세스 권한 부여 필요

사용 합니다 <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*> 규칙을 통해 <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> 추가할는 <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> 모든 지정된 된 경로에 폴더에서:

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage");
```

폴더 경로가 지정된 된 영역에 대 한 페이지 루트 디렉터리에 상대적인 폴더의 경로입니다. 예를 들어, 아래에 있는 파일에 대 한 폴더 경로 *영역/Identity/페이지/관리/* 됩니다 */관리*합니다.

지정 하는 [권한 부여 정책](xref:security/authorization/policies)를 사용 하 여는 [AuthorizeAreaFolder 오버 로드](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*):

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage", "AtLeast21");
```

## <a name="allow-anonymous-access-to-a-page"></a>페이지에 대 한 익명 액세스를 허용 합니다.

사용 합니다 <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToPage*> 규칙을 통해 <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> 추가할는 <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> 지정 된 경로의 페이지:

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,6)]

지정된 된 경로 경로인 뷰 엔진에만 슬래시를 포함 하 고 확장 없이 Razor 페이지 루트 상대 경로입니다.

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a>페이지의 폴더에 대 한 익명 액세스를 허용 합니다.

사용 된 <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToFolder*> 규칙을 통해 <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> 추가할는 <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> 지정된 된 경로에 폴더에 있는 페이지의 모든:

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,7)]

지정된 된 경로 경로인 뷰 엔진, Razor 페이지 루트 상대 경로입니다.

## <a name="note-on-combining-authorized-and-anonymous-access"></a>익명 액세스 및 권한이 결합에 대 한 참고

지정 하는 유효한 권한 부여를 요구 하는 페이지의 폴더 및 해당 폴더 내의 페이지 익명 액세스를 허용 하는지 지정 하는 보다:

```csharp
// This works.
.AuthorizeFolder("/Private").AllowAnonymousToPage("/Private/Public")
```

그러나 반대의 경우 유효 하지 않습니다. 익명 액세스에 대 한 페이지의 폴더를 선언 하 고 권한 부여는 해당 폴더 내의 페이지를 지정할 수 없습니다.

```csharp
// This doesn't work!
.AllowAnonymousToFolder("/Public").AuthorizePage("/Public/Private")
```

개인 페이지에 필요한 권한 부여에 실패 합니다. 경우 모두를 <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> 및 <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> 페이지에 적용 되는 <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> 우선적으로 적용 하 고 액세스를 제어 합니다.

## <a name="additional-resources"></a>추가 자료

* <xref:razor-pages/razor-pages-conventions>
* <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>
