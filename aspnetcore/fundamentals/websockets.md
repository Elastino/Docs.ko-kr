---
title: ASP.NET Core에서 WebSocket 지원
author: rick-anderson
description: ASP.NET Core에서 Websocket을 시작하는 방법을 알아봅니다.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/28/2018
uid: fundamentals/websockets
ms.openlocfilehash: a9fe13ef7895ea3ab43257dbbaf4521f883c0804
ms.sourcegitcommit: 18339e3cb5a891a3ca36d8146fa83cf91c32e707
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37433989"
---
# <a name="websockets-support-in-aspnet-core"></a>ASP.NET Core에서 WebSocket 지원

작성자: [Tom Dykstra](https://github.com/tdykstra) 및 [Andrew Stanton-Nurse](https://github.com/anurse)

본문에서는 ASP.NET Core에서 Websocket을 사용하는 방법을 알아봅니다. [WebSocket](https://wikipedia.org/wiki/WebSocket)([RFC 6455](https://tools.ietf.org/html/rfc6455))은 TCP 연결을 통해 지속적인 양방향 통신 채널을 사용할 수 있도록 해주는 프로토콜입니다. 채팅, 대시보드 및 게임 앱 등 신속한 실시간 통신을 활용하는 앱에서 사용됩니다.

[예제 코드 살펴보기 및 다운로드](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) ([다운로드 방법](xref:tutorials/index#how-to-download-a-sample)). 자세한 내용은 [다음 단계](#next-steps) 섹션을 참조하세요.

## <a name="prerequisites"></a>전제 조건

* ASP.NET Core 1.1 이상
* ASP.NET Core를 지원하는 모든 OS:
  
  * Windows 7/Windows Server 2008 이상
  * Linux
  * macOS
  
* IIS가 있는 Windows에서 앱을 실행하는 경우:

  * Windows 8 / Windows Server 2012 이상
  * IIS 8 / IIS 8 Express
  * WebSockets는 IIS에서 활성화되어야 합니다([IIS/IIS Express 지원](#iisiis-express-support) 섹션 참조).
  
* 앱이 [HTTP.sys](xref:fundamentals/servers/httpsys)에서 실행되는 경우:

  * Windows 8 / Windows Server 2012 이상

* 지원되는 브라우저는 https://caniuse.com/#feat=websockets를 참조하세요.

## <a name="when-to-use-websockets"></a>WebSockets를 사용하는 경우

WebSockets를 사용하여 소켓 연결로 직접 작업하세요. 예를 들어, 실시간 게임에서 가능한 최상의 성능을 얻으려면 WebSockets를 사용하세요.

[ASP.NET Core SignalR](xref:signalr/introduction)은 앱에 실시간 웹 기능을 추가하는 것을 간소화하는 라이브러리입니다. 가능하면 Websocket을 사용합니다.

## <a name="how-to-use-websockets"></a>WebSocket 사용 방법

* [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/) 패키지를 설치합니다.
* 미들웨어를 구성합니다.
* WebSocket 요청을 수락합니다.
* 메시지를 전송 및 수신합니다.

### <a name="configure-the-middleware"></a>미들웨어 구성하기

`Startup` 클래스의 `Configure` 메서드에 WebSockets 미들웨어를 추가합니다.

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSockets)]

이때 다음과 같은 설정을 구성할 수 있습니다.

* `KeepAliveInterval` - 프록시가 연결을 유지할 수 있도록 클라이언트로 "핑(ping)" 프레임을 전송하는 빈도입니다.
* `ReceiveBufferSize` - 데이터 수신에 사용되는 버퍼의 크기입니다. 고급 사용자는 데이터 크기에 따라 성능 튜닝이 필요할 경우 이 값을 변경해야 할 수도 있습니다.

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSocketsOptions)]

### <a name="accept-websocket-requests"></a>WebSocket 요청 수락하기

요청 수명 주기의 뒷부분에서 (예를 들어, `Configure` 메서드나 MVC 액션의 뒷부분에서) WebSocket 요청 여부를 확인하고 WebSocket 요청을 수락합니다.

다음 예제는 `Configure` 메서드의 뒷부분에서 나옵니다.

[!code-csharp[](websockets/sample/Startup.cs?name=AcceptWebSocket&highlight=7)]

WebSocket 요청은 모든 URL을 통해서 전달될 수 있지만, 이 예제 코드에서는 `/ws` 에 대한 요청만 수락합니다.

### <a name="send-and-receive-messages"></a>메시지 보내기 및 받기

`AcceptWebSocketAsync` 메서드는 TCP 연결을 WebSocket 연결로 업그레이드하고 [WebSocket](/dotnet/core/api/system.net.websockets.websocket) 개체를 제공합니다. `WebSocket` 개체를 사용하여 메시지를 보내고 받습니다.

앞에서 살펴본 WebSocket 요청을 수락하는 코드는 `WebSocket` 개체를 `Echo` 메서드로 전달합니다. 코드는 메시지를 수신하고 동일한 메시지를 즉시 보냅니다. 메시지는 클라이언트가 연결을 닫을 때까지 루프에서 보내고 받습니다.

[!code-csharp[](websockets/sample/Startup.cs?name=Echo)]

루프가 시작되기 전에 WebSocket 연결을 수락하면 미들웨어 파이프라인이 종료됩니다. 그리고 소켓을 닫으면 파이프라인이 풀립니다. 즉, 요청은 WebSocket이 수락될 때 파이프라인에서 앞으로 이동하지 않습니다. 루프가 완료되고 소켓이 닫히면 요청이 파이프라인 백업을 진행합니다.

## <a name="iisiis-express-support"></a>IIS/IIS Express 지원

IIS/IIS Express 8 이상이 있는 Windows Server 2012 이상 및 Windows 8 이상에서는 WebSocket 프로토콜을 지원합니다.

Windows Server 2012 이상에서 WebSocket 프로토콜을 지원하려면:

1. **관리** 메뉴 또는 **서버 관리자**의 링크를 통해 **역할 및 기능 추가** 마법사를 사용합니다.
1. **역할 기반 또는 기능 기반 설치**를 선택합니다. **새로 만들기**를 선택합니다.
1. 적절한 서버를 선택합니다(로컬 서버가 기본적으로 선택됨). **새로 만들기**를 선택합니다.
1. **역할** 트리에서 **Web Server(IIS)** 를 확장하고 **Web Server**를 확장한 다음, **응용 프로그램 개발**을 확장합니다.
1. **WebSocket 프로토콜**을 선택합니다. **새로 만들기**를 선택합니다.
1. 추가 기능이 필요 없는 경우 **다음**을 선택합니다.
1. **설치**를 선택합니다.
1. 설치가 완료되면 **닫기**를 선택하여 마법사를 종료합니다.

Windows 8 이상에서 WebSocket 프로토콜을 지원하려면:

1. **제어판** > **프로그램** > **프로그램 및 기능** > **Windows 기능 Windows 기능 사용/사용 안 함**(화면 왼쪽)으로 차례로 이동합니다.
1. **인터넷 정보 서비스** > **World Wide Web 서비스** > **응용 프로그램 개발 기능** 노드를 엽니다.
1. **WebSocket 프로토콜** 기능을 선택합니다. **확인**을 선택합니다.

**node.js에서 socket.io를 사용할 때 WebSocket 비활성화**

[Node.js](https://nodejs.org/)의 [socket.io](https://socket.io/)에서 WebSocket 지원을 사용하는 경우 *web.config* 또는 *applicationHost.config*의 `webSocket` 요소를 사용하여 기본 IIS WebSocket 모듈을 비활성화합니다. 이 단계를 수행하지 않으면 IIS WebSocket 모듈이 Node.js 및 앱이 아닌 WebSocket 통신을 처리하려고 시도합니다.

```xml
<system.webServer>
  <webSocket enabled="false" />
</system.webServer>
```

## <a name="next-steps"></a>다음 단계

이 아티클과 함께 제공되는 [샘플 앱](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample)은 에코 앱입니다. WebSocket 연결을 생성하는 웹 페이지가 제공되며 서버는 수신한 메시지를 클라이언트로 재전송합니다. 명령 프롬프트에서 앱을 실행하고(IIS Express가 있는 Visual Studio에서 실행되도록 설정되지 않음) http://localhost:5000으로 이동합니다. 웹 페이지 왼쪽 상단에 연결 상태가 표시됩니다.

![웹 페이지의 초기 상태](websockets/_static/start.png)

**Connect** 를 선택해서 지정한 URL로 WebSocket 요청을 전송합니다. 그리고 테스트 메시지를 입력한 다음 **Send** 를 선택합니다. 테스트를 모두 마쳤으면 **Close Socket** 을 선택하십시오. **Communication Log** 섹션에는 각각의 열기, 전송 및 닫기 동작이 발생한 순서대로 나타납니다.

![웹 페이지의 초기 상태](websockets/_static/end.png)
