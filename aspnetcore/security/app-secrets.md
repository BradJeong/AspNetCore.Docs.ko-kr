---
title: "ASP.NET Core에서 개발 하는 동안 앱 암호의 안전한 저장소"
author: rick-anderson
description: "안전 하 게 개발 하는 동안 암호를 저장 하는 방법을 보여 줍니다."
manager: wpickett
ms.author: riande
ms.date: 09/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/app-secrets
ms.openlocfilehash: 489c53c066af87e02e43ab0b42b0712d80d5ee5a
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/02/2018
---
# <a name="safe-storage-of-app-secrets-during-development-in-aspnet-core"></a><span data-ttu-id="cf3c0-103">ASP.NET Core에서 개발 하는 동안 앱 암호의 안전한 저장소</span><span class="sxs-lookup"><span data-stu-id="cf3c0-103">Safe storage of app secrets during development in ASP.NET Core</span></span>

<span data-ttu-id="cf3c0-104">여 [Rick Anderson](https://twitter.com/RickAndMSFT), [김 Roth](https://github.com/danroth27), 및 [Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="cf3c0-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), and [Scott Addie](https://scottaddie.com)</span></span> 

<span data-ttu-id="cf3c0-105">이 문서 코드 나가기를 비밀을 유지 하려면 개발에서 암호 관리자 도구를 사용 하는 방법을 보여줍니다.</span><span class="sxs-lookup"><span data-stu-id="cf3c0-105">This document shows how you can use the Secret Manager tool in development to keep secrets out of your code.</span></span> <span data-ttu-id="cf3c0-106">가장 중요 한 점은 소스 코드에서 암호 또는 기타 중요 한 데이터를 저장 하지 않으며 해야 하 고 개발 및 테스트 모드에서 프로덕션 암호를 사용 하지 않아야 합니다.</span><span class="sxs-lookup"><span data-stu-id="cf3c0-106">The most important point is you should never store passwords or other sensitive data in source code, and you shouldn't use production secrets in development and test mode.</span></span> <span data-ttu-id="cf3c0-107">대신 사용할 수 있습니다는 [구성](xref:fundamentals/configuration/index) 시스템 환경 변수에서 이러한 값을 읽거나 암호 관리자를 사용 하 여 저장 된 값에서 도구입니다.</span><span class="sxs-lookup"><span data-stu-id="cf3c0-107">You can instead use the [configuration](xref:fundamentals/configuration/index) system to read these values from environment variables or from values stored using the Secret Manager tool.</span></span> <span data-ttu-id="cf3c0-108">암호 관리자 도구에서 소스 제어에 체크 인 되 고 중요 한 데이터를 방지할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="cf3c0-108">The Secret Manager tool helps prevent sensitive data from being checked into source control.</span></span> <span data-ttu-id="cf3c0-109">[구성](xref:fundamentals/configuration/index) 시스템이이 문서에 설명 된 비밀 관리자 도구와 함께 저장 된 암호를 읽을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="cf3c0-109">The [configuration](xref:fundamentals/configuration/index) system can read secrets stored with the Secret Manager tool described in this article.</span></span>

<span data-ttu-id="cf3c0-110">암호 관리자 도구 개발에만 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="cf3c0-110">The Secret Manager tool is used only in development.</span></span> <span data-ttu-id="cf3c0-111">와 Azure 테스트 및 프로덕션 비밀을 보호할 수는 [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) 구성 공급자입니다.</span><span class="sxs-lookup"><span data-stu-id="cf3c0-111">You can safeguard Azure test and production secrets with the [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) configuration provider.</span></span> <span data-ttu-id="cf3c0-112">참조 [Azure 키 자격 증명 모음 구성 공급자](https://docs.microsoft.com/aspnet/core/security/key-vault-configuration) 자세한 정보에 대 한 합니다.</span><span class="sxs-lookup"><span data-stu-id="cf3c0-112">See [Azure Key Vault configuration provider](https://docs.microsoft.com/aspnet/core/security/key-vault-configuration) for more information.</span></span>

## <a name="environment-variables"></a><span data-ttu-id="cf3c0-113">환경 변수</span><span class="sxs-lookup"><span data-stu-id="cf3c0-113">Environment variables</span></span>

<span data-ttu-id="cf3c0-114">에 로컬 구성 파일 또는 코드에서 앱 암호를 저장 하지 않도록 하려면 환경 변수에서 암호를 저장 합니다.</span><span class="sxs-lookup"><span data-stu-id="cf3c0-114">To avoid storing app secrets in code or in local configuration files, you store secrets in environment variables.</span></span> <span data-ttu-id="cf3c0-115">설정할 수 있습니다는 [구성](xref:fundamentals/configuration/index) 프레임 워크를 호출 하 여 환경 변수에서 값을 읽을 `AddEnvironmentVariables`합니다.</span><span class="sxs-lookup"><span data-stu-id="cf3c0-115">You can setup the [configuration](xref:fundamentals/configuration/index) framework to read values from environment variables by calling `AddEnvironmentVariables`.</span></span> <span data-ttu-id="cf3c0-116">그런 다음 환경 변수를 사용 하 여 모든 이전에 지정 된 구성 소스에 대 한 구성 값을 재정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="cf3c0-116">You can then use environment variables to override configuration values for all previously specified configuration sources.</span></span>

<span data-ttu-id="cf3c0-117">예를 들어, 개별 사용자 계정으로 새 ASP.NET Core 웹 앱을 만들면 기본 연결 문자열을 추가 되는 *appsettings.json* 키를 사용 하 여 프로젝트의 파일 `DefaultConnection`합니다.</span><span class="sxs-lookup"><span data-stu-id="cf3c0-117">For example, if you create a new ASP.NET Core web app with individual user accounts, it will add a default connection string to the *appsettings.json* file in the project with the key `DefaultConnection`.</span></span> <span data-ttu-id="cf3c0-118">기본 연결 문자열에는 사용자 모드에서 실행 되 고 암호가 필요 하지 않습니다는 LocalDB를 사용 하도록 설정 되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="cf3c0-118">The default connection string is setup to use LocalDB, which runs in user mode and doesn't require a password.</span></span> <span data-ttu-id="cf3c0-119">테스트 또는 프로덕션 서버에 응용 프로그램을 배포할 때 재정의할 수 있습니다는 `DefaultConnection` 테스트 또는 프로덕션 데이터베이스에 대 한 연결 문자열 (잠재적으로 중요 한 자격 증명)을 포함 하는 환경 변수 설정을 사용 하 여 키 값 서버입니다.</span><span class="sxs-lookup"><span data-stu-id="cf3c0-119">When you deploy your application to a test or production server, you can override the `DefaultConnection` key value with an environment variable setting that contains the connection string (potentially with sensitive credentials) for a test or production database server.</span></span>

>[!WARNING]
> <span data-ttu-id="cf3c0-120">환경 변수는 일반적으로 일반 텍스트로 저장 되며 암호화 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="cf3c0-120">Environment variables are generally stored in plain text and are not encrypted.</span></span> <span data-ttu-id="cf3c0-121">컴퓨터 또는 프로세스가 손상 된 환경 변수 신뢰할 수 없는 당사자가 액세스할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="cf3c0-121">If the machine or process is compromised, then environment variables can be accessed by untrusted parties.</span></span> <span data-ttu-id="cf3c0-122">여전히 사용자 비밀 정보를 공개 하지 않도록 추가 조치가 필요할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="cf3c0-122">Additional measures to prevent disclosure of user secrets may still be required.</span></span>

## <a name="secret-manager"></a><span data-ttu-id="cf3c0-123">암호 관리자</span><span class="sxs-lookup"><span data-stu-id="cf3c0-123">Secret Manager</span></span>

<span data-ttu-id="cf3c0-124">암호 관리자 도구는 프로젝트 트리 외부의 개발 작업에 대 한 중요 한 데이터를 저장합니다.</span><span class="sxs-lookup"><span data-stu-id="cf3c0-124">The Secret Manager tool stores sensitive data for development work outside of your project tree.</span></span> <span data-ttu-id="cf3c0-125">암호 관리자 도구는에 대 한 기밀 정보를 사용할 수 있는 프로젝트 도구는 [.NET Core](https://www.microsoft.com/net/core) 개발 중 프로젝트.</span><span class="sxs-lookup"><span data-stu-id="cf3c0-125">The Secret Manager tool is a project tool that can be used to store secrets for a [.NET Core](https://www.microsoft.com/net/core) project during development.</span></span> <span data-ttu-id="cf3c0-126">암호 관리자 도구를 사용 특정 프로젝트와 앱 암호를 연결 하 고 여러 프로젝트 간에 공유할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="cf3c0-126">With the Secret Manager tool, you can associate app secrets with a specific project and share them across multiple projects.</span></span>

>[!WARNING]
> <span data-ttu-id="cf3c0-127">암호 관리자 도구 저장 된 암호를 암호화 하지 않습니다 하 고 신뢰할 수 있는 저장소로 처리 하지 않아야 합니다.</span><span class="sxs-lookup"><span data-stu-id="cf3c0-127">The Secret Manager tool doesn't encrypt the stored secrets and shouldn't be treated as a trusted store.</span></span> <span data-ttu-id="cf3c0-128">개발 용도로입니다.</span><span class="sxs-lookup"><span data-stu-id="cf3c0-128">It's for development purposes only.</span></span> <span data-ttu-id="cf3c0-129">키와 값은 사용자 프로필 디렉터리에는 JSON 구성 파일에 저장 됩니다.</span><span class="sxs-lookup"><span data-stu-id="cf3c0-129">The keys and values are stored in a JSON configuration file in the user profile directory.</span></span>

## <a name="installing-the-secret-manager-tool"></a><span data-ttu-id="cf3c0-130">암호 관리자 도구를 설치합니다.</span><span class="sxs-lookup"><span data-stu-id="cf3c0-130">Installing the Secret Manager tool</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="cf3c0-131">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cf3c0-131">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="cf3c0-132">솔루션 탐색기에서 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 선택 **편집 \<project_name\>.csproj** 상황에 맞는 메뉴입니다.</span><span class="sxs-lookup"><span data-stu-id="cf3c0-132">Right-click the project in Solution Explorer, and select **Edit \<project_name\>.csproj** from the context menu.</span></span> <span data-ttu-id="cf3c0-133">강조 표시 된 줄을 추가 *.csproj* 파일과 연결 된 NuGet 패키지를 복원 하려면 저장:</span><span class="sxs-lookup"><span data-stu-id="cf3c0-133">Add the highlighted line to the *.csproj* file, and save to restore the associated NuGet package:</span></span>

[!code-xml[](app-secrets/sample/UserSecrets/UserSecrets-before.csproj?highlight=10)]

<span data-ttu-id="cf3c0-134">솔루션 탐색기에서 프로젝트를 다시 마우스 오른쪽 단추로 클릭 하 고 선택 **관리 사용자의 비밀** 상황에 맞는 메뉴입니다.</span><span class="sxs-lookup"><span data-stu-id="cf3c0-134">Right-click the project in Solution Explorer again, and select **Manage User Secrets** from the context menu.</span></span> <span data-ttu-id="cf3c0-135">이 제스처를 새로 추가 `UserSecretsId` 내에서 노드는 `PropertyGroup` 의 *.csproj* 다음 샘플에 강조 표시 된 대로 파일:</span><span class="sxs-lookup"><span data-stu-id="cf3c0-135">This gesture adds a new `UserSecretsId` node within a `PropertyGroup` of the *.csproj* file, as highlighted in the following sample:</span></span>

[!code-xml[](app-secrets/sample/UserSecrets/UserSecrets-after.csproj?highlight=4)]

<span data-ttu-id="cf3c0-136">수정 된 저장 *.csproj* 파일도 열립니다는 `secrets.json` 파일 텍스트 편집기에서.</span><span class="sxs-lookup"><span data-stu-id="cf3c0-136">Saving the modified *.csproj* file also opens a `secrets.json` file in the text editor.</span></span> <span data-ttu-id="cf3c0-137">내용을 대체는 `secrets.json` 를 다음 코드로 파일:</span><span class="sxs-lookup"><span data-stu-id="cf3c0-137">Replace the contents of the `secrets.json` file with the following code:</span></span>

```json
{
    "MySecret": "ValueOfMySecret"
}
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="cf3c0-138">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="cf3c0-138">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="cf3c0-139">추가 `Microsoft.Extensions.SecretManager.Tools` 에 *.csproj* 파일을 실행 [dotnet 복원](/dotnet/core/tools/dotnet-restore)합니다.</span><span class="sxs-lookup"><span data-stu-id="cf3c0-139">Add `Microsoft.Extensions.SecretManager.Tools` to the *.csproj* file and run [dotnet restore](/dotnet/core/tools/dotnet-restore).</span></span> <span data-ttu-id="cf3c0-140">명령줄을 사용 하 여 암호 관리자 도구를 설치 하려면 동일한 단계를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="cf3c0-140">You can use the same steps to install the Secret Manager Tool using for the command line.</span></span>

[!code-xml[](app-secrets/sample/UserSecrets/UserSecrets-before.csproj?highlight=10)]

<span data-ttu-id="cf3c0-141">다음 명령을 실행 하 여 암호 관리자 도구를 테스트 합니다.</span><span class="sxs-lookup"><span data-stu-id="cf3c0-141">Test the Secret Manager tool by running the following command:</span></span>

```console
dotnet user-secrets -h
```

<span data-ttu-id="cf3c0-142">암호 관리자 도구는 사용, 옵션 및 명령 도움말 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="cf3c0-142">The Secret Manager tool will display usage, options and command help.</span></span>

> [!NOTE]
> <span data-ttu-id="cf3c0-143">와 같은 디렉터리에 있어야는 *.csproj* 파일에 정의 된 도구를 실행 하는 *.csproj* 파일의 `DotNetCliToolReference` 노드.</span><span class="sxs-lookup"><span data-stu-id="cf3c0-143">You must be in the same directory as the *.csproj* file to run tools defined in the *.csproj* file's `DotNetCliToolReference` nodes.</span></span>

<span data-ttu-id="cf3c0-144">암호 관리자 도구는 사용자 프로필에 저장 되어 있는 프로젝트 관련 구성 설정에서 작동 합니다.</span><span class="sxs-lookup"><span data-stu-id="cf3c0-144">The Secret Manager tool operates on project-specific configuration settings that are stored in your user profile.</span></span> <span data-ttu-id="cf3c0-145">사용자 암호를 사용 하려면 프로젝트 지정 해야 합니다는 `UserSecretsId` 에서 값을 해당 *.csproj* 파일입니다.</span><span class="sxs-lookup"><span data-stu-id="cf3c0-145">To use user secrets, the project must specify a `UserSecretsId` value in its *.csproj* file.</span></span> <span data-ttu-id="cf3c0-146">값 `UserSecretsId` 은 선택적 요소 이지만 프로젝트에 일반적으로 고유 합니다.</span><span class="sxs-lookup"><span data-stu-id="cf3c0-146">The value of `UserSecretsId` is arbitrary, but is generally unique to the project.</span></span> <span data-ttu-id="cf3c0-147">에 대 한 GUID를 일반적으로 생성 하는 개발자는 `UserSecretsId`합니다.</span><span class="sxs-lookup"><span data-stu-id="cf3c0-147">Developers typically generate a GUID for the `UserSecretsId`.</span></span>

<span data-ttu-id="cf3c0-148">추가 `UserSecretsId` 에서 프로젝트에 대 한는 *.csproj* 파일:</span><span class="sxs-lookup"><span data-stu-id="cf3c0-148">Add a `UserSecretsId` for your project in the *.csproj* file:</span></span>

[!code-xml[](app-secrets/sample/UserSecrets/UserSecrets-after.csproj?highlight=4)]

<span data-ttu-id="cf3c0-149">암호 관리자 도구를 사용 하 여 암호를 설정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="cf3c0-149">Use the Secret Manager tool to set a secret.</span></span> <span data-ttu-id="cf3c0-150">예를 들어 프로젝트 디렉터리에서 명령 창에서 다음을 입력 합니다.</span><span class="sxs-lookup"><span data-stu-id="cf3c0-150">For example, in a command window from the project directory, enter the following:</span></span>

```console
dotnet user-secrets set MySecret ValueOfMySecret
```

<span data-ttu-id="cf3c0-151">다른 디렉터리에서 암호 관리자 도구를 실행할 수 있지만 사용 해야 합니다는 `--project` 옵션에 대 한 경로 전달 하는 *.csproj* 파일:</span><span class="sxs-lookup"><span data-stu-id="cf3c0-151">You can run the Secret Manager tool from other directories, but you must use the `--project` option to pass in the path to the *.csproj* file:</span></span>
 
```console
dotnet user-secrets set MySecret ValueOfMySecret --project c:\work\WebApp1\src\webapp1
```

<span data-ttu-id="cf3c0-152">또한 나열, 제거 및 앱 암호를 지울 수는 암호 관리자 도구를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="cf3c0-152">You can also use the Secret Manager tool to list, remove and clear app secrets.</span></span>

-----

## <a name="accessing-user-secrets-via-configuration"></a><span data-ttu-id="cf3c0-153">구성을 통해 사용자의 비밀 정보에 액세스</span><span class="sxs-lookup"><span data-stu-id="cf3c0-153">Accessing user secrets via configuration</span></span>

<span data-ttu-id="cf3c0-154">구성 시스템을 통해 보안 관리자 암호에 액세스 합니다.</span><span class="sxs-lookup"><span data-stu-id="cf3c0-154">You access Secret Manager secrets through the configuration system.</span></span> <span data-ttu-id="cf3c0-155">추가 `Microsoft.Extensions.Configuration.UserSecrets` 패키지 및 실행 [dotnet 복원](/dotnet/core/tools/dotnet-restore)합니다.</span><span class="sxs-lookup"><span data-stu-id="cf3c0-155">Add the `Microsoft.Extensions.Configuration.UserSecrets` package and run [dotnet restore](/dotnet/core/tools/dotnet-restore).</span></span>

<span data-ttu-id="cf3c0-156">사용자 암호 구성 소스를 추가 `Startup` 메서드:</span><span class="sxs-lookup"><span data-stu-id="cf3c0-156">Add the user secrets configuration source to the `Startup` method:</span></span>

[!code-csharp[](app-secrets/sample/UserSecrets/Startup.cs?highlight=16-19)]

<span data-ttu-id="cf3c0-157">구성 API 통해 사용자의 비밀을 액세스할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="cf3c0-157">You can access user secrets via the configuration API:</span></span>

[!code-csharp[](app-secrets/sample/UserSecrets/Startup.cs?highlight=26-29)]

## <a name="how-the-secret-manager-tool-works"></a><span data-ttu-id="cf3c0-158">암호 관리자 도구 작동 방식</span><span class="sxs-lookup"><span data-stu-id="cf3c0-158">How the Secret Manager tool works</span></span>

<span data-ttu-id="cf3c0-159">암호 관리자 도구 값을 저장 하는 위치와 방법을 같은 구현 세부 정보를 추상화 합니다.</span><span class="sxs-lookup"><span data-stu-id="cf3c0-159">The Secret Manager tool abstracts away the implementation details, such as where and how the values are stored.</span></span> <span data-ttu-id="cf3c0-160">이러한 구현 정보를 알 필요 없이 도구를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="cf3c0-160">You can use the tool without knowing these implementation details.</span></span> <span data-ttu-id="cf3c0-161">현재 버전의 값에 저장 됩니다는 [JSON](http://json.org/) 사용자 프로필 디렉터리에 구성 파일:</span><span class="sxs-lookup"><span data-stu-id="cf3c0-161">In the current version, the values are stored in a [JSON](http://json.org/) configuration file in the user profile directory:</span></span>

* <span data-ttu-id="cf3c0-162">Windows: `%APPDATA%\microsoft\UserSecrets\<userSecretsId>\secrets.json`</span><span class="sxs-lookup"><span data-stu-id="cf3c0-162">Windows: `%APPDATA%\microsoft\UserSecrets\<userSecretsId>\secrets.json`</span></span>

* <span data-ttu-id="cf3c0-163">Linux: `~/.microsoft/usersecrets/<userSecretsId>/secrets.json`</span><span class="sxs-lookup"><span data-stu-id="cf3c0-163">Linux: `~/.microsoft/usersecrets/<userSecretsId>/secrets.json`</span></span>

* <span data-ttu-id="cf3c0-164">Mac: `~/.microsoft/usersecrets/<userSecretsId>/secrets.json`</span><span class="sxs-lookup"><span data-stu-id="cf3c0-164">Mac: `~/.microsoft/usersecrets/<userSecretsId>/secrets.json`</span></span>

<span data-ttu-id="cf3c0-165">값 `userSecretsId` 에 지정 된 값에서 가져온 *.csproj* 파일입니다.</span><span class="sxs-lookup"><span data-stu-id="cf3c0-165">The value of `userSecretsId` comes from the value specified in *.csproj* file.</span></span>

<span data-ttu-id="cf3c0-166">이러한 구현 정보 변경 될 수 있으므로 위치나 암호 관리자 도구와 함께 저장 된 데이터의 형식에 따라 달라 지 코드를 작성 하지 않아야 합니다.</span><span class="sxs-lookup"><span data-stu-id="cf3c0-166">You shouldn't write code that depends on the location or format of the data saved with the Secret Manager tool, as these implementation details might change.</span></span> <span data-ttu-id="cf3c0-167">예를 들어 비밀 값은 현재 *하지* 오늘 암호화 되지만 언젠가 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="cf3c0-167">For example, the secret values are currently *not* encrypted today, but could be someday.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="cf3c0-168">추가 리소스</span><span class="sxs-lookup"><span data-stu-id="cf3c0-168">Additional resources</span></span>

* [<span data-ttu-id="cf3c0-169">구성</span><span class="sxs-lookup"><span data-stu-id="cf3c0-169">Configuration</span></span>](xref:fundamentals/configuration/index)
