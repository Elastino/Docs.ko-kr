---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-cs
title: "사용자 지정 경로 제약 조건 (C#) 만들기 | Microsoft Docs"
author: StephenWalther
description: "Stephen Walther 사용자 지정 경로 제약 조건을 만드는 방법을 보여 줍니다. 간단한 구현 되는 경로 방지 하는 사용자 지정 제약 조건을 w 일치 하는 중..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2009
ms.topic: article
ms.assetid: a4f4bf4e-abcc-4650-8f43-527e48b52fe6
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-cs
msc.type: authoredcontent
ms.openlocfilehash: c31ba3382b9dbe22a6826b9f858944c223efdd9d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/10/2017
---
<a name="creating-a-custom-route-constraint-c"></a><span data-ttu-id="d5158-104">사용자 지정 경로 제약 조건 (C#) 만들기</span><span class="sxs-lookup"><span data-stu-id="d5158-104">Creating a Custom Route Constraint (C#)</span></span>
====================
<span data-ttu-id="d5158-105">으로 [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="d5158-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="d5158-106">Stephen Walther 사용자 지정 경로 제약 조건을 만드는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="d5158-106">Stephen Walther demonstrates how you can create a custom route constraint.</span></span> <span data-ttu-id="d5158-107">경로 원격 컴퓨터에서 브라우저 요청이 수행 될 때 일치 하는 것을 금지 하는 간단한 사용자 지정 제약 조건을 구현 합니다.</span><span class="sxs-lookup"><span data-stu-id="d5158-107">We implement a simple custom constraint that prevents a route from being matched when a browser request is made from a remote computer.</span></span>


<span data-ttu-id="d5158-108">이 자습서의 목표는 사용자 지정 경로 제약 조건을 만드는 방법을 보여 주기 위한 것입니다.</span><span class="sxs-lookup"><span data-stu-id="d5158-108">The goal of this tutorial is to demonstrate how you can create a custom route constraint.</span></span> <span data-ttu-id="d5158-109">사용자 지정 경로 제약 조건을 사용 하면 일부 사용자 지정 조건을 일치 하지 않는 일치에서 경로 방지 합니다.</span><span class="sxs-lookup"><span data-stu-id="d5158-109">A custom route constraint enables you to prevent a route from being matched unless some custom condition is matched.</span></span>

<span data-ttu-id="d5158-110">이 자습서에서는 Localhost 경로 제약 조건을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="d5158-110">In this tutorial, we create a Localhost route constraint.</span></span> <span data-ttu-id="d5158-111">Localhost 경로 제약 조건이 로컬 컴퓨터에서 만든 요청에만 일치 합니다.</span><span class="sxs-lookup"><span data-stu-id="d5158-111">The Localhost route constraint only matches requests made from the local computer.</span></span> <span data-ttu-id="d5158-112">인터넷을 통해 원격 요청에서 일치 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="d5158-112">Remote requests from across the Internet are not matched.</span></span>

<span data-ttu-id="d5158-113">IRouteConstraint 인터페이스를 구현 하 여 사용자 지정 경로 상수를 구현 합니다.</span><span class="sxs-lookup"><span data-stu-id="d5158-113">You implement a custom route constraint by implementing the IRouteConstraint interface.</span></span> <span data-ttu-id="d5158-114">다음은 단일 메서드를 설명 하는 매우 간단한 인터페이스입니다.</span><span class="sxs-lookup"><span data-stu-id="d5158-114">This is an extremely simple interface which describes a single method:</span></span>

[!code-csharp[Main](creating-a-custom-route-constraint-cs/samples/sample1.cs)]

<span data-ttu-id="d5158-115">메서드는 부울 값을 반환합니다.</span><span class="sxs-lookup"><span data-stu-id="d5158-115">The method returns a Boolean value.</span></span> <span data-ttu-id="d5158-116">False를 반환 하는 경우이 제약 조건과 연관 된 경로는 브라우저 요청와 일치 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="d5158-116">If you return false, the route associated with the constraint won't match the browser request.</span></span>

<span data-ttu-id="d5158-117">Localhost 제약 조건 목록 1에 포함 됩니다.</span><span class="sxs-lookup"><span data-stu-id="d5158-117">The Localhost constraint is contained in Listing 1.</span></span>

<span data-ttu-id="d5158-118">**1-LocalhostConstraint.cs 나열**</span><span class="sxs-lookup"><span data-stu-id="d5158-118">**Listing 1 - LocalhostConstraint.cs**</span></span>

[!code-csharp[Main](creating-a-custom-route-constraint-cs/samples/sample2.cs)]

<span data-ttu-id="d5158-119">목록 1에서 제약 조건을 HttpRequest 클래스에 의해 노출 IsLocal 속성을 활용 합니다.</span><span class="sxs-lookup"><span data-stu-id="d5158-119">The constraint in Listing 1 takes advantage of the IsLocal property exposed by the HttpRequest class.</span></span> <span data-ttu-id="d5158-120">이 속성이 요청의 IP 주소는 어느 127.0.0.1 또는 요청의 IP 서버의 IP 주소와 동일 하면 true를 반환 합니다.</span><span class="sxs-lookup"><span data-stu-id="d5158-120">This property returns true when the IP address of the request is either 127.0.0.1 or when the IP of the request is the same as the server's IP address.</span></span>

<span data-ttu-id="d5158-121">Global.asax 파일에 정의 된 경로 내에서 사용자 지정 제한을 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="d5158-121">You use a custom constraint within a route defined in the Global.asax file.</span></span> <span data-ttu-id="d5158-122">Global.asax 파일 목록 2의 모든 사용자가 로컬 서버에서 요청을 구성 하지 않는 한 관리 페이지를 요청 하지 않도록 하려면 Localhost 제약 조건을 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="d5158-122">The Global.asax file in Listing 2 uses the Localhost constraint to prevent anyone from requesting an Admin page unless they make the request from the local server.</span></span> <span data-ttu-id="d5158-123">예를 들어 원격 서버에서 만든 /Admin/DeleteAll에 대 한 요청이 실패 합니다.</span><span class="sxs-lookup"><span data-stu-id="d5158-123">For example, a request for /Admin/DeleteAll will fail when made from a remote server.</span></span>

<span data-ttu-id="d5158-124">**2-Global.asax 나열**</span><span class="sxs-lookup"><span data-stu-id="d5158-124">**Listing 2 - Global.asax**</span></span>

[!code-csharp[Main](creating-a-custom-route-constraint-cs/samples/sample3.cs)]

<span data-ttu-id="d5158-125">Localhost 제약 조건은 관리 경로 정의에 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="d5158-125">The Localhost constraint is used in the definition of the Admin route.</span></span> <span data-ttu-id="d5158-126">원격 브라우저 요청에 의해이 경로 일치 시킬 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="d5158-126">This route won't be matched by a remote browser request.</span></span> <span data-ttu-id="d5158-127">그러나 궁극적인 Global.asax에 정의 된 다른 경로 동일한 요청을 일치 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d5158-127">Realize, however, that other routes defined in Global.asax might match the same request.</span></span> <span data-ttu-id="d5158-128">제약 조건으로 인해 일치 한 요청에서 특정 경로 및 Global.asax 파일에 정의 된 모든 경로 이해 하는 것이 유용 합니다.</span><span class="sxs-lookup"><span data-stu-id="d5158-128">It is important to understand that a constraint prevents a particular route from matching a request and not all routes defined in the Global.asax file.</span></span>

<span data-ttu-id="d5158-129">기본 경로 주석 처리 되었습니다 목록 2의 Global.asax 파일에서 확인 합니다.</span><span class="sxs-lookup"><span data-stu-id="d5158-129">Notice that the Default route has been commented out from the Global.asax file in Listing 2.</span></span> <span data-ttu-id="d5158-130">기본 경로 포함 하는 경우 기본 경로 관리 컨트롤러에 대 한 요청 동일 합니다.</span><span class="sxs-lookup"><span data-stu-id="d5158-130">If you include the Default route, then the Default route would match requests for the Admin controller.</span></span> <span data-ttu-id="d5158-131">이 경우 원격 사용자의 요청에는 관리 경로 일치 하지 않아 하는 경우에 관리 컨트롤러의 동작을 여전히 호출할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d5158-131">In that case, remote users could still invoke actions of the Admin controller even though their requests wouldn't match the Admin route.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="d5158-132">[이전](creating-a-route-constraint-cs.md)
[다음](asp-net-mvc-controller-overview-vb.md)</span><span class="sxs-lookup"><span data-stu-id="d5158-132">[Previous](creating-a-route-constraint-cs.md)
[Next](asp-net-mvc-controller-overview-vb.md)</span></span>