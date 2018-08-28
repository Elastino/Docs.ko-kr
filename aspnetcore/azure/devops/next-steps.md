---
title: ASP.NET Core 및 Azure를 사용 하 여 DevOps | 다음 단계
author: CamSoper
description: Azure에서 호스팅되는 ASP.NET Core 앱에 대한 DevOps 파이프라인을 빌드하는 방법에 대한 종단 간 지침을 제공하는 가이드입니다.
ms.author: casoper
ms.date: 08/07/2018
uid: azure/devops/next-steps
ms.openlocfilehash: 7a0f1b1b56a33b1870e0657d8ba465adb84f5a02
ms.sourcegitcommit: d53e0cc71542b92de867bcce51575b054886f529
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/16/2018
ms.locfileid: "42909435"
---
# <a name="next-steps"></a><span data-ttu-id="c9bab-103">다음 단계</span><span class="sxs-lookup"><span data-stu-id="c9bab-103">Next steps</span></span>

<span data-ttu-id="c9bab-104">이 가이드에서는 ASP.NET Core 샘플 앱에 대 한 DevOps 파이프라인을 만들었습니다.</span><span class="sxs-lookup"><span data-stu-id="c9bab-104">In this guide, you created a DevOps pipeline for an ASP.NET Core sample app.</span></span> <span data-ttu-id="c9bab-105">지금까지</span><span class="sxs-lookup"><span data-stu-id="c9bab-105">Congratulations!</span></span> <span data-ttu-id="c9bab-106">Azure App Service에 ASP.NET Core 웹 앱을 게시 하 고 변경의 연속 통합 자동화를 학습 유익를 바랍니다.</span><span class="sxs-lookup"><span data-stu-id="c9bab-106">We hope you enjoyed learning to publish ASP.NET Core web apps to Azure App Service and automate the continuous integration of changes.</span></span>

<span data-ttu-id="c9bab-107">웹 호스팅 및 DevOps를 Azure에는 다양 한 ASP.NET Core 개발자에 게 유용한 플랫폼-as a Service (PaaS) 서비스에 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c9bab-107">Beyond web hosting and DevOps, Azure has a wide array of Platform-as-a-Service (PaaS) services useful to ASP.NET Core developers.</span></span> <span data-ttu-id="c9bab-108">이 섹션에서는 몇 가지 자주 사용 하는 서비스의 간략 한 개요를 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="c9bab-108">This section gives a brief overview of some of the most commonly used services.</span></span>

## <a name="storage-and-databases"></a><span data-ttu-id="c9bab-109">저장소 및 데이터베이스</span><span class="sxs-lookup"><span data-stu-id="c9bab-109">Storage and databases</span></span>

<span data-ttu-id="c9bab-110">[Redis Cache](https://docs.microsoft.com/azure/redis-cache/) 높은 처리량, 짧은 대기 시간 데이터 캐싱 서비스로 제공 됩니다.</span><span class="sxs-lookup"><span data-stu-id="c9bab-110">[Redis Cache](https://docs.microsoft.com/azure/redis-cache/) is high-throughput, low-latency data caching available as a service.</span></span> <span data-ttu-id="c9bab-111">페이지 출력 캐싱, 데이터베이스 요청 수를 줄이고, 앱의 여러 인스턴스에서 Asp.net 세션 상태 제공에 대해 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c9bab-111">It can be used for caching page output, reducing database requests, and providing ASP.NET Core session state across multiple instances of an app.</span></span>

<span data-ttu-id="c9bab-112">[Azure Storage](https://docs.microsoft.com/azure/storage/) 은 Azure의 뛰어난 클라우드 저장소입니다.</span><span class="sxs-lookup"><span data-stu-id="c9bab-112">[Azure Storage](https://docs.microsoft.com/azure/storage/) is Azure's massively scalable cloud storage.</span></span> <span data-ttu-id="c9bab-113">개발자의 활용할 수 있습니다 [Queue Storage](https://docs.microsoft.com/azure/storage/queues/storage-queues-introduction) 신뢰할 수 있는 메시지 큐에 대 한 및 [Table Storage](https://docs.microsoft.com/azure/storage/tables/table-storage-overview) 는 반 구조화 된 대규모 데이터 집합을 사용한 신속한 개발을 위한 NoSQL 키-값 저장소입니다.</span><span class="sxs-lookup"><span data-stu-id="c9bab-113">Developers can take advantage of [Queue Storage](https://docs.microsoft.com/azure/storage/queues/storage-queues-introduction) for reliable message queuing, and [Table Storage](https://docs.microsoft.com/azure/storage/tables/table-storage-overview) is a NoSQL key-value store designed for rapid development using massive, semi-structured data sets.</span></span>

<span data-ttu-id="c9bab-114">[Azure SQL Database](https://docs.microsoft.com/azure/sql-database/) Microsoft SQL Server 엔진을 사용 하는 서비스와 친숙 한 관계형 데이터베이스 기능을 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="c9bab-114">[Azure SQL Database](https://docs.microsoft.com/azure/sql-database/) provides familiar relational database functionality as a service using the Microsoft SQL Server Engine.</span></span>

<span data-ttu-id="c9bab-115">[Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/) 전역적으로 분산 된 다중 모델 NoSQL 데이터베이스 서비스입니다.</span><span class="sxs-lookup"><span data-stu-id="c9bab-115">[Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/) globally distributed, multi-model NoSQL database service.</span></span> <span data-ttu-id="c9bab-116">여러 Api (이전의 DocumentDB)는 SQL API, Cassandra, MongoDB 등에서 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c9bab-116">Multiple APIs are available, including SQL API (formerly called DocumentDB), Cassandra, and MongoDB.</span></span>

## <a name="identity"></a><span data-ttu-id="c9bab-117">클레임</span><span class="sxs-lookup"><span data-stu-id="c9bab-117">Identity</span></span>

<span data-ttu-id="c9bab-118">[Azure Active Directory](https://docs.microsoft.com/azure/active-directory/) 하 고 [Azure Active Directory B2C](https://docs.microsoft.com/azure/active-directory-b2c/) 두 id 서비스 됩니다.</span><span class="sxs-lookup"><span data-stu-id="c9bab-118">[Azure Active Directory](https://docs.microsoft.com/azure/active-directory/) and [Azure Active Directory B2C](https://docs.microsoft.com/azure/active-directory-b2c/) are both identity services.</span></span> <span data-ttu-id="c9bab-119">Azure Active Directory는 엔터프라이즈 시나리오를 위해 설계 되었습니다 및 Azure Active Directory B2C는 의도 한 비즈니스 고객 시나리오, 소셜 네트워크 로그인 포함 하 여 Azure AD B2B (기업 간) 공동 작업을 사용 하도록 설정 합니다.</span><span class="sxs-lookup"><span data-stu-id="c9bab-119">Azure Active Directory is designed for enterprise scenarios and enables Azure AD B2B (business-to-business) collaboration, while Azure Active Directory B2C is intended business-to-customer scenarios, including social network sign-in.</span></span>

## <a name="mobile"></a><span data-ttu-id="c9bab-120">휴대폰</span><span class="sxs-lookup"><span data-stu-id="c9bab-120">Mobile</span></span>

<span data-ttu-id="c9bab-121">[Notification Hubs](https://docs.microsoft.com/azure/notification-hubs/) 는 다중 플랫폼의 확장성이 뛰어난 푸시 알림 엔진으로 다양 한 종류의 장치에서 실행 되는 앱에 수백만 개의 메시지를 신속 하 게 보내려고 합니다.</span><span class="sxs-lookup"><span data-stu-id="c9bab-121">[Notification Hubs](https://docs.microsoft.com/azure/notification-hubs/) is a multi-platform, scalable push-notification engine to quickly send millions of messages to apps running on various types of devices.</span></span>

## <a name="web-infrastructure"></a><span data-ttu-id="c9bab-122">웹 인프라</span><span class="sxs-lookup"><span data-stu-id="c9bab-122">Web infrastructure</span></span>

<span data-ttu-id="c9bab-123">[Azure Container Service](https://docs.microsoft.com/azure/aks/) 을 빠르고 쉽게 배포 및 컨테이너 오케스트레이션 전문 지식 없이 컨테이너 화 된 앱 관리에 호스트 된 Kubernetes 환경을 관리 합니다.</span><span class="sxs-lookup"><span data-stu-id="c9bab-123">[Azure Container Service](https://docs.microsoft.com/azure/aks/) manages your hosted Kubernetes environment, making it quick and easy to deploy and manage containerized apps without container orchestration expertise.</span></span>

<span data-ttu-id="c9bab-124">[Azure Search](https://docs.microsoft.com/azure/search/) 비공개, 이기종 콘텐츠에 대해 엔터프라이즈 검색 솔루션을 만드는 데 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="c9bab-124">[Azure Search](https://docs.microsoft.com/azure/search/) is used to create an enterprise search solution over private, heterogenous content.</span></span>

<span data-ttu-id="c9bab-125">[Service Fabric](https://docs.microsoft.com/azure/service-fabric/) 은 쉽게 패키지, 배포 및 관리에 확장성 있는 분산된 시스템 플랫폼 및 안정성이 뛰어난 마이크로 서비스 및 컨테이너.</span><span class="sxs-lookup"><span data-stu-id="c9bab-125">[Service Fabric](https://docs.microsoft.com/azure/service-fabric/) is a distributed systems platform that makes it easy to package, deploy, and manage scalable and reliable microservices and containers.</span></span>