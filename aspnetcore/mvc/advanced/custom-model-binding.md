---
title: ASP.NET Core의 사용자 지정 모델 바인딩
author: ardalis
description: 모델 바인딩을 통해 컨트롤러 작업이 ASP.NET Core의 모델 형식과 함께 직접 작동할 수 있게 하는 방법을 알아봅니다.
manager: wpickett
ms.author: riande
ms.date: 04/10/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/advanced/custom-model-binding
ms.openlocfilehash: a687753083d3b11898e9ff35828780a5ad240854
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/03/2018
---
# <a name="custom-model-binding-in-aspnet-core"></a><span data-ttu-id="0e8d0-103">ASP.NET Core의 사용자 지정 모델 바인딩</span><span class="sxs-lookup"><span data-stu-id="0e8d0-103">Custom Model Binding in ASP.NET Core</span></span>

<span data-ttu-id="0e8d0-104">작성자: [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="0e8d0-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="0e8d0-105">모델 바인딩을 통해 컨트롤러 작업에서 HTTP 요청이 아닌 모델 형식(메서드 인수로 전달된)을 직접 작업할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0e8d0-105">Model binding allows controller actions to work directly with model types (passed in as method arguments), rather than HTTP requests.</span></span> <span data-ttu-id="0e8d0-106">들어오는 요청 데이터와 응용 프로그램 모델 간의 매핑은 모델 바인더를 통해 처리됩니다.</span><span class="sxs-lookup"><span data-stu-id="0e8d0-106">Mapping between incoming request data and application models is handled by model binders.</span></span> <span data-ttu-id="0e8d0-107">개발자는 사용자 지정 모델 바인더를 구현하여 기본 모델 바인딩 기능을 확장할 수 있습니다(일반적으로 개발자가 고유의 공급자를 작성할 필요는 없음).</span><span class="sxs-lookup"><span data-stu-id="0e8d0-107">Developers can extend the built-in model binding functionality by implementing custom model binders (though typically, you don't need to write your own provider).</span></span>

[<span data-ttu-id="0e8d0-108">GitHub에서 샘플 보기 또는 다운로드</span><span class="sxs-lookup"><span data-stu-id="0e8d0-108">View or download sample from GitHub</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/custom-model-binding/)

## <a name="default-model-binder-limitations"></a><span data-ttu-id="0e8d0-109">기본 모델 바인더 제한 사항</span><span class="sxs-lookup"><span data-stu-id="0e8d0-109">Default model binder limitations</span></span>

<span data-ttu-id="0e8d0-110">기본 모델 바인더는 대부분의 공용 .NET Core 데이터 형식을 지원하며 개발자 요구 사항을 대부분 충족합니다.</span><span class="sxs-lookup"><span data-stu-id="0e8d0-110">The default model binders support most of the common .NET Core data types and should meet most developers\` needs.</span></span> <span data-ttu-id="0e8d0-111">요청의 텍스트 기반 입력을 모델 형식에 직접 바인딩할 것으로 예상됩니다.</span><span class="sxs-lookup"><span data-stu-id="0e8d0-111">They expect to bind text-based input from the request directly to model types.</span></span> <span data-ttu-id="0e8d0-112">바인딩하려면 입력을 변환해야 할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0e8d0-112">You might need to transform the input prior to binding it.</span></span> <span data-ttu-id="0e8d0-113">예를 들어 모델 데이터를 조회하는 데 사용할 수 있는 키를 갖고 있는 경우</span><span class="sxs-lookup"><span data-stu-id="0e8d0-113">For example, when you have a key that can be used to look up model data.</span></span> <span data-ttu-id="0e8d0-114">그 키를 기반으로 사용자 지정 모델 바인더를 사용하여 데이터를 가져올 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0e8d0-114">You can use a custom model binder to fetch data based on the key.</span></span>

## <a name="model-binding-review"></a><span data-ttu-id="0e8d0-115">모델 바인딩 검토</span><span class="sxs-lookup"><span data-stu-id="0e8d0-115">Model binding review</span></span>

<span data-ttu-id="0e8d0-116">모델 바인딩은 작업을 수행하는 형식에 대한 특정 정의를 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="0e8d0-116">Model binding uses specific definitions for the types it operates on.</span></span> <span data-ttu-id="0e8d0-117">*단순 형식*은 입력의 단일 문자열에서 변환됩니다.</span><span class="sxs-lookup"><span data-stu-id="0e8d0-117">A *simple type* is converted from a single string in the input.</span></span> <span data-ttu-id="0e8d0-118">*복합 형식*은 여러 입력 값에서 변환됩니다.</span><span class="sxs-lookup"><span data-stu-id="0e8d0-118">A *complex type* is converted from multiple input values.</span></span> <span data-ttu-id="0e8d0-119">프레임워크는 `TypeConverter`의 존재 여부에 따라 차이점을 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="0e8d0-119">The framework determines the difference based on the existence of a `TypeConverter`.</span></span> <span data-ttu-id="0e8d0-120">외부 리소스가 필요 없는 간단한 `string` -> `SomeType` 매핑이 있는 경우 형식 변환기를 만드는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="0e8d0-120">We recommended you create a type converter if you have a simple `string` -> `SomeType` mapping that doesn't require external resources.</span></span>

<span data-ttu-id="0e8d0-121">개발자 고유의 사용자 지정 모델 바인더를 만들기 전에 기존 모델 바인더가 구현되는 방식을 검토하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="0e8d0-121">Before creating your own custom model binder, it's worth reviewing how existing model binders are implemented.</span></span> <span data-ttu-id="0e8d0-122">base64로 인코딩된 문자열을 바이트 배열로 변환하는 데 사용할 수 있는 [ByteArrayModelBinder](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinder)를 고려해 보세요.</span><span class="sxs-lookup"><span data-stu-id="0e8d0-122">Consider the [ByteArrayModelBinder](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinder) which can be used to convert base64-encoded strings into byte arrays.</span></span> <span data-ttu-id="0e8d0-123">바이트 배열은 종종 파일 또는 데이터베이스 BLOB 필드로 저장됩니다.</span><span class="sxs-lookup"><span data-stu-id="0e8d0-123">The byte arrays are often stored as files or database BLOB fields.</span></span>

### <a name="working-with-the-bytearraymodelbinder"></a><span data-ttu-id="0e8d0-124">ByteArrayModelBinder 사용</span><span class="sxs-lookup"><span data-stu-id="0e8d0-124">Working with the ByteArrayModelBinder</span></span>

<span data-ttu-id="0e8d0-125">Base64로 인코딩된 문자열은 이진 데이터를 나타내는 데 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0e8d0-125">Base64-encoded strings can be used to represent binary data.</span></span> <span data-ttu-id="0e8d0-126">예를 들어 다음 이미지를 문자열로 인코딩할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0e8d0-126">For example, the following image can be encoded as a string.</span></span>

<span data-ttu-id="0e8d0-127">![dotnet bot](custom-model-binding/images/bot.png "dotnet bot")</span><span class="sxs-lookup"><span data-stu-id="0e8d0-127">![dotnet bot](custom-model-binding/images/bot.png "dotnet bot")</span></span>

<span data-ttu-id="0e8d0-128">인코딩된 문자열의 일부가 다음 그림에 표시되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0e8d0-128">A small portion of the encoded string is shown in the following image:</span></span>

<span data-ttu-id="0e8d0-129">![인코딩된 dotnet bot](custom-model-binding/images/encoded-bot.png "인코딩된 dotnet bot")</span><span class="sxs-lookup"><span data-stu-id="0e8d0-129">![dotnet bot encoded](custom-model-binding/images/encoded-bot.png "dotnet bot encoded")</span></span>

<span data-ttu-id="0e8d0-130">[샘플의 추가 정보](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/advanced/custom-model-binding/sample/CustomModelBindingSample/README.md)에 제공된 지침에 따라 base64로 인코딩된 문자열을 파일로 변환합니다.</span><span class="sxs-lookup"><span data-stu-id="0e8d0-130">Follow the instructions in the [sample's README](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/advanced/custom-model-binding/sample/CustomModelBindingSample/README.md) to convert the base64-encoded string into a file.</span></span>

<span data-ttu-id="0e8d0-131">ASP.NET Core MVC는 base64로 인코딩된 문자열을 가져온 후 `ByteArrayModelBinder`를 사용하여 바이트 배열로 변환할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0e8d0-131">ASP.NET Core MVC can take a base64-encoded strings and use a `ByteArrayModelBinder` to convert it into a byte array.</span></span> <span data-ttu-id="0e8d0-132">[IModelBinderProvider](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.imodelbinderprovider)를 구현하는 [ByteArrayModelBinderProvider](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinderprovider)는 `byte[]` 인수를 `ByteArrayModelBinder`에 매핑합니다.</span><span class="sxs-lookup"><span data-stu-id="0e8d0-132">The [ByteArrayModelBinderProvider](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinderprovider) which implements [IModelBinderProvider](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.imodelbinderprovider) maps `byte[]` arguments to `ByteArrayModelBinder`:</span></span>

```csharp
public IModelBinder GetBinder(ModelBinderProviderContext context)
{
    if (context == null)
    {
        throw new ArgumentNullException(nameof(context));
    }

    if (context.Metadata.ModelType == typeof(byte[]))
    {
        return new ByteArrayModelBinder();
    }

    return null;
}
```

<span data-ttu-id="0e8d0-133">개발자 고유의 사용자 지정 모델 바인더를 만들 때 고유의 `IModelBinderProvider` 형식을 구현해도 되고 [ModelBinderAttribute](/dotnet/api/microsoft.aspnetcore.mvc.modelbinderattribute)를 사용해도 됩니다.</span><span class="sxs-lookup"><span data-stu-id="0e8d0-133">When creating your own custom model binder, you can implement your own `IModelBinderProvider` type, or use the [ModelBinderAttribute](/dotnet/api/microsoft.aspnetcore.mvc.modelbinderattribute).</span></span>

<span data-ttu-id="0e8d0-134">다음 예제에서는 `ByteArrayModelBinder`를 사용하여 base64로 인코딩된 문자열을 `byte[]`로 변환하고 그 결과를 파일에 저장하는 방법을 보여줍니다.</span><span class="sxs-lookup"><span data-stu-id="0e8d0-134">The following example shows how to use `ByteArrayModelBinder` to convert a base64-encoded string to a `byte[]` and save the result to a file:</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Controllers/ImageController.cs?name=post1&highlight=3)]

<span data-ttu-id="0e8d0-135">[Postman](https://www.getpostman.com/) 같은 도구를 사용하여 base64로 인코딩된 문자열을 이 api 메서드에 게시할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0e8d0-135">You can POST a base64-encoded string to this api method using a tool like [Postman](https://www.getpostman.com/):</span></span>

<span data-ttu-id="0e8d0-136">![postman](custom-model-binding/images/postman.png "postman")</span><span class="sxs-lookup"><span data-stu-id="0e8d0-136">![postman](custom-model-binding/images/postman.png "postman")</span></span>

<span data-ttu-id="0e8d0-137">바인더가 요청 데이터를 적절한 이름의 속성 또는 인수로 바인딩할 수 있는 한, 모델 바인딩은 성공합니다.</span><span class="sxs-lookup"><span data-stu-id="0e8d0-137">As long as the binder can bind request data to appropriately named properties or arguments, model binding will succeed.</span></span> <span data-ttu-id="0e8d0-138">다음 예제에서는 보기 모델에 `ByteArrayModelBinder`를 사용하는 방법을 보여줍니다.</span><span class="sxs-lookup"><span data-stu-id="0e8d0-138">The following example shows how to use `ByteArrayModelBinder` with a view model:</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Controllers/ImageController.cs?name=post2&highlight=2)]

## <a name="custom-model-binder-sample"></a><span data-ttu-id="0e8d0-139">사용자 지정 모델 바인더 샘플</span><span class="sxs-lookup"><span data-stu-id="0e8d0-139">Custom model binder sample</span></span>

<span data-ttu-id="0e8d0-140">이 섹션에서는 다음과 같은 사용자 지정 모델 바인더를 구현하겠습니다.</span><span class="sxs-lookup"><span data-stu-id="0e8d0-140">In this section we'll implement a custom model binder that:</span></span>

- <span data-ttu-id="0e8d0-141">들어오는 요청 데이터를 강력한 형식의 키 인수로 변환합니다.</span><span class="sxs-lookup"><span data-stu-id="0e8d0-141">Converts incoming request data into strongly typed key arguments.</span></span>
- <span data-ttu-id="0e8d0-142">Entity Framework Core를 사용하여 연결된 엔터티를 가져옵니다.</span><span class="sxs-lookup"><span data-stu-id="0e8d0-142">Uses Entity Framework Core to fetch the associated entity.</span></span>
- <span data-ttu-id="0e8d0-143">연결된 엔터티를 작업 메서드에 인수로 전달합니다.</span><span class="sxs-lookup"><span data-stu-id="0e8d0-143">Passes the associated entity as an argument to the action method.</span></span>

<span data-ttu-id="0e8d0-144">다음 샘플은 `Author` 모델에서 `ModelBinder` 특성을 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="0e8d0-144">The following sample uses the `ModelBinder` attribute on the `Author` model:</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Data/Author.cs?highlight=10)]

<span data-ttu-id="0e8d0-145">이전 코드에서 `ModelBinder` 특성은 `Author` 작업 매개 변수를 바인딩하는 데 사용할 `IModelBinder` 형식을 지정합니다.</span><span class="sxs-lookup"><span data-stu-id="0e8d0-145">In the preceding code, the `ModelBinder` attribute specifies the type of `IModelBinder` that should be used to bind `Author` action parameters.</span></span> 

<span data-ttu-id="0e8d0-146">`AuthorEntityBinder`는 Entity Framework Core 및 `authorId`를 사용하여 데이터 원본에서 엔터티를 가져와 `Author` 매개 변수를 바인딩하는 데 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="0e8d0-146">The `AuthorEntityBinder` is used to bind an `Author` parameter by fetching the entity from a data source using Entity Framework Core and an `authorId`:</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Binders/AuthorEntityBinder.cs?name=demo)]

<span data-ttu-id="0e8d0-147">다음 코드는 작업 메서드에 `AuthorEntityBinder`를 사용하는 방법을 보여줍니다.</span><span class="sxs-lookup"><span data-stu-id="0e8d0-147">The following code shows how to use the `AuthorEntityBinder` in an action method:</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Controllers/BoundAuthorsController.cs?name=demo2&highlight=2)]

<span data-ttu-id="0e8d0-148">`ModelBinder` 특성은 기본 규칙을 사용하지 않는 매개 변수에 `AuthorEntityBinder`를 적용하는 데 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0e8d0-148">The `ModelBinder` attribute can be used to apply the `AuthorEntityBinder` to parameters that don't use default conventions:</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Controllers/BoundAuthorsController.cs?name=demo1&highlight=2)]

<span data-ttu-id="0e8d0-149">이 예에서는 인수 이름이 기본 `authorId`가 아니기 때문에 `ModelBinder` 특성을 사용하여 매개 변수에 지정됩니다.</span><span class="sxs-lookup"><span data-stu-id="0e8d0-149">In this example, since the name of the argument isn't the default `authorId`, it's specified on the parameter using `ModelBinder` attribute.</span></span> <span data-ttu-id="0e8d0-150">컨트롤러와 작업 메서드는 작업 메서드에서 엔터티를 조회하는 것에 비해 간단합니다.</span><span class="sxs-lookup"><span data-stu-id="0e8d0-150">Note that both the controller and action method are simplified compared to looking up the entity in the action method.</span></span> <span data-ttu-id="0e8d0-151">Entity Framework Core를 사용하여 작성자를 가져오는 논리는 모델 바인더로 이동되었습니다.</span><span class="sxs-lookup"><span data-stu-id="0e8d0-151">The logic to fetch the author using Entity Framework Core is moved to the model binder.</span></span> <span data-ttu-id="0e8d0-152">작성자 모델에 바인딩하는 메서드가 여러 개 있는 경우 상당히 간소화될 수 있으며 [DRY 원칙](http://deviq.com/don-t-repeat-yourself/)을 준수하는 데 도움이 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0e8d0-152">This can be considerable simplification when you have several methods that bind to the author model, and can help you to follow the [DRY principle](http://deviq.com/don-t-repeat-yourself/).</span></span>

<span data-ttu-id="0e8d0-153">개별 모델 속성(viewmodel처럼) 또는 작업 메서드 매개 변수에 `ModelBinder` 특성을 적용하여 그 형식 또는 작업에만 해당하는 특정 모델 바인더 또는 모델을 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0e8d0-153">You can apply the `ModelBinder` attribute to individual model properties (such as on a viewmodel) or to action method parameters to specify a certain model binder or model name for just that type or action.</span></span>

### <a name="implementing-a-modelbinderprovider"></a><span data-ttu-id="0e8d0-154">ModelBinderProvider 구현</span><span class="sxs-lookup"><span data-stu-id="0e8d0-154">Implementing a ModelBinderProvider</span></span>

<span data-ttu-id="0e8d0-155">특성을 적용하는 대신, `IModelBinderProvider`를 구현할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0e8d0-155">Instead of applying an attribute, you can implement `IModelBinderProvider`.</span></span> <span data-ttu-id="0e8d0-156">기본 제공 프레임워크 바인더는 이 방식으로 구현됩니다.</span><span class="sxs-lookup"><span data-stu-id="0e8d0-156">This is how the built-in framework binders are implemented.</span></span> <span data-ttu-id="0e8d0-157">바인더가 작동하는 형식을 지정할 때 바인더가 허용하는 입력이 **아니라** 바인더가 생성하는 인수 형식을 지정합니다.</span><span class="sxs-lookup"><span data-stu-id="0e8d0-157">When you specify the type your binder operates on, you specify the type of argument it produces, **not** the input your binder accepts.</span></span> <span data-ttu-id="0e8d0-158">다음 바인더 공급자는 `AuthorEntityBinder`를 작업합니다.</span><span class="sxs-lookup"><span data-stu-id="0e8d0-158">The following binder provider works with the `AuthorEntityBinder`.</span></span> <span data-ttu-id="0e8d0-159">공급자의 MVC 컬렉션에 추가할 때 `Author` 또는 `Author` 형식 매개 변수에서 `ModelBinder` 특성을 사용할 필요가 없습니다.</span><span class="sxs-lookup"><span data-stu-id="0e8d0-159">When it's added to MVC's collection of providers, you don't need to use the `ModelBinder` attribute on `Author` or `Author` typed parameters.</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Binders/AuthorEntityBinderProvider.cs?highlight=17-20)]

> <span data-ttu-id="0e8d0-160">참고: 앞의 코드는 `BinderTypeModelBinder`를 반환합니다.</span><span class="sxs-lookup"><span data-stu-id="0e8d0-160">Note: The preceding code returns a `BinderTypeModelBinder`.</span></span> <span data-ttu-id="0e8d0-161">`BinderTypeModelBinder`는 모델 바인더에 대한 팩터리 역할을 하며 DI(종속성 주입)를 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="0e8d0-161">`BinderTypeModelBinder` acts as a factory for model binders and provides dependency injection (DI).</span></span> <span data-ttu-id="0e8d0-162">`AuthorEntityBinder`는 EF Core에 액세스하려면 DI가 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="0e8d0-162">The `AuthorEntityBinder` requires DI to access EF Core.</span></span> <span data-ttu-id="0e8d0-163">모델 바인더에서 DI의 서비스를 요구하는 경우 `BinderTypeModelBinder`를 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="0e8d0-163">Use `BinderTypeModelBinder` if your model binder requires services from DI.</span></span>

<span data-ttu-id="0e8d0-164">사용자 지정 모델 바인더 공급자를 사용하려면 `ConfigureServices`에서 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="0e8d0-164">To use a custom model binder provider, add it in `ConfigureServices`:</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Startup.cs?name=callout&highlight=5-9)]

<span data-ttu-id="0e8d0-165">모델 바인더를 평가할 때 공급자 컬렉션은 순서대로 검사됩니다.</span><span class="sxs-lookup"><span data-stu-id="0e8d0-165">When evaluating model binders, the collection of providers is examined in order.</span></span> <span data-ttu-id="0e8d0-166">바인더를 반환하는 첫 번째 공급자가 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="0e8d0-166">The first provider that returns a binder is used.</span></span>

<span data-ttu-id="0e8d0-167">다음 이미지는 디버거의 기본 모델 바인더를 보여줍니다.</span><span class="sxs-lookup"><span data-stu-id="0e8d0-167">The following image shows the default model binders from the debugger.</span></span>

<span data-ttu-id="0e8d0-168">![기본 모델 바인더](custom-model-binding/images/default-model-binders.png "기본 모델 바인더")</span><span class="sxs-lookup"><span data-stu-id="0e8d0-168">![default model binders](custom-model-binding/images/default-model-binders.png "default model binders")</span></span>

<span data-ttu-id="0e8d0-169">컬렉션 끝에 공급자를 추가하면 사용자 지정 바인더가 기회를 얻기도 전에 기본 제공 모델 바인더가 호출될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0e8d0-169">Adding your provider to the end of the collection may result in a built-in model binder being called before your custom binder has a chance.</span></span> <span data-ttu-id="0e8d0-170">이 예제에서는 `Author` 작업 인수에 사용되도록 사용자 지정 공급자를 컬렉션의 시작 부분에 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="0e8d0-170">In this example, the custom provider is added to the beginning of the collection to ensure it's used for `Author` action arguments.</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Startup.cs?name=callout&highlight=5-9)]

## <a name="recommendations-and-best-practices"></a><span data-ttu-id="0e8d0-171">권장 사항 및 모범 사례</span><span class="sxs-lookup"><span data-stu-id="0e8d0-171">Recommendations and best practices</span></span>

<span data-ttu-id="0e8d0-172">사용자 지정 모델 바인더:</span><span class="sxs-lookup"><span data-stu-id="0e8d0-172">Custom model binders:</span></span>
- <span data-ttu-id="0e8d0-173">상태 코드를 설정하거나 결과를 반환하려고 시도하면 안 됩니다(예를 들어 404 찾을 수 없음).</span><span class="sxs-lookup"><span data-stu-id="0e8d0-173">Shouldn't attempt to set status codes or return results (for example, 404 Not Found).</span></span> <span data-ttu-id="0e8d0-174">모델 바인딩이 실패하면 [작업 필터](xref:mvc/controllers/filters) 또는 작업 메서드 자체의 논리에서 오류를 처리해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="0e8d0-174">If model binding fails, an [action filter](xref:mvc/controllers/filters) or logic within the action method itself should handle the failure.</span></span>
- <span data-ttu-id="0e8d0-175">작업 메서드에서 반복 코드와 교차 편집 문제를 제거하는 데 가장 유용합니다.</span><span class="sxs-lookup"><span data-stu-id="0e8d0-175">Are most useful for eliminating repetitive code and cross-cutting concerns from action methods.</span></span>
- <span data-ttu-id="0e8d0-176">일반적으로 문자열을 사용자 지정 형식으로 변환하는 데 사용되지 않습니다. [`TypeConverter`](/dotnet/api/system.componentmodel.typeconverter)가 더 좋은 옵션입니다.</span><span class="sxs-lookup"><span data-stu-id="0e8d0-176">Typically shouldn't be used to convert a string into a custom type, a [`TypeConverter`](/dotnet/api/system.componentmodel.typeconverter) is usually a better option.</span></span>
