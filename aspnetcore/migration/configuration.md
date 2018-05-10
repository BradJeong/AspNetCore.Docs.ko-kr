---
title: ASP.NET Core로 구성 마이그레이션
author: ardalis
description: ASP.NET MVC 프로젝트에서 ASP.NET Core MVC 프로젝트 구성을 마이그레이션하는 방법에 알아봅니다.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/configuration
ms.openlocfilehash: ead4f96aa0041cd919caa972d3bb05bd94a857b3
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/08/2018
---
# <a name="migrate-configuration-to-aspnet-core"></a>ASP.NET Core로 구성 마이그레이션

작성자: [Steve Smith](https://ardalis.com/) 및 [Scott Addie](https://scottaddie.com)

이전 문서에서는 하기 시작 [ASP.NET Core mvc는 ASP.NET MVC 프로젝트 마이그레이션](xref:migration/mvc)합니다. 이 문서에서는 구성을 마이그레이션 했습니다.

[예제 코드 살펴보기 및 다운로드](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/configuration/samples)([다운로드 방법](xref:tutorials/index#how-to-download-a-sample))

## <a name="setup-configuration"></a>설치 구성

ASP.NET Core 더 이상 사용 하지는 *Global.asax* 및 *web.config* 이전 버전의 ASP.NET 사용 하는 파일입니다. 이전 버전의 ASP.NET 응용 프로그램 시작 논리에 배치 된는 `Application_StartUp` 메서드 내에서 *Global.asax*합니다. ASP.NET MVC의 뒷부분에 나오는 *Startup.cs* 고 파일; 프로젝트의 루트에 포함 된 응용 프로그램이 시작 될 때 호출 되었습니다. ASP.NET Core가이 접근 방법을 채택 완전히 모든 시작 논리에 배치 하 여는 *Startup.cs* 파일입니다.

*web.config* 파일 에서도 ASP.NET Core 대체 되었습니다. 자체 구성 이제을 구성할 수 있으며 응용 프로그램 시작에서 설명한 절차의 일부로 *Startup.cs*합니다. 구성 XML 파일을 사용할 수 있지만 일반적으로 ASP.NET Core 프로젝트에에서 배치 됩니다 구성 값을 JSON 형식 파일을 같은 *appsettings.json*합니다. ASP.NET Core 구성 시스템 환경 변수를 제공할 수 있는도 쉽게 액세스할 수는 [더 안전 하 고 강력한 위치](xref:security/app-secrets) 환경 관련 값에 대 한 합니다. 연결 문자열 및 소스 제어에 체크 인할 수 하지 않아야 하는 API 키와 같은 기밀 정보에 대 한 경우 특히 유용 합니다. 참조 [구성](xref:fundamentals/configuration/index) ASP.NET Core에서 구성에 대 한 자세한 내용을 보려면 합니다.

부분적으로 마이그레이션된 ASP.NET Core 프로젝트와 시작 되며이 문서에 대 한 [이전 문서](xref:migration/mvc)합니다. 구성 설정, 다음 생성자 및 속성을 추가 하는 *Startup.cs* 프로젝트의 루트에 있는 파일:

[!code-csharp[](configuration/samples/WebApp1/src/WebApp1/Startup.cs?range=11-16)]

이 시점에서 즉 참고는 *Startup.cs* 파일에 다음 추가 해야 하는 대로 컴파일되지 않습니다 `using` 문:

```csharp
using Microsoft.Extensions.Configuration;
```

추가 *appsettings.json* 적절 한 항목 템플릿을 사용 하 여 프로젝트의 루트에 파일:

![AppSettings JSON 추가](configuration/_static/add-appsettings-json.png)

## <a name="migrate-configuration-settings-from-webconfig"></a>Web.config에서 구성 설정을 마이그레이션하십시오

ASP.NET MVC 프로젝트 진행에 필요한 데이터베이스 연결 문자열을 포함 *web.config*에 `<connectionStrings>` 요소입니다. ASP.NET Core 프로젝트 진행에 하겠습니다에이 정보를 저장 하는 *appsettings.json* 파일입니다. 열기 *appsettings.json*, 다음을 이미 포함 하는 것을 확인 합니다.

[!code-json[](../migration/configuration/samples/WebApp1/src/WebApp1/appsettings.json?highlight=4)]

위에 표시 하는 강조 표시 된 줄에서 데이터베이스의 이름을 변경 **_CHANGE_ME** 데이터베이스의 이름입니다.

## <a name="summary"></a>요약

ASP.NET Core 응용 프로그램에 대 한 모든 시작 논리는 필요한 서비스 및 종속성 수 정의 되어 있으며 구성 된 단일 파일을 배치 합니다. 대체는 *web.config* 다양 한 환경 변수 뿐만 아니라 JSON와 같은 파일 형식의 활용할 수 있는 유연한 구성 기능으로는 파일입니다.
