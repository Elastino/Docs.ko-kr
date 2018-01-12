---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file
title: "명령 파일 만들기 및 배포를 실행 합니다. | Microsoft Docs"
author: jrjlee
description: "이 항목에서는 다시 단일 단계로, Microsoft Build Engine (MSBuild) 프로젝트 파일을 사용 하 여 배포를 실행할 수 있도록 명령 파일을 작성 하는 방법에 설명..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: c61560e9-9f6c-4985-834a-08a3eabf9c3c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file
msc.type: authoredcontent
ms.openlocfilehash: 729b4fa4c461eedbd0447371102010451eb51586
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/10/2017
---
<a name="creating-and-running-a-deployment-command-file"></a><span data-ttu-id="1a4eb-103">만들기 및 배포 명령 파일을 실행 합니다.</span><span class="sxs-lookup"><span data-stu-id="1a4eb-103">Creating and Running a Deployment Command File</span></span>
====================
<span data-ttu-id="1a4eb-104">으로 [Jason lee 공저](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="1a4eb-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="1a4eb-105">PDF 다운로드</span><span class="sxs-lookup"><span data-stu-id="1a4eb-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="1a4eb-106">이 항목에서는 단일 단계, 반복 프로세스로 Microsoft Build Engine (MSBuild) 프로젝트 파일을 사용 하 여 배포를 실행할 수 있도록 명령 파일을 작성 하는 방법을 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="1a4eb-106">This topic describes how to build a command file that will let you run a deployment using Microsoft Build Engine (MSBuild) project files as a single-step, repeatable process.</span></span>


<span data-ttu-id="1a4eb-107">이 항목의 Fabrikam, inc. 라는 가상 회사의 엔터프라이즈 배포 요구 사항을 바탕으로 하는 자습서 시리즈의 일부를 형성 합니다. 이 자습서 시리즈 샘플 솔루션 & #x 2014;을 사용 하는 [않아](the-contact-manager-solution.md) 솔루션 & #x 2014; 현실적인 수준의 복잡성을 Windows ASP.NET MVC 3 응용 프로그램을 포함 하 여 웹 응용 프로그램을 나타내기 위해 Communication Foundation (WCF) 서비스 및 데이터베이스 프로젝트를 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="1a4eb-107">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager](the-contact-manager-solution.md) solution&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

<span data-ttu-id="1a4eb-108">이 자습서의 핵심에는 배포 방법에 설명 된 분할 프로젝트 파일 접근 방식에 따라 [빌드 프로세스를 이해](understanding-the-build-process.md)합니다 빌드 프로세스에 의해 제어 되는 두 프로젝트에 파일 & #x 2014; 포함 환경 관련 빌드 및 배포 설정을 포함 하는 하나 및 모든 대상 환경에 적용 되는 지침을 빌드하십시오.</span><span class="sxs-lookup"><span data-stu-id="1a4eb-108">The deployment method at the heart of these tutorials is based on the split project file approach described in [Understanding the Build Process](understanding-the-build-process.md), in which the build process is controlled by two project files&#x2014;one containing build instructions that apply to every destination environment, and one containing environment-specific build and deployment settings.</span></span> <span data-ttu-id="1a4eb-109">빌드 시 환경 관련 프로젝트 파일은 빌드 지침의 전체 집합을 구성 하기 위해 환경을 알 수 없는 프로젝트 파일에 병합 됩니다.</span><span class="sxs-lookup"><span data-stu-id="1a4eb-109">At build time, the environment-specific project file is merged into the environment-agnostic project file to form a complete set of build instructions.</span></span>

## <a name="process-overview"></a><span data-ttu-id="1a4eb-110">프로세스 개요</span><span class="sxs-lookup"><span data-stu-id="1a4eb-110">Process Overview</span></span>

<span data-ttu-id="1a4eb-111">이 항목을 만들고이 프로젝트 파일을 사용 하 여 대상 환경에는 반복 가능한 배포를 수행 하는 명령 파일을 실행 하는 방법을 배웁니다.</span><span class="sxs-lookup"><span data-stu-id="1a4eb-111">In this topic, you'll learn how to create and run a command file that uses these project files to perform a repeatable deployment to your target environment.</span></span> <span data-ttu-id="1a4eb-112">기본적으로, 명령 파일 단순히 포함 되어야 MSBuild 명령입니다.</span><span class="sxs-lookup"><span data-stu-id="1a4eb-112">Essentially, the command file simply needs to contain an MSBuild command that:</span></span>

- <span data-ttu-id="1a4eb-113">환경 중립적을 실행 하는 MSBuild 지시 *Publish.proj* 파일입니다.</span><span class="sxs-lookup"><span data-stu-id="1a4eb-113">Tells MSBuild to execute the environment-agnostic *Publish.proj* file.</span></span>
- <span data-ttu-id="1a4eb-114">지시는 *Publish.proj* 파일 환경 관련 프로젝트 설정과 그 위치를 파일에 포함 되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="1a4eb-114">Tells the *Publish.proj* file which file contains the environment-specific project settings and where to find it.</span></span>

## <a name="create-an-msbuild-command"></a><span data-ttu-id="1a4eb-115">MSBuild 명령 만들기</span><span class="sxs-lookup"><span data-stu-id="1a4eb-115">Create an MSBuild Command</span></span>

<span data-ttu-id="1a4eb-116">에 설명 된 대로 [빌드 프로세스를 이해](understanding-the-build-process.md), 환경별 프로젝트 파일 & #x 2014; 예를 들어 *Env Dev.proj*& #x 2014;으로 가져올 수 있도록 설계 된 알 수 없는 환경 *Publish.proj* 빌드 시에는 파일입니다.</span><span class="sxs-lookup"><span data-stu-id="1a4eb-116">As described in [Understanding the Build Process](understanding-the-build-process.md), the environment-specific project file&#x2014;for example, *Env-Dev.proj*&#x2014;is designed to be imported into the environment-agnostic *Publish.proj* file at build time.</span></span> <span data-ttu-id="1a4eb-117">함께이 두 파일 전체 집합이 MSBuild를 빌드 및 솔루션을 배포 하는 방법을 지시 하는 지침을 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="1a4eb-117">Together, these two files provide a complete set of instructions that tell MSBuild how to build and deploy your solution.</span></span>

<span data-ttu-id="1a4eb-118">*Publish.proj* 파일을 사용 하 여 프로그램 **가져올** 환경별 프로젝트 파일을 가져올 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="1a4eb-118">The *Publish.proj* file uses an **Import** element to import the environment-specific project file.</span></span>


[!code-xml[Main](creating-and-running-a-deployment-command-file/samples/sample1.xml)]


<span data-ttu-id="1a4eb-119">따라서 MSBuild.exe 빌드하고 Contact Manager 솔루션 배포를 사용 하 여 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="1a4eb-119">As such, when you use MSBuild.exe to build and deploy the Contact Manager solution, you need to:</span></span>

- <span data-ttu-id="1a4eb-120">MSBuild.exe에서 실행 된 *Publish.proj* 파일입니다.</span><span class="sxs-lookup"><span data-stu-id="1a4eb-120">Run MSBuild.exe on the *Publish.proj* file.</span></span>
- <span data-ttu-id="1a4eb-121">라는 명령줄 매개 변수를 제공 하 여 환경 관련 프로젝트 파일의 위치 지정 **TargetEnvPropsFile**합니다.</span><span class="sxs-lookup"><span data-stu-id="1a4eb-121">Specify the location of the environment-specific project file by supplying a command-line parameter named **TargetEnvPropsFile**.</span></span>

<span data-ttu-id="1a4eb-122">이 위해 MSBuild 명령을 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="1a4eb-122">To do this, your MSBuild command should resemble this:</span></span>


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample2.cmd)]


<span data-ttu-id="1a4eb-123">여기에서는 반복 가능 하 고 1 단계 배포로 이동 하려면 간단한 단계를 수행 합니다.</span><span class="sxs-lookup"><span data-stu-id="1a4eb-123">From here, it's a simple step to move to a repeatable, single-step deployment.</span></span> <span data-ttu-id="1a4eb-124">하기만 하면.cmd 파일을 MSBuild 명령을 추가 하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="1a4eb-124">All you need to do is to add your MSBuild command to a .cmd file.</span></span> <span data-ttu-id="1a4eb-125">Contact Manager 솔루션에서는 게시 폴더에는 *게시 Dev.cmd* 정확 하 게이 수행 하는 합니다.</span><span class="sxs-lookup"><span data-stu-id="1a4eb-125">In the Contact Manager solution, the Publish folder includes a file named *Publish-Dev.cmd* that does exactly this.</span></span>


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample3.cmd)]


> [!NOTE]
> <span data-ttu-id="1a4eb-126">**/fl** 스위치를 지정 하면 명명 된 로그 파일을 만들 MSBuild *msbuild.log* MSBuild.exe 호출 된 작업 디렉터리에 있습니다.</span><span class="sxs-lookup"><span data-stu-id="1a4eb-126">The **/fl** switch instructs MSBuild to create a log file named *msbuild.log* in the working directory in which MSBuild.exe was invoked.</span></span>


<span data-ttu-id="1a4eb-127">를 배포 하거나 Contact Manager 솔루션을 재배포 하기만 하면 실행 되는 *게시 Dev.cmd* 파일입니다.</span><span class="sxs-lookup"><span data-stu-id="1a4eb-127">To deploy or redeploy the Contact Manager solution, all you need to do is run the *Publish-Dev.cmd* file.</span></span> <span data-ttu-id="1a4eb-128">파일을 실행 하면 때 MSBuild 취소 됩니다.</span><span class="sxs-lookup"><span data-stu-id="1a4eb-128">When you run the file, MSBuild will:</span></span>

- <span data-ttu-id="1a4eb-129">솔루션의 모든 프로젝트를 빌드하십시오.</span><span class="sxs-lookup"><span data-stu-id="1a4eb-129">Build all the projects in the solution.</span></span>
- <span data-ttu-id="1a4eb-130">웹 응용 프로그램 프로젝트에 대 한 웹 배포 가능한 패키지를 생성 합니다.</span><span class="sxs-lookup"><span data-stu-id="1a4eb-130">Generate deployable web packages for the web application projects.</span></span>
- <span data-ttu-id="1a4eb-131">데이터베이스 프로젝트에 대 한.dbschema 및.deploymanifest 파일을 생성 합니다.</span><span class="sxs-lookup"><span data-stu-id="1a4eb-131">Generate .dbschema and .deploymanifest files for the database projects.</span></span>
- <span data-ttu-id="1a4eb-132">웹 서버에 웹 패키지를 배포 합니다.</span><span class="sxs-lookup"><span data-stu-id="1a4eb-132">Deploy the web packages to the web server.</span></span>
- <span data-ttu-id="1a4eb-133">데이터베이스 서버에 데이터베이스를 배포 합니다.</span><span class="sxs-lookup"><span data-stu-id="1a4eb-133">Deploy the database to the database server.</span></span>

## <a name="run-the-deployment"></a><span data-ttu-id="1a4eb-134">배포 실행</span><span class="sxs-lookup"><span data-stu-id="1a4eb-134">Run the Deployment</span></span>

<span data-ttu-id="1a4eb-135">대상 환경에 대 한 명령 파일을 만들었으면 단순히 파일을 실행 하 여 전체 배포를 완료할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="1a4eb-135">When you've created a command file for your target environment, you should be able to complete the entire deployment by simply running the file.</span></span>

<span data-ttu-id="1a4eb-136">**테스트 환경에 않아 솔루션을 배포 하려면**</span><span class="sxs-lookup"><span data-stu-id="1a4eb-136">**To deploy the Contact Manager solution to your test environment**</span></span>

1. <span data-ttu-id="1a4eb-137">개발자 워크스테이션에서 Windows 탐색기를 열고 다음의 위치를 찾습니다는 *게시 Dev.cmd* 파일입니다.</span><span class="sxs-lookup"><span data-stu-id="1a4eb-137">On your developer workstation, open Windows Explorer, and then browse to the location of the *Publish-Dev.cmd* file.</span></span>
2. <span data-ttu-id="1a4eb-138">실행할 파일을 두 번 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="1a4eb-138">Double-click the file to run it.</span></span>
3. <span data-ttu-id="1a4eb-139">경우는 **파일 열기-보안 경고** 대화 상자가 나타나면 클릭 **실행**합니다.</span><span class="sxs-lookup"><span data-stu-id="1a4eb-139">If an **Open File – Security Warning** dialog box appears, click **Run**.</span></span>
4. <span data-ttu-id="1a4eb-140">구성 설정 및 테스트 서버를 설정 된 경우 제대로 명령 프롬프트 창에 표시 됩니다는 **빌드 성공** MSBuild 프로젝트 파일의 처리를 완료 하는 경우 메시지입니다.</span><span class="sxs-lookup"><span data-stu-id="1a4eb-140">If your configuration settings and test servers are set up correctly, the Command Prompt window will show a **Build succeeded** message when MSBuild has finished processing the project files.</span></span>

    ![](creating-and-running-a-deployment-command-file/_static/image1.png)
5. <span data-ttu-id="1a4eb-141">에 테스트 웹 서버 컴퓨터 계정을 추가 해야 처음으로이 환경에 솔루션을 배포한 경우는 **db\_datawriter** 및 **db\_datareader**에 대 한 역할의 **ContactManager** 데이터베이스입니다.</span><span class="sxs-lookup"><span data-stu-id="1a4eb-141">If this is the first time you've deployed the solution to this environment, you'll need to add the test web server machine account to the **db\_datawriter** and **db\_datareader** roles on the **ContactManager** database.</span></span> <span data-ttu-id="1a4eb-142">이 절차에 설명 되어 [웹 배포 게시에 대 한 데이터베이스 서버를 구성](../configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing.md)합니다.</span><span class="sxs-lookup"><span data-stu-id="1a4eb-142">This procedure is described in [Configure a Database Server for Web Deploy Publishing](../configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing.md).</span></span>

    > [!NOTE]
    > <span data-ttu-id="1a4eb-143">데이터베이스를 만들 때 이러한 권한을 할당 하기만 하면 됩니다.</span><span class="sxs-lookup"><span data-stu-id="1a4eb-143">You only need to assign these permissions when you create the database.</span></span> <span data-ttu-id="1a4eb-144">기본적으로 빌드 프로세스에서 모든 배포 & #x 2014에서 데이터베이스를 다시; 대신, 최신 스키마로의 기존 데이터베이스를 비교 하 고 필요한 변경 내용만 확인 하십시오.</span><span class="sxs-lookup"><span data-stu-id="1a4eb-144">By default, the build process will not recreate the database on every deployment&#x2014;instead, it will compare the existing database to the latest schema and make only the changes required.</span></span> <span data-ttu-id="1a4eb-145">결과적으로, 솔루션을 배포한 처음으로 이러한 데이터베이스 역할을 매핑할 필요가 합니다.</span><span class="sxs-lookup"><span data-stu-id="1a4eb-145">As a result, you should only need to map these database roles the first time you deploy the solution.</span></span>
6. <span data-ttu-id="1a4eb-146">Internet Explorer를 열고 않아 응용 프로그램의 URL로 이동 (예를 들어 `http://testweb1:85/ContactManager/`).</span><span class="sxs-lookup"><span data-stu-id="1a4eb-146">Open Internet Explorer and browse to the URL of the Contact Manager application (for example, `http://testweb1:85/ContactManager/`).</span></span>
7. <span data-ttu-id="1a4eb-147">응용 프로그램이 예상 대로 작동 하는지 확인 하 고 연락처를 추가할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="1a4eb-147">Verify that the application works as expected and you're able to add contacts.</span></span>

    ![](creating-and-running-a-deployment-command-file/_static/image2.png)

## <a name="conclusion"></a><span data-ttu-id="1a4eb-148">결론</span><span class="sxs-lookup"><span data-stu-id="1a4eb-148">Conclusion</span></span>

<span data-ttu-id="1a4eb-149">MSBuild 지침이 포함 된 명령 파일 만들기의 구축 및 특정 대상 환경으로 다중 프로젝트 솔루션을 배포 하는 빠르고 쉬운 방법을 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="1a4eb-149">Creating a command file containing your MSBuild instructions provides you with a quick and easy way of building and deploying a multi-project solution to a specific destination environment.</span></span> <span data-ttu-id="1a4eb-150">반복 해 서 여러 대상 환경에 솔루션을 배포 해야 할 경우에 여러 개의 명령 파일을 만들 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="1a4eb-150">If you need to repeatedly deploy your solution to multiple destination environments, you can create multiple command files.</span></span> <span data-ttu-id="1a4eb-151">각 명령 파일의 MSBuild 명령이 빌드할 같은 유니버설 프로젝트 파일 하지만 다른 환경 관련 프로젝트 파일을 지정 합니다.</span><span class="sxs-lookup"><span data-stu-id="1a4eb-151">In each command file, the MSBuild command will build the same universal project file, but it will specify a different environment-specific project file.</span></span> <span data-ttu-id="1a4eb-152">예를 들어 테스트 환경 또는 개발자에 게시 하는 명령 파일에는 MSBuild 명령을이 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="1a4eb-152">For example, a command file to publish to a developer or test environment might contain this MSBuild command:</span></span>


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample4.cmd)]


<span data-ttu-id="1a4eb-153">스테이징 환경에 게시 하는 명령 파일에는이 MSBuild 명령을 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="1a4eb-153">A command file to publish to a staging environment might contain this MSBuild command:</span></span>


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample5.cmd)]


> [!NOTE]
> <span data-ttu-id="1a4eb-154">사용자 고유의 서버 환경에 대 한 환경 관련 프로젝트 파일 사용자 지정 하는 방법에 대 한 지침을 참조 하십시오. [대상 환경에 대 한 배포 속성 구성](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md)합니다.</span><span class="sxs-lookup"><span data-stu-id="1a4eb-154">For guidance on how to customize the environment-specific project files for your own server environments, see [Configure Deployment Properties for a Target Environment](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).</span></span>


<span data-ttu-id="1a4eb-155">또한 속성을 재정의 하거나 MSBuild 명령에 다른 다양 한 스위치를 설정 하 여 각 환경에 대 한 빌드 프로세스를 사용자 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="1a4eb-155">You can also customize the build process for each environment by overriding properties or setting various other switches in your MSBuild command.</span></span> <span data-ttu-id="1a4eb-156">자세한 내용은 참조 [MSBuild 명령줄 참조](https://msdn.microsoft.com/en-us/library/ms164311.aspx)합니다.</span><span class="sxs-lookup"><span data-stu-id="1a4eb-156">For more information, see [MSBuild Command Line Reference](https://msdn.microsoft.com/en-us/library/ms164311.aspx).</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="1a4eb-157">[이전](deploying-database-projects.md)
[다음](manually-installing-web-packages.md)</span><span class="sxs-lookup"><span data-stu-id="1a4eb-157">[Previous](deploying-database-projects.md)
[Next](manually-installing-web-packages.md)</span></span>