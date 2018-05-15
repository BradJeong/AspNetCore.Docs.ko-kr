---
title: 명령줄 도구를 사용하여 Azure에 ASP.NET Core 앱 게시
author: camsoper
description: Git 명령줄 클라이언트를 사용하여 Azure App Service에 ASP.NET Core 앱을 게시하는 방법을 알아봅니다.
manager: wpickett
ms.author: casoper
ms.custom: mvc
ms.date: 11/03/2017
ms.devlang: dotnet
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
services: multiple
uid: tutorials/publish-to-azure-webapp-using-cli
ms.openlocfilehash: 0462a4cf18bba23643ed3b1b4e6b76bdbceb24a8
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/06/2018
---
# <a name="publish-an-aspnet-core-app-to-azure-with-command-line-tools"></a><span data-ttu-id="7b0bf-103">명령줄 도구를 사용하여 Azure에 ASP.NET Core 앱 게시</span><span class="sxs-lookup"><span data-stu-id="7b0bf-103">Publish an ASP.NET Core app to Azure with command line tools</span></span>

<span data-ttu-id="7b0bf-104">작성자: [Cam Soper](https://twitter.com/camsoper)</span><span class="sxs-lookup"><span data-stu-id="7b0bf-104">By [Cam Soper](https://twitter.com/camsoper)</span></span>

[!INCLUDE [Azure App Service Preview Notice](../includes/azure-apps-preview-notice.md)]

<span data-ttu-id="7b0bf-105">이 자습서에서는 명령줄 도구를 사용하여 Microsoft Azure App Service에 ASP.NET Core 응용 프로그램을 빌드하고 배포하는 방법을 보여줍니다.</span><span class="sxs-lookup"><span data-stu-id="7b0bf-105">This tutorial will show you how to build and deploy an ASP.NET Core application to Microsoft Azure App Service using command line tools.</span></span>  <span data-ttu-id="7b0bf-106">완료되면 Azure App Service 웹앱으로 호스트된 ASP.NET MVC Core에서 웹 응용 프로그램이 빌드된 결과를 얻을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="7b0bf-106">When finished, you'll have a web application built in ASP.NET MVC Core hosted as an Azure App Service Web App.</span></span>  <span data-ttu-id="7b0bf-107">이 자습서는 Windows 명령줄 도구를 사용하여 작성되지만 macOS 및 Linux 환경에도 적용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="7b0bf-107">This tutorial is written using Windows command line tools, but can be applied to macOS and Linux environments, as well.</span></span>  

<span data-ttu-id="7b0bf-108">이 자습서에서는 다음 방법을 학습합니다.</span><span class="sxs-lookup"><span data-stu-id="7b0bf-108">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="7b0bf-109">Azure CLI를 사용하여 Azure App Service 웹 사이트 만들기</span><span class="sxs-lookup"><span data-stu-id="7b0bf-109">Create an Azure App Service website using Azure CLI</span></span>
> * <span data-ttu-id="7b0bf-110">Git 명령줄 도구를 사용하여 ASP.NET Core 응용 프로그램을 Azure App Service 배포</span><span class="sxs-lookup"><span data-stu-id="7b0bf-110">Deploy an ASP.NET Core application to Azure App Service using the Git command line tool</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7b0bf-111">전제 조건</span><span class="sxs-lookup"><span data-stu-id="7b0bf-111">Prerequisites</span></span>

<span data-ttu-id="7b0bf-112">이 자습서를 완료하려면 다음이 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="7b0bf-112">To complete this tutorial, you'll need:</span></span>

* <span data-ttu-id="7b0bf-113">[Microsoft Azure 구독](https://azure.microsoft.com/free/)</span><span class="sxs-lookup"><span data-stu-id="7b0bf-113">A [Microsoft Azure subscription](https://azure.microsoft.com/free/)</span></span>
* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]
* <span data-ttu-id="7b0bf-114">[Git](https://www.git-scm.com/) 명령줄 클라이언트</span><span class="sxs-lookup"><span data-stu-id="7b0bf-114">[Git](https://www.git-scm.com/) command line client</span></span>

## <a name="create-a-web-application"></a><span data-ttu-id="7b0bf-115">웹 응용 프로그램 만들기</span><span class="sxs-lookup"><span data-stu-id="7b0bf-115">Create a web application</span></span>

<span data-ttu-id="7b0bf-116">웹 응용 프로그램의 새 디렉터리를 만들고 새 ASP.NET Core MVC 응용 프로그램을 만든 다음 웹 사이트를 로컬로 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="7b0bf-116">Create a new directory for the web application, create a new ASP.NET Core MVC application, and then run the website locally.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="7b0bf-117">Windows</span><span class="sxs-lookup"><span data-stu-id="7b0bf-117">Windows</span></span>](#tab/windows)
```cmd
REM Create a new ASP.NET Core MVC application
dotnet new razor -o MyApplication

REM Change to the new directory that was just created
cd MyApplication

REM Run the application
dotnet run
```

# <a name="othertabother"></a>[<span data-ttu-id="7b0bf-118">기타</span><span class="sxs-lookup"><span data-stu-id="7b0bf-118">Other</span></span>](#tab/other)
```bash
# Create a new ASP.NET Core MVC application
dotnet new razor -o MyApplication

# Change to the new directory that was just created
cd MyApplication

# Run the application
dotnet run
```
---

![명령줄 출력](publish-to-azure-webapp-using-cli/_static/new_prj.png)

<span data-ttu-id="7b0bf-120">http://localhost:5000으로 이동하여 응용 프로그램을 테스트합니다.</span><span class="sxs-lookup"><span data-stu-id="7b0bf-120">Test the application by browsing to http://localhost:5000.</span></span>

![로컬로 실행되는 웹 사이트](publish-to-azure-webapp-using-cli/_static/app_test.png)


## <a name="create-the-azure-app-service-instance"></a><span data-ttu-id="7b0bf-122">Azure App Service 인스턴스 만들기</span><span class="sxs-lookup"><span data-stu-id="7b0bf-122">Create the Azure App Service instance</span></span>

<span data-ttu-id="7b0bf-123">[Azure Cloud Shell](/azure/cloud-shell/quickstart)을 사용하여 리소스 그룹, App Service 계획 및 App Service 웹앱을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="7b0bf-123">Using the [Azure Cloud Shell](/azure/cloud-shell/quickstart), create a resource group, App Service plan, and an App Service web app.</span></span>

```azurecli-interactive
# Generate a unique Web App name
let randomNum=$RANDOM*$RANDOM
webappname=tutorialApp$randomNum

# Create the DotNetAzureTutorial resource group
az group create --name DotNetAzureTutorial --location EastUS

# Create an App Service plan.
az appservice plan create --name $webappname --resource-group DotNetAzureTutorial --sku FREE

# Create the Web App
az webapp create --name $webappname --resource-group DotNetAzureTutorial --plan $webappname
```

<span data-ttu-id="7b0bf-124">배포 전에 다음 명령을 사용하여 계정 수준 배포 자격 증명을 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="7b0bf-124">Before deployment, set the account-level deployment credentials using the following command:</span></span>

```azurecli-interactive
az webapp deployment user set --user-name <desired user name> --password <desired password>
```

<span data-ttu-id="7b0bf-125">Git을 사용하여 응용 프로그램을 배포하려면 배포 URL이 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="7b0bf-125">A deployment URL is needed to deploy the application using Git.</span></span>  <span data-ttu-id="7b0bf-126">다음과 같이 URL을 검색합니다.</span><span class="sxs-lookup"><span data-stu-id="7b0bf-126">Retrieve the URL like this.</span></span>

```azurecli-interactive
az webapp deployment source config-local-git -n $webappname -g DotNetAzureTutorial --query [url] -o tsv
```
<span data-ttu-id="7b0bf-127">표시된 URL이 `.git`으로 끝나는 것을 알 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="7b0bf-127">Note the displayed URL ending in `.git`.</span></span> <span data-ttu-id="7b0bf-128">다음 단계에서 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="7b0bf-128">It's used in the next step.</span></span>

## <a name="deploy-the-application-using-git"></a><span data-ttu-id="7b0bf-129">Git을 사용하여 응용 프로그램 배포</span><span class="sxs-lookup"><span data-stu-id="7b0bf-129">Deploy the application using Git</span></span>

<span data-ttu-id="7b0bf-130">Git을 사용하여 로컬 컴퓨터에서 배포할 준비가 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="7b0bf-130">You're ready to deploy from your local machine using Git.</span></span>

> [!NOTE]
> <span data-ttu-id="7b0bf-131">줄 끝에 대한 Git 경고는 무시해도 됩니다.</span><span class="sxs-lookup"><span data-stu-id="7b0bf-131">It's safe to ignore any warnings from Git about line endings.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="7b0bf-132">Windows</span><span class="sxs-lookup"><span data-stu-id="7b0bf-132">Windows</span></span>](#tab/windows)
```cmd
REM Initialize the local Git repository
git init

REM Add the contents of the working directory to the repo
git add --all

REM Commit the changes to the local repo
git commit -a -m "Initial commit"

REM Add the URL as a Git remote repository
git remote add azure <THE GIT URL YOU NOTED EARLIER>

REM Push the local repository to the remote
git push azure master
```

# <a name="othertabother"></a>[<span data-ttu-id="7b0bf-133">기타</span><span class="sxs-lookup"><span data-stu-id="7b0bf-133">Other</span></span>](#tab/other)
```bash
# Initialize the local Git repository
git init

# Add the contents of the working directory to the repo
git add --all

# Commit the changes to the local repo
git commit -a -m "Initial commit"

# Add the URL as a Git remote repository
git remote add azure <THE GIT URL YOU NOTED EARLIER>

# Push the local repository to the remote
git push azure master
```
---

<span data-ttu-id="7b0bf-134">Git은 이전에 설정된 배포 자격 증명을 요구합니다.</span><span class="sxs-lookup"><span data-stu-id="7b0bf-134">Git prompts for the deployment credentials that were set earlier.</span></span> <span data-ttu-id="7b0bf-135">인증 후에 응용 프로그램은 원격 위치로 푸시되고, 빌드되며 배포됩니다.</span><span class="sxs-lookup"><span data-stu-id="7b0bf-135">After authenticating, the application will be pushed to the remote location, built, and deployed.</span></span>

![Git 배포 출력](publish-to-azure-webapp-using-cli/_static/post_deploy.png)

## <a name="test-the-application"></a><span data-ttu-id="7b0bf-137">응용 프로그램 테스트</span><span class="sxs-lookup"><span data-stu-id="7b0bf-137">Test the application</span></span>

<span data-ttu-id="7b0bf-138">`https://<web app name>.azurewebsites.net`으로 이동하여 응용 프로그램을 테스트합니다.</span><span class="sxs-lookup"><span data-stu-id="7b0bf-138">Test the application by browsing to `https://<web app name>.azurewebsites.net`.</span></span>  <span data-ttu-id="7b0bf-139">Cloud Shell(또는 Azure CLI)에 주소를 표시하려면 다음을 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="7b0bf-139">To display the address in the Cloud Shell (or Azure CLI), use the following:</span></span>

```azurecli-interactive
az webapp show -n $webappname -g DotNetAzureTutorial --query defaultHostName -o tsv
```

![Azure에서 실행 중인 앱](publish-to-azure-webapp-using-cli/_static/app_deployed.png)

## <a name="clean-up"></a><span data-ttu-id="7b0bf-141">정리</span><span class="sxs-lookup"><span data-stu-id="7b0bf-141">Clean up</span></span>

<span data-ttu-id="7b0bf-142">앱 테스트와 코드 및 리소스를 검사가 끝나면 리소스 그룹을 삭제하여 웹앱 및 계획을 삭제합니다.</span><span class="sxs-lookup"><span data-stu-id="7b0bf-142">When finished testing the app and inspecting the code and resources, delete the web app and plan by deleting the resource group.</span></span>

```azurecli-interactive
az group delete -n DotNetAzureTutorial
```

## <a name="next-steps"></a><span data-ttu-id="7b0bf-143">다음 단계</span><span class="sxs-lookup"><span data-stu-id="7b0bf-143">Next steps</span></span>

<span data-ttu-id="7b0bf-144">이 자습서에서는 다음 방법을 학습했습니다.</span><span class="sxs-lookup"><span data-stu-id="7b0bf-144">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="7b0bf-145">Azure CLI를 사용하여 Azure App Service 웹 사이트 만들기</span><span class="sxs-lookup"><span data-stu-id="7b0bf-145">Create an Azure App Service website using Azure CLI</span></span>
> * <span data-ttu-id="7b0bf-146">Git 명령줄 도구를 사용하여 ASP.NET Core 응용 프로그램을 Azure App Service 배포</span><span class="sxs-lookup"><span data-stu-id="7b0bf-146">Deploy an ASP.NET Core application to Azure App Service using the Git command line tool</span></span>

<span data-ttu-id="7b0bf-147">다음으로, 명령줄을 사용하여 CosmosDB를 사용하는 기존 웹앱을 배포하는 방법을 알아보세요.</span><span class="sxs-lookup"><span data-stu-id="7b0bf-147">Next, learn to use the command line to deploy an existing web app that uses CosmosDB.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="7b0bf-148">.NET Core를 사용하여 명령줄에서 Azure에 배포</span><span class="sxs-lookup"><span data-stu-id="7b0bf-148">Deploy to Azure from the command line with .NET Core</span></span>](/dotnet/azure/dotnet-quickstart-xplat)
