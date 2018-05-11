---
title: ASP.NET Core Id에 대 한 사용자 지정 저장소 공급자
author: ardalis
description: ASP.NET Core Id에 대 한 사용자 지정 저장소 공급자를 구성 하는 방법에 알아봅니다.
manager: wpickett
ms.author: riande
ms.date: 05/24/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/identity-custom-storage-providers
ms.openlocfilehash: a50069586c702152e81961773114ea75901ab23d
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/08/2018
---
# <a name="custom-storage-providers-for-aspnet-core-identity"></a><span data-ttu-id="76e14-103">ASP.NET Core Id에 대 한 사용자 지정 저장소 공급자</span><span class="sxs-lookup"><span data-stu-id="76e14-103">Custom storage providers for ASP.NET Core Identity</span></span>

<span data-ttu-id="76e14-104">작성자: [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="76e14-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="76e14-105">ASP.NET Core Id는 사용자 지정 저장소 공급자를 만들고 응용 프로그램에 연결할 수 있도록 하는 확장 가능한 시스템입니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-105">ASP.NET Core Identity is an extensible system which enables you to create a custom storage provider and connect it to your app.</span></span> <span data-ttu-id="76e14-106">이 항목에서는 ASP.NET Core Id에 대 한 사용자 지정 된 저장소 공급자를 만드는 방법을 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-106">This topic describes how to create a customized storage provider for ASP.NET Core Identity.</span></span> <span data-ttu-id="76e14-107">저장소 공급자를 만들기 위한 중요 한 개념을 다루지만 단계별 연습 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-107">It covers the important concepts for creating your own storage provider, but isn't a step-by-step walkthrough.</span></span>

<span data-ttu-id="76e14-108">[GitHub에서 샘플 보기 또는 다운로드](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/identity/sample)</span><span class="sxs-lookup"><span data-stu-id="76e14-108">[View or download sample from GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/identity/sample).</span></span>

## <a name="introduction"></a><span data-ttu-id="76e14-109">소개</span><span class="sxs-lookup"><span data-stu-id="76e14-109">Introduction</span></span>

<span data-ttu-id="76e14-110">기본적으로 ASP.NET Core Id 시스템 Entity Framework Core를 사용 하 여 SQL Server 데이터베이스에 사용자 정보를 저장 합니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-110">By default, the ASP.NET Core Identity system stores user information in a SQL Server database using Entity Framework Core.</span></span> <span data-ttu-id="76e14-111">많은 응용 프로그램에 대 한이 방법은 잘 작동합니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-111">For many apps, this approach works well.</span></span> <span data-ttu-id="76e14-112">그러나 데이터 스키마 또는 다른 지 속성 메커니즘을 사용 하도록 할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-112">However, you may prefer to use a different persistence mechanism or data schema.</span></span> <span data-ttu-id="76e14-113">예를 들어:</span><span class="sxs-lookup"><span data-stu-id="76e14-113">For example:</span></span>

* <span data-ttu-id="76e14-114">사용 하면 [Azure 테이블 저장소](https://docs.microsoft.com/azure/storage/) 또는 다른 데이터 저장소입니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-114">You use [Azure Table Storage](https://docs.microsoft.com/azure/storage/) or another data store.</span></span>
* <span data-ttu-id="76e14-115">데이터베이스 테이블에는 다른 구조가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-115">Your database tables have a different structure.</span></span> 
* <span data-ttu-id="76e14-116">와 같은 다른 데이터 액세스 접근 방식을 사용 하려면 [Dapper](https://github.com/StackExchange/Dapper)합니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-116">You may wish to use a different data access approach, such as [Dapper](https://github.com/StackExchange/Dapper).</span></span> 

<span data-ttu-id="76e14-117">각이 경우에서 저장 메커니즘에 대 한 사용자 지정된 공급자를 작성할 수 있으며 응용 프로그램에 해당 공급자를 연결 합니다. 키를 누릅니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-117">In each of these cases, you can write a customized provider for your storage mechanism and plug that provider into your app.</span></span>

<span data-ttu-id="76e14-118">ASP.NET Core Id는 "개별 사용자 계정" 옵션을 사용 하 여 Visual Studio에서 프로젝트 템플릿을에 포함 됩니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-118">ASP.NET Core Identity is included in project templates in Visual Studio with the "Individual User Accounts" option.</span></span>

<span data-ttu-id="76e14-119">.NET Core CLI를 사용 하는 경우 추가 `-au Individual`:</span><span class="sxs-lookup"><span data-stu-id="76e14-119">When using the .NET Core CLI, add `-au Individual`:</span></span>

```console
dotnet new mvc -au Individual
dotnet new webapi -au Individual
```

## <a name="the-aspnet-core-identity-architecture"></a><span data-ttu-id="76e14-120">ASP.NET Core Id 아키텍처</span><span class="sxs-lookup"><span data-stu-id="76e14-120">The ASP.NET Core Identity architecture</span></span>

<span data-ttu-id="76e14-121">ASP.NET Core Id 관리자와 저장소를 호출 하는 클래스로 구성 됩니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-121">ASP.NET Core Identity consists of classes called managers and stores.</span></span> <span data-ttu-id="76e14-122">*관리자* 높은 수준의 하는 클래스를 응용 프로그램 개발자는 Id 사용자를 만드는 등의 작업을 수행 하기 위해 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-122">*Managers* are high-level classes which an app developer uses to perform operations, such as creating an Identity user.</span></span> <span data-ttu-id="76e14-123">*저장소* 는 사용자 및 역할을 하는 등의 엔터티는 유지 하는 방법을 지정 하는 하위 클래스입니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-123">*Stores* are lower-level classes that specify how entities, such as users and roles, are persisted.</span></span> <span data-ttu-id="76e14-124">저장소에 따라는 [리포지토리 패턴](http://deviq.com/repository-pattern/) 고 밀접 하 게 지 속성 메커니즘으로 결합 되어 수행 됩니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-124">Stores follow the [repository pattern](http://deviq.com/repository-pattern/) and are closely coupled with the persistence mechanism.</span></span> <span data-ttu-id="76e14-125">관리자는 (구성)를 제외한 응용 프로그램 코드를 변경 하지 않고 지 속성 메커니즘을 바꿀 수 있습니다 의미 하는 저장소에서 분리 됩니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-125">Managers are decoupled from stores, which means you can replace the persistence mechanism without changing your application code (except for configuration).</span></span>

<span data-ttu-id="76e14-126">다음 다이어그램에서는 저장소 데이터 액세스 계층 상호 작용 하는 동안 웹 응용 프로그램 관리자, 상호 작용 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-126">The following diagram shows how a web app interacts with the managers, while stores interact with the data access layer.</span></span>

![ASP.NET Core 응용 프로그램 관리자 (예를 들어 'UserManager', 'RoleManager')와 함께 작동합니다.](identity-custom-storage-providers/_static/identity-architecture-diagram.png)

<span data-ttu-id="76e14-129">사용자 지정 저장소 공급자를 만들려면 (위의 다이어그램에서 녹색, 회색 상자)이 데이터 액세스 계층으로 데이터 원본, 데이터 액세스 계층 및 상호 작용 하는 저장소 클래스를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-129">To create a custom storage provider, create the data source, the data access layer, and the store classes that interact with this data access layer (the green and grey boxes in the diagram above).</span></span> <span data-ttu-id="76e14-130">관리자 또는 그 (위의 파란색 상자는)와 상호 작용 하는 응용 프로그램 코드를 사용자 지정할 필요가 없습니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-130">You don't need to customize the managers or your app code that interacts with them (the blue boxes above).</span></span>

<span data-ttu-id="76e14-131">새 인스턴스를 만들 때 `UserManager` 또는 `RoleManager` 사용자 클래스의 형식을 제공 하 고 저장소 클래스의 인스턴스를 인수로 전달 합니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-131">When creating a new instance of `UserManager` or `RoleManager` you provide the type of the user class and pass an instance of the store class as an argument.</span></span> <span data-ttu-id="76e14-132">이 방법을 사용 하면 ASP.NET Core에 사용자 지정 된 클래스를 연결할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-132">This approach enables you to plug your customized classes into ASP.NET Core.</span></span> 

<span data-ttu-id="76e14-133">[새 저장소 공급자를 사용 하도록 응용 프로그램을 다시 구성](#reconfigure-app-to-use-new-storage-provider) 인스턴스화하는 방법을 보여 줍니다. `UserManager` 및 `RoleManager` 사용자 지정 된 저장소와 함께 합니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-133">[Reconfigure app to use new storage provider](#reconfigure-app-to-use-new-storage-provider) shows how to instantiate `UserManager` and `RoleManager` with a customized store.</span></span>

## <a name="aspnet-core-identity-stores-data-types"></a><span data-ttu-id="76e14-134">ASP.NET Core Identity 저장 데이터 형식</span><span class="sxs-lookup"><span data-stu-id="76e14-134">ASP.NET Core Identity stores data types</span></span>

<span data-ttu-id="76e14-135">[ASP.NET Core Id](https://github.com/aspnet/identity) 데이터 형식은 다음 섹션에서 자세히 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-135">[ASP.NET Core Identity](https://github.com/aspnet/identity) data types are detailed in the following sections:</span></span>

### <a name="users"></a><span data-ttu-id="76e14-136">사용자</span><span class="sxs-lookup"><span data-stu-id="76e14-136">Users</span></span>

<span data-ttu-id="76e14-137">웹 사이트의 등록 된 사용자입니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-137">Registered users of your web site.</span></span> <span data-ttu-id="76e14-138">[IdentityUser](/aspnet/core/api/microsoft.aspnet.identity.corecompat.identityuser) 형식을 확장 하거나 사용자 고유의 사용자 지정 형식에 대 한 예제로 사용 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-138">The [IdentityUser](/aspnet/core/api/microsoft.aspnet.identity.corecompat.identityuser) type may be extended or used as an example for your own custom type.</span></span> <span data-ttu-id="76e14-139">사용자 지정 id 저장소 솔루션을 구현 하는 특정 형식에서 상속할 필요가 없습니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-139">You don't need to inherit from a particular type to implement your own custom identity storage solution.</span></span>

### <a name="user-claims"></a><span data-ttu-id="76e14-140">사용자 클레임</span><span class="sxs-lookup"><span data-stu-id="76e14-140">User Claims</span></span>

<span data-ttu-id="76e14-141">문 집합 (또는 [클레임](/dotnet/api/system.security.claims.claim))는 사용자의 id를 나타내는 사용자에 대 한 합니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-141">A set of statements (or [Claims](/dotnet/api/system.security.claims.claim)) about the user that represent the user's identity.</span></span> <span data-ttu-id="76e14-142">더 나은 식의 역할을 통해 액세스가 가능 보다는 사용자의 id 사용 하도록 설정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-142">Can enable greater expression of the user's identity than can be achieved through roles.</span></span>

### <a name="user-logins"></a><span data-ttu-id="76e14-143">사용자 로그인</span><span class="sxs-lookup"><span data-stu-id="76e14-143">User Logins</span></span>

<span data-ttu-id="76e14-144">외부 인증 공급자 (예: Facebook 또는 Microsoft 계정)에 대 한 정보는 사용자를 로그인 할 때 사용 하도록 합니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-144">Information about the external authentication provider (like Facebook or a Microsoft account) to use when logging in a user.</span></span> [<span data-ttu-id="76e14-145">예제</span><span class="sxs-lookup"><span data-stu-id="76e14-145">Example</span></span>](/aspnet/core/api/microsoft.aspnet.identity.corecompat.identityuserlogin)

### <a name="roles"></a><span data-ttu-id="76e14-146">역할</span><span class="sxs-lookup"><span data-stu-id="76e14-146">Roles</span></span>

<span data-ttu-id="76e14-147">사이트에 대 한 권한 부여 그룹입니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-147">Authorization groups for your site.</span></span> <span data-ttu-id="76e14-148">역할 Id 및 역할 이름 (예: "Admin" 또는 "Employee")를 포함합니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-148">Includes the role Id and role name (like "Admin" or "Employee").</span></span> [<span data-ttu-id="76e14-149">예제</span><span class="sxs-lookup"><span data-stu-id="76e14-149">Example</span></span>](/aspnet/core/api/microsoft.aspnet.identity.corecompat.identityrole)

## <a name="the-data-access-layer"></a><span data-ttu-id="76e14-150">데이터 액세스 계층</span><span class="sxs-lookup"><span data-stu-id="76e14-150">The data access layer</span></span>

<span data-ttu-id="76e14-151">이 항목에서는 사용 하고자 하는 지 속성 메커니즘 및 해당 메커니즘에 대 한 엔터티를 만드는 방법에 익숙한 가정 합니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-151">This topic assumes you are familiar with the persistence mechanism that you are going to use and how to create entities for that mechanism.</span></span> <span data-ttu-id="76e14-152">이 항목을 저장소 또는 데이터 액세스 클래스; 만드는 방법에 대 한 세부 정보를 제공 하지 않습니다. ASP.NET Core Id 작업할 때 디자인 결정에 대 한 몇 가지 제안 사항 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-152">This topic doesn't provide details about how to create the repositories or data access classes; it provides some suggestions about design decisions when working with ASP.NET Core Identity.</span></span>

<span data-ttu-id="76e14-153">사용자 지정 된 저장소 공급자에 대 한 데이터 액세스 계층을 디자인할 때 원하는 대로 많은 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-153">You have a lot of freedom when designing the data access layer for a customized store provider.</span></span> <span data-ttu-id="76e14-154">응용 프로그램에서 사용 하려는 기능에 대 한 지 속성 메커니즘을 만드는 하기만 하면 됩니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-154">You only need to create persistence mechanisms for features that you intend to use in your app.</span></span> <span data-ttu-id="76e14-155">예를 들어 앱에서 역할을 사용 하지 않는 경우에 역할 또는 사용자 역할 연결에 대 한 저장소를 만들 필요가 없습니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-155">For example, if you are not using roles in your app, you don't need to create storage for roles or user role associations.</span></span> <span data-ttu-id="76e14-156">기술 및 기존 인프라에는 ASP.NET Core Id의 기본 구현에서 매우 다른 구조가 필요할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-156">Your technology and existing infrastructure may require a structure that's very different from the default implementation of ASP.NET Core Identity.</span></span> <span data-ttu-id="76e14-157">데이터 액세스 계층, 저장소 구현 구조와 함께 작동 하는 논리를 제공할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-157">In your data access layer, you provide the logic to work with the structure of your storage implementation.</span></span>

<span data-ttu-id="76e14-158">데이터 액세스 계층을 데이터 소스에서 ASP.NET Core Id 데이터를 저장 하는 논리를 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-158">The data access layer provides the logic to save the data from ASP.NET Core Identity to a data source.</span></span> <span data-ttu-id="76e14-159">사용자 지정 된 저장소 공급자에 대 한 데이터 액세스 계층에는 사용자 및 역할 정보를 저장 하는 다음 클래스가 포함 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-159">The data access layer for your customized storage provider might include the following classes to store user and role information.</span></span>

### <a name="context-class"></a><span data-ttu-id="76e14-160">Context 클래스</span><span class="sxs-lookup"><span data-stu-id="76e14-160">Context class</span></span>

<span data-ttu-id="76e14-161">지 속성 메커니즘에 연결 하 고 쿼리를 실행 하는 정보를 캡슐화 합니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-161">Encapsulates the information to connect to your persistence mechanism and execute queries.</span></span> <span data-ttu-id="76e14-162">종속성 주입을 통해 일반적으로 제공 되는이 클래스의 인스턴스를 필요로 하는 여러 데이터 클래스입니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-162">Several data classes require an instance of this class, typically provided through dependency injection.</span></span> <span data-ttu-id="76e14-163">[예제](/aspnet/core/api/microsoft.aspnet.identity.corecompat.identitydbcontext-1)합니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-163">[Example](/aspnet/core/api/microsoft.aspnet.identity.corecompat.identitydbcontext-1).</span></span>

### <a name="user-storage"></a><span data-ttu-id="76e14-164">사용자 저장소</span><span class="sxs-lookup"><span data-stu-id="76e14-164">User Storage</span></span>

<span data-ttu-id="76e14-165">저장 하 고 사용자 정보 (예: 사용자 이름 및 암호 해시)를 검색 합니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-165">Stores and retrieves user information (such as user name and password hash).</span></span> [<span data-ttu-id="76e14-166">예제</span><span class="sxs-lookup"><span data-stu-id="76e14-166">Example</span></span>](/aspnet/core/api/microsoft.aspnet.identity.corecompat.userstore-1)

### <a name="role-storage"></a><span data-ttu-id="76e14-167">역할 저장소</span><span class="sxs-lookup"><span data-stu-id="76e14-167">Role Storage</span></span>

<span data-ttu-id="76e14-168">저장 하 고 (예: 역할 이름) 역할 정보를 검색 합니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-168">Stores and retrieves role information (such as the role name).</span></span> [<span data-ttu-id="76e14-169">예제</span><span class="sxs-lookup"><span data-stu-id="76e14-169">Example</span></span>](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.rolestore-1)

### <a name="userclaims-storage"></a><span data-ttu-id="76e14-170">UserClaims 저장소</span><span class="sxs-lookup"><span data-stu-id="76e14-170">UserClaims Storage</span></span>

<span data-ttu-id="76e14-171">저장 하 고 사용자 클레임 정보 (예: 클레임 유형 및 값)를 검색 합니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-171">Stores and retrieves user claim information (such as the claim type and value).</span></span> [<span data-ttu-id="76e14-172">예제</span><span class="sxs-lookup"><span data-stu-id="76e14-172">Example</span></span>](/aspnet/core/api/microsoft.aspnet.identity.corecompat.userstore-1)

### <a name="userlogins-storage"></a><span data-ttu-id="76e14-173">UserLogins 저장소</span><span class="sxs-lookup"><span data-stu-id="76e14-173">UserLogins Storage</span></span>

<span data-ttu-id="76e14-174">저장 하 고 (예: 외부 인증 공급자 사용 하 는) 사용자 로그인 정보를 검색 합니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-174">Stores and retrieves user login information (such as an external authentication provider).</span></span> [<span data-ttu-id="76e14-175">예제</span><span class="sxs-lookup"><span data-stu-id="76e14-175">Example</span></span>](/aspnet/core/api/microsoft.aspnet.identity.corecompat.userstore-1)

### <a name="userrole-storage"></a><span data-ttu-id="76e14-176">UserRole 저장소</span><span class="sxs-lookup"><span data-stu-id="76e14-176">UserRole Storage</span></span>

<span data-ttu-id="76e14-177">저장 하 고 있는 사용자에 게 할당 된 역할을 검색 합니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-177">Stores and retrieves which roles are assigned to which users.</span></span> [<span data-ttu-id="76e14-178">예제</span><span class="sxs-lookup"><span data-stu-id="76e14-178">Example</span></span>](/aspnet/core/api/microsoft.aspnet.identity.corecompat.userstore-1)

<span data-ttu-id="76e14-179">**팁:** 만 응용 프로그램에서 사용 하려는 클래스를 구현 합니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-179">**TIP:** Only implement the classes you intend to use in your app.</span></span>

<span data-ttu-id="76e14-180">데이터 액세스 클래스에서 지 속성 메커니즘에 대 한 데이터 작업을 수행 하는 코드를 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-180">In the data access classes, provide code to perform data operations for your persistence mechanism.</span></span> <span data-ttu-id="76e14-181">예를 들어 사용자 지정 공급자 내에서 할 수 있습니다에 새 사용자를 만들려면 다음 코드는 *저장* 클래스:</span><span class="sxs-lookup"><span data-stu-id="76e14-181">For example, within a custom provider, you might have the following code to create a new user in the *store* class:</span></span>

[!code-csharp[](identity-custom-storage-providers/sample/CustomIdentityProviderSample/CustomProvider/CustomUserStore.cs?name=createuser&highlight=7)]

<span data-ttu-id="76e14-182">사용자 만들기에 대 한 구현 논리에는 `_usersTable.CreateAsync` 메서드를 아래에 표시 합니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-182">The implementation logic for creating the user is in the `_usersTable.CreateAsync` method, shown below.</span></span>

## <a name="customize-the-user-class"></a><span data-ttu-id="76e14-183">사용자 클래스를 사용자 지정</span><span class="sxs-lookup"><span data-stu-id="76e14-183">Customize the user class</span></span>

<span data-ttu-id="76e14-184">저장소 공급자를 구현할 때에 사용자 클래스를 만듭니다는 [ `IdentityUser` 클래스](/aspnet/core/api/microsoft.aspnet.identity.corecompat.identityuser)합니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-184">When implementing a storage provider, create a user class which is equivalent to the [`IdentityUser` class](/aspnet/core/api/microsoft.aspnet.identity.corecompat.identityuser).</span></span>

<span data-ttu-id="76e14-185">여기에 최소한 사용자 클래스에 포함 해야 합니다는 `Id` 및 `UserName` 속성입니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-185">At a minimum, your user class must include an `Id` and a `UserName` property.</span></span>

<span data-ttu-id="76e14-186">`IdentityUser` 클래스 속성을 정의 하는 `UserManager` 호출을 수행할 때 요청 작업입니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-186">The `IdentityUser` class defines the properties that the `UserManager` calls when performing requested operations.</span></span> <span data-ttu-id="76e14-187">기본 유형을 `Id` 속성은 문자열 이지만에서 상속할 수 있습니다 `IdentityUser<TKey, TUserClaim, TUserRole, TUserLogin, TUserToken>` 하 고 다른 형식을 지정 합니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-187">The default type of the `Id` property is a string, but you can inherit from `IdentityUser<TKey, TUserClaim, TUserRole, TUserLogin, TUserToken>` and specify a different type.</span></span> <span data-ttu-id="76e14-188">프레임 워크는 데이터 형식 변환을 처리 하는 저장소 구현이 필요 합니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-188">The framework expects the storage implementation to handle data type conversions.</span></span>

## <a name="customize-the-user-store"></a><span data-ttu-id="76e14-189">사용자 저장소에 사용자 지정</span><span class="sxs-lookup"><span data-stu-id="76e14-189">Customize the user store</span></span>

<span data-ttu-id="76e14-190">만들기는 `UserStore` 사용자에 대 한 모든 데이터 작업에 대 한 메서드를 제공 하는 클래스입니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-190">Create a `UserStore` class that provides the methods for all data operations on the user.</span></span> <span data-ttu-id="76e14-191">이 클래스는 해당 하는 [UserStore<TUser> ](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.userstore-1) 클래스입니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-191">This class is equivalent to the [UserStore<TUser>](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.userstore-1) class.</span></span> <span data-ttu-id="76e14-192">사용자 `UserStore` 클래스를 구현 `IUserStore<TUser>` 및 필요한 선택적 인터페이스입니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-192">In your `UserStore` class, implement `IUserStore<TUser>` and the optional interfaces required.</span></span> <span data-ttu-id="76e14-193">응용 프로그램에서 제공 하는 기능에 따라 구현 해야 하는 선택적 인터페이스를 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-193">You select which optional interfaces to implement based on the functionality provided in your app.</span></span>

### <a name="optional-interfaces"></a><span data-ttu-id="76e14-194">선택적 인터페이스</span><span class="sxs-lookup"><span data-stu-id="76e14-194">Optional interfaces</span></span>

- <span data-ttu-id="76e14-195">IUserRoleStore /dotnet/api/microsoft.aspnetcore.identity.iuserrolestore-1</span><span class="sxs-lookup"><span data-stu-id="76e14-195">IUserRoleStore /dotnet/api/microsoft.aspnetcore.identity.iuserrolestore-1</span></span>
- <span data-ttu-id="76e14-196">IUserClaimStore /dotnet/api/microsoft.aspnetcore.identity.iuserclaimstore-1</span><span class="sxs-lookup"><span data-stu-id="76e14-196">IUserClaimStore /dotnet/api/microsoft.aspnetcore.identity.iuserclaimstore-1</span></span>
- <span data-ttu-id="76e14-197">S w /dotnet/api/microsoft.aspnetcore.identity.iuserpasswordstore-1</span><span class="sxs-lookup"><span data-stu-id="76e14-197">IUserPasswordStore /dotnet/api/microsoft.aspnetcore.identity.iuserpasswordstore-1</span></span>
- <span data-ttu-id="76e14-198">IUserSecurityStampStore <!-- make these all links and remove / --></span><span class="sxs-lookup"><span data-stu-id="76e14-198">IUserSecurityStampStore <!-- make these all links and remove / --></span></span>
- <span data-ttu-id="76e14-199">O r e</span><span class="sxs-lookup"><span data-stu-id="76e14-199">IUserEmailStore</span></span>
- <span data-ttu-id="76e14-200">IPhoneNumberStore</span><span class="sxs-lookup"><span data-stu-id="76e14-200">IPhoneNumberStore</span></span>
- <span data-ttu-id="76e14-201">IQueryableUserStore</span><span class="sxs-lookup"><span data-stu-id="76e14-201">IQueryableUserStore</span></span>
- <span data-ttu-id="76e14-202">IUserLoginStore</span><span class="sxs-lookup"><span data-stu-id="76e14-202">IUserLoginStore</span></span>
- <span data-ttu-id="76e14-203">IUserTwoFactorStore</span><span class="sxs-lookup"><span data-stu-id="76e14-203">IUserTwoFactorStore</span></span>
- <span data-ttu-id="76e14-204">IUserLockoutStore</span><span class="sxs-lookup"><span data-stu-id="76e14-204">IUserLockoutStore</span></span>

<span data-ttu-id="76e14-205">선택적 인터페이스에서 상속 하므로 `IUserStore`합니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-205">The optional interfaces inherit from `IUserStore`.</span></span> <span data-ttu-id="76e14-206">저장 부분적으로 구현 된 샘플 사용자를 볼 수 있습니다 [여기](https://github.com/aspnet/Docs/blob/master/aspnetcore/security/authentication/identity-custom-storage-providers/sample/CustomIdentityProviderSample/CustomProvider/CustomUserStore.cs)합니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-206">You can see a partially implemented sample user store [here](https://github.com/aspnet/Docs/blob/master/aspnetcore/security/authentication/identity-custom-storage-providers/sample/CustomIdentityProviderSample/CustomProvider/CustomUserStore.cs).</span></span>

<span data-ttu-id="76e14-207">내에서 `UserStore` 작업을 수행 하기 위해 만든 데이터 액세스 클래스를 사용할 클래스입니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-207">Within the `UserStore` class, you use the data access classes that you created to perform operations.</span></span> <span data-ttu-id="76e14-208">이러한 종속성 주입을 사용 하 여 전달 됩니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-208">These are passed in using dependency injection.</span></span> <span data-ttu-id="76e14-209">예를 들어 Dapper 구현으로 SQL Server에에서는 `UserStore` 클래스에는 `CreateAsync` 의 인스턴스를 사용 하는 메서드 `DapperUsersTable` 새 레코드를 삽입:</span><span class="sxs-lookup"><span data-stu-id="76e14-209">For example, in the SQL Server with Dapper implementation, the `UserStore` class has the `CreateAsync` method which uses an instance of `DapperUsersTable` to insert a new record:</span></span>

[!code-csharp[](identity-custom-storage-providers/sample/CustomIdentityProviderSample/CustomProvider/DapperUsersTable.cs?name=createuser&highlight=7)]

### <a name="interfaces-to-implement-when-customizing-user-store"></a><span data-ttu-id="76e14-210">사용자 저장소에 사용자 지정 하는 경우 구현 하는 인터페이스</span><span class="sxs-lookup"><span data-stu-id="76e14-210">Interfaces to implement when customizing user store</span></span>

- <span data-ttu-id="76e14-211">**IUserStore**</span><span class="sxs-lookup"><span data-stu-id="76e14-211">**IUserStore**</span></span>  
 <span data-ttu-id="76e14-212">[IUserStore&lt;s e r&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iuserstore-1) 인터페이스는 인터페이스는 사용자 저장소에 구현 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-212">The [IUserStore&lt;TUser&gt;](/dotnet/api/microsoft.aspnetcore.identity.iuserstore-1) interface is the only interface you must implement in the user store.</span></span> <span data-ttu-id="76e14-213">만들기, 업데이트, 삭제 및 사용자를 검색 하기 위한 메서드를 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-213">It defines methods for creating, updating, deleting, and retrieving users.</span></span>
- <span data-ttu-id="76e14-214">**IUserClaimStore**</span><span class="sxs-lookup"><span data-stu-id="76e14-214">**IUserClaimStore**</span></span>  
 <span data-ttu-id="76e14-215">[IUserClaimStore&lt;s e r&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iuserclaimstore-1) 인터페이스 사용자 클레임을 사용 하도록 설정 하기 위해 구현할 메서드를 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-215">The [IUserClaimStore&lt;TUser&gt;](/dotnet/api/microsoft.aspnetcore.identity.iuserclaimstore-1) interface defines the methods you implement to enable user claims.</span></span> <span data-ttu-id="76e14-216">추가, 제거 및 사용자 클레임을 검색 하기 위한 메서드를 포함 합니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-216">It contains methods for adding, removing and retrieving user claims.</span></span>
- <span data-ttu-id="76e14-217">**IUserLoginStore**</span><span class="sxs-lookup"><span data-stu-id="76e14-217">**IUserLoginStore**</span></span>  
 <span data-ttu-id="76e14-218">[IUserLoginStore&lt;s e r&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iuserloginstore-1) 외부 인증 공급자를 사용 하도록 설정 하기 위해 구현할 메서드를 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-218">The [IUserLoginStore&lt;TUser&gt;](/dotnet/api/microsoft.aspnetcore.identity.iuserloginstore-1) defines the methods you implement to enable external authentication providers.</span></span> <span data-ttu-id="76e14-219">추가, 제거 및 사용자 로그인 및 로그인 정보에 따라 사용자를 검색 하기 위한 메서드를 검색 하기 위한 메서드를 포함 합니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-219">It contains methods for adding, removing and retrieving user logins, and a method for retrieving a user based on the login information.</span></span>
- <span data-ttu-id="76e14-220">**IUserRoleStore**</span><span class="sxs-lookup"><span data-stu-id="76e14-220">**IUserRoleStore**</span></span>  
 <span data-ttu-id="76e14-221">[IUserRoleStore&lt;s e r&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iuserrolestore-1) 인터페이스 역할에 사용자에 매핑하기 위해 구현할 메서드를 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-221">The [IUserRoleStore&lt;TUser&gt;](/dotnet/api/microsoft.aspnetcore.identity.iuserrolestore-1) interface defines the methods you implement to map a user to a role.</span></span> <span data-ttu-id="76e14-222">추가, 제거 및 사용자의 역할 및 사용자를 역할에 할당 된 경우를 확인 하는 메서드를 검색 하는 메서드를 포함 합니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-222">It contains methods to add, remove, and retrieve a user's roles, and a method to check if a user is assigned to a role.</span></span>
- <span data-ttu-id="76e14-223">**IUserPasswordStore**</span><span class="sxs-lookup"><span data-stu-id="76e14-223">**IUserPasswordStore**</span></span>  
 <span data-ttu-id="76e14-224">[s w&lt;s e r&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iuserpasswordstore-1) 인터페이스 해시 된 암호를 유지 하기 위해 구현할 메서드를 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-224">The [IUserPasswordStore&lt;TUser&gt;](/dotnet/api/microsoft.aspnetcore.identity.iuserpasswordstore-1) interface defines the methods you implement to persist hashed passwords.</span></span> <span data-ttu-id="76e14-225">가져오기 및 해시 된 암호와 사용자가 암호를 설정 하는지 여부를 나타내는 방법을 설정 하는 메서드를 포함 합니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-225">It contains methods for getting and setting the hashed password, and a method that indicates whether the user has set a password.</span></span>
- <span data-ttu-id="76e14-226">**IUserSecurityStampStore**</span><span class="sxs-lookup"><span data-stu-id="76e14-226">**IUserSecurityStampStore**</span></span>  
 <span data-ttu-id="76e14-227">[IUserSecurityStampStore&lt;s e r&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iusersecuritystampstore-1) 인터페이스를 사용자의 계정 정보가 변경 되었는지 여부를 나타내는 보안 스탬프를 사용 하기 위해 구현할 메서드를 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-227">The [IUserSecurityStampStore&lt;TUser&gt;](/dotnet/api/microsoft.aspnetcore.identity.iusersecuritystampstore-1) interface defines the methods you implement to use a security stamp for indicating whether the user's account information has changed.</span></span> <span data-ttu-id="76e14-228">사용자의 암호 변경 또는 추가 하거나 로그인을 제거 하는 경우이 스탬프 업데이트 됩니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-228">This stamp is updated when a user changes the password, or adds or removes logins.</span></span> <span data-ttu-id="76e14-229">가져오기 및 설정의 보안 스탬프에 대 한 메서드를 포함 합니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-229">It contains methods for getting and setting the security stamp.</span></span>
- <span data-ttu-id="76e14-230">**IUserTwoFactorStore**</span><span class="sxs-lookup"><span data-stu-id="76e14-230">**IUserTwoFactorStore**</span></span>  
 <span data-ttu-id="76e14-231">[t w&lt;s e r&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iusertwofactorstore-1) 인터페이스 2 단계 인증을 지원 하기 위해 구현할 메서드를 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-231">The [IUserTwoFactorStore&lt;TUser&gt;](/dotnet/api/microsoft.aspnetcore.identity.iusertwofactorstore-1) interface defines the methods you implement to support two factor authentication.</span></span> <span data-ttu-id="76e14-232">가져오기 및 사용자에 대해 2 단계 인증이 사용 되는지 여부를 설정 하는 메서드를 포함 합니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-232">It contains methods for getting and setting whether two factor authentication is enabled for a user.</span></span>
- <span data-ttu-id="76e14-233">**IUserPhoneNumberStore**</span><span class="sxs-lookup"><span data-stu-id="76e14-233">**IUserPhoneNumberStore**</span></span>  
 <span data-ttu-id="76e14-234">[p h&lt;s e r&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iuserphonenumberstore-1) 인터페이스 사용자 전화 번호를 저장 하기 위해 구현할 메서드를 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-234">The [IUserPhoneNumberStore&lt;TUser&gt;](/dotnet/api/microsoft.aspnetcore.identity.iuserphonenumberstore-1) interface defines the methods you implement to store user phone numbers.</span></span> <span data-ttu-id="76e14-235">가져오기 및 전화 번호와 전화 번호가 확인 되었는지 여부를 설정 하는 메서드를 포함 합니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-235">It contains methods for getting and setting the phone number and whether the phone number is confirmed.</span></span>
- <span data-ttu-id="76e14-236">**IUserEmailStore**</span><span class="sxs-lookup"><span data-stu-id="76e14-236">**IUserEmailStore**</span></span>  
 <span data-ttu-id="76e14-237">[o r e&lt;s e r&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iuseremailstore-1) 인터페이스 사용자 전자 메일 주소를 저장 하기 위해 구현할 메서드를 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-237">The [IUserEmailStore&lt;TUser&gt;](/dotnet/api/microsoft.aspnetcore.identity.iuseremailstore-1) interface defines the methods you implement to store user email addresses.</span></span> <span data-ttu-id="76e14-238">가져오기 및 전자 메일 주소 및 전자 메일이 확인 여부를 설정 하는 메서드를 포함 합니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-238">It contains methods for getting and setting the email address and whether the email is confirmed.</span></span>
- <span data-ttu-id="76e14-239">**IUserLockoutStore**</span><span class="sxs-lookup"><span data-stu-id="76e14-239">**IUserLockoutStore**</span></span>  
 <span data-ttu-id="76e14-240">[o r e&lt;s e r&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iuserlockoutstore-1) 인터페이스 계정 잠금에 대 한 정보를 저장 하기 위해 구현할 메서드를 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-240">The [IUserLockoutStore&lt;TUser&gt;](/dotnet/api/microsoft.aspnetcore.identity.iuserlockoutstore-1) interface defines the methods you implement to store information about locking an account.</span></span> <span data-ttu-id="76e14-241">실패 한 액세스 시도 횟수와 잠금 추적을 위한 메서드를 포함 합니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-241">It contains methods for tracking failed access attempts and lockouts.</span></span>
- <span data-ttu-id="76e14-242">**IQueryableUserStore**</span><span class="sxs-lookup"><span data-stu-id="76e14-242">**IQueryableUserStore**</span></span>  
 <span data-ttu-id="76e14-243">[i q&lt;s e r&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iqueryableuserstore-1) 인터페이스 멤버 구현 쿼리 가능한 사용자 저장소를 제공 하도록 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-243">The [IQueryableUserStore&lt;TUser&gt;](/dotnet/api/microsoft.aspnetcore.identity.iqueryableuserstore-1) interface defines the members implement to provide a queryable user store.</span></span>

<span data-ttu-id="76e14-244">앱에서 필요한는 인터페이스를 구현 합니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-244">You implement only the interfaces that are needed in your app.</span></span> <span data-ttu-id="76e14-245">예를 들어:</span><span class="sxs-lookup"><span data-stu-id="76e14-245">For example:</span></span>

```csharp
public class UserStore : IUserStore<IdentityUser>,
                         IUserClaimStore<IdentityUser>,
                         IUserLoginStore<IdentityUser>,
                         IUserRoleStore<IdentityUser>,
                         IUserPasswordStore<IdentityUser>,
                         IUserSecurityStampStore<IdentityUser>
{
    // interface implementations not shown
}
```

### <a name="identityuserclaim-identityuserlogin-and-identityuserrole"></a><span data-ttu-id="76e14-246">IdentityUserClaim, IdentityUserLogin, 및 IdentityUserRole</span><span class="sxs-lookup"><span data-stu-id="76e14-246">IdentityUserClaim, IdentityUserLogin, and IdentityUserRole</span></span>

<span data-ttu-id="76e14-247">`Microsoft.AspNet.Identity.EntityFramework` 네임 스페이스에 구현 된 [IdentityUserClaim](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuserclaim-1), [IdentityUserLogin](/aspnet/core/api/microsoft.aspnet.identity.corecompat.identityuserlogin), 및 [IdentityUserRole](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuserrole-1) 클래스입니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-247">The `Microsoft.AspNet.Identity.EntityFramework` namespace contains implementations of the [IdentityUserClaim](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuserclaim-1), [IdentityUserLogin](/aspnet/core/api/microsoft.aspnet.identity.corecompat.identityuserlogin), and [IdentityUserRole](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuserrole-1) classes.</span></span> <span data-ttu-id="76e14-248">이러한 기능을 사용 하는 경우에 이러한 클래스의 사용자가 자체 버전을 만들고 응용 프로그램에 대 한 속성을 정의 하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-248">If you are using these features, you may want to create your own versions of these classes and define the properties for your app.</span></span> <span data-ttu-id="76e14-249">그러나 때로는 것이 효율적 기본 작업 (예: 추가 또는 제거 하는 사용자의 클레임)를 수행할 때 이러한 엔터티를 메모리에 로드 하지 않도록 합니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-249">However, sometimes it's more efficient to not load these entities into memory when performing basic operations (such as adding or removing a user's claim).</span></span> <span data-ttu-id="76e14-250">대신, 백 엔드 저장소 클래스는 데이터 원본에 직접 이러한 작업을 실행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-250">Instead, the backend store classes can execute these operations directly on the data source.</span></span> <span data-ttu-id="76e14-251">예를 들어는 `UserStore.GetClaimsAsync` 메서드를 호출할 수는 `userClaimTable.FindByUserId(user.Id)` 에 대 한 쿼리를 실행 하는 메서드를 직접 테이블 클레임의 목록을 반환 합니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-251">For example, the `UserStore.GetClaimsAsync` method can call the `userClaimTable.FindByUserId(user.Id)` method to execute a query on that table directly and return a list of claims.</span></span>

## <a name="customize-the-role-class"></a><span data-ttu-id="76e14-252">Role 클래스를 사용자 지정</span><span class="sxs-lookup"><span data-stu-id="76e14-252">Customize the role class</span></span>

<span data-ttu-id="76e14-253">역할 저장소 공급자를 구현할 때 사용자 지정 역할 유형을 만들 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-253">When implementing a role storage provider, you can create a custom role type.</span></span> <span data-ttu-id="76e14-254">특정 인터페이스를 구현 하지 필요 하지만 있어야는 `Id` 갖습니다 일반적으로 `Name` 속성입니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-254">It need not implement a particular interface, but it must have an `Id` and typically it will have a `Name` property.</span></span>

<span data-ttu-id="76e14-255">다음은 예제 역할 클래스입니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-255">The following is an example role class:</span></span>

[!code-csharp[](identity-custom-storage-providers/sample/CustomIdentityProviderSample/CustomProvider/ApplicationRole.cs)]

## <a name="customize-the-role-store"></a><span data-ttu-id="76e14-256">역할 저장소의 사용자 지정</span><span class="sxs-lookup"><span data-stu-id="76e14-256">Customize the role store</span></span>

<span data-ttu-id="76e14-257">만들 수는 `RoleStore` 역할에 대 한 모든 데이터 작업에 대 한 메서드를 제공 하는 클래스입니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-257">You can create a `RoleStore` class that provides the methods for all data operations on roles.</span></span> <span data-ttu-id="76e14-258">이 클래스는 해당 하는 [RoleStore<TRole> ](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.rolestore-1) 클래스입니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-258">This class is equivalent to the [RoleStore<TRole>](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.rolestore-1) class.</span></span> <span data-ttu-id="76e14-259">에 `RoleStore` 구현 클래스는 `IRoleStore<TRole>` 하 고 선택적으로 `IQueryableRoleStore<TRole>` 인터페이스입니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-259">In the `RoleStore` class, you implement the `IRoleStore<TRole>` and optionally the `IQueryableRoleStore<TRole>` interface.</span></span>

- <span data-ttu-id="76e14-260">**IRoleStore&lt;TRole&gt;**</span><span class="sxs-lookup"><span data-stu-id="76e14-260">**IRoleStore&lt;TRole&gt;**</span></span>  
 <span data-ttu-id="76e14-261">[IRoleStore](/dotnet/api/microsoft.aspnetcore.identity.irolestore-1) 인터페이스 역할 저장소 클래스에서 구현 하는 메서드를 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-261">The [IRoleStore](/dotnet/api/microsoft.aspnetcore.identity.irolestore-1) interface defines the methods to implement in the role store class.</span></span> <span data-ttu-id="76e14-262">만들기, 업데이트, 삭제 및 역할을 검색 하는 메서드를 포함 합니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-262">It contains methods for creating, updating, deleting and retrieving roles.</span></span>
- <span data-ttu-id="76e14-263">**RoleStore&lt;TRole&gt;**</span><span class="sxs-lookup"><span data-stu-id="76e14-263">**RoleStore&lt;TRole&gt;**</span></span>  
 <span data-ttu-id="76e14-264">사용자 지정 하려면 `RoleStore`를 구현 하는 클래스를 만들는 `IRoleStore` 인터페이스입니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-264">To customize `RoleStore`, create a class that implements the `IRoleStore` interface.</span></span> 

## <a name="reconfigure-app-to-use-new-storage-provider"></a><span data-ttu-id="76e14-265">새 저장소 공급자를 사용 하도록 응용 프로그램을 다시 구성</span><span class="sxs-lookup"><span data-stu-id="76e14-265">Reconfigure app to use new storage provider</span></span>

<span data-ttu-id="76e14-266">저장소 공급자를 구현한 후에 앱 사용을 구성 합니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-266">Once you have implemented a storage provider, you configure your app to use it.</span></span> <span data-ttu-id="76e14-267">응용 프로그램의 기본 공급자를 사용 하는 경우 사용자 지정 공급자로 바꿉니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-267">If your app used the default provider, replace it with your custom provider.</span></span>

1. <span data-ttu-id="76e14-268">제거는 `Microsoft.AspNetCore.EntityFramework.Identity` NuGet 패키지 합니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-268">Remove the `Microsoft.AspNetCore.EntityFramework.Identity` NuGet package.</span></span>
1. <span data-ttu-id="76e14-269">저장소 공급자와는 별도 프로젝트 또는 패키지에 있는 경우에 대 한 참조를 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-269">If the storage provider resides in a separate project or package, add a reference to it.</span></span>
1. <span data-ttu-id="76e14-270">에 대 한 모든 참조 바꿉니다 `Microsoft.AspNetCore.EntityFramework.Identity` 을 사용 하 여 저장소 공급자의 네임 스페이스에 대 한 문입니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-270">Replace all references to `Microsoft.AspNetCore.EntityFramework.Identity` with a using statement for the namespace of your storage provider.</span></span>
1. <span data-ttu-id="76e14-271">에 `ConfigureServices` change 메서드에 `AddIdentity` 사용자 지정 형식을 사용 하는 메서드.</span><span class="sxs-lookup"><span data-stu-id="76e14-271">In the `ConfigureServices` method, change the `AddIdentity` method to use your custom types.</span></span> <span data-ttu-id="76e14-272">이 용도 대 한 확장 메서드를 만들 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-272">You can create your own extension methods for this purpose.</span></span> <span data-ttu-id="76e14-273">참조 [IdentityServiceCollectionExtensions](https://github.com/aspnet/Identity/blob/rel/1.1.0/src/Microsoft.AspNetCore.Identity/IdentityServiceCollectionExtensions.cs) 예에 대 한 합니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-273">See [IdentityServiceCollectionExtensions](https://github.com/aspnet/Identity/blob/rel/1.1.0/src/Microsoft.AspNetCore.Identity/IdentityServiceCollectionExtensions.cs) for an example.</span></span>
1. <span data-ttu-id="76e14-274">역할을 사용 하는 업데이트는 `RoleManager` 사용 하 여 `RoleStore` 클래스입니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-274">If you are using Roles, update the `RoleManager` to use your `RoleStore` class.</span></span>
1. <span data-ttu-id="76e14-275">응용 프로그램의 구성에는 연결 문자열 및 자격 증명을 업데이트 합니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-275">Update the connection string and credentials to your app's configuration.</span></span>

<span data-ttu-id="76e14-276">예제:</span><span class="sxs-lookup"><span data-stu-id="76e14-276">Example:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add identity types
    services.AddIdentity<ApplicationUser, ApplicationRole>()
        .AddDefaultTokenProviders();

    // Identity Services
    services.AddTransient<IUserStore<ApplicationUser>, CustomUserStore>();
    services.AddTransient<IRoleStore<ApplicationRole>, CustomRoleStore>();
    string connectionString = Configuration.GetConnectionString("DefaultConnection");
    services.AddTransient<SqlConnection>(e => new SqlConnection(connectionString));
    services.AddTransient<DapperUsersTable>();

    // additional configuration
}
```

## <a name="references"></a><span data-ttu-id="76e14-277">참조</span><span class="sxs-lookup"><span data-stu-id="76e14-277">References</span></span>

- [<span data-ttu-id="76e14-278">ASP.NET Id에 대 한 사용자 지정 저장소 공급자</span><span class="sxs-lookup"><span data-stu-id="76e14-278">Custom Storage Providers for ASP.NET Identity</span></span>](/aspnet/identity/overview/extensibility/overview-of-custom-storage-providers-for-aspnet-identity)
- <span data-ttu-id="76e14-279">[ASP.NET Core Id](https://github.com/aspnet/identity) -이 저장소에 저장소 공급자를 유지 관리 하는 커뮤니티에 대 한 링크를 포함 합니다.</span><span class="sxs-lookup"><span data-stu-id="76e14-279">[ASP.NET Core Identity](https://github.com/aspnet/identity) - This repository includes links to community maintained store providers.</span></span>
