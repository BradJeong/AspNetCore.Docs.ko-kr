---
title: "ASP.NET Core MVC에서 모델 유효성 검사"
author: rachelappel
description: "ASP.NET Core MVC에서 모델 유효성 검사에 알아봅니다."
keywords: "ASP.NET Core, MVC에서 유효성 검사"
ms.author: riande
manager: wpickett
ms.date: 12/18/2016
ms.topic: article
ms.assetid: 3a8676dd-7ed8-4a05-bca2-44e288ab99ee
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/models/validation
ms.openlocfilehash: 7f641c247cb672934e76fa13bc7b7beb3990dd82
ms.sourcegitcommit: f5a7f0198628f0d152257d90dba6c3a0747a355a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/19/2017
---
# <a name="introduction-to-model-validation-in-aspnet-core-mvc"></a><span data-ttu-id="8f0b3-104">ASP.NET Core MVC에서 모델 유효성 검사 소개</span><span class="sxs-lookup"><span data-stu-id="8f0b3-104">Introduction to model validation in ASP.NET Core MVC</span></span>

<span data-ttu-id="8f0b3-105">으로 [Rachel Appel](https://github.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="8f0b3-105">By [Rachel Appel](https://github.com/rachelappel)</span></span>

## <a name="introduction-to-model-validation"></a><span data-ttu-id="8f0b3-106">모델 유효성 검사 소개</span><span class="sxs-lookup"><span data-stu-id="8f0b3-106">Introduction to model validation</span></span>

<span data-ttu-id="8f0b3-107">응용 프로그램 데이터베이스에 데이터를 저장 하기 전에 응용 프로그램 데이터를 확인 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-107">Before an app stores data in a database, the app must validate the data.</span></span> <span data-ttu-id="8f0b3-108">데이터 유형 및 크기로 적절 하 게 서식이 지정을 여 규칙을 준수 해야 하는 잠재적 보안 위협을 확인 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-108">Data must be checked for potential security threats, verified that it is appropriately formatted by type and size, and it must conform to your rules.</span></span> <span data-ttu-id="8f0b3-109">중복 되 고 구현 하기 번거로운 울 수 있지만 유효성 검사는 필요 합니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-109">Validation is necessary although it can be redundant and tedious to implement.</span></span> <span data-ttu-id="8f0b3-110">Mvc에서 유효성 검사는 클라이언트와 서버 모두에서 발생합니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-110">In MVC, validation happens on both the client and server.</span></span>

<span data-ttu-id="8f0b3-111">다행히.NET에 유효성 검사 특성에 대 한 유효성 검사를 추상화 합니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-111">Fortunately, .NET has abstracted validation into validation attributes.</span></span> <span data-ttu-id="8f0b3-112">이러한 특성에는 유효성 검사 코드를 작성 해야 하는 코드의 양을 줄이는 포함 합니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-112">These attributes contain validation code, thereby reducing the amount of code you must write.</span></span>

<span data-ttu-id="8f0b3-113">[보기 또는 GitHub에서 샘플을 다운로드](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/validation/sample)합니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-113">[View or download sample from GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/validation/sample).</span></span>

## <a name="validation-attributes"></a><span data-ttu-id="8f0b3-114">유효성 검사 특성</span><span class="sxs-lookup"><span data-stu-id="8f0b3-114">Validation Attributes</span></span>

<span data-ttu-id="8f0b3-115">유효성 검사 특성은 데이터베이스 테이블의 필드에 유효성 검사를 개념적으로 유사 하므로 모델 유효성 검사를 구성 하는 방법입니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-115">Validation attributes are a way to configure model validation so it's similar conceptually to validation on fields in database tables.</span></span> <span data-ttu-id="8f0b3-116">데이터 형식이 나 필수 필드를 할당 하는 등 제약 조건을 포함 됩니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-116">This includes constraints such as assigning data types or required fields.</span></span> <span data-ttu-id="8f0b3-117">다른 종류의 유효성 검사 전자 메일 주소 또는 전화 번호, 신용 카드와 같은 비즈니스 규칙을 적용 하는 데이터 패턴을 적용 하는 합니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-117">Other types of validation include applying patterns to data to enforce business rules, such as a credit card, phone number, or email address.</span></span> <span data-ttu-id="8f0b3-118">유효성 검사 특성 확인 훨씬 간단 하 고 사용 하기 쉽게 이러한 요구 사항을 적용 합니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-118">Validation attributes make enforcing these requirements much simpler and easier to use.</span></span>

<span data-ttu-id="8f0b3-119">다음은 이러한는 주석이 추가 된 `Movie` 동영상과 TV 프로그램에 대 한 정보를 저장 하는 응용 프로그램에서 모델입니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-119">Below is an annotated `Movie` model from an app that stores information about movies and TV shows.</span></span> <span data-ttu-id="8f0b3-120">대부분의 속성 필요 하며 여러 문자열 속성에는 길이 요구 사항이입니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-120">Most of the properties are required and several string properties have length requirements.</span></span> <span data-ttu-id="8f0b3-121">또한는에 대 한 숫자 범위 제한이 `Price` 속성을 0에서 $999.99, 사용자 지정 유효성 검사 특성을 합니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-121">Additionally, there is a numeric range restriction in place for the `Price` property from 0 to $999.99, along with a custom validation attribute.</span></span>

[!code-csharp[Main](validation/sample/Movie.cs?range=6-29)]

<span data-ttu-id="8f0b3-122">단순히 모델을 통해 읽을 쉽게 코드를 유지 하기 위해이 앱에 대 한 데이터에 대 한 규칙이 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-122">Simply reading through the model reveals the rules about data for this app, making it easier to maintain the code.</span></span> <span data-ttu-id="8f0b3-123">다음은 여러 인기 있는 기본 제공 유효성 검사 특성입니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-123">Below are several popular built-in validation attributes:</span></span>

* <span data-ttu-id="8f0b3-124">`[CreditCard]`: 유효성을 검사 속성에는 신용 카드 형식입니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-124">`[CreditCard]`: Validates the property has a credit card format.</span></span>

* <span data-ttu-id="8f0b3-125">`[Compare]`: 일치 하는 모델 항목에 두 속성의 유효성을 검사 합니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-125">`[Compare]`: Validates two properties in a model match.</span></span>

* <span data-ttu-id="8f0b3-126">`[EmailAddress]`: 유효성을 검사 속성에는 전자 메일 형식입니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-126">`[EmailAddress]`: Validates the property has an email format.</span></span>

* <span data-ttu-id="8f0b3-127">`[Phone]`: 유효성을 검사 속성에는 전화 형식입니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-127">`[Phone]`: Validates the property has a telephone format.</span></span>

* <span data-ttu-id="8f0b3-128">`[Range]`: 지정된 된 범위 내에서 속성 값 변경의 유효성을 검사 합니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-128">`[Range]`: Validates the property value falls within the given range.</span></span>

* <span data-ttu-id="8f0b3-129">`[RegularExpression]`: 데이터에 지정된 된 정규식과 일치 하는지 확인 합니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-129">`[RegularExpression]`: Validates that the data matches the specified regular expression.</span></span>

* <span data-ttu-id="8f0b3-130">`[Required]`: 필수 속성으로 설정 합니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-130">`[Required]`: Makes a property required.</span></span>

* <span data-ttu-id="8f0b3-131">`[StringLength]`: 문자열 속성에 지정 된 최대 길이 최대 있는지 확인 합니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-131">`[StringLength]`: Validates that a string property has at most the given maximum length.</span></span>

* <span data-ttu-id="8f0b3-132">`[Url]`: 유효성을 검사 속성이 URL 형식입니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-132">`[Url]`: Validates the property has a URL format.</span></span>

<span data-ttu-id="8f0b3-133">MVC에서 파생 된 모든 특성을 지원 `ValidationAttribute` 유효성 검사를 위해서입니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-133">MVC supports any attribute that derives from `ValidationAttribute` for validation purposes.</span></span> <span data-ttu-id="8f0b3-134">많은 유용한 유효성 검사 특성에서 확인할 수 있습니다는 [System.ComponentModel.DataAnnotations](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations) 네임 스페이스입니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-134">Many useful validation attributes can be found in the [System.ComponentModel.DataAnnotations](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations) namespace.</span></span>

<span data-ttu-id="8f0b3-135">제공 하는 기본 제공 특성 보다 더 많은 기능을 할 수 있는 인스턴스가 있을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-135">There may be instances where you need more features than built-in attributes provide.</span></span> <span data-ttu-id="8f0b3-136">해당 시간에 대 한 사용자 지정 유효성 검사 특성에서 파생 하 여 만들 수 있습니다 `ValidationAttribute` 구현 하 여 모델을 변경 하거나 `IValidatableObject`합니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-136">For those times, you can create custom validation attributes by deriving from `ValidationAttribute` or changing your model to implement `IValidatableObject`.</span></span>

## <a name="notes-on-the-use-of-the-required-attribute"></a><span data-ttu-id="8f0b3-137">필수 특성의 사용에 대 한 참고 사항</span><span class="sxs-lookup"><span data-stu-id="8f0b3-137">Notes on the use of the Required attribute</span></span>

<span data-ttu-id="8f0b3-138">nullable 형식이 아닌 [값 형식](/dotnet/csharp/language-reference/keywords/value-types)(`decimal`, `int`, `float`,`DateTime`)은 기본적으로 필요하며 `Required` 특성은 필요하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-138">Non-nullable [value types](/dotnet/csharp/language-reference/keywords/value-types) (such as `decimal`, `int`, `float`, and `DateTime`) are inherently required and don't need the `Required` attribute.</span></span> <span data-ttu-id="8f0b3-139">표시 된 null이 아닌 형식에 대 한 없는 서버 쪽 유효성 검사를 수행 하는 응용 프로그램 `Required`합니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-139">The app performs no server-side validation checks for non-nullable types that are marked `Required`.</span></span>

<span data-ttu-id="8f0b3-140">MVC 모델 바인딩 유효성 검사 및 유효성 검사 특성으로 관련 되지 않습니다는 누락 값 또는 공백 nullable이 아닌 형식에 대 한 포함 된 양식 필드 전송을 거부 합니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-140">MVC model binding, which isn't concerned with validation and validation attributes, rejects a form field submission containing a missing value or whitespace for a non-nullable type.</span></span> <span data-ttu-id="8f0b3-141">없는 경우에는 `BindRequired` 특성 모델 바인딩 대상 속성에 양식 필드 없는 nullable이 아닌 형식에 대 한 누락 된 데이터를 무시에서 들어오는 양식 데이터입니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-141">In the absence of a `BindRequired` attribute on the target property, model binding ignores missing data for non-nullable types, where the form field is absent from the incoming form data.</span></span>

<span data-ttu-id="8f0b3-142">[BindRequired 특성](/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.bindrequiredattribute) (참조도 [특성으로 모델 바인딩 동작 사용자 지정](xref:mvc/models/model-binding#customize-model-binding-behavior-with-attributes))는 양식 데이터를 완료 하는 데 유용 합니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-142">The [BindRequired attribute](/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.bindrequiredattribute) (also see [Customize model binding behavior with attributes](xref:mvc/models/model-binding#customize-model-binding-behavior-with-attributes)) is useful to ensure form data is complete.</span></span> <span data-ttu-id="8f0b3-143">속성에 적용 하는 경우 모델 바인딩 시스템이 해당 속성에 대 한 값이 필요 합니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-143">When applied to a property, the model binding system requires a value for that property.</span></span> <span data-ttu-id="8f0b3-144">형식에 적용 하는 경우 모델 바인딩 시스템이 모든 해당 형식의 속성에 대 한 값이 필요 합니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-144">When applied to a type, the model binding system requires values for all of the properties of that type.</span></span>

<span data-ttu-id="8f0b3-145">사용 하는 경우는 [Nullable\<T > 형식](/dotnet/csharp/programming-guide/nullable-types/) (예를 들어 `decimal?` 또는 `System.Nullable<decimal>`) 하 고 표시 `Required`, 속성 (nullable 표준 형식인 경우 처럼 서버 쪽 유효성 검사가 수행 됩니다 예제에서는 한 `string`).</span><span class="sxs-lookup"><span data-stu-id="8f0b3-145">When you use a [Nullable\<T> type](/dotnet/csharp/programming-guide/nullable-types/) (for example, `decimal?` or `System.Nullable<decimal>`) and mark it `Required`, a server-side validation check is performed as if the property were a standard nullable type (for example, a `string`).</span></span>

<span data-ttu-id="8f0b3-146">클라이언트 쪽 유효성 검사 모델 속성을 표시 한에 해당 하는 양식 필드의 값이 필요 `Required` 하 고 nullable이 아닌 형식 속성으로 표시 하지 않은 `Required`합니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-146">Client-side validation requires a value for a form field that corresponds to a model property that you've marked `Required` and for a non-nullable type property that you haven't marked `Required`.</span></span> <span data-ttu-id="8f0b3-147">`Required`클라이언트 쪽 유효성 검사 오류 메시지를 제어 하 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-147">`Required` can be used to control the client-side validation error message.</span></span>

## <a name="model-state"></a><span data-ttu-id="8f0b3-148">모델 상태</span><span class="sxs-lookup"><span data-stu-id="8f0b3-148">Model State</span></span>

<span data-ttu-id="8f0b3-149">모델 상태 제출 된 HTML 폼 값의 유효성 검사 오류를 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-149">Model state represents validation errors in submitted HTML form values.</span></span>

<span data-ttu-id="8f0b3-150">MVC는 계속 필드에 도달할 때까지 유효성 검사 오류 (기본적으로 200)의 최대 수입니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-150">MVC will continue validating fields until reaches the maximum number of errors (200 by default).</span></span> <span data-ttu-id="8f0b3-151">다음 코드를 삽입 하 여이 번호를 구성할 수 있습니다는 `ConfigureServices` 에서 메서드는 *Startup.cs* 파일:</span><span class="sxs-lookup"><span data-stu-id="8f0b3-151">You can configure this number by inserting the following code into the `ConfigureServices` method in the *Startup.cs* file:</span></span>

[!code-csharp[Main](validation/sample/Startup.cs?range=27)]

## <a name="handling-model-state-errors"></a><span data-ttu-id="8f0b3-152">처리 모델 상태 오류</span><span class="sxs-lookup"><span data-stu-id="8f0b3-152">Handling Model State Errors</span></span>

<span data-ttu-id="8f0b3-153">모델 유효성 검사가 호출 되는 각 컨트롤러 동작 하기 전에 발생 하 고 검사 해야 하는 작업 메서드의 `ModelState.IsValid` 하 고 적절 하 게 대처 합니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-153">Model validation occurs prior to each controller action being invoked, and it is the action method’s responsibility to inspect `ModelState.IsValid` and react appropriately.</span></span> <span data-ttu-id="8f0b3-154">대부분의 경우에서 적합 한 반응 이상적 모델 유효성 검사에 실패 한 이유를 자세히 보여 주는 오류 응답을 반환 하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-154">In many cases, the appropriate reaction is to return an error response, ideally detailing the reason why model validation failed.</span></span>

<span data-ttu-id="8f0b3-155">모델 유효성 검사 오류,이 경우 필터 수 이어야 이러한 정책을 구현 하는 적절 한 위치를 처리 하기 위한 표준 규칙을 따르는 일부 응용 프로그램을 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-155">Some apps will choose to follow a standard convention for dealing with model validation errors, in which case a filter may be an appropriate place to implement such a policy.</span></span> <span data-ttu-id="8f0b3-156">유효 하지 않은 모델 상태와 작업을 수행할 때 동작 하는 방법을 테스트 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-156">You should test how your actions behave with valid and invalid model states.</span></span>

## <a name="manual-validation"></a><span data-ttu-id="8f0b3-157">수동 유효성 검사</span><span class="sxs-lookup"><span data-stu-id="8f0b3-157">Manual validation</span></span>

<span data-ttu-id="8f0b3-158">모델 바인딩 및 유효성 검사 완료 되 면 일부를 반복 하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-158">After model binding and validation are complete, you may want to repeat parts of it.</span></span> <span data-ttu-id="8f0b3-159">예를 들어, 사용자, 정수가 필요한 필드에 텍스트 입력 한 수 또는 모델의 속성에 대 한 값을 계산 해야 할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-159">For example, a user may have entered text in a field expecting an integer, or you may need to compute a value for a model's property.</span></span>

<span data-ttu-id="8f0b3-160">유효성 검사를 수동으로 실행 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-160">You may need to run validation manually.</span></span> <span data-ttu-id="8f0b3-161">이렇게 하려면 호출 된 `TryValidateModel` 메서드를 다음과 같이 합니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-161">To do so, call the `TryValidateModel` method, as shown here:</span></span>

[!code-csharp[Main](validation/sample/MoviesController.cs?range=52)]

## <a name="custom-validation"></a><span data-ttu-id="8f0b3-162">사용자 지정 유효성 검사</span><span class="sxs-lookup"><span data-stu-id="8f0b3-162">Custom validation</span></span>

<span data-ttu-id="8f0b3-163">유효성 검사 특성에 대 한 대부분의 유효성 검사 요구 사항을 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-163">Validation attributes work for most validation needs.</span></span> <span data-ttu-id="8f0b3-164">그러나 일부 유효성 검사 규칙은 비즈니스 관련이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-164">However, some validation rules are specific to your business.</span></span> <span data-ttu-id="8f0b3-165">규칙을 직접 만들 필드를 확인 하는 것이 필요 하거나 값의 범위를 준수와 같은 일반적인 데이터 유효성 검사 방법 아닐 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-165">Your rules might not be common data validation techniques such as ensuring a field is required or that it conforms to a range of values.</span></span> <span data-ttu-id="8f0b3-166">이러한 시나리오에 대 한 사용자 지정 유효성 검사 특성은이 가장 좋은 방법입니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-166">For these scenarios, custom validation attributes are a great solution.</span></span> <span data-ttu-id="8f0b3-167">MVC에서 사용자 고유의 사용자 지정 유효성 검사 특성을 만들기 쉽습니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-167">Creating your own custom validation attributes in MVC is easy.</span></span> <span data-ttu-id="8f0b3-168">상속 하는 것은 `ValidationAttribute`, 재정의 `IsValid` 메서드.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-168">Just inherit from the `ValidationAttribute`, and override the `IsValid` method.</span></span> <span data-ttu-id="8f0b3-169">`IsValid` 메서드는 두 개의 매개 변수를 첫 번째는 라는 개체 *값* 고 두 번째는 `ValidationContext` 라는 개체 *validationContext*합니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-169">The `IsValid` method accepts two parameters, the first is an object named *value* and the second is a `ValidationContext` object named *validationContext*.</span></span> <span data-ttu-id="8f0b3-170">*값* 지정 유효성 검사기의 유효성을 검사할 필드의 실제 값을 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-170">*Value* refers to the actual value from the field that your custom validator is validating.</span></span>

<span data-ttu-id="8f0b3-171">다음 샘플에서는 비즈니스 규칙에 따르면 사용자가 장르를 설정 하지 않을 수 있습니다 *클래식* 1960 이후에 출시 동영상에 대 한 합니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-171">In the following sample, a business rule states that users may not set the genre to *Classic* for a movie released after 1960.</span></span> <span data-ttu-id="8f0b3-172">`[ClassicMovie]` 특성 장르를 먼저 확인 하 고 기존 인 경우 다음 검사 릴리스 날짜 이후 1960 중인지 확인 합니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-172">The `[ClassicMovie]` attribute checks the genre first, and if it is a classic, then it checks the release date to see that it is later than 1960.</span></span> <span data-ttu-id="8f0b3-173">1960 후 해제 되 면 유효성 검사가 실패 합니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-173">If it is released after 1960, validation fails.</span></span> <span data-ttu-id="8f0b3-174">특성은 데이터 유효성을 검사 하는 데 사용할 수 있는 연도 나타내는 정수 매개 변수를 허용 합니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-174">The attribute accepts an integer parameter representing the year that you can use to validate data.</span></span> <span data-ttu-id="8f0b3-175">다음과 같이 특성의 생성자에서 매개 변수의 값을 캡처할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-175">You can capture the value of the parameter in the attribute's constructor, as shown here:</span></span>

[!code-csharp[Main](validation/sample/ClassicMovieAttribute.cs?range=9-29)]

<span data-ttu-id="8f0b3-176">`movie` 나타냅니다 위에 변수는 `Movie` 유효성을 검사할 양식 전송에서 데이터를 포함 하는 개체입니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-176">The `movie` variable above represents a `Movie` object that contains the data from the form submission to validate.</span></span> <span data-ttu-id="8f0b3-177">이 경우 유효성 검사를 확인 하는 날짜에 장르는 `IsValid` 의 메서드는 `ClassicMovieAttribute` 규칙에 따라 클래스입니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-177">In this case, the validation code checks the date and genre in the `IsValid` method of the `ClassicMovieAttribute` class as per the rules.</span></span> <span data-ttu-id="8f0b3-178">유효성을 검사 `IsValid` 반환는 `ValidationResult.Success` 코드 시점과 유효성 검사에 실패 한 `ValidationResult` 오류 메시지와 함께 합니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-178">Upon successful validation `IsValid` returns a `ValidationResult.Success` code, and when validation fails, a `ValidationResult` with an error message.</span></span> <span data-ttu-id="8f0b3-179">사용자가 수정 하는 경우는 `Genre` 필드에서 폼을 전송 하 고는 `IsValid` 의 메서드는 `ClassicMovieAttribute` 영화 기존 인지 확인 합니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-179">When a user modifies the `Genre` field and submits the form, the `IsValid` method of the `ClassicMovieAttribute` will verify whether the movie is a classic.</span></span> <span data-ttu-id="8f0b3-180">기본 제공 특성과 마찬가지로 적용 된 `ClassicMovieAttribute` 와 같은 속성에 `ReleaseDate` 앞의 코드 예제에 표시 된 대로 발생 하는 유효성 검사를 진행 하려면.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-180">Like any built-in attribute, apply the `ClassicMovieAttribute` to a property such as `ReleaseDate` to ensure validation happens, as shown in the previous code sample.</span></span> <span data-ttu-id="8f0b3-181">이 예제 에서만 작동 하므로 `Movie` 형식, 더 나은 옵션은 사용 하 `IValidatableObject` 다음 단락에서 표시 된 것 처럼 합니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-181">Since the example works only with `Movie` types, a better option is to use `IValidatableObject` as shown in the following paragraph.</span></span>

<span data-ttu-id="8f0b3-182">구현 하 여 모델에이 동일한 코드에 배치 수는 또는 `Validate` 에서 메서드는 `IValidatableObject` 인터페이스입니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-182">Alternatively, this same code could be placed in the model by implementing the `Validate` method on the `IValidatableObject` interface.</span></span> <span data-ttu-id="8f0b3-183">사용자 지정 유효성 검사 특성을 개별 속성 유효성 검사 잘 작동 하는 동안 구현 `IValidatableObject` 는 다음 그림과 같이 클래스 수준 유효성 검사를 구현 하는 데 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-183">While custom validation attributes work well for validating individual properties, implementing `IValidatableObject` can be used to implement class-level validation as seen here.</span></span>

[!code-csharp[Main](validation/sample/MovieIValidatable.cs?range=32-40)]

## <a name="client-side-validation"></a><span data-ttu-id="8f0b3-184">클라이언트 쪽 유효성 검사</span><span class="sxs-lookup"><span data-stu-id="8f0b3-184">Client side validation</span></span>

<span data-ttu-id="8f0b3-185">클라이언트 쪽 유효성 검사는 사용자에 대 한 매우 편리 합니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-185">Client side validation is a great convenience for users.</span></span> <span data-ttu-id="8f0b3-186">시간을 절약 그렇지 않은 경우 많은 비용이 들어가는지는 왕복에 대 한 대기 중인 서버.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-186">It saves time they would otherwise spend waiting for a round trip to the server.</span></span> <span data-ttu-id="8f0b3-187">비즈니스 관계에서 몇 초 부분을 곱한 수백 번 매일 추가 최대 많은 시간과 비용, 노력 합니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-187">In business terms, even a few fractions of seconds multiplied hundreds of times each day adds up to be a lot of time, expense, and frustration.</span></span> <span data-ttu-id="8f0b3-188">간단 하 고 즉시 유효성 검사를 사용 하면 보다 효율적으로 작동 하 고 더 좋은 품질 입력 및 출력을 생성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-188">Straightforward and immediate validation enables users to work more efficiently and produce better quality input and output.</span></span>

<span data-ttu-id="8f0b3-189">여기에 표시 된 대로 작동 하도록 클라이언트 쪽 유효성 검사에 대 한 위치에서 적절 한 JavaScript 스크립트 참조 하 여 뷰에 있어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-189">You must have a view with the proper JavaScript script references in place for client side validation to work as you see here.</span></span>

[!code-cshtml[Main](validation/sample/Views/Shared/_Layout.cshtml?range=37)]

[!code-cshtml[Main](validation/sample/Views/Shared/_ValidationScriptsPartial.cshtml)]

<span data-ttu-id="8f0b3-190">[유효성 검사 비 가시적인 jQuery](https://github.com/aspnet/jquery-validation-unobtrusive) 스크립트는 인기 있는에 작성 되는 사용자 지정 Microsoft 프런트 엔드 라이브러리 [jQuery 유효성 검사](https://jqueryvalidation.org/) 플러그 인 합니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-190">The [jQuery Unobtrusive Validation](https://github.com/aspnet/jquery-validation-unobtrusive) script is a custom Microsoft front-end library that builds on the popular [jQuery Validate](https://jqueryvalidation.org/) plugin.</span></span> <span data-ttu-id="8f0b3-191">JQuery 비간섭 유효성 검사 없이 두 위치에서 동일한 유효성 검사 논리를 코딩 해야: 모델 속성에 대 한 서버 쪽 유효성 검사 특성에 두 번 한 다음 클라이언트 쪽 스크립트에서 다시 (jQuery 유효성 검사의에 대 한 예제 [ `validate()` ](https://jqueryvalidation.org/validate/) 메서드 표시 하 고 복잡 한 정도이 될 수 있습니다).</span><span class="sxs-lookup"><span data-stu-id="8f0b3-191">Without jQuery Unobtrusive Validation, you would have to code the same validation logic in two places: once in the server side validation attributes on model properties, and then again in client side scripts (the examples for jQuery Validate's [`validate()`](https://jqueryvalidation.org/validate/) method shows how complex this could become).</span></span> <span data-ttu-id="8f0b3-192">대신, MVC의 [태그 도우미](xref:mvc/views/tag-helpers/intro) 및 [HTML 도우미](xref:mvc/views/overview) HTML 5 렌더링 하는 모델 속성의 메타 데이터를 입력 하 고 유효성 검사 특성을 사용 하 여 [데이터 특성](http://w3c.github.io/html/dom.html#embedding-custom-non-visible-data-with-the-data-attributes) 에서 유효성 검사는 폼 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-192">Instead, MVC's [Tag Helpers](xref:mvc/views/tag-helpers/intro) and [HTML helpers](xref:mvc/views/overview) are able to use the validation attributes and type metadata from model properties to render HTML 5 [data- attributes](http://w3c.github.io/html/dom.html#embedding-custom-non-visible-data-with-the-data-attributes) in the form elements that need validation.</span></span> <span data-ttu-id="8f0b3-193">MVC에서는 오류가 발생 하는 `data-` 기본 제공 및 사용자 지정 특성에 대 한 특성입니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-193">MVC generates the `data-` attributes for both built-in and custom attributes.</span></span> <span data-ttu-id="8f0b3-194">jQuery 비간섭 유효성 검사 후 구문 분석 하 thes `data-` 특성 및 jQuery 유효성 검사를 효과적으로 "복사" 서버 쪽 유효성 검사 논리를 클라이언트에는 논리를 전달 합니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-194">jQuery Unobtrusive Validation then parses thes `data-` attributes and passes the logic to jQuery Validate, effectively "copying" the server side validation logic to the client.</span></span> <span data-ttu-id="8f0b3-195">다음과 같이 관련 태그 도우미를 사용 하 여 클라이언트에서 유효성 검사 오류를 표시할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-195">You can display validation errors on the client using the relevant tag helpers as shown here:</span></span>

[!code-cshtml[Main](validation/sample/Views/Movies/Create.cshtml?highlight=4,5&range=19-25)]

<span data-ttu-id="8f0b3-196">위의 태그 도우미 아래 HTML을 렌더링합니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-196">The tag helpers above render the HTML below.</span></span> <span data-ttu-id="8f0b3-197">다음에 유의 `data-` HTML의 특성에 대 한 유효성 검사 특성에 해당 출력은 `ReleaseDate` 속성입니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-197">Notice that the `data-` attributes in the HTML output correspond to the validation attributes for the `ReleaseDate` property.</span></span> <span data-ttu-id="8f0b3-198">`data-val-required` 특성 아래 릴리스 날짜 필드에 사용자 이름을 입력 하지 않는 경우를 표시 하려면 오류 메시지를 포함 합니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-198">The `data-val-required` attribute below contains an error message to display if the user doesn't fill in the release date field.</span></span> <span data-ttu-id="8f0b3-199">jQuery 유효성 검사 비 가시적인 jQuery 유효성 검사에이 값을 전달 [ `required()` ](https://jqueryvalidation.org/required-method/) 다음 함께 제공 되의 해당 메시지를 표시 하는 메서드를  **\<걸쳐 >** 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-199">jQuery Unobtrusive Validation passes this value to the jQuery Validate [`required()`](https://jqueryvalidation.org/required-method/) method, which then displays that message in the accompanying **\<span>** element.</span></span>

```html
<form action="/Movies/Create" method="post">
    <div class="form-horizontal">
        <h4>Movie</h4>
        <div class="text-danger"></div>
        <div class="form-group">
            <label class="col-md-2 control-label" for="ReleaseDate">ReleaseDate</label>
            <div class="col-md-10">
                <input class="form-control" type="datetime"
                data-val="true" data-val-required="The ReleaseDate field is required."
                id="ReleaseDate" name="ReleaseDate" value="" />
                <span class="text-danger field-validation-valid"
                data-valmsg-for="ReleaseDate" data-valmsg-replace="true"></span>
            </div>
        </div>
    </div>
</form>
```

<span data-ttu-id="8f0b3-200">폼이 유효한 될 때까지 전송을 방지 하는 클라이언트 쪽 유효성 검사 합니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-200">Client-side validation prevents submission until the form is valid.</span></span> <span data-ttu-id="8f0b3-201">제출 단추가 양식을 전송 또는 오류 메시지를 표시 하는 JavaScript를 실행 합니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-201">The Submit button runs JavaScript that either submits the form or displays error messages.</span></span>

<span data-ttu-id="8f0b3-202">MVC 유형 특성 값을 사용 하 여를 재정의할 수 있는 속성의.NET 데이터 형식을 기반으로 결정 `[DataType]` 특성입니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-202">MVC determines type attribute values based on the .NET data type of a property, possibly overridden using `[DataType]` attributes.</span></span> <span data-ttu-id="8f0b3-203">기본 `[DataType]` 특성 실제 서버 쪽 유효성 검사가 수행 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-203">The base `[DataType]` attribute does no real server-side validation.</span></span> <span data-ttu-id="8f0b3-204">그러나 브라우저 자신의 오류 메시지를 선택 하 고 만들려는, jQuery 유효성 검사 비간섭 패키지는 메시지를 무시 하 고 다른 사용자와 일관 되 게 표시 수도 있지만 이러한 오류를 표시 합니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-204">Browsers choose their own error messages and display those errors however they wish, however the jQuery Validation Unobtrusive package can override the messages and display them consistently with others.</span></span> <span data-ttu-id="8f0b3-205">사용자를 적용할 때 가장 분명 이런 `[DataType]` 하위 클래스와 같은 `[EmailAddress]`합니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-205">This happens most obviously when users apply `[DataType]` subclasses such as `[EmailAddress]`.</span></span>

### <a name="add-validation-to-dynamic-forms"></a><span data-ttu-id="8f0b3-206">동적 양식을에 유효성 검사 추가</span><span class="sxs-lookup"><span data-stu-id="8f0b3-206">Add Validation to Dynamic Forms</span></span>

<span data-ttu-id="8f0b3-207">유효성 검사 비 가시적인 jQuery 유효성 검사 논리 및 매개 변수 jQuery 유효성 검사 페이지 처음 로드 될 때를 전달 하기 때문에 동적으로 생성 된 양식 유효성 검사를 자동으로 발생 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-207">Because jQuery Unobtrusive Validation passes validation logic and parameters to jQuery Validate when the page first loads, dynamically generated forms will not automatically exhibit validation.</span></span> <span data-ttu-id="8f0b3-208">대신, jQuery 비간섭 유효성 검사 만든 후 즉시 동적 폼의 구문 분석을 알려야 합니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-208">Instead, you must tell jQuery Unobtrusive Validation to parse the dynamic form immediately after creating it.</span></span> <span data-ttu-id="8f0b3-209">예를 들어 아래 코드는 AJAX를 통해 추가 폼의 클라이언트 쪽 유효성 검사를 설정할 수 있는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-209">For example, the code below shows how you might set up client side validation on a form added via AJAX.</span></span>

```js
$.get({
    url: "https://url/that/returns/a/form",
    dataType: "html",
    error: function(jqXHR, textStatus, errorThrown) {
        alert(textStatus + ": Could not add form. " + errorThrown);
    },
    success: function(newFormHTML) {
        var container = document.getElementById("form-container");
        container.insertAdjacentHTML("beforeend", newFormHTML);
        var forms = container.getElementsByTagName("form");
        var newForm = forms[forms.length - 1];
        $.validator.unobtrusive.parse(newForm);
    }
})
```

<span data-ttu-id="8f0b3-210">`$.validator.unobtrusive.parse()` 메서드는 하나의 인수에 필요한 jQuery 선택기를 허용 합니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-210">The `$.validator.unobtrusive.parse()` method accepts a jQuery selector for its one argument.</span></span> <span data-ttu-id="8f0b3-211">이 메서드는 jQuery의 구문 분석을 비 가시적인 유효성 검사를 지시는 `data-` 해당 선택기 내 폼의 특성입니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-211">This method tells jQuery Unobtrusive Validation to parse the `data-` attributes of forms within that selector.</span></span> <span data-ttu-id="8f0b3-212">이러한 특성의 값을 양식에서 원하는 클라이언트 쪽 유효성 검사 규칙을 보여 줍니다.에 다음 jQuery 유효성 검사 플러그 인에 전달 됩니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-212">The values of those attributes are then passed to the jQuery Validate plugin so that the form exhibits the desired client side validation rules.</span></span>

### <a name="add-validation-to-dynamic-controls"></a><span data-ttu-id="8f0b3-213">동적 컨트롤에 유효성 검사 추가</span><span class="sxs-lookup"><span data-stu-id="8f0b3-213">Add Validation to Dynamic Controls</span></span>

<span data-ttu-id="8f0b3-214">등의 개별 컨트롤 때에 폼에 대 한 유효성 검사 규칙을 업데이트할 수 있습니다 `<input/>`s 및 `<select/>`s, 동적으로 생성 됩니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-214">You can also update the validation rules on a form when individual controls, such as `<input/>`s and `<select/>`s, are dynamically generated.</span></span> <span data-ttu-id="8f0b3-215">이러한 요소에 대 한 선택기를 전달할 수 없습니다는 `parse()` 메서드를 직접 주변 폼 이미 구문 분석 하 고 업데이트 되지 것입니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-215">You cannot pass selectors for these elements to the `parse()` method directly because the surrounding form has already been parsed and will not update.</span></span>  <span data-ttu-id="8f0b3-216">대신, 있습니다 먼저 기존 유효성 검사 데이터를 제거한 다음 아래와 같이 전체 폼 재분석:</span><span class="sxs-lookup"><span data-stu-id="8f0b3-216">Instead, you first remove the existing validation data, then reparse the entire form, as shown below:</span></span>

```js
$.get({
    url: "https://url/that/returns/a/control",
    dataType: "html",
    error: function(jqXHR, textStatus, errorThrown) {
        alert(textStatus + ": Could not add form. " + errorThrown);
    },
    success: function(newInputHTML) {
        var form = document.getElementById("my-form");
        form.insertAdjacentHTML("beforeend", newInputHTML);
        form.removeData("validator")    // Added by the raw jQuery Validate
            .removeData("unobtrusiveValidation");   // Added by jQuery Unobtrusive Validation
        $.validator.unobtrusive.parse(form);
    }
})
```

## <a name="iclientmodelvalidator"></a><span data-ttu-id="8f0b3-217">IClientModelValidator</span><span class="sxs-lookup"><span data-stu-id="8f0b3-217">IClientModelValidator</span></span>

<span data-ttu-id="8f0b3-218">사용자 지정 특성에 대 한 클라이언트 쪽 논리를 만들 수 있습니다 및 [비간섭 유효성 검사](http://jqueryvalidation.org/documentation/) 클라이언트에 유효성 검사의 일부로 자동으로 실행 됩니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-218">You may create client side logic for your custom attribute, and [unobtrusive validation](http://jqueryvalidation.org/documentation/) will execute it on the client for you automatically as part of validation.</span></span> <span data-ttu-id="8f0b3-219">첫 번째 단계를 구현 하 여 어떤 데이터 특성 추가 제어 하는 것은 `IClientModelValidator` 다음과 같이 인터페이스:</span><span class="sxs-lookup"><span data-stu-id="8f0b3-219">The first step is to control what data- attributes are added by implementing the `IClientModelValidator` interface as shown here:</span></span>

[!code-csharp[Main](validation/sample/ClassicMovieAttribute.cs?range=30-42)]

<span data-ttu-id="8f0b3-220">이 인터페이스를 구현 하는 특성이 생성 된 필드를 HTML 특성을 추가할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-220">Attributes that implement this interface can add HTML attributes to generated fields.</span></span> <span data-ttu-id="8f0b3-221">에 대 한 출력을 검사 하는 `ReleaseDate` 요소는 이전 예제와 비슷하지만 이제는 HTML을 표시는 `data-val-classicmovie` 에 정의 된 특성의 `AddValidation` 방식의 `IClientModelValidator`합니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-221">Examining the output for the `ReleaseDate` element reveals HTML that is similar to the previous example, except now there is a `data-val-classicmovie` attribute that was defined in the `AddValidation` method of `IClientModelValidator`.</span></span>

```html
<input class="form-control" type="datetime"
    data-val="true"
    data-val-classicmovie="Classic movies must have a release year earlier than 1960."
    data-val-classicmovie-year="1960"
    data-val-required="The ReleaseDate field is required."
    id="ReleaseDate" name="ReleaseDate" value="" />
```

<span data-ttu-id="8f0b3-222">비 가시적인 유효성 검사에서 데이터를 사용 하는 `data-` 특성 오류 메시지를 표시 합니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-222">Unobtrusive validation uses the data in the `data-` attributes to display error messages.</span></span> <span data-ttu-id="8f0b3-223">그러나 jQuery 규칙을 인식 하지 못합니다 또는 jQuery의에 추가 될 때까지 메시지 `validator` 개체입니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-223">However, jQuery doesn't know about rules or messages until you add them to jQuery's `validator` object.</span></span> <span data-ttu-id="8f0b3-224">라는 메서드를 추가 하는 아래 예제에서이 확인할 `classicmovie` jQuery로 사용자 지정 클라이언트 유효성 검사 코드가 포함 된 `validator` 개체입니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-224">This is shown in the example below that adds a method named `classicmovie` containing custom client validation code to the jQuery `validator` object.</span></span>

[!code-javascript[Main](validation/sample/Views/Movies/Create.cshtml?range=71-93)]

<span data-ttu-id="8f0b3-225">이제 jQuery 유효성 검사 코드는 false를 반환 하는 경우 표시할 오류 메시지 뿐 아니라 사용자 지정 JavaScript 유효성 검사를 실행 하는 정보를 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-225">Now jQuery has the information to execute the custom JavaScript validation as well as the error message to display if that validation code returns false.</span></span>

## <a name="remote-validation"></a><span data-ttu-id="8f0b3-226">원격 유효성 검사</span><span class="sxs-lookup"><span data-stu-id="8f0b3-226">Remote validation</span></span>

<span data-ttu-id="8f0b3-227">원격 유효성 검사는 서버에서 데이터에 대 한 클라이언트에서 데이터의 유효성을 검사 해야 할 때 사용할 수 있는 유용한 기능입니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-227">Remote validation is a great feature to use when you need to validate data on the client against data on the server.</span></span> <span data-ttu-id="8f0b3-228">예를 들어 앱 여부는 전자 메일 또는 사용자 이름이 이미 사용에서 되 고 많은 양의 데이터를 쿼리해야 할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-228">For example, your app may need to verify whether an email or user name is already in use, and it must query a large amount of data to do so.</span></span> <span data-ttu-id="8f0b3-229">하나는 유효성 검사에 대 한 데이터 집합 다운로드 큰 또는 할 필드가 단 몇 너무 많은 리소스를 소비 합니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-229">Downloading large sets of data for validating one or a few fields consumes too many resources.</span></span> <span data-ttu-id="8f0b3-230">중요 한 정보를 노출할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-230">It may also expose sensitive information.</span></span> <span data-ttu-id="8f0b3-231">필드의 유효성을 검사 하는 왕복 요청 하는 대신이입니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-231">An alternative is to make a round-trip request to validate a field.</span></span>

<span data-ttu-id="8f0b3-232">2 단계 프로세스에서 원격 유효성 검사를 구현할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-232">You can implement remote validation in a two step process.</span></span> <span data-ttu-id="8f0b3-233">첫째, 주석으로 처리 해야 사용 하 여 모델의 `[Remote]` 특성입니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-233">First, you must annotate your model with the `[Remote]` attribute.</span></span> <span data-ttu-id="8f0b3-234">`[Remote]` 특성 직접 클라이언트 쪽 JavaScript는 해당 코드를 호출 하는 데 사용할 수 있는 여러 오버 로드를 허용 합니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-234">The `[Remote]` attribute accepts multiple overloads you can use to direct client side JavaScript to the appropriate code to call.</span></span> <span data-ttu-id="8f0b3-235">다음 예제에서는 가리키는 `VerifyEmail` 의 동작 메서드는 `Users` 컨트롤러입니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-235">The example below points to the `VerifyEmail` action method of the `Users` controller.</span></span>

[!code-csharp[Main](validation/sample/User.cs?range=7-8)]

<span data-ttu-id="8f0b3-236">두 번째 단계는 지정 하는 것 유효성 검사 코드는 해당 작업 메서드에서 정의 된 대로 `[Remote]` 특성입니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-236">The second step is putting the validation code in the corresponding action method as defined in the `[Remote]` attribute.</span></span> <span data-ttu-id="8f0b3-237">JQuery 유효성 검사에 따라 [ `remote()` ](https://jqueryvalidation.org/remote-method/) 메서드 설명서:</span><span class="sxs-lookup"><span data-stu-id="8f0b3-237">According to the jQuery Validate [`remote()`](https://jqueryvalidation.org/remote-method/) method documentation:</span></span>

> <span data-ttu-id="8f0b3-238">Serverside 응답 해야 하는 JSON 문자열 이어야 합니다. `"true"` 유효한 요소에 대 한 일 수 있습니다 및 `"false"`, `undefined`, 또는 `null` 잘못 된 요소에 대 한 기본 오류 메시지를 사용 하 여 합니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-238">The serverside response must be a JSON string that must be `"true"` for valid elements, and can be `"false"`, `undefined`, or `null` for invalid elements, using the default error message.</span></span> <span data-ttu-id="8f0b3-239">Serverside 응답 문자열인 경우를 제외 합니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-239">If the serverside response is a string, eg.</span></span> <span data-ttu-id="8f0b3-240">`"That name is already taken, try peter123 instead"`이 문자열의 기본 대신 사용자 지정 오류 메시지로 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-240">`"That name is already taken, try peter123 instead"`, this string will be displayed as a custom error message in place of the default.</span></span>

<span data-ttu-id="8f0b3-241">정의 `VerifyEmail()` 메서드 아래 표시 된 대로 다음이 규칙을 따릅니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-241">The definition of the `VerifyEmail()` method follows these rules, as shown below.</span></span> <span data-ttu-id="8f0b3-242">유효성 검사 오류가 반환 전자 메일 사용 된 경우 메시지 또는 `true` 전자 메일 것은 무료 이며 사용 하는지를 확인 하 고 결과를 래핑하는 `JsonResult` 개체입니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-242">It returns a validation error message if the email is taken, or `true` if the email is free, and wraps the result in a `JsonResult` object.</span></span> <span data-ttu-id="8f0b3-243">클라이언트 쪽 계속 필요한 경우 오류를 표시 하려면 반환된 된 값을 유도할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-243">The client side can then use the returned value to proceed or display the error if needed.</span></span>

[!code-csharp[Main](validation/sample/UsersController.cs?range=19-28)]

<span data-ttu-id="8f0b3-244">이제 사용자가 전자 메일을 입력 하면 보기에는 JavaScript에서는 한 원격 호출을 참조 하는 경우 해당 전자 메일 작업이 수행 되 고,이 경우 오류 메시지를 표시 합니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-244">Now when users enter an email, JavaScript in the view makes a remote call to see if that email has been taken and, if so, displays the error message.</span></span> <span data-ttu-id="8f0b3-245">그렇지 않으면 사용자는 일반적으로 폼을 제출할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-245">Otherwise, the user can submit the form as usual.</span></span>

<span data-ttu-id="8f0b3-246">`AdditionalFields` 의 속성은 `[Remote]` 특성은 조합에 서버에서 데이터에 대 한 유효성을 검사 하는 데 유용 합니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-246">The `AdditionalFields` property of the `[Remote]` attribute is useful for validating combinations of fields against data on the server.</span></span>  <span data-ttu-id="8f0b3-247">예를 들어 경우는 `User` 위쪽에서 모델에 라는 두 개의 추가 속성 `FirstName` 및 `LastName`, 기존 사용자가 이미 있는 해당 쌍의 이름 확인 해야 할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-247">For example, if the `User` model from above had two additional properties called `FirstName` and `LastName`, you might want to verify that no existing users already have that pair of names.</span></span>  <span data-ttu-id="8f0b3-248">다음 코드 에서처럼 새 속성을 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-248">You define the new properties as shown in the following code:</span></span>

[!code-csharp[Main](validation/sample/User.cs?range=10-13)]

<span data-ttu-id="8f0b3-249">`AdditionalFields`설정 될 수 명시적으로 문자열에 `"FirstName"` 및 `"LastName"`, 하지만 사용 하 여는 [ `nameof` ](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/nameof) 리팩터링 나중에 다음과 같은 연산자를 간소화 합니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-249">`AdditionalFields` could have been set explicitly to the strings `"FirstName"` and `"LastName"`, but using the [`nameof`](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/nameof) operator like this simplifies later refactoring.</span></span>  <span data-ttu-id="8f0b3-250">유효성 검사를 수행 하는 작업 메서드를 다음의 값에 대해 하나씩 두 개의 인수를 받아들여야 `FirstName` 및의 값에 대해 하나씩 `LastName`합니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-250">The action method to perform the validation must then accept two arguments, one for the value of `FirstName` and one for the value of `LastName`.</span></span>


[!code-csharp[Main](validation/sample/UsersController.cs?range=30-39)]

<span data-ttu-id="8f0b3-251">이제 때 사용자가 입력 한 이름과 성, JavaScript:</span><span class="sxs-lookup"><span data-stu-id="8f0b3-251">Now when users enter a first and last name, JavaScript:</span></span>

* <span data-ttu-id="8f0b3-252">해당 쌍의 이름 사용 중인 경우를 확인 하려면 원격 호출을 수행 합니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-252">Makes a remote call to see if that pair of names has been taken.</span></span>
* <span data-ttu-id="8f0b3-253">쌍은 수행 된 경우 오류 메시지가 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-253">If the pair has been taken, an error message is displayed.</span></span> 
* <span data-ttu-id="8f0b3-254">되지 않은 경우, 사용자는 양식을 전송할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-254">If not taken, the user can submit the form.</span></span>

<span data-ttu-id="8f0b3-255">두 개 이상의 추가 필드와 유효성을 검사 해야 하는 경우는 `[Remote]` 특성을 제공 하는 쉼표로 구분 된 목록으로 합니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-255">If you need to validate two or more additional fields with the `[Remote]` attribute, you provide them as a comma-delimited list.</span></span>  <span data-ttu-id="8f0b3-256">예를 들어, 추가 하는 `MiddleName` 모델에 속성이 설정는 `[Remote]` 다음 코드와 같이 특성:</span><span class="sxs-lookup"><span data-stu-id="8f0b3-256">For example, to add a  `MiddleName` property to the model, set the `[Remote]` attribute as shown in the following code:</span></span>

```cs
[Remote(action: "VerifyName", controller: "Users", AdditionalFields = nameof(FirstName) + "," + nameof(LastName))]
public string MiddleName { get; set; }
```

<span data-ttu-id="8f0b3-257">`AdditionalFields`모든 특성 인수가 같은 상수 식 이어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-257">`AdditionalFields`, like all attribute arguments, must be a constant expression.</span></span>  <span data-ttu-id="8f0b3-258">따라서 사용 하면 안는 [문자열 보간](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/interpolated-strings) 호출 또는 [ `string.Join()` ](https://msdn.microsoft.com/en-us/library/system.string.join(v=vs.110).aspx) 초기화 `AdditionalFields`합니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-258">Therefore, you must not use an [interpolated string](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/interpolated-strings) or call [`string.Join()`](https://msdn.microsoft.com/en-us/library/system.string.join(v=vs.110).aspx) to initialize `AdditionalFields`.</span></span> <span data-ttu-id="8f0b3-259">에 추가 하는 모든 추가 필드에는 `[Remote]` 특성을 해당 컨트롤러 동작 메서드에 다른 인수를 추가 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="8f0b3-259">For every additional field that you add to the `[Remote]` attribute, you must add another argument to the corresponding controller action method.</span></span>
