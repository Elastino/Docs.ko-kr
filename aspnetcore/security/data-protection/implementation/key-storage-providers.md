---
title: ASP.NET Core에서 키 저장소 공급자
author: rick-anderson
description: ASP.NET Core 및 키 저장소 위치를 구성 하는 방법에 대 한 키 저장소 공급자에 알아봅니다.
ms.author: riande
ms.date: 07/16/2018
uid: security/data-protection/implementation/key-storage-providers
ms.openlocfilehash: e712ff09b5306bc4481c4cc105448d7cbfa39f3a
ms.sourcegitcommit: d99a8554c91f626cf5e466911cf504dcbff0e02e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/31/2018
ms.locfileid: "39356768"
---
# <a name="key-storage-providers-in-aspnet-core"></a>ASP.NET Core에서 키 저장소 공급자

데이터 보호 시스템 [기본적으로 검색 메커니즘을 사용 하 여](xref:security/data-protection/configuration/default-settings) 에 암호화 키를 유지할지를 결정 합니다. 개발자는 기본 검색 메커니즘을 무시 하 고 수동으로 위치를 지정할 수 있습니다.

> [!WARNING]
> 명시적 키 지 속성 위치를 지정 하는 경우 데이터 보호 시스템 키는 더 이상 휴지 상태로 암호화 되므로 rest 메커니즘을 기본 키 암호화를 등록 취소 합니다. 하는 것이 좋습니다 있습니다 또한 [명시적 키 암호화 메커니즘을 지정](xref:security/data-protection/implementation/key-encryption-at-rest) 프로덕션 배포에 대 한 합니다.

## <a name="file-system"></a>파일 시스템

파일 시스템 기반 키 저장소를 구성 하려면 다음을 호출 합니다 [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) 아래와 같이 구성 루틴입니다. 제공 된 [DirectoryInfo](/dotnet/api/system.io.directoryinfo) 키를 저장할 저장소를 가리키는:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"c:\temp-keys\"));
}
```

`DirectoryInfo` 로컬 컴퓨터의 디렉터리를 가리킬 수 있습니다 또는 네트워크 공유에 폴더를 가리킬 수 있습니다. 로컬 컴퓨터의 디렉터리를 가리키는 경우 (및 로컬 컴퓨터의 앱만이 리포지토리를 사용 하 여 액세스 해야 하는 시나리오는) 사용 하는 것이 좋습니다 [Windows DPAPI](xref:security/data-protection/implementation/key-encryption-at-rest) 미사용 키 암호화 (on Windows). 그렇지 않은 경우 사용 하는 것이 좋습니다는 [X.509 인증서](xref:security/data-protection/implementation/key-encryption-at-rest) 미사용 키를 암호화 합니다.

## <a name="azure-and-redis"></a>Azure 및 Redis

합니다 [Microsoft.AspNetCore.DataProtection.AzureStorage](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.AzureStorage/) 하 고 [Microsoft.AspNetCore.DataProtection.Redis](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Redis/) 패키지를 사용 하면 Azure Storage 또는 Redis cache에서 데이터 보호 키를 저장 합니다. 웹 앱의 여러 인스턴스 키를 공유할 수 있습니다. 앱에는 여러 서버에서 인증 쿠키 또는 CSRF 보호 공유할 수 있습니다. Azure Blob 저장소 공급자를 구성 하려면 중 하나를 호출 합니다 [PersistKeysToAzureBlobStorage](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.persistkeystoazureblobstorage) 오버 로드 합니다.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToAzureBlobStorage(new Uri("<blob URI including SAS token>"));
}
```

Redis를 구성 하려면 중 하나를 호출 합니다 [PersistKeysToRedis](/dotnet/api/microsoft.aspnetcore.dataprotection.redisdataprotectionbuilderextensions.persistkeystoredis) 오버 로드 합니다.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    var redis = ConnectionMultiplexer.Connect("<URI>");
    services.AddDataProtection()
        .PersistKeysToRedis(redis, "DataProtection-Keys");
}
```

자세한 내용은 다음 항목을 참조하세요.

* [StackExchange.Redis ConnectionMultiplexer](https://github.com/StackExchange/StackExchange.Redis/blob/master/docs/Basics.md)
* [Azure Redis Cache](/azure/redis-cache/cache-dotnet-how-to-use-azure-redis-cache#connect-to-the-cache)
* [aspnet/DataProtection 샘플](https://github.com/aspnet/DataProtection/tree/master/samples)

## <a name="registry"></a>레지스트리

**Windows 배포에만 적용 됩니다.**

경우에 따라 앱 파일 시스템에 쓰기 액세스 하지 못할 수 있습니다. 가상 서비스 계정으로 앱 실행 되 고 있는 경우를 고려해 보십시오 (같은 *w3wp.exe*의 앱 풀 id)입니다. 이러한 경우 관리자 서비스 계정 id에서 액세스할 수 있는 레지스트리 키를 프로 비전 할 수 있습니다. 호출 된 [PersistKeysToRegistry](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystoregistry) 아래와 같이 확장 메서드. 제공 된 [RegistryKey](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.registryxmlrepository.registrykey) 암호화 키를 저장할 위치를 가리키는:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToRegistry(Registry.CurrentUser.OpenSubKey(@"SOFTWARE\Sample\keys"));
}
```

> [!IMPORTANT]
> 사용 하는 것이 좋습니다 [Windows DPAPI](xref:security/data-protection/implementation/key-encryption-at-rest) 미사용 키를 암호화 합니다.

## <a name="custom-key-repository"></a>사용자 지정 키 저장소

개발자가 사용자 지정을 제공 하 여 고유한 키 지 속성 메커니즘을 지정할 수 있습니다 기본 메커니즘은 적절 한가 없는 경우 [IXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.ixmlrepository)합니다.
