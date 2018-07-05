---
title: TypeScript 및 WebPack과 함께 ASP.NET Core SignalR 사용
author: ssougnez
description: 이 자습서에서는 해당 클라이언트가 TypeScript로 작성된 ASP.NET Core SignalR 웹앱을 번들링 및 빌드하도록 WebPack을 구성합니다.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 06/29/2018
uid: tutorials/signalr-typescript-webpack
ms.openlocfilehash: e4b00fa61ea0becba7d678a0b7c94d2d30f06740
ms.sourcegitcommit: 2941e24d7f3fd3d5e88d27e5f852aaedd564deda
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37126475"
---
# <a name="use-aspnet-core-signalr-with-typescript-and-webpack"></a><span data-ttu-id="5f9a8-103">TypeScript 및 WebPack과 함께 ASP.NET Core SignalR 사용</span><span class="sxs-lookup"><span data-stu-id="5f9a8-103">Use ASP.NET Core SignalR with TypeScript and Webpack</span></span>

<span data-ttu-id="5f9a8-104">작성자: [Sébastien Sougnez](https://twitter.com/ssougnez) 및 [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="5f9a8-104">By [Sébastien Sougnez](https://twitter.com/ssougnez) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="5f9a8-105">[WebPack](https://webpack.js.org/)을 사용하여 개발자가 웹앱의 클라이언트 쪽 리소스를 번들링 및 빌드할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-105">[Webpack](https://webpack.js.org/) enables developers to bundle and build the client-side resources of a web app.</span></span> <span data-ttu-id="5f9a8-106">이 자습서에서는 해당 클라이언트가 [TypeScript](https://www.typescriptlang.org/)로 작성된 ASP.NET Core SignalR 웹앱에서 WebPack 사용을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-106">This tutorial demonstrates using Webpack in an ASP.NET Core SignalR web app whose client is written in [TypeScript](https://www.typescriptlang.org/).</span></span>

<span data-ttu-id="5f9a8-107">이 자습서에서는 다음 방법을 학습합니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-107">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="5f9a8-108">ASP.NET Core SignalR 앱 스캐폴드</span><span class="sxs-lookup"><span data-stu-id="5f9a8-108">Scaffold a starter ASP.NET Core SignalR app</span></span>
> * <span data-ttu-id="5f9a8-109">SignalR TypeScript 클라이언트 구성</span><span class="sxs-lookup"><span data-stu-id="5f9a8-109">Configure the SignalR TypeScript client</span></span>
> * <span data-ttu-id="5f9a8-110">WebPack을 사용하여 빌드 파이프라인 구성</span><span class="sxs-lookup"><span data-stu-id="5f9a8-110">Configure a build pipeline using Webpack</span></span>
> * <span data-ttu-id="5f9a8-111">SignalR 서버 구성</span><span class="sxs-lookup"><span data-stu-id="5f9a8-111">Configure the SignalR server</span></span>
> * <span data-ttu-id="5f9a8-112">클라이언트 및 서버 간 통신 활성화</span><span class="sxs-lookup"><span data-stu-id="5f9a8-112">Enable communication between client and server</span></span>

<span data-ttu-id="5f9a8-113">[예제 코드 살펴보기 및 다운로드](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/signalr-typescript-webpack/sample)([다운로드 방법](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="5f9a8-113">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/signalr-typescript-webpack/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5f9a8-114">전제 조건</span><span class="sxs-lookup"><span data-stu-id="5f9a8-114">Prerequisites</span></span>

<span data-ttu-id="5f9a8-115">다음 소프트웨어를 설치합니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-115">Install the following software:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5f9a8-116">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5f9a8-116">Visual Studio</span></span>](#tab/visual-studio)

* [<span data-ttu-id="5f9a8-117">.NET Core SDK 2.1 이상</span><span class="sxs-lookup"><span data-stu-id="5f9a8-117">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="5f9a8-118">[npm](https://www.npmjs.com/) 포함 [Node.js](https://nodejs.org/)</span><span class="sxs-lookup"><span data-stu-id="5f9a8-118">[Node.js](https://nodejs.org/) with [npm](https://www.npmjs.com/)</span></span>
* <span data-ttu-id="5f9a8-119">[Visual Studio 2017](https://www.visualstudio.com/downloads/) 버전 15.7 이상(**ASP.NET 및 웹 개발** 워크로드 포함)</span><span class="sxs-lookup"><span data-stu-id="5f9a8-119">[Visual Studio 2017](https://www.visualstudio.com/downloads/) version 15.7 or later with the **ASP.NET and web development** workload</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="5f9a8-120">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="5f9a8-120">.NET Core CLI</span></span>](#tab/netcore-cli)

* [<span data-ttu-id="5f9a8-121">.NET Core SDK 2.1 이상</span><span class="sxs-lookup"><span data-stu-id="5f9a8-121">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="5f9a8-122">[npm](https://www.npmjs.com/) 포함 [Node.js](https://nodejs.org/)</span><span class="sxs-lookup"><span data-stu-id="5f9a8-122">[Node.js](https://nodejs.org/) with [npm](https://www.npmjs.com/)</span></span>

---

## <a name="create-the-aspnet-core-web-app"></a><span data-ttu-id="5f9a8-123">ASP.NET Core 웹앱 만들기</span><span class="sxs-lookup"><span data-stu-id="5f9a8-123">Create the ASP.NET Core web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5f9a8-124">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5f9a8-124">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="5f9a8-125">*PATH* 환경 변수에서 npm을 찾도록 Visual Studio를 구성합니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-125">Configure Visual Studio to look for npm in the *PATH* environment variable.</span></span> <span data-ttu-id="5f9a8-126">기본적으로 Visual Studio는 설치 디렉터리에 있는 npm의 버전을 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-126">By default, Visual Studio uses the version of npm found in its installation directory.</span></span> <span data-ttu-id="5f9a8-127">Visual Studio에서 다음 지침을 따릅니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-127">Follow these instructions in Visual Studio:</span></span>

1. <span data-ttu-id="5f9a8-128">**도구** > **옵션** > **프로젝트 및 솔루션** > **웹 패키지 관리** > **외부 웹 도구**를 탐색합니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-128">Navigate to **Tools** > **Options** > **Projects and solutions** > **Web Package Management** > **External Web Tools**.</span></span>
1. <span data-ttu-id="5f9a8-129">목록에서 *$(PATH)* 항목을 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-129">Select the *$(PATH)* entry from the list.</span></span> <span data-ttu-id="5f9a8-130">위쪽 화살표를 클릭하여 항목을 목록의 두 번째 위치로 이동합니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-130">Click the up arrow to move the entry to the second position in the list.</span></span> <span data-ttu-id="5f9a8-131">참고로 첫 번째 항목은 프로젝트의 로컬 패키지를 가리킵니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-131">As an aside, the first entry refers to the project's local packages.</span></span>

    ![Visual Studio 구성](signalr-typescript-webpack/_static/signalr-configure-path-visual-studio.png)

<span data-ttu-id="5f9a8-133">Visual Studio 구성이 완료되었습니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-133">Visual Studio configuration is completed.</span></span> <span data-ttu-id="5f9a8-134">이제 프로젝트를 만들어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-134">It's time to create the project.</span></span>

1. <span data-ttu-id="5f9a8-135">**파일** > **새로 만들기** > **프로젝트** 메뉴 옵션을 사용하고 **ASP.NET Core 웹 응용 프로그램** 템플릿을 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-135">Use the **File** > **New** > **Project** menu option and choose the **ASP.NET Core Web Application** template.</span></span>
1. <span data-ttu-id="5f9a8-136">프로젝트를 *SignalRWebPack*으로 이름 지정하고 **확인** 단추를 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-136">Name the project *SignalRWebPack*, and click the **OK** button.</span></span>
1. <span data-ttu-id="5f9a8-137">대상 프레임워크 드롭다운에서 *.NET Core*를 선택하고 프레임워크 선택기 드롭다운에서 *ASP.NET Core 2.1*을 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-137">Select *.NET Core* from the target framework drop-down, and select *ASP.NET Core 2.1* from the framework selector drop-down.</span></span> <span data-ttu-id="5f9a8-138">**빈** 템플릿에서 **확인** 단추를 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-138">Select the **Empty** template, and click the **OK** button.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="5f9a8-139">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="5f9a8-139">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="5f9a8-140">**통합 터미널**에서 다음 명령을 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-140">Run the following command in the **Integrated Terminal**:</span></span>

```console
dotnet new web -o SignalRWebPack
```

<span data-ttu-id="5f9a8-141">.NET Core를 대상으로 하는 빈 ASP.NET Core 웹앱이 *SignalRWebPack* 디렉터리에 생성됩니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-141">An empty ASP.NET Core web app, targeting .NET Core, is created in a *SignalRWebPack* directory.</span></span>

---

## <a name="configure-webpack-and-typescript"></a><span data-ttu-id="5f9a8-142">WebPack 및 TypeScript 구성</span><span class="sxs-lookup"><span data-stu-id="5f9a8-142">Configure Webpack and TypeScript</span></span>

<span data-ttu-id="5f9a8-143">다음 단계에서는 TypeScript에서 JavaScript로 변환 및 클라이언트 쪽 리소스의 번들링을 구성합니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-143">The following steps configure the conversion of TypeScript to JavaScript and the bundling of client-side resources.</span></span>

1. <span data-ttu-id="5f9a8-144">프로젝트 루트에서 다음 명령을 실행하여 *package.json* 파일을 생성합니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-144">Execute the following command in the project root to create a *package.json* file:</span></span>

    ```console
    npm init -y
    ```

1. <span data-ttu-id="5f9a8-145">강조 표시된 속성을 *package.json* 파일에 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-145">Add the highlighted property to the *package.json* file:</span></span>

    [!code-json[package.json](signalr-typescript-webpack/sample/snippets/package1.json?highlight=4)]

    <span data-ttu-id="5f9a8-146">`private` 속성을 `true`로 설정하면 다음 단계에서 패키지 설치 경고가 나타나지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-146">Setting the `private` property to `true` prevents package installation warnings in the next step.</span></span>

1. <span data-ttu-id="5f9a8-147">필요한 npm 패키지를 설치합니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-147">Install the required npm packages.</span></span> <span data-ttu-id="5f9a8-148">프로젝트 루트에서 다음 명령을 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-148">Execute the following command from the project root:</span></span>

    ```console
    npm install -D -E clean-webpack-plugin@0.1.19 css-loader@0.28.11 html-webpack-plugin@3.2.0 mini-css-extract-plugin@0.4.0 ts-loader@4.4.1 typescript@2.9.2 webpack@4.12.0 webpack-cli@3.0.6
    ```

    <span data-ttu-id="5f9a8-149">참고할 몇몇 명령 세부 정보:</span><span class="sxs-lookup"><span data-stu-id="5f9a8-149">Some command details to note:</span></span>

    * <span data-ttu-id="5f9a8-150">버전 번호는 각 패키지 이름에 대해 `@` 부호 뒤에 옵니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-150">A version number follows the `@` sign for each package name.</span></span> <span data-ttu-id="5f9a8-151">npm은 해당 특정 패키지 버전을 설치합니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-151">npm installs those specific package versions.</span></span>
    * <span data-ttu-id="5f9a8-152">`-E` 옵션은 [유의적 버전](https://semver.org/) 범위 연산자를 *package.json*에 쓰는 npm의 기본 동작을 비활성화합니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-152">The `-E` option disables npm's default behavior of writing [semantic versioning](https://semver.org/) range operators to *package.json*.</span></span> <span data-ttu-id="5f9a8-153">예를 들어 `"webpack": "^4.12.0"` 대신 `"webpack": "4.12.0"`을 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-153">For example, `"webpack": "4.12.0"` is used instead of `"webpack": "^4.12.0"`.</span></span> <span data-ttu-id="5f9a8-154">이 옵션은 최신 패키지 버전으로 의도하지 않은 업그레이드를 방지합니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-154">This option prevents unintended upgrades to newer package versions.</span></span>

    <span data-ttu-id="5f9a8-155">자세한 내용은 [npm-install](https://docs.npmjs.com/cli/install) 문서를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-155">See the official [npm-install](https://docs.npmjs.com/cli/install) docs for more detail.</span></span>

1. <span data-ttu-id="5f9a8-156">*package.json* 파일의 `scripts` 속성을 다음 코드 조각으로 바꿉니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-156">Replace the `scripts` property of the *package.json* file with the following snippet:</span></span>

    ```json
    "scripts": {
      "build": "webpack --mode=development --watch",
      "release": "webpack --mode=production",
      "publish": "npm run release && dotnet publish -c Release"
    },
    ```

    <span data-ttu-id="5f9a8-157">스크립트에 대한 간략한 설명:</span><span class="sxs-lookup"><span data-stu-id="5f9a8-157">Some explanation of the scripts:</span></span>

    * <span data-ttu-id="5f9a8-158">`build`: 개발 모드에서 클라이언트 쪽 리소스를 번들링하고 파일 변경을 감시합니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-158">`build`: Bundles your client-side resources in development mode and watches for file changes.</span></span> <span data-ttu-id="5f9a8-159">파일 감시자는 프로젝트 파일이 변경될 때마다 번들이 다시 생성되도록 합니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-159">The file watcher causes the bundle to regenerate each time a project file changes.</span></span> <span data-ttu-id="5f9a8-160">`mode` 옵션은 프로덕션 최적화(예: 트리 셰이킹 및 축소)를 비활성화합니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-160">The `mode` option disables production optimizations, such as tree shaking and minification.</span></span> <span data-ttu-id="5f9a8-161">개발 시에는 `build`만 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-161">Only use `build` in development.</span></span>
    * <span data-ttu-id="5f9a8-162">`release`: 프로덕션 모드에서 클라이언트 쪽 리소스를 번들링합니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-162">`release`: Bundles your client-side resources in production mode.</span></span>
    * <span data-ttu-id="5f9a8-163">`publish`: 프로덕션 모드에서 `release` 스크립트를 실행하여 클라이언트 쪽 리소스를 번들링합니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-163">`publish`: Runs the `release` script to bundle the client-side resources in production mode.</span></span> <span data-ttu-id="5f9a8-164">이는 .NET Core CLI의 [게시](/dotnet/core/tools/dotnet-publish) 명령을 호출하여 앱을 게시합니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-164">It calls the .NET Core CLI's [publish](/dotnet/core/tools/dotnet-publish) command to publish the app.</span></span>

1. <span data-ttu-id="5f9a8-165">프로젝트 루트에 다음 내용을 포함한 *webpack.config.js*라는 파일을 생성합니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-165">Create a file named *webpack.config.js*, in the project root, with the following content:</span></span>

    [!code-javascript[webpack.config.js](signalr-typescript-webpack/sample/webpack.config.js)]

    <span data-ttu-id="5f9a8-166">앞의 파일은 WebPack 컴파일을 구성합니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-166">The preceding file configures the Webpack compilation.</span></span> <span data-ttu-id="5f9a8-167">참고할 일부 구성 세부 정보:</span><span class="sxs-lookup"><span data-stu-id="5f9a8-167">Some configuration details to note:</span></span>

    * <span data-ttu-id="5f9a8-168">`output` 속성은 *dist*의 기본값을 재정의합니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-168">The `output` property overrides the default value of *dist*.</span></span> <span data-ttu-id="5f9a8-169">번들은 *wwwroot* 디렉터리에 대신 내보내집니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-169">The bundle is instead emitted in the *wwwroot* directory.</span></span>
    * <span data-ttu-id="5f9a8-170">`resolve.extensions` 배열은 SignalR 클라이언트 JavaScript를 가져오기 위한 *.js*를 포함합니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-170">The `resolve.extensions` array includes *.js* to import the SignalR client JavaScript.</span></span>

1. <span data-ttu-id="5f9a8-171">프로젝트 루트에 새 *src* 디렉터리를 생성합니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-171">Create a new *src* directory in the project root.</span></span> <span data-ttu-id="5f9a8-172">그 목적은 프로젝트의 클라이언트 쪽 자산을 저장하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-172">Its purpose is to store the project's client-side assets.</span></span>

1. <span data-ttu-id="5f9a8-173">다음 내용을 포함한 *src/index.html*을 생성합니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-173">Create *src/index.html* with the following content.</span></span>

    [!code-html[index.html](signalr-typescript-webpack/sample/src/index.html)]

    <span data-ttu-id="5f9a8-174">앞의 HTML은 홈 페이지의 상용구 마크업을 정의합니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-174">The preceding HTML defines the homepage's boilerplate markup.</span></span>

1. <span data-ttu-id="5f9a8-175">새 *src/css* 디렉터리를 생성합니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-175">Create a new *src/css* directory.</span></span> <span data-ttu-id="5f9a8-176">그 목적은 프로젝트의 *.css* 파일을 저장하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-176">Its purpose is to store the project's *.css* files.</span></span>

1. <span data-ttu-id="5f9a8-177">다음 내용을 포함한 *src/css/main.css*를 생성합니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-177">Create *src/css/main.css* with the following content:</span></span>

    [!code-css[main.css](signalr-typescript-webpack/sample/src/css/main.css)]

    <span data-ttu-id="5f9a8-178">앞의 *main.css* 파일은 앱의 스타일을 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-178">The preceding *main.css* file styles the app.</span></span>

1. <span data-ttu-id="5f9a8-179">다음 내용을 포함한 *src/tsconfig.json*을 생성합니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-179">Create *src/tsconfig.json* with the following content:</span></span>

    [!code-json[tsconfig.json](signalr-typescript-webpack/sample/src/tsconfig.json)]

    <span data-ttu-id="5f9a8-180">앞의 코드는 [ECMAScript](https://wikipedia.org/wiki/ECMAScript) 5 호환 JavaScript를 생성하도록 TypeScript 컴파일러를 구성합니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-180">The preceding code configures the TypeScript compiler to produce [ECMAScript](https://wikipedia.org/wiki/ECMAScript) 5-compatible JavaScript.</span></span>

1. <span data-ttu-id="5f9a8-181">다음 내용을 포함한 *src/index.ts*를 생성합니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-181">Create *src/index.ts* with the following content:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/snippets/index1.ts?name=snippet_IndexTsPhase1File)]

    <span data-ttu-id="5f9a8-182">앞의 TypeScript는 DOM 요소에 대한 참조를 검색하고 이벤트 핸들러 두 개를 연결합니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-182">The preceding TypeScript retrieves references to DOM elements and attaches two event handlers:</span></span>

    * <span data-ttu-id="5f9a8-183">`keyup`: 이 이벤트는 사용자가 텍스트 상자에 `tbMessage`로 식별된 내용을 입력할 때 시작합니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-183">`keyup`: This event fires when the user types something in the textbox identified as `tbMessage`.</span></span> <span data-ttu-id="5f9a8-184">`send` 함수는 사용자가 **Enter** 키를 누를 때 호출됩니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-184">The `send` function is called when the user presses the **Enter** key.</span></span>
    * <span data-ttu-id="5f9a8-185">`click`: 이 이벤트는 사용자가 **보내기** 단추를 클릭할 때 시작합니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-185">`click`: This event fires when the user clicks the **Send** button.</span></span> <span data-ttu-id="5f9a8-186">`send` 함수가 호출됩니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-186">The `send` function is called.</span></span>

## <a name="configure-the-aspnet-core-app"></a><span data-ttu-id="5f9a8-187">ASP.NET Core 앱 구성</span><span class="sxs-lookup"><span data-stu-id="5f9a8-187">Configure the ASP.NET Core app</span></span>

1. <span data-ttu-id="5f9a8-188">`Startup.Configure` 메서드에 제공된 코드는 *Hello World!* 를 표시합니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-188">The code provided in the `Startup.Configure` method displays *Hello World!*.</span></span> <span data-ttu-id="5f9a8-189">`app.Run` 메서드를 [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) 및 [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_)에 대한 호출로 바꿉니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-189">Replace the `app.Run` method call with calls to [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/Startup.cs?name=snippet_UseStaticDefaultFiles)]

    <span data-ttu-id="5f9a8-190">앞의 코드는 사용자가 전체 URL을 입력하는지 아니면 웹앱의 루트 URL을 입력하는지에 따라 서버가 *index.html* 파일을 찾아서 제공할 수 있도록 합니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-190">The preceding code allows the server to locate and serve the *index.html* file, whether the user enters its full URL or the root URL of the web app.</span></span>

1. <span data-ttu-id="5f9a8-191">`Startup.ConfigureServices` 메서드에서 [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr#Microsoft_Extensions_DependencyInjection_SignalRDependencyInjectionExtensions_AddSignalR_Microsoft_Extensions_DependencyInjection_IServiceCollection_)을 호출합니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-191">Call [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr#Microsoft_Extensions_DependencyInjection_SignalRDependencyInjectionExtensions_AddSignalR_Microsoft_Extensions_DependencyInjection_IServiceCollection_) in the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="5f9a8-192">이는 SignalR 서비스를 프로젝트에 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-192">It adds the SignalR services to your project.</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/Startup.cs?name=snippet_AddSignalR)]

1. <span data-ttu-id="5f9a8-193">*/hub* 경로를 `ChatHub` 허브에 매핑합니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-193">Map a */hub* route to the `ChatHub` hub.</span></span> <span data-ttu-id="5f9a8-194">`Startup.Configure` 메서드의 끝에 다음 줄을 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-194">Add the following lines at the end of the `Startup.Configure` method:</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/Startup.cs?name=snippet_UseSignalR)]

1. <span data-ttu-id="5f9a8-195">프로젝트 루트에 *Hubs*라는 새 디렉터리를 생성합니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-195">Create a new directory, called *Hubs*, in the project root.</span></span> <span data-ttu-id="5f9a8-196">그 목적은 다음 단계에서 생성되는 SignalR 허브를 저장하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-196">Its purpose is to store the SignalR hub, which is created in the next step.</span></span>

1. <span data-ttu-id="5f9a8-197">다음 내용을 포함한 허브 *Hubs/ChatHub.cs*를 생성합니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-197">Create hub *Hubs/ChatHub.cs* with the following code:</span></span>

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/snippets/ChatHub.cs?name=snippet_ChatHubStubClass)]

1. <span data-ttu-id="5f9a8-198">`ChatHub` 참조를 확인하기 위해 *Startup.cs* 파일의 맨 위에 다음 코드를 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-198">Add the following code at the top of the *Startup.cs* file to resolve the `ChatHub` reference:</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/Startup.cs?name=snippet_HubsNamespace)]

## <a name="enable-client-and-server-communication"></a><span data-ttu-id="5f9a8-199">클라이언트 및 서버 통신 활성화</span><span class="sxs-lookup"><span data-stu-id="5f9a8-199">Enable client and server communication</span></span>

<span data-ttu-id="5f9a8-200">앱은 현재 메시지를 보낼 간단한 양식을 표시합니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-200">The app currently displays a simple form to send messages.</span></span> <span data-ttu-id="5f9a8-201">그런 시도를 할 때 아무 일도 일어나지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-201">Nothing happens when you try to do so.</span></span> <span data-ttu-id="5f9a8-202">서버는 특정 경로를 수신 대기하지만 보낸 메시지를 사용하여 아무 일도 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-202">The server is listening to a specific route but does nothing with sent messages.</span></span>

1. <span data-ttu-id="5f9a8-203">프로젝트 루트에서 다음 명령을 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-203">Execute the following command at the project root:</span></span>

    ```console
    npm install @aspnet/signalr
    ```

    <span data-ttu-id="5f9a8-204">앞의 명령은 클라이언트가 서버에 메시지를 보낼 수 있도록 하는 [SignalR TypeScript 클라이언트](https://www.npmjs.com/package/@aspnet/signalr)를 설치합니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-204">The preceding command installs the [SignalR TypeScript client](https://www.npmjs.com/package/@aspnet/signalr), which allows the client to send messages to the server.</span></span>

1. <span data-ttu-id="5f9a8-205">강조 표시된 코드를 *src/index.ts* 파일에 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-205">Add the highlighted code to the *src/index.ts* file:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/snippets/index2.ts?name=snippet_IndexTsPhase2File&highlight=2,9-23)]

    <span data-ttu-id="5f9a8-206">앞의 코드는 서버에서 오는 메시지의 수신을 지원합니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-206">The preceding code supports receiving messages from the server.</span></span> <span data-ttu-id="5f9a8-207">`HubConnectionBuilder` 클래스는 서버 연결을 구성하기 위한 새 빌더를 생성합니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-207">The `HubConnectionBuilder` class creates a new builder for configuring the server connection.</span></span> <span data-ttu-id="5f9a8-208">`withUrl` 함수는 허브 URL을 구성합니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-208">The `withUrl` function configures the hub URL.</span></span>

    <span data-ttu-id="5f9a8-209">SignalR은 클라이언트 및 서버 간 메시지 교환을 활성화합니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-209">SignalR enables the exchange of messages between a client and a server.</span></span> <span data-ttu-id="5f9a8-210">각 메시지는 특정 이름을 가집니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-210">Each message has a specific name.</span></span> <span data-ttu-id="5f9a8-211">예를 들어 메시지 영역에 새 메시지를 표시하는 로직을 실행하는 `messageReceived`라는 이름을 가진 메시지가 있을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-211">For example, you can have messages with the name `messageReceived` that execute the logic responsible for displaying the new message in the messages zone.</span></span> <span data-ttu-id="5f9a8-212">특정 메시지 수신 대기는 `on` 함수를 통해 수행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-212">Listening to a specific message can be done via the `on` function.</span></span> <span data-ttu-id="5f9a8-213">임의 개수의 메시지 이름을 수신 대기할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-213">You can listen to any number of message names.</span></span> <span data-ttu-id="5f9a8-214">또한 수신 메시지의 작성자 이름 및 내용 등을 메시지에 파라미터로 전달할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-214">It's also possible to pass parameters to the message, such as the author's name and the content of the message received.</span></span> <span data-ttu-id="5f9a8-215">클라이언트가 메시지를 수신한 후 `innerHTML` 특성의 작성자 이름 및 메시지 내용을 사용하여 새 `div` 요소가 생성됩니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-215">Once the client receives a message, a new `div` element is created with the author's name and the message content in its `innerHTML` attribute.</span></span> <span data-ttu-id="5f9a8-216">이 요소가 주 `div` 요소에 추가되어 메시지를 표시합니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-216">It's added to the main `div` element displaying the messages.</span></span>

1. <span data-ttu-id="5f9a8-217">이제 클라이언트가 메시지를 수신할 수 있으므로, 메시지를 보내도록 구성합니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-217">Now that the client can receive a message, configure it to send messages.</span></span> <span data-ttu-id="5f9a8-218">강조 표시된 코드를 *src/index.ts* 파일에 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-218">Add the highlighted code to the *src/index.ts* file:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/src/index.ts?highlight=34-35)]

    <span data-ttu-id="5f9a8-219">WebSockets 연결을 통해 메시지를 보내면 `send` 메서드를 호출해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-219">Sending a message through the WebSockets connection requires calling the `send` method.</span></span> <span data-ttu-id="5f9a8-220">메서드의 첫 번째 매개 변수는 메시지 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-220">The method's first parameter is the message name.</span></span> <span data-ttu-id="5f9a8-221">메시지 데이터는 다른 매개 변수를 통해 지정합니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-221">The message data inhabits the other parameters.</span></span> <span data-ttu-id="5f9a8-222">이 예에서 `newMessage`로 식별한 메시지를 서버에 보냅니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-222">In this example, a message identified as `newMessage` is sent to the server.</span></span> <span data-ttu-id="5f9a8-223">메시지는 사용자 이름 및 텍스트 상자에서의 사용자 입력으로 구성됩니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-223">The message consists of the username and the user input from a text box.</span></span> <span data-ttu-id="5f9a8-224">보내기가 작동하면 텍스트 상자 값이 지워집니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-224">If the send works, the text box value is cleared.</span></span>

1. <span data-ttu-id="5f9a8-225">강조 표시된 메서드를 `ChatHub` 클래스에 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-225">Add the highlighted method to the `ChatHub` class:</span></span>

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/Hubs/ChatHub.cs?highlight=8-11)]

    <span data-ttu-id="5f9a8-226">앞의 코드는 서버가 메시지를 수신한 후 수신된 메시지를 모든 연결된 사용자에게 브로드캐스트합니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-226">The preceding code broadcasts received messages to all connected users once the server receives them.</span></span> <span data-ttu-id="5f9a8-227">모든 메시지를 수신하기 위해 일반 `on` 메서드를 포함할 필요는 없습니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-227">It's unnecessary to have a generic `on` method to receive all the messages.</span></span> <span data-ttu-id="5f9a8-228">메시지 이름 뒤에 명명한 메서드로 충분합니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-228">A method named after the message name suffices.</span></span>

    <span data-ttu-id="5f9a8-229">이 예에서 TypeScript 클라이언트는 `newMessage`로 식별된 메시지를 보냅니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-229">In this example, the TypeScript client sends a message identified as `newMessage`.</span></span> <span data-ttu-id="5f9a8-230">C# `NewMessage` 메서드에는 클라이언트가 보낸 데이터가 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-230">The C# `NewMessage` method expects the data sent by the client.</span></span> <span data-ttu-id="5f9a8-231">[Clients.All](/dotnet/api/microsoft.aspnetcore.signalr.ihubclients-1.all)에 대한 [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) 메서드를 호출합니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-231">A call is made to the [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) method on [Clients.All](/dotnet/api/microsoft.aspnetcore.signalr.ihubclients-1.all).</span></span> <span data-ttu-id="5f9a8-232">수신된 메시지를 허브에 연결된 모든 클라이언트에 보냅니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-232">The received messages are sent to all clients connected to the hub.</span></span>

## <a name="test-the-app"></a><span data-ttu-id="5f9a8-233">앱 테스트</span><span class="sxs-lookup"><span data-stu-id="5f9a8-233">Test the app</span></span>

<span data-ttu-id="5f9a8-234">다음 단계를 사용하여 앱이 작동하는지 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-234">Confirm that the app works with the following steps.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5f9a8-235">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5f9a8-235">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="5f9a8-236">WebPack을 *릴리스* 모드로 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-236">Run Webpack in *release* mode.</span></span> <span data-ttu-id="5f9a8-237">**패키지 관리자 콘솔** 창을 사용하여 프로젝트 루트에서 다음 명령을 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-237">Using the **Package Manager Console** window, execute the following command in the project root:</span></span>

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. <span data-ttu-id="5f9a8-238">**디버그** > **디버깅 없이 시작**을 선택하여 디버거를 연결하지 않고 브라우저에서 엽을 시작합니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-238">Select **Debug** > **Start without debugging** to launch the app in a browser without attaching the debugger.</span></span> <span data-ttu-id="5f9a8-239">*wwwroot/index.html* 파일은 `http://localhost:<port_number>`에 제공됩니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-239">The *wwwroot/index.html* file is served at `http://localhost:<port_number>`.</span></span>

1. <span data-ttu-id="5f9a8-240">다른 브라우저 인스턴스(임의 브라우저)를 엽니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-240">Open another browser instance (any browser).</span></span> <span data-ttu-id="5f9a8-241">URL을 주소 표시줄에 붙여넣습니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-241">Paste the URL in the address bar.</span></span>

1. <span data-ttu-id="5f9a8-242">브라우저를 선택하고 **메시지** 텍스트 상자에 내용을 입력하고 **보내기** 단추를 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-242">Choose either browser, type something in the **Message** text box, and click the **Send** button.</span></span> <span data-ttu-id="5f9a8-243">고유한 사용자 이름과 메시지는 두 페이지 모두에 즉시 표시됩니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-243">The unique user name and message are displayed on both pages instantly.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="5f9a8-244">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="5f9a8-244">.NET Core CLI</span></span>](#tab/netcore-cli)

1. <span data-ttu-id="5f9a8-245">프로젝트 루트에서 다음 명령을 실행하여 WebPack을 *릴리스* 모드로 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-245">Run Webpack in *release* mode by executing the following command in the project root:</span></span>

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. <span data-ttu-id="5f9a8-246">프로젝트 루트에서 다음 명령을 실행하여 앱을 빌드하고 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-246">Build and run the app by executing the following command in the project root:</span></span>

    ```console
    dotnet run
    ```

    <span data-ttu-id="5f9a8-247">웹 서버가 앱을 시작하고 로컬 호스트에서 사용할 수 있도록 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-247">The web server starts the app and makes it available on localhost.</span></span>

1. <span data-ttu-id="5f9a8-248">`http://localhost:<port_number>`에 대해 브라우저를 엽니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-248">Open a browser to `http://localhost:<port_number>`.</span></span> <span data-ttu-id="5f9a8-249">*wwwroot/index.html* 파일이 제공됩니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-249">The *wwwroot/index.html* file is served.</span></span> <span data-ttu-id="5f9a8-250">주소 표시줄에서 URL을 복사합니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-250">Copy the URL from the address bar.</span></span>

1. <span data-ttu-id="5f9a8-251">다른 브라우저 인스턴스(임의 브라우저)를 엽니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-251">Open another browser instance (any browser).</span></span> <span data-ttu-id="5f9a8-252">URL을 주소 표시줄에 붙여넣습니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-252">Paste the URL in the address bar.</span></span>

1. <span data-ttu-id="5f9a8-253">브라우저를 선택하고 **메시지** 텍스트 상자에 내용을 입력하고 **보내기** 단추를 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-253">Choose either browser, type something in the **Message** text box, and click the **Send** button.</span></span> <span data-ttu-id="5f9a8-254">고유한 사용자 이름과 메시지는 두 페이지 모두에 즉시 표시됩니다.</span><span class="sxs-lookup"><span data-stu-id="5f9a8-254">The unique user name and message are displayed on both pages instantly.</span></span>

---

![두 브라우저 창 모두에 표시된 메시지](signalr-typescript-webpack/_static/browsers-message-broadcast.png)

## <a name="additional-resources"></a><span data-ttu-id="5f9a8-256">추가 자료</span><span class="sxs-lookup"><span data-stu-id="5f9a8-256">Additional resources</span></span>

* <xref:signalr/javascript-client>
* <xref:signalr/hubs>