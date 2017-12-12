---
title: "요구 사항 처리기의 종속성 주입"
author: rick-anderson
description: "이 문서에서는 종속성 주입을 사용 하 여 ASP.NET Core 응용 프로그램에 권한 부여 요구 사항을 처리기를 삽입 하는 방법에 설명 합니다."
keywords: "ASP.NET 필수 요소 종속성 주입, 권한 부여 처리기"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 5fb6625c-173a-4feb-8380-73c9844dc23c
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/dependencyinjection
ms.openlocfilehash: b5e590cc63387553af7385b611cdf8cd6b255db7
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/10/2017
---
# <a name="dependency-injection-in-requirement-handlers"></a>요구 사항 처리기의 종속성 주입

<a name="security-authorization-di"></a>

[인증 처리기를 등록 해야](policies.md#handler-registration) 구성 하는 동안 서비스 컬렉션에 있는 (사용 하 여 [종속성 주입](../../fundamentals/dependency-injection.md#fundamentals-dependency-injection)).

권한 부여 처리기 내 계산 하려는 규칙의 리포지토리 있다고 가정 하 고 해당 저장소 서비스 컬렉션에 등록 되었습니다. 합니다. 권한 부여 해결 하 고 생성자에는 삽입 됩니다.

예를 들어 ASP를 사용 하려는 경우. NET 삽입 하려는 경우 인프라의 로깅 `ILoggerFactory` 프로그램 처리기에 있습니다. 업데이트 처리기가 같을 수 있습니다.

```csharp
public class LoggingAuthorizationHandler : AuthorizationHandler<MyRequirement>
   {
       ILogger _logger;

       public LoggingAuthorizationHandler(ILoggerFactory loggerFactory)
       {
           _logger = loggerFactory.CreateLogger(this.GetType().FullName);
       }

       protected override Task HandleRequirementAsync(AuthorizationHandlerContext context, MyRequirement requirement)
       {
           _logger.LogInformation("Inside my handler");
           // Check if the requirement is fulfilled.
           return Task.CompletedTask;
       }
   }
   ```

처리기를 등록 합니다 `services.AddSingleton()`:

```csharp
services.AddSingleton<IAuthorizationHandler, LoggingAuthorizationHandler>();
```

응용 프로그램이 시작 될 때 만들 수 하는 처리기의 인스턴스, 등록 된 삽입 DI가 `ILoggerFactory` 생성자에 있습니다.

> [!NOTE]
> 단일 항목으로 Entity Framework를 사용 하는 처리기를 등록 하지 않아야 합니다.
