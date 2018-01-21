---
title: "요구 사항 처리기의 종속성 주입"
author: rick-anderson
description: "이 문서에서는 종속성 주입을 사용 하 여 ASP.NET Core 응용 프로그램에 권한 부여 요구 사항을 처리기를 삽입 하는 방법에 설명 합니다."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/dependencyinjection
ms.openlocfilehash: 9aaa356fe67a7e2c8177ffa1b886ec6b3dc13ef0
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/19/2018
---
# <a name="dependency-injection-in-requirement-handlers"></a><span data-ttu-id="93d54-103">요구 사항 처리기의 종속성 주입</span><span class="sxs-lookup"><span data-stu-id="93d54-103">Dependency Injection in requirement handlers</span></span>

<a name="security-authorization-di"></a>

<span data-ttu-id="93d54-104">[인증 처리기를 등록 해야](policies.md#handler-registration) 구성 하는 동안 서비스 컬렉션에 있는 (사용 하 여 [종속성 주입](../../fundamentals/dependency-injection.md#fundamentals-dependency-injection)).</span><span class="sxs-lookup"><span data-stu-id="93d54-104">[Authorization handlers must be registered](policies.md#handler-registration) in the service collection during configuration (using [dependency injection](../../fundamentals/dependency-injection.md#fundamentals-dependency-injection)).</span></span>

<span data-ttu-id="93d54-105">권한 부여 처리기 내 계산 하려는 규칙의 리포지토리 있다고 가정 하 고 해당 저장소 서비스 컬렉션에 등록 되었습니다. 합니다.</span><span class="sxs-lookup"><span data-stu-id="93d54-105">Suppose you had a repository of rules you wanted to evaluate inside an authorization handler and that repository was registered in the service collection.</span></span> <span data-ttu-id="93d54-106">권한 부여 해결 하 고 생성자에는 삽입 됩니다.</span><span class="sxs-lookup"><span data-stu-id="93d54-106">Authorization will resolve and inject that into your constructor.</span></span>

<span data-ttu-id="93d54-107">예를 들어 ASP를 사용 하려는 경우. NET 삽입 하려는 경우 인프라의 로깅 `ILoggerFactory` 프로그램 처리기에 있습니다.</span><span class="sxs-lookup"><span data-stu-id="93d54-107">For example, if you wanted to use ASP.NET's logging infrastructure you would want to inject `ILoggerFactory` into your handler.</span></span> <span data-ttu-id="93d54-108">업데이트 처리기가 같을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="93d54-108">Such a handler might look like:</span></span>

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

<span data-ttu-id="93d54-109">처리기를 등록 합니다 `services.AddSingleton()`:</span><span class="sxs-lookup"><span data-stu-id="93d54-109">You would register the handler with `services.AddSingleton()`:</span></span>

```csharp
services.AddSingleton<IAuthorizationHandler, LoggingAuthorizationHandler>();
```

<span data-ttu-id="93d54-110">응용 프로그램이 시작 될 때 만들 수 하는 처리기의 인스턴스, 등록 된 삽입 DI가 `ILoggerFactory` 생성자에 있습니다.</span><span class="sxs-lookup"><span data-stu-id="93d54-110">An instance of the handler will be created when your application starts, and DI will inject the registered `ILoggerFactory` into your constructor.</span></span>

> [!NOTE]
> <span data-ttu-id="93d54-111">단일 항목으로 Entity Framework를 사용 하는 처리기를 등록 하지 않아야 합니다.</span><span class="sxs-lookup"><span data-stu-id="93d54-111">Handlers that use Entity Framework shouldn't be registered as singletons.</span></span>
