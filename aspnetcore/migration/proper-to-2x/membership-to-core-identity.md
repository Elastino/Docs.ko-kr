---
title: ASP.NET 멤버 자격 인증에서 ASP.NET Core 2.0 Id로 마이그레이션
author: isaac2004
description: ASP.NET Core 2.0 Id를 인증 멤버 자격을 사용 하는 기존 ASP.NET 응용 프로그램을 마이그레이션하는 방법에 알아봅니다.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 04/24/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/proper-to-2x/membership-to-core-identity
ms.openlocfilehash: f0d1099bfda01d036831350e0888ae3830ad3d58
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/08/2018
---
# <a name="migrate-from-aspnet-membership-authentication-to-aspnet-core-20-identity"></a>ASP.NET 멤버 자격 인증에서 ASP.NET Core 2.0 Id로 마이그레이션

작성자: [Isaac Levin](https://isaaclevin.com)

이 문서 구성원 인증 ASP.NET 코어 2.0 id를 사용 하 여 ASP.NET 응용 프로그램에 대 한 데이터베이스 스키마를 마이그레이션하는 방법을 보여 줍니다.

> [!NOTE]
> 이 문서는 ASP.NET Core Id에 사용 되는 데이터베이스 스키마를 ASP.NET 멤버 자격 기반 앱에 대 한 데이터베이스 스키마를 마이그레이션하는 데 필요한 단계를 제공 합니다. ASP.NET Id에 ASP.NET 멤버 자격 기반 인증에서 마이그레이션에 대 한 자세한 내용은 참조 [SQL 멤버 자격에서 ASP.NET Identity에는 기존 응용 프로그램을 마이그레이션할](/aspnet/identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity)합니다. ASP.NET Core Id에 대 한 자세한 내용은 참조 [ASP.NET Core에 Id 소개](xref:security/authentication/identity)합니다.

## <a name="review-of-membership-schema"></a>멤버 자격 스키마의 검토

ASP.NET 2.0 이전 버전의 앱에 대 한 전체 인증 및 권한 부여 프로세스를 만드는 개발자 해야 된 합니다. ASP.NET 2.0을 ASP.NET 앱 내에서 보안을 처리 하는 상용구 솔루션 제공 멤버 자격 도입 되었습니다. 개발자와 SQL Server 데이터베이스에 스키마를 부트스트랩 하 못했습니다 이제는 [aspnet_regsql.exe](https://msdn.microsoft.com/library/ms229862.aspx) 명령입니다. 이 명령을 실행 한 후 다음 표에서 데이터베이스에 만들어졌습니다.

  ![멤버 자격 테이블](identity/_static/membership-tables.png)

ASP.NET Core 2.0 Id를 기존 앱으로 마이그레이션할 하려면 이러한 테이블의 데이터를 새 Id 스키마에서 사용 하는 테이블을 마이그레이션할 수 있어야 합니다.

## <a name="aspnet-core-identity-20-schema"></a>ASP.NET Core Identity 2.0 스키마

ASP.NET Core 2.0 뒤에 오는 [Identity](/aspnet/identity/index) 원칙 ASP.NET 4.5에서 도입 되었습니다. ASP.NET Core의 버전 간에 서로 다른는 프레임 워크 간에 구현을 원칙 전체에서 공유 하는 경우 (참조 [ASP.NET 코어 2.0 인증 및 Id로](xref:migration/1x-to-2x/index)).

ASP.NET Core 2.0 Id에 대 한 스키마를 볼 수 있는 가장 빠른 방법은 새 ASP.NET 코어 2.0 응용 프로그램을 만드는 것입니다. Visual Studio 2017에 다음이 단계를 수행 합니다.

* **파일** > **새로 만들기** > **프로젝트**를 선택합니다.
* 새 **ASP.NET Core 웹 응용 프로그램**, 선택한 프로젝트의 이름을 *CoreIdentitySample*합니다.
* 드롭다운에서 **ASP.NET Core 2.0**을 선택하고 **웹 응용 프로그램**을 선택합니다. 이 서식 파일을 생성 한 [Razor 페이지](xref:mvc/razor-pages/index) 응용 프로그램입니다. 클릭 하기 전에 **확인**, 클릭 **인증 변경**합니다.
* 선택 **개별 사용자 계정** Identity 템플릿에 대 한 합니다. 마지막으로, 클릭 **확인**, 다음 **확인**합니다. Visual Studio에서 ASP.NET Core Id 템플릿을 사용 하 여 프로젝트를 만듭니다.

사용 하 여 ASP.NET Core 2.0 Identity [Entity Framework Core](/ef/core) 인증 데이터를 저장 하는 데이터베이스와 상호 작용할 수 있습니다. 새로 만든된 응용 프로그램에서 작업을 위해 필요이 데이터를 저장할 데이터베이스를 사용 해야 합니다. 새 응용 프로그램을 만든 후 데이터베이스 환경에서 스키마를 검사 하는 가장 빠른 방법은 Entity Framework 마이그레이션을 사용 하 여 데이터베이스를 만드는 것입니다. 이 프로세스에서는 데이터베이스 중 하나를 로컬로 또는 다른 위치에 해당 스키마를 유사한 합니다. 자세한 내용은 위의 설명서를 검토 합니다.

ASP.NET Core Id 스키마를 사용 하 여 데이터베이스를 만들려면 실행는 `Update-Database` Visual Studio에서 명령을 **패키지 관리자 콘솔** (PMC) 창&mdash;에 위치한 **도구**  >  **NuGet 패키지 관리자** > **패키지 관리자 콘솔**합니다. PMC는 Entity Framework 명령 실행을 지원합니다.

에 지정 된 데이터베이스에 대 한 연결 문자열을 사용 하는 entity Framework 명령을 *appsettings.json*합니다. 다음 연결 문자열에는 데이터베이스를 대상으로 *localhost* 라는 *asp net-코어 id*합니다. Entity Framework 사용 하도록 구성 된이 설정에는 `DefaultConnection` 연결 문자열입니다.

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=localhost;Database=aspnet-core-identity;Trusted_Connection=True;MultipleActiveResultSets=true"
  }
}
```

이 명령은 스키마와 함께 지정 된 데이터베이스 및 응용 프로그램 초기화에 필요한 모든 데이터를 빌드합니다. 다음 그림에서는 위의 단계를 사용 하 여 만든 테이블 구조를 보여 줍니다.

   ![Identity 테이블](identity/_static/identity-tables.png)

## <a name="migrate-the-schema"></a>스키마 마이그레이션

테이블 구조와 멤버 자격 및 ASP.NET 코어 Id 모두에 대 한 필드에서 약간의 차이가 있습니다. ASP.NET 및 ASP.NET Core 응용 프로그램과 인증/권한 부여에 대 한 패턴 크게 변경 되었습니다. Id를 가진 여전히 사용 되는 주요 개체는 *사용자* 및 *역할*합니다. 다음에 대 한 매핑 테이블은 *사용자*, *역할*, 및 *UserRoles*합니다.

### <a name="users"></a>사용자

| *Identity(AspNetUsers)* |   | *Membership(aspnet_Users/aspnet_Membership)* ||
| --- | --- | --- | --- | --- | --- |
| **필드 이름** | **Type**  |   **필드 이름** | **Type**  |
|`Id` | string | `aspnet_Users.UserId` | string
|`UserName` | string | `aspnet_Users.UserName` | string
|`Email` | string | `aspnet_Membership.Email` | string
|`NormalizedUserName` | string | `aspnet_Users.LoweredUserName` | string
|`NormalizedEmail` | string | `aspnet_Membership.LoweredEmail` | string
|`PhoneNumber` | string | `aspnet_Users.MobileAlias` | string
|`LockoutEnabled` | 비트 | `aspnet_Membership.IsLockedOut` | 비트

> [!NOTE]
> 일부 필드 매핑 ASP.NET Core Id에 멤버 자격에서 한 일 관계와 유사합니다. 앞의 표에에서는 기본 멤버 자격 사용자 스키마 및 ASP.NET 코어 Id 스키마로 매핑합니다. 멤버 자격에 사용 된 다른 모든 사용자 지정 필드를 수동으로 매핑할 수 해야 합니다. 이 매핑이 암호의 맵이 없습니다 암호 기준과 암호 솔트 둘 간의 마이그레이션 하지 않습니다. **null로 암호를 그대로 두고가 암호를 재설정 하도록 요청 하려면 것이 좋습니다.** ASP.NET Core Id에서 `LockoutEnd` 경우에 사용자가 잠겨 일부 미래 날짜를 설정 해야 합니다. 마이그레이션 스크립트에서이 확인할 수 있습니다.

### <a name="roles"></a>역할

| *Identity(AspNetRoles)* |   | *Membership(aspnet_Roles)* ||
| --- | --- | --- | --- | --- | --- |
| **필드 이름** | **Type**  |   **필드 이름** | **Type**  |
|`Id` | string | `RoleId` | string
|`Name` | string | `RoleName` | string
|`NormalizedName` | string | `LoweredRoleName` | string

### <a name="user-roles"></a>사용자 역할

| *Identity(AspNetUserRoles)* |   | *Membership(aspnet_UsersInRoles)* ||
| --- | --- | --- | --- | --- | --- |
| **필드 이름** | **Type**  |   **필드 이름** | **Type**  |
|`RoleId` | string | `RoleId` | string
|`UserId` | string | `UserId` | string

에 대 한 마이그레이션 스크립트를 만들 때 앞의 매핑 테이블을 참조할 *사용자* 및 *역할*합니다. 다음 예제에서는 두 개의 데이터베이스가 데이터베이스 서버에 있는 가정 합니다. 한 데이터베이스는 기존 ASP.NET 멤버 자격 스키마와 데이터를 포함합니다. 다른 데이터베이스를 앞에서 설명한 단계를 사용 하 여 만들어졌습니다. 주석은 자세한 세부 정보에 대 한 인라인에 포함된 되어 있습니다.

```sql
-- THIS SCRIPT NEEDS TO RUN FROM THE CONTEXT OF THE MEMBERSHIP DB
BEGIN TRANSACTION MigrateUsersAndRoles
use aspnetdb

-- INSERT USERS
INSERT INTO coreidentity.dbo.aspnetusers
            (id,
             username,
             normalizedusername,
             passwordhash,
             securitystamp,
             emailconfirmed,
             phonenumber,
             phonenumberconfirmed,
             twofactorenabled,
             lockoutend,
             lockoutenabled,
             accessfailedcount,
             email,
             normalizedemail)
SELECT aspnet_users.userid,
       aspnet_users.username,
       aspnet_users.loweredusername,
       --Creates an empty password since passwords don't map between the two schemas
       '',
       --Security Stamp is a token used to verify the state of an account and is subject to change at any time. It should be intialized as a new ID.
       NewID(),
       --EmailConfirmed is set when a new user is created and confirmed via email. Users must have this set during migration to ensure they're able to reset passwords.
       1,
       aspnet_users.mobilealias,
       CASE
         WHEN aspnet_Users.MobileAlias is null THEN 0
         ELSE 1
       END,
       --2-factor Auth likely wasn't setup in Membership for users, so setting as false.
       0,
       CASE
         --Setting lockout date to time in the future (1000 years)
         WHEN aspnet_membership.islockedout = 1 THEN Dateadd(year, 1000,
                                                     Sysutcdatetime())
         ELSE NULL
       END,
       aspnet_membership.islockedout,
       --AccessFailedAccount is used to track failed logins. This is stored in membership in multiple columns. Setting to 0 arbitrarily.
       0,
       aspnet_membership.email,
       aspnet_membership.loweredemail
FROM   aspnet_users
       LEFT OUTER JOIN aspnet_membership
                    ON aspnet_membership.applicationid =
                       aspnet_users.applicationid
                       AND aspnet_users.userid = aspnet_membership.userid
       LEFT OUTER JOIN coreidentity.dbo.aspnetusers
                    ON aspnet_membership.userid = aspnetusers.id
WHERE  aspnetusers.id IS NULL

-- INSERT ROLES
INSERT INTO coreIdentity.dbo.aspnetroles(id,name)
SELECT roleId,rolename
FROM aspnet_roles;

-- INSERT USER ROLES
INSERT INTO coreidentity.dbo.aspnetuserroles(userid,roleid)
SELECT userid,roleid
FROM aspnet_usersinroles;

IF @@ERROR <> 0
  BEGIN
    ROLLBACK TRANSACTION MigrateUsersAndRoles
    RETURN
  END

COMMIT TRANSACTION MigrateUsersAndRoles
```

이 스크립트의 작업이 완료 되 면 앞에서 만든 ASP.NET Core Id 응용 프로그램 멤버 자격 사용자로 채워집니다. 사용자 로그인 시 암호를 변경 해야 합니다.

> [!NOTE]
> 멤버 자격 시스템에는 사용자가 자신의 전자 메일 주소를 일치 하지 않는 사용자 이름으로, 변경 내용이 수용 하기 위해 앞에서 만든 앱에 필요한 됩니다. 기본 서식 파일에서는 `UserName` 및 `Email` 동일 해야 합니다. 다른 하는 경우에 대 한 로그인 프로세스를 사용 하도록 수정 해야 `UserName` 대신 `Email`합니다.

에 `PageModel` 에 있는 로그인 페이지의 *Pages\Account\Login.cshtml.cs*, 제거는 `[EmailAddress]` 에서 특성의 *전자 메일* 속성입니다. 이 파일 이름을 *UserName*합니다. 이렇게 변경 하려면 때마다 `EmailAddress` 에서 언급 되는 *보기* 및 *PageModel*합니다. 결과 다음과 같습니다.

 ![고정된 로그인](identity/_static/fixed-login.png)

## <a name="next-steps"></a>다음 단계

이 자습서에서는 SQL 멤버 자격에서 사용자가 ASP.NET 코어 2.0 Id로 이식 하는 방법을 배웠습니다. ASP.NET Core Id에 대 한 자세한 내용은 참조 [Id 소개](xref:security/authentication/identity)합니다.