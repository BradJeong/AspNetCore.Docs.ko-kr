---
title: "컨트롤러에 대 한 종속성 주입"
author: ardalis
description: 
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/controllers/dependency-injection
ms.openlocfilehash: 946d695c572379c3ebc2eda1569f186f25ab9bfc
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/24/2018
---
# <a name="dependency-injection-into-controllers"></a><span data-ttu-id="3d618-102">컨트롤러에 대 한 종속성 주입</span><span class="sxs-lookup"><span data-stu-id="3d618-102">Dependency injection into controllers</span></span>

<a name="dependency-injection-controllers"></a>

<span data-ttu-id="3d618-103">으로 [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="3d618-103">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="3d618-104">ASP.NET Core MVC 컨트롤러의 생성자를 통해 명시적으로 해당 종속성을 요청 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="3d618-104">ASP.NET Core MVC controllers should request their dependencies explicitly via their constructors.</span></span> <span data-ttu-id="3d618-105">경우에 따라 개별 컨트롤러 작업에는 서비스 필요할 수 있습니다 및 컨트롤러 수준에서 요청할 수 있는 적합할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3d618-105">In some instances, individual controller actions may require a service, and it may not make sense to request at the controller level.</span></span> <span data-ttu-id="3d618-106">이 경우에 작업 메서드의 매개 변수로 서비스를 삽입할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3d618-106">In this case, you can also choose to inject a service as a parameter on the action method.</span></span>

<span data-ttu-id="3d618-107">[샘플 코드 보기 또는 다운로드](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/dependency-injection/sample)([다운로드 방법](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="3d618-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/dependency-injection/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="dependency-injection"></a><span data-ttu-id="3d618-108">종속성 주입</span><span class="sxs-lookup"><span data-stu-id="3d618-108">Dependency Injection</span></span>

<span data-ttu-id="3d618-109">종속성 주입은 뒤에 기술에서 [종속성 반전 원칙](http://deviq.com/dependency-inversion-principle/), 응용 프로그램을 느슨하게 결합 된 모듈의 구성 될 수 있도록 합니다.</span><span class="sxs-lookup"><span data-stu-id="3d618-109">Dependency injection is a technique that follows the [Dependency Inversion Principle](http://deviq.com/dependency-inversion-principle/), allowing for applications to be composed of loosely coupled modules.</span></span> <span data-ttu-id="3d618-110">ASP.NET Core에서 기본적으로 지원 [종속성 주입](../../fundamentals/dependency-injection.md), 그러면 응용 프로그램 쉽게 테스트 하 고 유지 관리 합니다.</span><span class="sxs-lookup"><span data-stu-id="3d618-110">ASP.NET Core has built-in support for [dependency injection](../../fundamentals/dependency-injection.md), which makes applications easier to test and maintain.</span></span>

## <a name="constructor-injection"></a><span data-ttu-id="3d618-111">생성자 삽입</span><span class="sxs-lookup"><span data-stu-id="3d618-111">Constructor Injection</span></span>

<span data-ttu-id="3d618-112">ASP.NET Core 기본적으로 지원 되는 생성자 기반 종속성 주입 MVC 컨트롤러까지 확장 됩니다.</span><span class="sxs-lookup"><span data-stu-id="3d618-112">ASP.NET Core's built-in support for constructor-based dependency injection extends to MVC controllers.</span></span> <span data-ttu-id="3d618-113">서비스 종류를 생성자 매개 변수로 컨트롤러에 단순히 추가 ASP.NET Core 서비스 컨테이너에서 기본 제공를 사용 하 여 해당 형식을 확인 하려고 합니다.</span><span class="sxs-lookup"><span data-stu-id="3d618-113">By simply adding a service type to your controller as a constructor parameter, ASP.NET Core will attempt to resolve that type using its built in service container.</span></span> <span data-ttu-id="3d618-114">서비스는 항상 그렇지는 않지만 일반적으로 정의 된 인터페이스를 사용 하 여 합니다.</span><span class="sxs-lookup"><span data-stu-id="3d618-114">Services are typically, but not always, defined using interfaces.</span></span> <span data-ttu-id="3d618-115">예를 들어 현재 시간에 따라 달라 지는 비즈니스 논리가 응용 프로그램에 설정된 된 시간을 사용 하는 구현에 전달 하 여 테스트 수 있는 시간 (아니라 하드 코딩 것)를 검색 하는 서비스를 삽입할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3d618-115">For example, if your application has business logic that depends on the current time, you can inject a service that retrieves the time (rather than hard-coding it), which would allow your tests to pass in implementations that use a set time.</span></span>

[!code-csharp[Main](dependency-injection/sample/src/ControllerDI/Interfaces/IDateTime.cs)]


<span data-ttu-id="3d618-116">런타임 시 시스템 클록을 사용 하는 이와 같은 인터페이스를 구현 하는 매우 간단 합니다.</span><span class="sxs-lookup"><span data-stu-id="3d618-116">Implementing an interface like this one so that it uses the system clock at runtime is trivial:</span></span>

[!code-csharp[Main](dependency-injection/sample/src/ControllerDI/Services/SystemDateTime.cs)]


<span data-ttu-id="3d618-117">이 위치에 컨트롤러에서 서비스를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3d618-117">With this in place, we can use the service in our controller.</span></span> <span data-ttu-id="3d618-118">이 경우 일부 논리를 추가 했습니다는 `HomeController` `Index` 하루 중 시간을 기반으로 하는 메서드를 사용자에 게 인사말을 표시 합니다.</span><span class="sxs-lookup"><span data-stu-id="3d618-118">In this case, we have added some logic to the `HomeController` `Index` method to display a greeting to the user based on the time of day.</span></span>

[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Controllers/HomeController.cs?highlight=8,10,12,17,18,19,20,21,22,23,24,25,26,27,28,29,30&range=1-31,51-52)]

<span data-ttu-id="3d618-119">지금 응용 프로그램을 실행 하는 경우 오류를 발생 가능성이 합니다.</span><span class="sxs-lookup"><span data-stu-id="3d618-119">If we run the application now, we will most likely encounter an error:</span></span>

```
An unhandled exception occurred while processing the request.

InvalidOperationException: Unable to resolve service for type 'ControllerDI.Interfaces.IDateTime' while attempting to activate 'ControllerDI.Controllers.HomeController'.
Microsoft.Extensions.DependencyInjection.ActivatorUtilities.GetService(IServiceProvider sp, Type type, Type requiredBy, Boolean isDefaultParameterRequired)
```

<span data-ttu-id="3d618-120">म에서 서비스를 구성 하지 않은 경우이 오류가 발생는 `ConfigureServices` 메서드에서 우리의 `Startup` 클래스입니다.</span><span class="sxs-lookup"><span data-stu-id="3d618-120">This error occurs when we have not configured a service in the `ConfigureServices` method in our `Startup` class.</span></span> <span data-ttu-id="3d618-121">에 대 한 요청을 지정 하려면 `IDateTime` 의 인스턴스를 사용 하 여 해결 해야 `SystemDateTime`, 하려면 아래 목록에서 강조 표시 된 줄을 추가 하면 `ConfigureServices` 메서드:</span><span class="sxs-lookup"><span data-stu-id="3d618-121">To specify that requests for `IDateTime` should be resolved using an instance of `SystemDateTime`, add the highlighted line in the listing below to your `ConfigureServices` method:</span></span>

[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Startup.cs?highlight=4&range=26-27,42-44)]

> [!NOTE]
> <span data-ttu-id="3d618-122">여러 가지 다른 수명 옵션 중 하나를 사용 하 여이 특정 서비스를 구현할 수 있습니다 (`Transient`, `Scoped`, 또는 `Singleton`).</span><span class="sxs-lookup"><span data-stu-id="3d618-122">This particular service could be implemented using any of several different lifetime options (`Transient`, `Scoped`, or `Singleton`).</span></span> <span data-ttu-id="3d618-123">참조 [종속성 주입](../../fundamentals/dependency-injection.md) 각 범위 옵션 내용이 미치는 영향을 서비스의 동작을 이해할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3d618-123">See [Dependency Injection](../../fundamentals/dependency-injection.md) to understand how each of these scope options will affect the behavior of your service.</span></span>

<span data-ttu-id="3d618-124">서비스를 구성한 후 응용 프로그램을 실행 하 고 홈 페이지로 이동 해야 시간 기반 메시지 예상 대로 표시 합니다.</span><span class="sxs-lookup"><span data-stu-id="3d618-124">Once the service has been configured, running the application and navigating to the home page should display the time-based message as expected:</span></span>

![서버 인사말](dependency-injection/_static/server-greeting.png)

>[!TIP]
> <span data-ttu-id="3d618-126">참조 [테스트 컨트롤러 논리](testing.md) 종속성을 명시적으로 요청 하는 방법에 알아보려면 [http://deviq.com/explicit-dependencies-principle/](http://deviq.com/explicit-dependencies-principle/) 컨트롤러에 더 쉽게 코드를 테스트 합니다.</span><span class="sxs-lookup"><span data-stu-id="3d618-126">See [Testing Controller Logic](testing.md) to learn how to explicitly request dependencies [http://deviq.com/explicit-dependencies-principle/](http://deviq.com/explicit-dependencies-principle/) in controllers makes code easier to test.</span></span>

<span data-ttu-id="3d618-127">ASP.NET Core 기본 종속성 주입 서비스를 요청 하는 클래스에 대 한 단일 생성자만 지원 합니다.</span><span class="sxs-lookup"><span data-stu-id="3d618-127">ASP.NET Core's built-in dependency injection supports having only a single constructor for classes requesting services.</span></span> <span data-ttu-id="3d618-128">둘 이상의 생성자가 있으면 내용의 된 예외가 나타날 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3d618-128">If you have more than one constructor, you may get an exception stating:</span></span>

```
An unhandled exception occurred while processing the request.

InvalidOperationException: Multiple constructors accepting all given argument types have been found in type 'ControllerDI.Controllers.HomeController'. There should only be one applicable constructor.
Microsoft.Extensions.DependencyInjection.ActivatorUtilities.FindApplicableConstructor(Type instanceType, Type[] argumentTypes, ConstructorInfo& matchingConstructor, Nullable`1[]& parameterMap)
```

<span data-ttu-id="3d618-129">오류 메시지에서 설명 하는 대로 단일 생성자만이 문제를 해결할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3d618-129">As the error message states, you can correct this problem having just a single constructor.</span></span> <span data-ttu-id="3d618-130">수도 있습니다 [제 3 자 구현은 기본 종속성 주입 지원 바꿉니다](../../fundamentals/dependency-injection.md#replacing-the-default-services-container)지 원하는 여러 명의 생성자의 여러 합니다.</span><span class="sxs-lookup"><span data-stu-id="3d618-130">You can also [replace the default dependency injection support with a third party implementation](../../fundamentals/dependency-injection.md#replacing-the-default-services-container), many of which support multiple constructors.</span></span>

## <a name="action-injection-with-fromservices"></a><span data-ttu-id="3d618-131">FromServices와 삽입 작업</span><span class="sxs-lookup"><span data-stu-id="3d618-131">Action Injection with FromServices</span></span>

<span data-ttu-id="3d618-132">경우에 따라 컨트롤러 내에서 둘 이상의 작업에 대 한 서비스를 필요 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="3d618-132">Sometimes you don't need a service for more than one action within your controller.</span></span> <span data-ttu-id="3d618-133">이 경우 서비스 작업 메서드에 매개 변수로 삽입을 하면 됩니다.</span><span class="sxs-lookup"><span data-stu-id="3d618-133">In this case, it may make sense to inject the service as a parameter to the action method.</span></span> <span data-ttu-id="3d618-134">특성으로 매개 변수를 표시 하 여 이렇게 `[FromServices]` 다음과 같이 합니다.</span><span class="sxs-lookup"><span data-stu-id="3d618-134">This is done by marking the parameter with the attribute `[FromServices]` as shown here:</span></span>

[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Controllers/HomeController.cs?highlight=1&range=33-38)]

## <a name="accessing-settings-from-a-controller"></a><span data-ttu-id="3d618-135">컨트롤러에서 설정 액세스</span><span class="sxs-lookup"><span data-stu-id="3d618-135">Accessing Settings from a Controller</span></span>

<span data-ttu-id="3d618-136">컨트롤러 내에서 응용 프로그램 또는 구성 설정에 액세스 하는 일반적인 패턴입니다.</span><span class="sxs-lookup"><span data-stu-id="3d618-136">Accessing application or configuration settings from within a controller is a common pattern.</span></span> <span data-ttu-id="3d618-137">이 액세스에 설명 된 옵션 패턴을 사용 해야 [구성](xref:fundamentals/configuration/index)합니다.</span><span class="sxs-lookup"><span data-stu-id="3d618-137">This access should use the Options pattern described in [configuration](xref:fundamentals/configuration/index).</span></span> <span data-ttu-id="3d618-138">일반적으로 종속성 주입을 사용 하 여 컨트롤러에서 직접 설정 요청 하지 않아야 합니다.</span><span class="sxs-lookup"><span data-stu-id="3d618-138">You generally shouldn't request settings directly from your controller using dependency injection.</span></span> <span data-ttu-id="3d618-139">하는 것이 좋습니다 요청은 `IOptions<T>` 인스턴스, 여기서 `T` 필요한 구성 클래스입니다.</span><span class="sxs-lookup"><span data-stu-id="3d618-139">A better approach is to request an `IOptions<T>` instance, where `T` is the configuration class you need.</span></span>

<span data-ttu-id="3d618-140">옵션 패턴을 사용 하려면이 다음과 같은 옵션을 나타내는 클래스를 만들려면:</span><span class="sxs-lookup"><span data-stu-id="3d618-140">To work with the options pattern, you need to create a class that represents the options, such as this one:</span></span>

[!code-csharp[Main](dependency-injection/sample/src/ControllerDI/Model/SampleWebSettings.cs)]

<span data-ttu-id="3d618-141">옵션 모델을 사용 하며 구성 클래스는 서비스 컬렉션에 추가 응용 프로그램을 구성 해야 `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="3d618-141">Then you need to configure the application to use the options model and add your configuration class to the services collection in `ConfigureServices`:</span></span>

[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Startup.cs?highlight=3,4,5,6,9,16,19&range=14-44)]

> [!NOTE]
> <span data-ttu-id="3d618-142">위의 목록에서 구성 하는 중 응용 프로그램을 JSON 형식 파일에서 설정을 읽습니다.</span><span class="sxs-lookup"><span data-stu-id="3d618-142">In the above listing, we are configuring the application to read the settings from a JSON-formatted file.</span></span> <span data-ttu-id="3d618-143">또한 위의 주석 처리 된 코드에 표시 된 대로 코드에서 완전히 설정을 구성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3d618-143">You can also configure the settings entirely in code, as is shown in the commented code above.</span></span> <span data-ttu-id="3d618-144">참조 [구성](xref:fundamentals/configuration/index) 추가 구성 옵션에 대 한 합니다.</span><span class="sxs-lookup"><span data-stu-id="3d618-144">See [Configuration](xref:fundamentals/configuration/index) for further configuration options.</span></span>

<span data-ttu-id="3d618-145">강력한 형식의 구성 개체를 지정한 후 (이 경우 `SampleWebSettings`) 추가할 서비스 컬렉션 요청할 수 있습니다이 모든 컨트롤러 또는 동작 메서드에서 인스턴스를 요청 하 여 `IOptions<T>` (이 경우 `IOptions<SampleWebSettings>`) .</span><span class="sxs-lookup"><span data-stu-id="3d618-145">Once you've specified a strongly-typed configuration object (in this case, `SampleWebSettings`) and added it to the services collection, you can request it from any Controller or Action method by requesting an instance of `IOptions<T>` (in this case, `IOptions<SampleWebSettings>`).</span></span> <span data-ttu-id="3d618-146">다음 코드는 컨트롤러에서 설정을 요청 것 하나를 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="3d618-146">The following code shows how one would request the settings from a controller:</span></span>

[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Controllers/SettingsController.cs?highlight=3,5,7&range=7-22)]

<span data-ttu-id="3d618-147">옵션 패턴을 설정 및 구성을 서로 분리할 수를 허용 하 고 되도록 컨트롤러 팔 로우 [문제의 분리](http://deviq.com/separation-of-concerns/)방법 또는 위치를 알 필요가 없으므로 않아야 하므로 설정을 찾을 수 정보입니다.</span><span class="sxs-lookup"><span data-stu-id="3d618-147">Following the Options pattern allows settings and configuration to be decoupled from one another, and ensures the controller is following [separation of concerns](http://deviq.com/separation-of-concerns/), since it doesn't need to know how or where to find the settings information.</span></span> <span data-ttu-id="3d618-148">하기가 컨트롤러 단위 테스트를 쉽게 [테스트 컨트롤러 논리](testing.md)있기 때문에, 없습니다 [정적 문에 붙이는](http://deviq.com/static-cling/) 또는 컨트롤러 클래스 내에서 설정 클래스의 직접 인스턴스화입니다.</span><span class="sxs-lookup"><span data-stu-id="3d618-148">It also makes the controller easier to unit test [Testing Controller Logic](testing.md), since there's no [static cling](http://deviq.com/static-cling/) or direct instantiation of settings classes within the controller class.</span></span>
