---
title: "폼에 ASP.NET Core 태그 도우미"
author: rick-anderson
description: "태그 도우미 폼과 함께 사용 하는 기본 제공을 설명 합니다."
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/working-with-forms
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 9fbe2c5cb495aabee0e1f0bdb3871641efa03599
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/19/2018
---
# <a name="introduction-to-using-tag-helpers-in-forms-in-aspnet-core"></a><span data-ttu-id="d47dc-103">태그 도우미를 사용 하 여 폼에 ASP.NET Core 소개</span><span class="sxs-lookup"><span data-stu-id="d47dc-103">Introduction to using tag helpers in forms in ASP.NET Core</span></span>

<span data-ttu-id="d47dc-104">여 [Rick Anderson](https://twitter.com/RickAndMSFT), [Dave Paquette](https://twitter.com/Dave_Paquette), 및 [Jerrie Pelser](https://github.com/jerriep)</span><span class="sxs-lookup"><span data-stu-id="d47dc-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Dave Paquette](https://twitter.com/Dave_Paquette), and [Jerrie Pelser](https://github.com/jerriep)</span></span>

<span data-ttu-id="d47dc-105">이 문서에 폼 및 폼에 일반적으로 사용 되는 HTML 요소 작업을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="d47dc-105">This document demonstrates working with Forms and the HTML elements commonly used on a Form.</span></span> <span data-ttu-id="d47dc-106">HTML [양식](https://www.w3.org/TR/html401/interact/forms.html) 요소는 서버에 데이터를 다시 게시 하는 기본 메커니즘 웹 앱 사용을 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="d47dc-106">The HTML [Form](https://www.w3.org/TR/html401/interact/forms.html) element provides the primary mechanism web apps use to post back data to the server.</span></span> <span data-ttu-id="d47dc-107">이 문서의 대부분 설명 [태그 도우미](tag-helpers/intro.md) 있고 어떻게 수 생산적으로 강력한 HTML 폼을 만들 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d47dc-107">Most of this document describes [Tag Helpers](tag-helpers/intro.md) and how they can help you productively create robust HTML forms.</span></span> <span data-ttu-id="d47dc-108">참조 [태그 도우미 소개](tag-helpers/intro.md) 이 문서를 읽기 전에 합니다.</span><span class="sxs-lookup"><span data-stu-id="d47dc-108">We recommend you read [Introduction to Tag Helpers](tag-helpers/intro.md) before you read this document.</span></span>

<span data-ttu-id="d47dc-109">대부분의 경우에서 HTML 도우미 특정 태그 도우미에는 다른 방법은 제공 하지만 태그 도우미는 HTML 도우미를 대체 하지 않고 각 HTML 도우미에 대 한 태그 도우미 없기 인식 하는 것이 중요 합니다.</span><span class="sxs-lookup"><span data-stu-id="d47dc-109">In many cases, HTML Helpers provide an alternative approach to a specific Tag Helper, but it's important to recognize that Tag Helpers do not replace HTML Helpers and there is not a Tag Helper for each HTML Helper.</span></span> <span data-ttu-id="d47dc-110">존재 하는 HTML 도우미 대신 경우 다룹니다.</span><span class="sxs-lookup"><span data-stu-id="d47dc-110">When an HTML Helper alternative exists, it is mentioned.</span></span>

<a name="my-asp-route-param-ref-label"></a>

## <a name="the-form-tag-helper"></a><span data-ttu-id="d47dc-111">Form 태그 도우미</span><span class="sxs-lookup"><span data-stu-id="d47dc-111">The Form Tag Helper</span></span>

<span data-ttu-id="d47dc-112">[양식](https://www.w3.org/TR/html401/interact/forms.html) 도우미 태그:</span><span class="sxs-lookup"><span data-stu-id="d47dc-112">The [Form](https://www.w3.org/TR/html401/interact/forms.html) Tag Helper:</span></span>

* <span data-ttu-id="d47dc-113">HTML을 생성 [ \<양식 >](https://www.w3.org/TR/html401/interact/forms.html) `action` MVC 컨트롤러 동작 또는 명명 된 경로 대 한 특성 값</span><span class="sxs-lookup"><span data-stu-id="d47dc-113">Generates the HTML [\<FORM>](https://www.w3.org/TR/html401/interact/forms.html) `action` attribute value for a MVC controller action or named route</span></span>

* <span data-ttu-id="d47dc-114">에서는 오류가 발생 하는 숨겨진 [요청 확인 토큰](https://docs.microsoft.com/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) 교차 사이트 요청 위조를 방지 하기 위해 (함께 사용할 경우의 `[ValidateAntiForgeryToken]` HTTP Post 작업 메서드에 특성)</span><span class="sxs-lookup"><span data-stu-id="d47dc-114">Generates a hidden [Request Verification Token](https://docs.microsoft.com/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) to prevent cross-site request forgery (when used with the `[ValidateAntiForgeryToken]` attribute in the HTTP Post action method)</span></span>

* <span data-ttu-id="d47dc-115">제공 된 `asp-route-<Parameter Name>` 특성, 여기서 `<Parameter Name>` 경로 값에 추가 됩니다.</span><span class="sxs-lookup"><span data-stu-id="d47dc-115">Provides the `asp-route-<Parameter Name>` attribute, where `<Parameter Name>` is added to the route values.</span></span> <span data-ttu-id="d47dc-116">`routeValues` 매개 변수를 `Html.BeginForm` 및 `Html.BeginRouteForm` 유사한 기능을 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="d47dc-116">The  `routeValues` parameters to `Html.BeginForm` and `Html.BeginRouteForm` provide similar functionality.</span></span>

* <span data-ttu-id="d47dc-117">에 HTML 도우미 대신 `Html.BeginForm` 및`Html.BeginRouteForm`</span><span class="sxs-lookup"><span data-stu-id="d47dc-117">Has an HTML Helper alternative `Html.BeginForm` and `Html.BeginRouteForm`</span></span>

<span data-ttu-id="d47dc-118">예제:</span><span class="sxs-lookup"><span data-stu-id="d47dc-118">Sample:</span></span>

[!code-HTML[Main](working-with-forms/sample/final/Views/Demo/RegisterFormOnly.cshtml)]

<span data-ttu-id="d47dc-119">위의 Form 태그 도우미에서는 다음 HTML을 생성합니다.</span><span class="sxs-lookup"><span data-stu-id="d47dc-119">The Form Tag Helper above generates the following HTML:</span></span>

```HTML
<form method="post" action="/Demo/Register">
     <!-- Input and Submit elements -->
     <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
    </form>
```

<span data-ttu-id="d47dc-120">MVC 런타임에서 생성 된 `action` Form 태그 도우미 특성에서 특성 값 `asp-controller` 및 `asp-action`합니다.</span><span class="sxs-lookup"><span data-stu-id="d47dc-120">The MVC runtime generates the `action` attribute value from the Form Tag Helper attributes `asp-controller` and `asp-action`.</span></span> <span data-ttu-id="d47dc-121">Form 태그 도우미도 숨겨진 생성 [요청 확인 토큰](https://docs.microsoft.com/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) 교차 사이트 요청 위조를 방지 하기 위해 (함께 사용할 경우의 `[ValidateAntiForgeryToken]` HTTP Post 작업 메서드에 특성).</span><span class="sxs-lookup"><span data-stu-id="d47dc-121">The Form Tag Helper also generates a hidden [Request Verification Token](https://docs.microsoft.com/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) to prevent cross-site request forgery (when used with the `[ValidateAntiForgeryToken]` attribute in the HTTP Post action method).</span></span> <span data-ttu-id="d47dc-122">순수 HTML 폼 교차 사이트 요청 위조 로부터 보호 하는 것은 어려운, Form 태그 도우미를이 서비스를 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="d47dc-122">Protecting a pure HTML Form from cross-site request forgery is difficult, the Form Tag Helper provides this service for you.</span></span>

### <a name="using-a-named-route"></a><span data-ttu-id="d47dc-123">명명된 된 경로 사용 하 여</span><span class="sxs-lookup"><span data-stu-id="d47dc-123">Using a named route</span></span>

<span data-ttu-id="d47dc-124">`asp-route` 태그 도우미 특성에 대 한 HTML 태그를 생성할 수도 `action` 특성입니다.</span><span class="sxs-lookup"><span data-stu-id="d47dc-124">The `asp-route` Tag Helper attribute can also generate markup for the HTML `action` attribute.</span></span> <span data-ttu-id="d47dc-125">사용 하 여 앱은 [경로](../../fundamentals/routing.md) 라는 `register` 등록 페이지에 다음 태그를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d47dc-125">An app with a [route](../../fundamentals/routing.md)  named `register` could use the following markup for the registration page:</span></span>

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterRoute.cshtml)]

<span data-ttu-id="d47dc-126">보기 중 많은 *뷰/계정* 폴더 (새 웹 앱을 만들 때 생성 *개별 사용자 계정*) 포함 된 [asp-경로-returnurl](https://docs.microsoft.com/aspnet/core/mvc/views/working-with-forms) 특성:</span><span class="sxs-lookup"><span data-stu-id="d47dc-126">Many of the views in the *Views/Account* folder (generated when you create a new web app with *Individual User Accounts*) contain the [asp-route-returnurl](https://docs.microsoft.com/aspnet/core/mvc/views/working-with-forms) attribute:</span></span>

```cshtml
<form asp-controller="Account" asp-action="Login"
     asp-route-returnurl="@ViewData["ReturnUrl"]"
     method="post" class="form-horizontal" role="form">
```

>[!NOTE]
><span data-ttu-id="d47dc-127">기본 제공된 템플릿과 함께 `returnUrl` 만 자동으로 채워집니다 권한 있는 리소스에 액세스 하려고 하지만 되지 인증 하거나 때 권한을 부여 합니다.</span><span class="sxs-lookup"><span data-stu-id="d47dc-127">With the built in templates, `returnUrl` is only populated automatically when you try to access an authorized resource but are not authenticated or authorized.</span></span> <span data-ttu-id="d47dc-128">무단된 액세스를 시도 하면 보안 미들웨어 리디렉션됩니다 사용 하 여 로그인 페이지에는 `returnUrl` 설정 합니다.</span><span class="sxs-lookup"><span data-stu-id="d47dc-128">When you attempt an unauthorized access, the security middleware redirects you to the login page with the `returnUrl` set.</span></span>

## <a name="the-input-tag-helper"></a><span data-ttu-id="d47dc-129">입력된 태그 도우미</span><span class="sxs-lookup"><span data-stu-id="d47dc-129">The Input Tag Helper</span></span>

<span data-ttu-id="d47dc-130">입력 태그 도우미 바인딩하는 HTML [ \<입력 >](https://www.w3.org/wiki/HTML/Elements/input) 요소 razor 뷰의 모델 식입니다.</span><span class="sxs-lookup"><span data-stu-id="d47dc-130">The Input Tag Helper binds an HTML [\<input>](https://www.w3.org/wiki/HTML/Elements/input) element to a model expression in your razor view.</span></span>

<span data-ttu-id="d47dc-131">구문:</span><span class="sxs-lookup"><span data-stu-id="d47dc-131">Syntax:</span></span>

```HTML
<input asp-for="<Expression Name>" />
```

<span data-ttu-id="d47dc-132">입력된 태그 도우미:</span><span class="sxs-lookup"><span data-stu-id="d47dc-132">The Input Tag Helper:</span></span>

* <span data-ttu-id="d47dc-133">생성 된 `id` 및 `name` HTML 특성에 지정 된 식 이름에 대 한는 `asp-for` 특성입니다.</span><span class="sxs-lookup"><span data-stu-id="d47dc-133">Generates the `id` and `name` HTML attributes for the expression name specified in the `asp-for` attribute.</span></span> <span data-ttu-id="d47dc-134">`asp-for="Property1.Property2"`는 `m => m.Property1.Property2`와 같습니다.</span><span class="sxs-lookup"><span data-stu-id="d47dc-134">`asp-for="Property1.Property2"` is equivalent to `m => m.Property1.Property2`.</span></span> <span data-ttu-id="d47dc-135">식의 이름에 사용 되는 것은 `asp-for` 특성 값입니다.</span><span class="sxs-lookup"><span data-stu-id="d47dc-135">The name of the expression is what is used for the `asp-for` attribute value.</span></span> <span data-ttu-id="d47dc-136">참조는 [식 이름은](#expression-names) 추가 정보에 대 한 섹션.</span><span class="sxs-lookup"><span data-stu-id="d47dc-136">See the [Expression names](#expression-names) section for additional information.</span></span>

* <span data-ttu-id="d47dc-137">HTML 설정 `type` 특성 값에는 모델 유형에 따라 및 [데이터 주석을](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) 모델 속성에 적용 된 특성</span><span class="sxs-lookup"><span data-stu-id="d47dc-137">Sets the HTML `type` attribute value based on the model type and  [data annotation](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) attributes applied to the model property</span></span>

* <span data-ttu-id="d47dc-138">HTML을 덮어쓰지 것입니다 `type` 특성 값이 지정 된 경우</span><span class="sxs-lookup"><span data-stu-id="d47dc-138">Will not overwrite the HTML `type` attribute value when one is specified</span></span>

* <span data-ttu-id="d47dc-139">생성 [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) 에서 유효성 검사 특성 [데이터 주석을](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) 모델 속성에 적용 되는 특성</span><span class="sxs-lookup"><span data-stu-id="d47dc-139">Generates [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5)  validation attributes from [data annotation](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) attributes applied to model properties</span></span>

* <span data-ttu-id="d47dc-140">에 HTML 도우미 기능이와 겹칠 `Html.TextBoxFor` 및 `Html.EditorFor`합니다.</span><span class="sxs-lookup"><span data-stu-id="d47dc-140">Has an HTML Helper feature overlap with `Html.TextBoxFor` and `Html.EditorFor`.</span></span> <span data-ttu-id="d47dc-141">참조는 **입력 태그 도우미에 대 한 HTML 도우미 대안** 자세한 내용은 섹션.</span><span class="sxs-lookup"><span data-stu-id="d47dc-141">See the **HTML Helper alternatives to Input Tag Helper** section for details.</span></span>

* <span data-ttu-id="d47dc-142">강력한 형식 지정을 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="d47dc-142">Provides strong typing.</span></span> <span data-ttu-id="d47dc-143">속성 변경의 이름과 태그 도우미를 업데이트 하지에 오류를 다음과 같이 얻게 됩니다.</span><span class="sxs-lookup"><span data-stu-id="d47dc-143">If the name of the property changes and you don't update the Tag Helper you'll get an error similar to the following:</span></span>

```HTML
An error occurred during the compilation of a resource required to process
this request. Please review the following specific error details and modify
your source code appropriately.

Type expected
 'RegisterViewModel' does not contain a definition for 'Email' and no
 extension method 'Email' accepting a first argument of type 'RegisterViewModel'
 could be found (are you missing a using directive or an assembly reference?)
```

<span data-ttu-id="d47dc-144">`Input` 설정 하는 HTML 태그 도우미 `type` 특성의.NET 유형을 기반으로 합니다.</span><span class="sxs-lookup"><span data-stu-id="d47dc-144">The `Input` Tag Helper sets the HTML `type` attribute based on the .NET type.</span></span> <span data-ttu-id="d47dc-145">다음 표에서 몇 가지 일반적인.NET 형식과 생성 되는 HTML 형식 (일부.NET 유형 설명 되어 있음)를 나열 합니다.</span><span class="sxs-lookup"><span data-stu-id="d47dc-145">The following table lists some common .NET types and generated HTML type (not every .NET type is listed).</span></span>

|<span data-ttu-id="d47dc-146">.NET 형식</span><span class="sxs-lookup"><span data-stu-id="d47dc-146">.NET type</span></span>|<span data-ttu-id="d47dc-147">입력된 형식</span><span class="sxs-lookup"><span data-stu-id="d47dc-147">Input Type</span></span>|
|---|---|
|<span data-ttu-id="d47dc-148">Bool</span><span class="sxs-lookup"><span data-stu-id="d47dc-148">Bool</span></span>|<span data-ttu-id="d47dc-149">type=”checkbox”</span><span class="sxs-lookup"><span data-stu-id="d47dc-149">type=”checkbox”</span></span>|
|<span data-ttu-id="d47dc-150">문자열</span><span class="sxs-lookup"><span data-stu-id="d47dc-150">String</span></span>|<span data-ttu-id="d47dc-151">type=”text”</span><span class="sxs-lookup"><span data-stu-id="d47dc-151">type=”text”</span></span>|
|<span data-ttu-id="d47dc-152">DateTime</span><span class="sxs-lookup"><span data-stu-id="d47dc-152">DateTime</span></span>|<span data-ttu-id="d47dc-153">type=”datetime”</span><span class="sxs-lookup"><span data-stu-id="d47dc-153">type=”datetime”</span></span>|
|<span data-ttu-id="d47dc-154">Byte</span><span class="sxs-lookup"><span data-stu-id="d47dc-154">Byte</span></span>|<span data-ttu-id="d47dc-155">type=”number”</span><span class="sxs-lookup"><span data-stu-id="d47dc-155">type=”number”</span></span>|
|<span data-ttu-id="d47dc-156">Int</span><span class="sxs-lookup"><span data-stu-id="d47dc-156">Int</span></span>|<span data-ttu-id="d47dc-157">type=”number”</span><span class="sxs-lookup"><span data-stu-id="d47dc-157">type=”number”</span></span>|
|<span data-ttu-id="d47dc-158">Single, Double</span><span class="sxs-lookup"><span data-stu-id="d47dc-158">Single, Double</span></span>|<span data-ttu-id="d47dc-159">type=”number”</span><span class="sxs-lookup"><span data-stu-id="d47dc-159">type=”number”</span></span>|


<span data-ttu-id="d47dc-160">다음 표에서 몇 가지 일반적인 [데이터 주석](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) 입력된 태그 도우미 (모든 유효성 검사 특성 설명 되어 있음)는 특정 입력된 형식에 매핑되는 특성:</span><span class="sxs-lookup"><span data-stu-id="d47dc-160">The following table shows some common [data annotations](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) attributes that the input tag helper will map to specific input types (not every validation attribute is listed):</span></span>


|<span data-ttu-id="d47dc-161">특성</span><span class="sxs-lookup"><span data-stu-id="d47dc-161">Attribute</span></span>|<span data-ttu-id="d47dc-162">입력된 형식</span><span class="sxs-lookup"><span data-stu-id="d47dc-162">Input Type</span></span>|
|---|---|
|<span data-ttu-id="d47dc-163">[EmailAddress]</span><span class="sxs-lookup"><span data-stu-id="d47dc-163">[EmailAddress]</span></span>|<span data-ttu-id="d47dc-164">type=”email”</span><span class="sxs-lookup"><span data-stu-id="d47dc-164">type=”email”</span></span>|
|<span data-ttu-id="d47dc-165">[Url]</span><span class="sxs-lookup"><span data-stu-id="d47dc-165">[Url]</span></span>|<span data-ttu-id="d47dc-166">type=”url”</span><span class="sxs-lookup"><span data-stu-id="d47dc-166">type=”url”</span></span>|
|<span data-ttu-id="d47dc-167">[HiddenInput]</span><span class="sxs-lookup"><span data-stu-id="d47dc-167">[HiddenInput]</span></span>|<span data-ttu-id="d47dc-168">type=”hidden”</span><span class="sxs-lookup"><span data-stu-id="d47dc-168">type=”hidden”</span></span>|
|<span data-ttu-id="d47dc-169">[Phone]</span><span class="sxs-lookup"><span data-stu-id="d47dc-169">[Phone]</span></span>|<span data-ttu-id="d47dc-170">type=”tel”</span><span class="sxs-lookup"><span data-stu-id="d47dc-170">type=”tel”</span></span>|
|<span data-ttu-id="d47dc-171">[DataType(DataType.Password)]</span><span class="sxs-lookup"><span data-stu-id="d47dc-171">[DataType(DataType.Password)]</span></span>| <span data-ttu-id="d47dc-172">type=”password”</span><span class="sxs-lookup"><span data-stu-id="d47dc-172">type=”password”</span></span>|
|<span data-ttu-id="d47dc-173">[DataType(DataType.Date)]</span><span class="sxs-lookup"><span data-stu-id="d47dc-173">[DataType(DataType.Date)]</span></span>| <span data-ttu-id="d47dc-174">type=”date”</span><span class="sxs-lookup"><span data-stu-id="d47dc-174">type=”date”</span></span>|
|<span data-ttu-id="d47dc-175">[DataType(DataType.Time)]</span><span class="sxs-lookup"><span data-stu-id="d47dc-175">[DataType(DataType.Time)]</span></span>| <span data-ttu-id="d47dc-176">type=”time”</span><span class="sxs-lookup"><span data-stu-id="d47dc-176">type=”time”</span></span>|


<span data-ttu-id="d47dc-177">예제:</span><span class="sxs-lookup"><span data-stu-id="d47dc-177">Sample:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/RegisterViewModel.cs)]

[!code-HTML[Main](working-with-forms/sample/final/Views/Demo/RegisterInput.cshtml)]

<span data-ttu-id="d47dc-178">위의 코드에서는 다음 HTML을 생성합니다.</span><span class="sxs-lookup"><span data-stu-id="d47dc-178">The code above generates the following HTML:</span></span>

```HTML
  <form method="post" action="/Demo/RegisterInput">
       Email:
       <input type="email" data-val="true"
              data-val-email="The Email Address field is not a valid e-mail address."
              data-val-required="The Email Address field is required."
              id="Email" name="Email" value="" /> <br>
       Password:
       <input type="password" data-val="true"
              data-val-required="The Password field is required."
              id="Password" name="Password" /><br>
       <button type="submit">Register</button>
     <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
   </form>
```

<span data-ttu-id="d47dc-179">에 적용할 데이터 주석은 `Email` 및 `Password` 속성은 모델에서 메타 데이터를 생성 합니다.</span><span class="sxs-lookup"><span data-stu-id="d47dc-179">The data annotations applied to the `Email` and `Password` properties generate metadata on the model.</span></span> <span data-ttu-id="d47dc-180">모델 메타 데이터를 사용 하 고 생성 하는 입력 태그 도우미 [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) `data-val-*` 특성 (참조 [모델 유효성 검사](../models/validation.md)).</span><span class="sxs-lookup"><span data-stu-id="d47dc-180">The Input Tag Helper consumes the model metadata and produces [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) `data-val-*` attributes (see [Model Validation](../models/validation.md)).</span></span> <span data-ttu-id="d47dc-181">이러한 특성에는 입력된 필드를 연결할 유효성 검사기에 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="d47dc-181">These attributes describe the validators to attach to the input fields.</span></span> <span data-ttu-id="d47dc-182">이렇게 하면 비 가시적인 HTML5 및 [jQuery](https://jquery.com/) 유효성 검사 합니다.</span><span class="sxs-lookup"><span data-stu-id="d47dc-182">This provides unobtrusive HTML5 and [jQuery](https://jquery.com/) validation.</span></span> <span data-ttu-id="d47dc-183">비 가시적인 특성 형식에는 `data-val-rule="Error Message"`, 여기서 규칙은 유효성 검사 규칙의 이름 (예: `data-val-required`, `data-val-email`, `data-val-maxlength`등.) 오류 메시지가 특성에서 제공 하는 경우에 대 한 값으로 표시 됩니다는 `data-val-rule` 특성입니다.</span><span class="sxs-lookup"><span data-stu-id="d47dc-183">The unobtrusive attributes have the format `data-val-rule="Error Message"`, where rule is the name of the validation rule (such as `data-val-required`, `data-val-email`, `data-val-maxlength`, etc.) If an error message is provided in the attribute, it is displayed as the value for the `data-val-rule` attribute.</span></span> <span data-ttu-id="d47dc-184">폼의 특성은 또한 `data-val-ruleName-argumentName="argumentValue"` 예를 들어,는 규칙에 대 한 추가 세부 정보를 제공 하는 `data-val-maxlength-max="1024"` 합니다.</span><span class="sxs-lookup"><span data-stu-id="d47dc-184">There are also attributes of the form `data-val-ruleName-argumentName="argumentValue"` that provide additional details about the rule, for example, `data-val-maxlength-max="1024"` .</span></span>

### <a name="html-helper-alternatives-to-input-tag-helper"></a><span data-ttu-id="d47dc-185">입력 태그 도우미에 대 한 HTML 도우미 대안</span><span class="sxs-lookup"><span data-stu-id="d47dc-185">HTML Helper alternatives to Input Tag Helper</span></span>

<span data-ttu-id="d47dc-186">`Html.TextBox``Html.TextBoxFor`, `Html.Editor` 및 `Html.EditorFor` 겹치는 입력 태그 도우미와 기능입니다.</span><span class="sxs-lookup"><span data-stu-id="d47dc-186">`Html.TextBox`, `Html.TextBoxFor`, `Html.Editor` and `Html.EditorFor` have overlapping features with the Input Tag Helper.</span></span> <span data-ttu-id="d47dc-187">입력 태그 도우미에서 자동으로 설정 된 `type` ; 특성 `Html.TextBox` 및 `Html.TextBoxFor` 되지 것입니다.</span><span class="sxs-lookup"><span data-stu-id="d47dc-187">The Input Tag Helper will automatically set the `type` attribute; `Html.TextBox` and `Html.TextBoxFor` will not.</span></span> <span data-ttu-id="d47dc-188">`Html.Editor`및 `Html.EditorFor` 컬렉션, 복잡 한 개체 및 템플릿; 처리 된 입력 태그 도우미는 그렇지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="d47dc-188">`Html.Editor` and `Html.EditorFor` handle collections, complex objects and templates; the Input Tag Helper does not.</span></span> <span data-ttu-id="d47dc-189">입력 태그 도우미 `Html.EditorFor` 및 `Html.TextBoxFor` 은 강력한 형식 (람다 식 사용). `Html.TextBox` 및 `Html.Editor` 없습니다 (식 이름 사용).</span><span class="sxs-lookup"><span data-stu-id="d47dc-189">The Input Tag Helper, `Html.EditorFor`  and  `Html.TextBoxFor` are strongly typed (they use lambda expressions); `Html.TextBox` and `Html.Editor` are not (they use expression names).</span></span>

### <a name="htmlattributes"></a><span data-ttu-id="d47dc-190">HtmlAttributes</span><span class="sxs-lookup"><span data-stu-id="d47dc-190">HtmlAttributes</span></span>

<span data-ttu-id="d47dc-191">`@Html.Editor()`및 `@Html.EditorFor()` 특수 `ViewDataDictionary` 라는 항목 `htmlAttributes` 자신의 기본 서식 파일을 실행 합니다.</span><span class="sxs-lookup"><span data-stu-id="d47dc-191">`@Html.Editor()` and `@Html.EditorFor()` use a special `ViewDataDictionary` entry named `htmlAttributes` when executing their default templates.</span></span> <span data-ttu-id="d47dc-192">이 문제는 사용 하 여 확장 필요에 따라 `additionalViewData` 매개 변수입니다.</span><span class="sxs-lookup"><span data-stu-id="d47dc-192">This behavior is optionally augmented using `additionalViewData` parameters.</span></span> <span data-ttu-id="d47dc-193">"HtmlAttributes" 키는 대 소문자를 구분 합니다.</span><span class="sxs-lookup"><span data-stu-id="d47dc-193">The key "htmlAttributes" is case-insensitive.</span></span> <span data-ttu-id="d47dc-194">키 "htmlAttributes" 유사 하 게 처리 되는 `htmlAttributes` 같은 도우미 입력에 전달 된 개체 `@Html.TextBox()`합니다.</span><span class="sxs-lookup"><span data-stu-id="d47dc-194">The key "htmlAttributes" is handled similarly to the `htmlAttributes` object passed to input helpers like `@Html.TextBox()`.</span></span>

```HTML
@Html.EditorFor(model => model.YourProperty, 
  new { htmlAttributes = new { @class="myCssClass", style="Width:100px" } })
```

### <a name="expression-names"></a><span data-ttu-id="d47dc-195">식 이름</span><span class="sxs-lookup"><span data-stu-id="d47dc-195">Expression names</span></span>

<span data-ttu-id="d47dc-196">`asp-for` 특성 값은 한 `ModelExpression` 및 람다 식의 오른쪽입니다.</span><span class="sxs-lookup"><span data-stu-id="d47dc-196">The `asp-for` attribute value is a `ModelExpression` and the right hand side of a lambda expression.</span></span> <span data-ttu-id="d47dc-197">따라서 `asp-for="Property1"` 됩니다 `m => m.Property1` 생성된 된 코드 의미 하기 때문에에서 접두사로 사용 하 여 필요가 없습니다 `Model`합니다.</span><span class="sxs-lookup"><span data-stu-id="d47dc-197">Therefore, `asp-for="Property1"` becomes `m => m.Property1` in the generated code which is why you don't need to prefix with `Model`.</span></span> <span data-ttu-id="d47dc-198">사용할 수는 "@" 문자는 인라인 식을 시작 하 고 앞으로 이동 하는 `m.`:</span><span class="sxs-lookup"><span data-stu-id="d47dc-198">You can use the "@" character to start an inline expression and move before the `m.`:</span></span>

```HTML
@{
       var joe = "Joe";
   }
   <input asp-for="@joe" />
```

<span data-ttu-id="d47dc-199">다음을 생성합니다.</span><span class="sxs-lookup"><span data-stu-id="d47dc-199">Generates the following:</span></span>

```HTML
<input type="text" id="joe" name="joe" value="Joe" />
```

<span data-ttu-id="d47dc-200">컬렉션 속성을 가진 `asp-for="CollectionProperty[23].Member"` 동일한 이름으로 생성 `asp-for="CollectionProperty[i].Member"` 때 `i` 값 `23`합니다.</span><span class="sxs-lookup"><span data-stu-id="d47dc-200">With collection properties, `asp-for="CollectionProperty[23].Member"` generates the same name as `asp-for="CollectionProperty[i].Member"` when `i` has the value `23`.</span></span>

### <a name="navigating-child-properties"></a><span data-ttu-id="d47dc-201">자식 속성을 탐색</span><span class="sxs-lookup"><span data-stu-id="d47dc-201">Navigating child properties</span></span>

<span data-ttu-id="d47dc-202">뷰 모델의 속성 경로 사용 하 여 자식 속성을 탐색할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d47dc-202">You can also navigate to child properties using the property path of the view model.</span></span> <span data-ttu-id="d47dc-203">자식을 포함 하는 보다 복잡 한 모델 클래스를 고려 `Address` 속성입니다.</span><span class="sxs-lookup"><span data-stu-id="d47dc-203">Consider a more complex model class that contains a child `Address` property.</span></span>

[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/AddressViewModel.cs?highlight=1,2,3,4&range=5-8)]

[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/RegisterAddressViewModel.cs?highlight=8&range=5-13)]

<span data-ttu-id="d47dc-204">보기에서에 바인딩한 `Address.AddressLine1`:</span><span class="sxs-lookup"><span data-stu-id="d47dc-204">In the view, we bind to `Address.AddressLine1`:</span></span>

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterAddress.cshtml?highlight=6)]

<span data-ttu-id="d47dc-205">다음 HTML이 생성 `Address.AddressLine1`:</span><span class="sxs-lookup"><span data-stu-id="d47dc-205">The following HTML is generated for `Address.AddressLine1`:</span></span>

```HTML
<input type="text" id="Address_AddressLine1" name="Address.AddressLine1" value="" />
```

### <a name="expression-names-and-collections"></a><span data-ttu-id="d47dc-206">식 이름 및 컬렉션</span><span class="sxs-lookup"><span data-stu-id="d47dc-206">Expression names and Collections</span></span>

<span data-ttu-id="d47dc-207">샘플의 배열을 포함 하는 모델 `Colors`:</span><span class="sxs-lookup"><span data-stu-id="d47dc-207">Sample, a model containing an array of `Colors`:</span></span>

[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/Person.cs?highlight=3&range=5-10)]

<span data-ttu-id="d47dc-208">작업 방법:</span><span class="sxs-lookup"><span data-stu-id="d47dc-208">The action method:</span></span>

```csharp
public IActionResult Edit(int id, int colorIndex)
   {
       ViewData["Index"] = colorIndex;
       return View(GetPerson(id));
   }
```

<span data-ttu-id="d47dc-209">다음 Razor 특정 액세스 하는 방법을 보여 줍니다. `Color` 요소:</span><span class="sxs-lookup"><span data-stu-id="d47dc-209">The following Razor shows how you access a specific `Color` element:</span></span>

[!code-HTML[Main](working-with-forms/sample/final/Views/Demo/EditColor.cshtml)]

<span data-ttu-id="d47dc-210">*Views/Shared/EditorTemplates/String.cshtml* 템플릿:</span><span class="sxs-lookup"><span data-stu-id="d47dc-210">The *Views/Shared/EditorTemplates/String.cshtml* template:</span></span>

[!code-HTML[Main](working-with-forms/sample/final/Views/Shared/EditorTemplates/String.cshtml)]

<span data-ttu-id="d47dc-211">사용 하 여 샘플 `List<T>`:</span><span class="sxs-lookup"><span data-stu-id="d47dc-211">Sample using `List<T>`:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/ToDoItem.cs?range=3-8)]

<span data-ttu-id="d47dc-212">다음 Razor 컬렉션을 반복 하는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="d47dc-212">The following Razor shows how to iterate over a collection:</span></span>

[!code-HTML[Main](working-with-forms/sample/final/Views/Demo/Edit.cshtml)]

<span data-ttu-id="d47dc-213">*Views/Shared/EditorTemplates/ToDoItem.cshtml* 템플릿:</span><span class="sxs-lookup"><span data-stu-id="d47dc-213">The *Views/Shared/EditorTemplates/ToDoItem.cshtml* template:</span></span>

[!code-HTML[Main](working-with-forms/sample/final/Views/Shared/EditorTemplates/ToDoItem.cshtml)]


>[!NOTE]
><span data-ttu-id="d47dc-214">항상 사용 `for` (및 *하지* `foreach`) 목록에 대 한 반복 합니다.</span><span class="sxs-lookup"><span data-stu-id="d47dc-214">Always use `for` (and *not* `foreach`) to iterate over a list.</span></span> <span data-ttu-id="d47dc-215">평가 인덱서는 LINQ의 식은 비용이 많이 들 수를 최소화 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="d47dc-215">Evaluating an indexer in a LINQ expression can be expensive and should be minimized.</span></span>

&nbsp;

>[!NOTE]
><span data-ttu-id="d47dc-216">위의 주석 처리 된 예제 코드는 람다 식을 바꿀 방법을 보여 줍니다.는 `@` 연산자에 액세스할 각 `ToDoItem` 목록에 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d47dc-216">The commented sample code above shows how you would replace the lambda expression with the `@` operator to access each `ToDoItem` in the list.</span></span>

## <a name="the-textarea-tag-helper"></a><span data-ttu-id="d47dc-217">Textarea 태그 도우미</span><span class="sxs-lookup"><span data-stu-id="d47dc-217">The Textarea Tag Helper</span></span>

<span data-ttu-id="d47dc-218">`Textarea Tag Helper` 태그 도우미 입력 태그 도우미를 것과 비슷합니다.</span><span class="sxs-lookup"><span data-stu-id="d47dc-218">The `Textarea Tag Helper` tag helper is  similar to the Input Tag Helper.</span></span>

* <span data-ttu-id="d47dc-219">생성 된 `id` 및 `name` 특성 및에 대 한 모델에서 데이터 유효성 검사 특성은 [ \<textarea >](https://www.w3.org/wiki/HTML/Elements/textarea) 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="d47dc-219">Generates the `id` and `name` attributes, and the data validation attributes from the model for a [\<textarea>](https://www.w3.org/wiki/HTML/Elements/textarea) element.</span></span>

* <span data-ttu-id="d47dc-220">강력한 형식 지정을 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="d47dc-220">Provides strong typing.</span></span>

* <span data-ttu-id="d47dc-221">HTML 도우미 대체:`Html.TextAreaFor`</span><span class="sxs-lookup"><span data-stu-id="d47dc-221">HTML Helper alternative: `Html.TextAreaFor`</span></span>

<span data-ttu-id="d47dc-222">예제:</span><span class="sxs-lookup"><span data-stu-id="d47dc-222">Sample:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/DescriptionViewModel.cs)]

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterTextArea.cshtml?highlight=4)]

<span data-ttu-id="d47dc-223">다음 HTML 생성 됩니다.</span><span class="sxs-lookup"><span data-stu-id="d47dc-223">The following HTML is generated:</span></span>

```HTML
<form method="post" action="/Demo/RegisterTextArea">
  <textarea data-val="true"
   data-val-maxlength="The field Description must be a string or array type with a maximum length of &#x27;1024&#x27;."
   data-val-maxlength-max="1024"
   data-val-minlength="The field Description must be a string or array type with a minimum length of &#x27;5&#x27;."
   data-val-minlength-min="5"
   id="Description" name="Description">
  </textarea>
  <button type="submit">Test</button>
  <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
</form>
```

## <a name="the-label-tag-helper"></a><span data-ttu-id="d47dc-224">레이블 태그 도우미</span><span class="sxs-lookup"><span data-stu-id="d47dc-224">The Label Tag Helper</span></span>

* <span data-ttu-id="d47dc-225">레이블 캡션 생성 및 `for` 특성에 [ <label> ](https://www.w3.org/wiki/HTML/Elements/label) 식 이름에 대 한 요소</span><span class="sxs-lookup"><span data-stu-id="d47dc-225">Generates the label caption and `for` attribute on a [<label>](https://www.w3.org/wiki/HTML/Elements/label) element for an expression name</span></span>

* <span data-ttu-id="d47dc-226">HTML 도우미 대체: `Html.LabelFor`합니다.</span><span class="sxs-lookup"><span data-stu-id="d47dc-226">HTML Helper alternative: `Html.LabelFor`.</span></span>

<span data-ttu-id="d47dc-227">`Label Tag Helper` 순수 HTML label 요소를 통해 다음과 같은 이점을 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="d47dc-227">The `Label Tag Helper`  provides the following benefits over a pure HTML label element:</span></span>

* <span data-ttu-id="d47dc-228">설명 레이블을 값을 자동으로 가져오기는 `Display` 특성입니다.</span><span class="sxs-lookup"><span data-stu-id="d47dc-228">You automatically get the descriptive label value from the `Display` attribute.</span></span> <span data-ttu-id="d47dc-229">의도 한 표시 이름을 조합해 서 고 시간에 따라 변경 될 수 있습니다 `Display` 특성 및 레이블 태그 도우미 적용 됩니다는 `Display` 모든 곳에서 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="d47dc-229">The intended display name might change over time, and the combination of `Display` attribute and Label Tag Helper will apply the `Display` everywhere it's used.</span></span>

* <span data-ttu-id="d47dc-230">소스 코드에서 덜 태그</span><span class="sxs-lookup"><span data-stu-id="d47dc-230">Less markup in source code</span></span>

* <span data-ttu-id="d47dc-231">강력한 모델 속성을 입력 합니다.</span><span class="sxs-lookup"><span data-stu-id="d47dc-231">Strong typing with the model property.</span></span>

<span data-ttu-id="d47dc-232">예제:</span><span class="sxs-lookup"><span data-stu-id="d47dc-232">Sample:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/SimpleViewModel.cs)]

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterLabel.cshtml?highlight=4)]

<span data-ttu-id="d47dc-233">다음 HTML에 대해 생성 되는 `<label>` 요소:</span><span class="sxs-lookup"><span data-stu-id="d47dc-233">The following HTML is generated for the `<label>` element:</span></span>

```HTML
<label for="Email">Email Address</label>
```

<span data-ttu-id="d47dc-234">생성 된 레이블 태그 도우미의 `for` 와 연결 된 id는 "Email"의 특성 값의 `<input>` 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="d47dc-234">The Label Tag Helper generated the `for` attribute value of "Email", which is the ID associated with the `<input>` element.</span></span> <span data-ttu-id="d47dc-235">태그 도우미 생성 일관 된 `id` 및 `for` 요소 제대로 연결 될 수 있도록 합니다.</span><span class="sxs-lookup"><span data-stu-id="d47dc-235">The Tag Helpers generate consistent `id` and `for` elements so they can be correctly associated.</span></span> <span data-ttu-id="d47dc-236">이 샘플의 캡션을에서 제공 되는 `Display` 특성입니다.</span><span class="sxs-lookup"><span data-stu-id="d47dc-236">The caption in this sample comes from the `Display` attribute.</span></span> <span data-ttu-id="d47dc-237">모델에 포함 하지 않은 경우는 `Display` 특성 캡션 식의 속성 이름 것입니다.</span><span class="sxs-lookup"><span data-stu-id="d47dc-237">If the model didn't contain a `Display` attribute, the caption would be the property name of the expression.</span></span>

## <a name="the-validation-tag-helpers"></a><span data-ttu-id="d47dc-238">유효성 검사 태그 도우미</span><span class="sxs-lookup"><span data-stu-id="d47dc-238">The Validation Tag Helpers</span></span>

<span data-ttu-id="d47dc-239">유효성 검사 태그 도우미 두 가지가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d47dc-239">There are two Validation Tag Helpers.</span></span> <span data-ttu-id="d47dc-240">`Validation Message Tag Helper` (표시할 단일 속성에 대 한 유효성 검사 메시지를 모델에), 및 `Validation Summary Tag Helper` (유효성 검사 오류의 요약 표시 입니다).</span><span class="sxs-lookup"><span data-stu-id="d47dc-240">The `Validation Message Tag Helper` (which displays a validation message for a single property on your model), and the `Validation Summary Tag Helper` (which displays a summary of validation errors).</span></span> <span data-ttu-id="d47dc-241">`Input Tag Helper` HTML5 모델 클래스에 주석 특성 데이터에 따라 요소 입력에 클라이언트 쪽 유효성 검사 특성을 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="d47dc-241">The `Input Tag Helper` adds HTML5 client side validation attributes to input elements based on data annotation attributes on your model classes.</span></span> <span data-ttu-id="d47dc-242">서버에서 유효성 검사도 수행 됩니다.</span><span class="sxs-lookup"><span data-stu-id="d47dc-242">Validation is also performed on the server.</span></span> <span data-ttu-id="d47dc-243">유효성 검사 태그 도우미 유효성 검사 오류가 발생 하는 경우 이러한 오류 메시지를 표시 합니다.</span><span class="sxs-lookup"><span data-stu-id="d47dc-243">The Validation Tag Helper displays these error messages when a validation error occurs.</span></span>

### <a name="the-validation-message-tag-helper"></a><span data-ttu-id="d47dc-244">유효성 검사 메시지 태그 도우미</span><span class="sxs-lookup"><span data-stu-id="d47dc-244">The Validation Message Tag Helper</span></span>

* <span data-ttu-id="d47dc-245">추가 [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) `data-valmsg-for="property"` 특성을 [에 걸쳐](https://developer.mozilla.org/docs/Web/HTML/Element/span) 지정된 모델 속성의 입력된 필드에 유효성 검사 오류 메시지를 연결 하는 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="d47dc-245">Adds the [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5)  `data-valmsg-for="property"` attribute to the [span](https://developer.mozilla.org/docs/Web/HTML/Element/span) element, which attaches the validation error messages on the input field of the specified model property.</span></span> <span data-ttu-id="d47dc-246">클라이언트 쪽 유효성 검사 오류가 발생할 때 [jQuery](https://jquery.com/) 의 오류 메시지를 표시는 `<span>` 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="d47dc-246">When a client side validation error occurs, [jQuery](https://jquery.com/) displays the error message in the `<span>` element.</span></span>

* <span data-ttu-id="d47dc-247">유효성 검사는 또한 서버에서 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="d47dc-247">Validation also takes place on the server.</span></span> <span data-ttu-id="d47dc-248">클라이언트는 사용 하지 않도록 설정 하는 JavaScript 있을 수 있습니다 및 서버 쪽에서 일부 유효성 검사에만 수행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d47dc-248">Clients may have JavaScript disabled and some validation can only be done on the server side.</span></span>

* <span data-ttu-id="d47dc-249">HTML 도우미 대체:`Html.ValidationMessageFor`</span><span class="sxs-lookup"><span data-stu-id="d47dc-249">HTML Helper alternative: `Html.ValidationMessageFor`</span></span>

<span data-ttu-id="d47dc-250">`Validation Message Tag Helper` 와 함께 사용 되는 `asp-validation-for` 특성에는 HTML [걸쳐](https://developer.mozilla.org/docs/Web/HTML/Element/span) 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="d47dc-250">The `Validation Message Tag Helper`  is used with the `asp-validation-for` attribute on a HTML [span](https://developer.mozilla.org/docs/Web/HTML/Element/span) element.</span></span>

```HTML
<span asp-validation-for="Email"></span>
```

<span data-ttu-id="d47dc-251">유효성 검사 메시지 태그 도우미 다음 HTML을 생성 합니다.</span><span class="sxs-lookup"><span data-stu-id="d47dc-251">The Validation Message Tag Helper will generate the following HTML:</span></span>

```HTML
<span class="field-validation-valid"
  data-valmsg-for="Email"
  data-valmsg-replace="true"></span>
```

<span data-ttu-id="d47dc-252">일반적으로 사용 된 `Validation Message Tag Helper` 후는 `Input` 동일한 속성에 대 한 태그 도우미입니다.</span><span class="sxs-lookup"><span data-stu-id="d47dc-252">You generally use the `Validation Message Tag Helper`  after an `Input` Tag Helper for the same property.</span></span> <span data-ttu-id="d47dc-253">이렇게 하면 근처 오류가 발생 하는 입력 유효성 검사 오류 메시지가 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="d47dc-253">Doing so displays any validation error messages near the input that caused the error.</span></span>

> [!NOTE]
> <span data-ttu-id="d47dc-254">올바른 javascript 보기 및 [jQuery](https://jquery.com/) 스크립트 클라이언트 쪽 유효성 검사에 대 한 위치에서 참조 합니다.</span><span class="sxs-lookup"><span data-stu-id="d47dc-254">You must have a view with the correct JavaScript and [jQuery](https://jquery.com/) script references in place for client side validation.</span></span> <span data-ttu-id="d47dc-255">참조 [모델 유효성 검사](../models/validation.md) 자세한 정보에 대 한 합니다.</span><span class="sxs-lookup"><span data-stu-id="d47dc-255">See [Model Validation](../models/validation.md) for more information.</span></span>

<span data-ttu-id="d47dc-256">서버 쪽 유효성 검사 오류가 발생 하는 (예를 들어 사용자 지정 서버 쪽 유효성 검사 또는 경우 클라이언트 쪽 유효성 검사가 해제 된) MVC 배치 해당 오류 메시지의 본문으로는 `<span>` 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="d47dc-256">When a server side validation error occurs (for example when you have custom server side validation or client-side validation is disabled), MVC places that error message as the body of the `<span>` element.</span></span>

```HTML
<span class="field-validation-error" data-valmsg-for="Email"
            data-valmsg-replace="true">
   The Email Address field is required.
</span>
```

### <a name="the-validation-summary-tag-helper"></a><span data-ttu-id="d47dc-257">유효성 검사 요약 태그 도우미</span><span class="sxs-lookup"><span data-stu-id="d47dc-257">The Validation Summary Tag Helper</span></span>

* <span data-ttu-id="d47dc-258">대상 `<div>` 사용 하 여 요소는 `asp-validation-summary` 특성</span><span class="sxs-lookup"><span data-stu-id="d47dc-258">Targets `<div>` elements with the `asp-validation-summary` attribute</span></span>

* <span data-ttu-id="d47dc-259">HTML 도우미 대체:`@Html.ValidationSummary`</span><span class="sxs-lookup"><span data-stu-id="d47dc-259">HTML Helper alternative: `@Html.ValidationSummary`</span></span>

<span data-ttu-id="d47dc-260">`Validation Summary Tag Helper` 유효성 검사 메시지의 요약 정보를 표시 하는 데 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="d47dc-260">The `Validation Summary Tag Helper`  is used to display a summary of validation messages.</span></span> <span data-ttu-id="d47dc-261">`asp-validation-summary` 특성 값은 다음 중 하나일 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d47dc-261">The `asp-validation-summary` attribute value can be any of the following:</span></span>

|<span data-ttu-id="d47dc-262">asp-validation-summary</span><span class="sxs-lookup"><span data-stu-id="d47dc-262">asp-validation-summary</span></span>|<span data-ttu-id="d47dc-263">표시 되는 유효성 검사 메시지</span><span class="sxs-lookup"><span data-stu-id="d47dc-263">Validation messages displayed</span></span>|
|--- |--- |
|<span data-ttu-id="d47dc-264">ValidationSummary.All</span><span class="sxs-lookup"><span data-stu-id="d47dc-264">ValidationSummary.All</span></span>|<span data-ttu-id="d47dc-265">속성 및 모델 수준</span><span class="sxs-lookup"><span data-stu-id="d47dc-265">Property and model level</span></span>|
|<span data-ttu-id="d47dc-266">ValidationSummary.ModelOnly</span><span class="sxs-lookup"><span data-stu-id="d47dc-266">ValidationSummary.ModelOnly</span></span>|<span data-ttu-id="d47dc-267">모델</span><span class="sxs-lookup"><span data-stu-id="d47dc-267">Model</span></span>|
|<span data-ttu-id="d47dc-268">ValidationSummary.None</span><span class="sxs-lookup"><span data-stu-id="d47dc-268">ValidationSummary.None</span></span>|<span data-ttu-id="d47dc-269">없음</span><span class="sxs-lookup"><span data-stu-id="d47dc-269">None</span></span>|

### <a name="sample"></a><span data-ttu-id="d47dc-270">샘플</span><span class="sxs-lookup"><span data-stu-id="d47dc-270">Sample</span></span>

<span data-ttu-id="d47dc-271">다음 예제에서는 데이터 모델으로 데코레이팅된 `DataAnnotation` 특성에 유효성 검사 오류 메시지를 생성 하는 메시지는 `<input>` 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="d47dc-271">In the following example, the data model is decorated with `DataAnnotation` attributes, which generates validation error messages on the `<input>` element.</span></span>  <span data-ttu-id="d47dc-272">유효성 검사 오류가 발생 하면 오류 메시지가 유효성 검사 태그 도우미에 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="d47dc-272">When a validation error occurs, the Validation Tag Helper displays the error message:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/RegisterViewModel.cs)]

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterValidation.cshtml?highlight=4,6,8&range=1-10)]

<span data-ttu-id="d47dc-273">생성 된 HTML (모델은 유효) 하는 경우:</span><span class="sxs-lookup"><span data-stu-id="d47dc-273">The generated HTML (when the model is valid):</span></span>

```HTML
<form action="/DemoReg/Register" method="post">
  <div class="validation-summary-valid" data-valmsg-summary="true">
  <ul><li style="display:none"></li></ul></div>
  Email:  <input name="Email" id="Email" type="email" value=""
   data-val-required="The Email field is required."
   data-val-email="The Email field is not a valid e-mail address."
   data-val="true"> <br>
  <span class="field-validation-valid" data-valmsg-replace="true"
   data-valmsg-for="Email"></span><br>
  Password: <input name="Password" id="Password" type="password"
   data-val-required="The Password field is required." data-val="true"><br>
  <span class="field-validation-valid" data-valmsg-replace="true"
   data-valmsg-for="Password"></span><br>
  <button type="submit">Register</button>
  <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
</form>
```

## <a name="the-select-tag-helper"></a><span data-ttu-id="d47dc-274">Select 태그 도우미</span><span class="sxs-lookup"><span data-stu-id="d47dc-274">The Select Tag Helper</span></span>

* <span data-ttu-id="d47dc-275">생성 [선택](https://www.w3.org/wiki/HTML/Elements/select) 및 관련 된 [옵션](https://www.w3.org/wiki/HTML/Elements/option) 모델의 속성에 대 한 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="d47dc-275">Generates [select](https://www.w3.org/wiki/HTML/Elements/select) and associated [option](https://www.w3.org/wiki/HTML/Elements/option) elements for properties of your model.</span></span>

* <span data-ttu-id="d47dc-276">에 HTML 도우미 대신 `Html.DropDownListFor` 및`Html.ListBoxFor`</span><span class="sxs-lookup"><span data-stu-id="d47dc-276">Has an HTML Helper alternative `Html.DropDownListFor` and `Html.ListBoxFor`</span></span>

<span data-ttu-id="d47dc-277">`Select Tag Helper` `asp-for` 에 대 한 모델 속성 이름에서 [선택](https://www.w3.org/wiki/HTML/Elements/select) 요소 및 `asp-items` 지정는 [옵션](https://www.w3.org/wiki/HTML/Elements/option) 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="d47dc-277">The `Select Tag Helper` `asp-for` specifies the model property  name for the [select](https://www.w3.org/wiki/HTML/Elements/select) element  and `asp-items` specifies the [option](https://www.w3.org/wiki/HTML/Elements/option) elements.</span></span>  <span data-ttu-id="d47dc-278">예:</span><span class="sxs-lookup"><span data-stu-id="d47dc-278">For example:</span></span>

[!code-HTML[Main](working-with-forms/sample/final/Views/Home/Index.cshtml?range=4)]

<span data-ttu-id="d47dc-279">예제:</span><span class="sxs-lookup"><span data-stu-id="d47dc-279">Sample:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/CountryViewModel.cs)]

<span data-ttu-id="d47dc-280">`Index` 메서드를 초기화 하는 `CountryViewModel`, 선택한 국가별 설정 및에 전달 된 `Index` 보기.</span><span class="sxs-lookup"><span data-stu-id="d47dc-280">The `Index` method initializes the `CountryViewModel`, sets the selected country and passes it to the `Index` view.</span></span>

[!code-csharp[Main](working-with-forms/sample/final/Controllers/HomeController.cs?range=114-119)]

<span data-ttu-id="d47dc-281">HTTP POST `Index` 메서드 선택 항목을 표시 합니다.</span><span class="sxs-lookup"><span data-stu-id="d47dc-281">The HTTP POST `Index` method displays the selection:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/Controllers/HomeController.cs?range=15-27)]

<span data-ttu-id="d47dc-282">`Index` 보기:</span><span class="sxs-lookup"><span data-stu-id="d47dc-282">The `Index` view:</span></span>

[!code-cshtml[Main](working-with-forms/sample/final/Views/Home/Index.cshtml?highlight=4)]

<span data-ttu-id="d47dc-283">("CA" 선택)로 다음 HTML을 생성 합니다.</span><span class="sxs-lookup"><span data-stu-id="d47dc-283">Which generates the following HTML (with "CA" selected):</span></span>

```html
<form method="post" action="/">
     <select id="Country" name="Country">
       <option value="MX">Mexico</option>
       <option selected="selected" value="CA">Canada</option>
       <option value="US">USA</option>
     </select>
       <br /><button type="submit">Register</button>
     <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
   </form>
```

> [!NOTE]
> <span data-ttu-id="d47dc-284">사용 하지 않는 것이 좋습니다 `ViewBag` 또는 `ViewData` 선택 태그 도우미와 합니다.</span><span class="sxs-lookup"><span data-stu-id="d47dc-284">We do not recommend using `ViewBag` or `ViewData` with the Select Tag Helper.</span></span> <span data-ttu-id="d47dc-285">뷰 모델은 MVC 메타 데이터를 제공할에 더 강력 하 고 문제를 줄일 일반적으로 합니다.</span><span class="sxs-lookup"><span data-stu-id="d47dc-285">A view model is more robust at providing MVC metadata and generally less problematic.</span></span>

<span data-ttu-id="d47dc-286">`asp-for` 특성 값은 특별 한 경우 있으며 필요 하지 않습니다는 `Model` 접두사, 다른 태그 도우미 특성 안 함 (예: `asp-items`)</span><span class="sxs-lookup"><span data-stu-id="d47dc-286">The `asp-for` attribute value is a special case and doesn't require a `Model` prefix, the other Tag Helper attributes do (such as `asp-items`)</span></span>

[!code-HTML[Main](working-with-forms/sample/final/Views/Home/Index.cshtml?range=4)]

### <a name="enum-binding"></a><span data-ttu-id="d47dc-287">Enum 바인딩</span><span class="sxs-lookup"><span data-stu-id="d47dc-287">Enum binding</span></span>

<span data-ttu-id="d47dc-288">사용 하기 어려운 방식은 `<select>` 와 `enum` 속성을 생성 하 고는 `SelectListItem` 에서 요소를는 `enum` 값.</span><span class="sxs-lookup"><span data-stu-id="d47dc-288">It's often convenient to use `<select>` with an `enum` property and generate the `SelectListItem` elements from the `enum` values.</span></span>

<span data-ttu-id="d47dc-289">예제:</span><span class="sxs-lookup"><span data-stu-id="d47dc-289">Sample:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/CountryEnumViewModel.cs?range=3-7)]

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/CountryEnum.cs)]

<span data-ttu-id="d47dc-290">`GetEnumSelectList` 메서드를 생성 한 `SelectList` 열거형에 대 한 개체입니다.</span><span class="sxs-lookup"><span data-stu-id="d47dc-290">The `GetEnumSelectList` method generates a `SelectList` object for an enum.</span></span>

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexEnum.cshtml?highlight=5)]

<span data-ttu-id="d47dc-291">으로 목록에 열거자를 데코레이팅 할 수 있습니다는 `Display` 보다 풍부한 UI 가져올 특성:</span><span class="sxs-lookup"><span data-stu-id="d47dc-291">You can decorate your enumerator list with the `Display` attribute to get a richer UI:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/CountryEnum.cs?highlight=5,7)]

<span data-ttu-id="d47dc-292">다음 HTML 생성 됩니다.</span><span class="sxs-lookup"><span data-stu-id="d47dc-292">The following HTML is generated:</span></span>

```HTML
  <form method="post" action="/Home/IndexEnum">
         <select data-val="true" data-val-required="The EnumCountry field is required."
                 id="EnumCountry" name="EnumCountry">
             <option value="0">United Mexican States</option>
             <option value="1">United States of America</option>
             <option value="2">Canada</option>
             <option value="3">France</option>
             <option value="4">Germany</option>
             <option selected="selected" value="5">Spain</option>
         </select>
         <br /><button type="submit">Register</button>
         <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
    </form>
```

### <a name="option-group"></a><span data-ttu-id="d47dc-293">옵션 그룹</span><span class="sxs-lookup"><span data-stu-id="d47dc-293">Option Group</span></span>

<span data-ttu-id="d47dc-294">HTML [ \<optgroup >](https://www.w3.org/wiki/HTML/Elements/optgroup) 뷰 모델 하나 이상 포함 하는 경우 요소가 생성 `SelectListGroup` 개체입니다.</span><span class="sxs-lookup"><span data-stu-id="d47dc-294">The HTML  [\<optgroup>](https://www.w3.org/wiki/HTML/Elements/optgroup) element is generated when the view model contains one or more `SelectListGroup` objects.</span></span>

<span data-ttu-id="d47dc-295">`CountryViewModelGroup` 그룹은 `SelectListItem` "North America" 및 "Europe" 그룹으로 요소:</span><span class="sxs-lookup"><span data-stu-id="d47dc-295">The `CountryViewModelGroup` groups the `SelectListItem` elements into the "North America" and "Europe" groups:</span></span>

[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/CountryViewModelGroup.cs?highlight=5,6,14,20,26,32,38,44&range=6-56)]

<span data-ttu-id="d47dc-296">두 개의 그룹은 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="d47dc-296">The two groups are shown below:</span></span>

![옵션 그룹 예제](working-with-forms/_static/grp.png)

<span data-ttu-id="d47dc-298">생성 된 HTML:</span><span class="sxs-lookup"><span data-stu-id="d47dc-298">The generated HTML:</span></span>

```HTML
 <form method="post" action="/Home/IndexGroup">
      <select id="Country" name="Country">
          <optgroup label="North America">
              <option value="MEX">Mexico</option>
              <option value="CAN">Canada</option>
              <option value="US">USA</option>
          </optgroup>
          <optgroup label="Europe">
              <option value="FR">France</option>
              <option value="ES">Spain</option>
              <option value="DE">Germany</option>
          </optgroup>
      </select>
      <br /><button type="submit">Register</button>
      <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
 </form>
```

### <a name="multiple-select"></a><span data-ttu-id="d47dc-299">다중 선택</span><span class="sxs-lookup"><span data-stu-id="d47dc-299">Multiple select</span></span>

<span data-ttu-id="d47dc-300">태그 선택 도우미 자동으로 생성 됩니다는 [여러 = "여러"](http://w3c.github.io/html-reference/select.html) 특성 속성에 지정 된 경우는 `asp-for` 특성은 한 `IEnumerable`합니다.</span><span class="sxs-lookup"><span data-stu-id="d47dc-300">The Select Tag Helper  will automatically generate the [multiple = "multiple"](http://w3c.github.io/html-reference/select.html)  attribute if the property specified in the `asp-for` attribute is an `IEnumerable`.</span></span> <span data-ttu-id="d47dc-301">예를 들어 다음 모델을 가정합니다.</span><span class="sxs-lookup"><span data-stu-id="d47dc-301">For example, given the following model:</span></span>

[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/CountryViewModelIEnumerable.cs?highlight=6)]

<span data-ttu-id="d47dc-302">다음 보기:</span><span class="sxs-lookup"><span data-stu-id="d47dc-302">With the following view:</span></span>

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexMultiSelect.cshtml?highlight=4)]

<span data-ttu-id="d47dc-303">다음 HTML을 생성합니다.</span><span class="sxs-lookup"><span data-stu-id="d47dc-303">Generates the following HTML:</span></span>

```HTML
<form method="post" action="/Home/IndexMultiSelect">
    <select id="CountryCodes"
    multiple="multiple"
    name="CountryCodes"><option value="MX">Mexico</option>
<option value="CA">Canada</option>
<option value="US">USA</option>
<option value="FR">France</option>
<option value="ES">Spain</option>
<option value="DE">Germany</option>
</select>
    <br /><button type="submit">Register</button>
  <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
</form>
```

### <a name="no-selection"></a><span data-ttu-id="d47dc-304">선택 영역이 없는</span><span class="sxs-lookup"><span data-stu-id="d47dc-304">No selection</span></span>

<span data-ttu-id="d47dc-305">여러 페이지에 "지정 안 됨된" 옵션을 사용 하 여 생각 되 면 HTML 반복을 제거 하는 템플릿을 만들 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d47dc-305">If you find yourself using the "not specified" option in multiple pages, you can create a template to eliminate repeating the HTML:</span></span>

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexEmptyTemplate.cshtml?highlight=5)]

<span data-ttu-id="d47dc-306">*Views/Shared/EditorTemplates/CountryViewModel.cshtml* 템플릿:</span><span class="sxs-lookup"><span data-stu-id="d47dc-306">The *Views/Shared/EditorTemplates/CountryViewModel.cshtml* template:</span></span>

[!code-HTML[Main](working-with-forms/sample/final/Views/Shared/EditorTemplates/CountryViewModel.cshtml)]

<span data-ttu-id="d47dc-307">추가 HTML [ \<옵션 >](https://www.w3.org/wiki/HTML/Elements/option) 요소에 국한 되지 않음는 *선택 영역이 없는* 대/소문자입니다.</span><span class="sxs-lookup"><span data-stu-id="d47dc-307">Adding HTML [\<option>](https://www.w3.org/wiki/HTML/Elements/option) elements is not limited to the *No selection* case.</span></span> <span data-ttu-id="d47dc-308">예를 들어 다음과 같은 보기 및 작업 메서드는 위의 코드와 유사한 HTML을 생성 합니다.</span><span class="sxs-lookup"><span data-stu-id="d47dc-308">For example, the following view and action method will generate HTML similar to the code above:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/Controllers/HomeController.cs?range=114-119)]

[!code-HTML[Main](working-with-forms/sample/final/Views/Home/IndexOption.cshtml)]

<span data-ttu-id="d47dc-309">올바른 `<option>` 요소가 선택 됩니다 (포함 된 `selected="selected"` 특성) 현재에 따라 `Country` 값입니다.</span><span class="sxs-lookup"><span data-stu-id="d47dc-309">The correct `<option>` element will be selected ( contain the `selected="selected"` attribute) depending on the current `Country` value.</span></span>

```HTML
 <form method="post" action="/Home/IndexEmpty">
      <select id="Country" name="Country">
          <option value="">&lt;none&gt;</option>
          <option value="MX">Mexico</option>
          <option value="CA" selected="selected">Canada</option>
          <option value="US">USA</option>
      </select>
      <br /><button type="submit">Register</button>
   <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
 </form>
 ```

## <a name="additional-resources"></a><span data-ttu-id="d47dc-310">추가 리소스</span><span class="sxs-lookup"><span data-stu-id="d47dc-310">Additional Resources</span></span>

* [<span data-ttu-id="d47dc-311">태그 도우미</span><span class="sxs-lookup"><span data-stu-id="d47dc-311">Tag Helpers</span></span>](tag-helpers/intro.md)

* [<span data-ttu-id="d47dc-312">HTML 폼 요소</span><span class="sxs-lookup"><span data-stu-id="d47dc-312">HTML Form element</span></span>](https://www.w3.org/TR/html401/interact/forms.html)

* [<span data-ttu-id="d47dc-313">확인 토큰을 요청 합니다.</span><span class="sxs-lookup"><span data-stu-id="d47dc-313">Request Verification Token</span></span>](https://docs.microsoft.com/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages)

* [<span data-ttu-id="d47dc-314">모델 바인딩</span><span class="sxs-lookup"><span data-stu-id="d47dc-314">Model Binding</span></span>](../models/model-binding.md)

* [<span data-ttu-id="d47dc-315">모델 유효성 검사</span><span class="sxs-lookup"><span data-stu-id="d47dc-315">Model Validation</span></span>](../models/validation.md)

* [<span data-ttu-id="d47dc-316">데이터 주석</span><span class="sxs-lookup"><span data-stu-id="d47dc-316">data annotations</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter)

* <span data-ttu-id="d47dc-317">[코드 조각이이 문서에 대 한](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/forms/sample)합니다.</span><span class="sxs-lookup"><span data-stu-id="d47dc-317">[Code snippets for this document](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/forms/sample).</span></span>
