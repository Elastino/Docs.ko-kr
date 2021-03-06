---
title: 개별 사용자 계정을 사용 하 여 만든 ASP.NET Core 프로젝트에 따라 문서
author: rick-anderson
description: 개별 사용자 계정을 사용 하 여 만든 ASP.NET Core 프로젝트에 따라 문서를 검색 합니다.
ms.author: riande
ms.date: 11/30/2017
uid: security/authentication/individual
ms.openlocfilehash: f7fb9e8cd1b5c4cc3283ddd7606a0bbd30f554d5
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36274429"
---
# <a name="articles-based-on-aspnet-core-projects-created-with-individual-user-accounts"></a>개별 사용자 계정을 사용 하 여 만든 ASP.NET Core 프로젝트에 따라 문서

ASP.NET Core Id는 "개별 사용자 계정" 옵션을 사용 하 여 Visual Studio에서 프로젝트 템플릿을에 포함 됩니다.

인증 템플릿와.NET Core CLI에서 사용할 수 있는 `-au Individual`:

::: moniker range=">= aspnetcore-2.1"

```console
dotnet new mvc -au Individual
dotnet new webapi -au Individual
dotnet new webapp -au Individual
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```console
dotnet new mvc -au Individual
dotnet new webapi -au Individual
dotnet new razor -au Individual
```

::: moniker-end

다음 문서에는 개별 사용자 계정을 사용 하는 ASP.NET Core 서식 파일에서 생성 된 코드를 사용 하는 방법을 보여 줍니다.

* [SMS를 이용한 2단계 인증](xref:security/authentication/2fa)
* [ASP.NET Core의 계정 확인 및 암호 복구](xref:security/authentication/accconfirm)
* [권한 부여에 의해 보호 되는 사용자 데이터와 ASP.NET Core 응용 프로그램 만들기](xref:security/authorization/secure-data)
