---
title: ASP.NET Core 소개
author: rick-anderson
description: 클라우드 기반 인터넷에 연결된 최신 응용 프로그램을 빌드하기 위한 플랫폼 간 고성능 오픈 소스 프레임워크인 ASP.NET Core에 대한 소개를 가져옵니다.
ms.author: riande
ms.custom: mvc
ms.date: 11/16/2018
uid: index
ms.openlocfilehash: 1b075518b3a744fe8d8506c9a17a3e9c67394be1
ms.sourcegitcommit: 8a65f6c2cbe290fb2418eed58f60fb74c95392c8
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/05/2018
ms.locfileid: "52892109"
---
# <a name="introduction-to-aspnet-core"></a>ASP.NET Core 소개

작성자: [Daniel Roth](https://github.com/danroth27), [Rick Anderson](https://twitter.com/RickAndMSFT) 및 [Shaun Luttin](https://twitter.com/dicshaunary)

ASP.NET Core는 클라우드 기반 인터넷에 연결된 최신 응용 프로그램을 빌드하기 위한 플랫폼 간 고성능 [오픈 소스](https://github.com/aspnet/home) 프레임워크입니다. ASP.NET Core를 사용하면 다음과 같은 작업을 수행할 수 있습니다.

* 웹앱 및 서비스, [IoT](https://www.microsoft.com/internet-of-things/) 앱 및 모바일 백 엔드를 빌드합니다.
* Windows, macOS 및 Linux에서 즐겨 찾는 개발 도구를 사용합니다.
* 클라우드 또는 온-프레미스에 배포합니다.
* [.NET Core 또는.NET Framework](/dotnet/articles/standard/choosing-core-framework-server)를 실행합니다.

## <a name="why-use-aspnet-core"></a>ASP.NET Core를 사용하는 이유는 무엇인가요?

수백만 명의 개발자가 [ASP.NET 4.x](/aspnet/overview)를 사용하여 웹앱을 만들었습니다(계속 사용 중). ASP.NET Core는 간결한 모듈식 프레임워크를 만드는 아키텍처 변경 내용을 포함한 ASP.NET 4.x의 새로운 디자인입니다.

[!INCLUDE[](~/includes/benefits.md)]

## <a name="build-web-apis-and-web-ui-using-aspnet-core-mvc"></a>ASP.NET Core MVC를 사용하여 웹 API 및 웹 UI 빌드

ASP.NET Core MVC에서는 [Web API](xref:tutorials/first-web-api) 및 [웹앱](xref:tutorials/razor-pages/index)을 빌드하는 기능을 제공합니다.

* [MVC(모델-뷰-컨트롤러) 패턴](xref:mvc/overview)을 통해 웹 API 및 웹앱을 테스트 가능하게 합니다.
* [Razor Pages](xref:razor-pages/index)는 웹 UI를 쉽게 빌드하고 생산성을 높일 수 있는 페이지 기반 프로그래밍 모델입니다.
* [Razor 태그](xref:mvc/views/razor)는 [Razor 페이지](xref:razor-pages/index) 및 [MVC 뷰](xref:mvc/views/overview)에 생산적인 구문을 제공합니다.
* [태그 도우미](xref:mvc/views/tag-helpers/intro)를 사용하면 서버 쪽 코드를 Razor 파일에서 HTML 요소를 만들고 렌더링하는 데 사용할 수 있습니다.
* [여러 데이터 형식 및 콘텐츠 협상](xref:web-api/advanced/formatting)에 대한 기본 제공 지원을 통해 웹 API를 브라우저 및 모바일 장치를 포함한 다양한 클라이언트에 연결할 수 있습니다.
* [모델 바인딩](xref:mvc/models/model-binding)은 작업 메서드 매개 변수에 HTTP 요청의 데이터를 자동으로 매핑합니다.
* [모델 유효성 검사](xref:mvc/models/validation)는 자동으로 클라이언트 쪽 및 서버 쪽 유효성 검사를 수행합니다.

## <a name="client-side-development"></a>클라이언트 쪽 개발

ASP.NET Core는 [Angular](xref:spa/angular), [React](xref:spa/react), [부트스트랩](https://getbootstrap.com/) 등 유명한 클라이언트 쪽 프레임워크 및 라이브러리와 원활하게 통합합니다. 자세한 내용은 [클라이언트 쪽 개발](xref:client-side/index)을 참조하세요.

<a name="target-framework"></a>

## <a name="aspnet-core-targeting-net-framework"></a>ASP.NET Core 대상 .NET Framework

ASP.NET Core 2.x는 .NET Core 또는 .NET Framework를 대상으로 지정할 수 있습니다. .NET Framework를 대상으로 지정한 ASP.NET Core 앱은 플랫폼 간 교차 사용이 불가능하며 &mdash;Windows에서만 실행됩니다. 일반적으로 ASP.NET Core 2.x는 [.NET 표준](/dotnet/standard/net-standard) 라이브러리로 구성됩니다. .NET Standard 2.0으로 작성된 앱은 .NET Standard 2.0이 지원되는 모든 위치에서 실행됩니다.

ASP.NET Core 2.x는 .NET Standard 2.0과 호환되는 .NET Framework 버전에서 지원됩니다.

* .NET Framework 4.7.1 이상이 권장됩니다.
* .NET Framework 4.6.1 이상

ASP.NET Core 3.0 이상은 .NET Core에서만 실행됩니다. 이 변경 사항에 대한 자세한 내용은 [ASP.NET Core 3.0에 도입되는 변경 사항 개요](https://blogs.msdn.microsoft.com/webdev/2018/10/29/a-first-look-at-changes-coming-in-asp-net-core-3-0/)를 참조하세요.

.NET Core를 대상으로 지정하면 여러 이점이 있으며 이러한 장점은 릴리스마다 늘어나고 있습니다. .NET Framework에서 .NET Core의 몇 가지 장점은 다음과 같습니다.

* 플랫폼 간 사용 가능. macOS, Linux 및 Windows에서 실행됩니다.
* 향상된 성능
* Side-by-side 버전 관리.
* 새로운 API
* 소스 열기

.NET Framework에서 .NET Core 사이의 API 차이를 줄이기 위해 최선을 다하고 있습니다. [Windows 호환 팩](/dotnet/core/porting/windows-compat-pack)을 통해 수천 개의 Windows 전용 API를 .NET Core에서 사용할 수 있습니다. 이러한 API는 .NET Core 1.x에서 사용할 수 없습니다.

## <a name="how-to-download-a-sample"></a>샘플 다운로드 방법

대부분의 문서 및 자습서에는 샘플 코드에 대한 링크가 포함됩니다.

1. [ASP.NET 리포지토리 zip 파일을 다운로드합니다](https://codeload.github.com/aspnet/Docs/zip/master).
1. *Docs-master.zip* 파일의 압축을 풉니다.
1. 샘플 링크의 URL을 사용하여 샘플 디렉터리로 이동할 수 있습니다.

### <a name="preprocessor-directives-in-sample-code"></a>샘플 코드의 전처리기 지시문

여러 시나리오를 보여주기 위해 샘플 앱은 `#define` 및 `#if-#else/#elif-#endif` C# 문을 사용하여 샘플 코드의 다양한 섹션을 선택적으로 컴파일하고 실행합니다. 이 방법을 사용하는 해당 샘플의 경우 C# 파일 상단에 있는 `#define` 문을 실행할 시나리오와 연결된 기호로 설정합니다. 시나리오를 실행하기 위해 일부 샘플에서는 여러 파일의 맨 위에 기호를 설정해야 합니다.

예를 들어, 다음 `#define` 기호 목록은 네 가지 시나리오를 사용할 수 있음을 나타냅니다(기호당 하나의 시나리오). 현재 샘플 구성에서는 `TemplateCode` 시나리오를 실행합니다.

```csharp
#define TemplateCode // or LogFromMain or ExpandDefault or FilterInCode
```

`ExpandDefault` 시나리오를 실행하도록 샘플을 변경하려면 `ExpandDefault` 기호를 정의하고 나머지 기호는 주석으로 처리하세요.

```csharp
#define ExpandDefault // TemplateCode or LogFromMain or FilterInCode
```

[C# 전 처리기 지시문](/dotnet/csharp/language-reference/preprocessor-directives/)을 사용하여 코드 섹션을 선택적으로 컴파일하는 방법에 대한 자세한 내용은 [#define(C# 참조)](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-define) 및 [#if(C# 참조)](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-if)를 참조하세요.

### <a name="regions-in-sample-code"></a>샘플 코드의 지역

일부 샘플 앱에는 [#region](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-region) 및 [#end-region](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-endregion) C# 문으로 둘러싼 코드의 섹션이 포함됩니다. 설명서 빌드 시스템은 렌더링된 설명서 토픽에 이러한 지역을 삽입합니다.  

지역 이름에는 일반적으로 "snippet"이라는 단어가 포함됩니다. 다음 예제에서는 `snippet_FilterInCode`라는 지역을 보여줍니다.

```csharp
#region snippet_FilterInCode
WebHost.CreateDefaultBuilder(args)
    .UseStartup<Startup>()
    .ConfigureLogging(logging =>
        logging.AddFilter("System", LogLevel.Debug)
            .AddFilter<DebugLoggerProvider>("Microsoft", LogLevel.Trace))
            .Build();
#endregion
```

이전 C# 코드 조각은 다음 줄을 포함한 항목의 markdown 파일에서 참조됩니다.

```
[!code-csharp[](sample/SampleApp/Program.cs?name=snippet_FilterInCode)]
```

코드를 둘러싸고 있는 `#region` 및 `#end-region` 문을 안전하게 무시(또는 제거)할 수 있습니다. 항목에 설명된 샘플 시나리오를 실행하려는 경우 이러한 명령문 내에서 코드를 변경하지 마십시오. 다른 시나리오를 실험하는 경우 자유롭게 코드를 변경할 수 있습니다.

자세한 내용은 [ASP.NET 설명서에 참여: 코드 조각](https://github.com/aspnet/Docs/blob/master/CONTRIBUTING.md#code-snippets)을 참조하세요.

## <a name="next-steps"></a>다음 단계

자세한 내용은 다음 리소스를 참조하세요.

* [Razor 페이지 시작](xref:tutorials/razor-pages/razor-pages-start)
* <xref:tutorials/publish-to-azure-webapp-using-vs>
* [ASP.NET Core 기본 사항](xref:fundamentals/index)
* [매주 ASP.NET 커뮤니티 스탠드업](https://live.asp.net/)은 팀의 진행률 및 계획을 다루고 새 블로그 및 타사 소프트웨어를 설명합니다.

> [!NOTE]
> ASP.NET Core 목차에 대해 제안된 새 구조의 유용성을 테스트합니다.  몇 분 동안 현재 또는 제안된 목차에서 다른 7개의 항목을 찾는 연습을 수행하는 경우 [여기를 클릭하여 연구에 참여하세요](https://dpk4xbh5.optimalworkshop.com/treejack/rps16hd5).
