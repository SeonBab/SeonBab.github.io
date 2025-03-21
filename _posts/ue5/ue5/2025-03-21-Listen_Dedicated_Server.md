---
layout: single

title: "[UE5] Listen/Dedicated Server"

categories:
    - UE5
tag: [Unreal Engine, UE5]

date: 2025-03-21
last_modified_at: 2025-03-21

order : 500010
---

# Server

게임에서 Server(서버)란 사용자의 데이터를 저장하는 기능 외에 게임의 심판 역할을 하는 로직 전체를 이야기합니다.

언리얼 엔진은 멀티플레이어 게임에서 `Server-Client` 구조를 사용합니다.  
`Server`로 정의된 인스턴스가 게임의 상태에 대한 `Authority`를 가지고, 다른 모든 인스턴스는 `Server`로부터 업데이트를 받는 `Client`입니다.

클라이언트끼리는 직접 통신하지 않고, 취하는 모든 행동(캐릭터 이동, 총기 발사 등)은 서버로 전송되고, 서버가 검증한 후 결과를 다시 클라이언트들에게 `Broadcast`합니다.

이런 모델을 사용하는 핵심 이유는 일관성을 유지하고 Cheat를 방지하기 위함입니다. (Never Trust the Client)

이러한 역할 분담을 통해 `Server`가 `Gameplay`에 관련된 결정에 독점적인 권한을 갖고, `Client`는 그에 따른 결과를 보여주며 입력을 `Server`에 보내는 구조를 명확히 합니다.

예로 들어, 클라이언트에서 캐릭터를 이동시킬 경우 다음과 같습니다.

1. 입력 즉시 캐릭터의 움직임을 예측 후 반영하여 실시간성을 확보합니다.
2. 이동 요청을 `Server`로 보냅니다.
3. `Server`는 요청을 받아 권한 있는 위치에서 캐릭터를 움직입니다.
4. 그 새로운 위치/상태를 모든 `Client`에게 `Broadcast` 합니다. (자신 포함)
5. Client의 자체적인 예측이 정확했다면, 수정할 것이 없으니 부드럽게 플레이됩니다. 
6. 만약 차이가 있었다면, `Server`의 업데이트로 캐릭터의 위치를 조정합니다.

이 과정은 보정(Reconciliation)의 일부입니다.

언리얼 엔진에서 `Server-Client`와 관련해서 지원하는 실행 모드는 다음과 같습니다.

+ Standalone: 네트워킹을 고려하지 않는 단일 인스턴스
    - 싱글 플레이어(Local 세션)
    - 어떤 것도 다른 곳에 Replicate 하지 않음
    - 모든 Gameplay 클래스가 하나의 프로세스에서 실행
+ Dedicated Server: Server 전용 인스턴스
    - Local 플레이어가 없음
    - 렌더링 하지 않고, GPU가 없이 구동 가능
    - 게임 로직과 네트워크 통신만 처리
    - 원격 머신이나 클라우드에 배포
+ Listen Server: 한 플레이어가 Server 호스트 역할
    - Server이자 Client 역할을 수행
    - 호스트에게 UI와 PlayerController 존재
    - 호스트가 게임을 종료시 Server 또한 종료
    - P2P 스타일 게임에 많이 사용
+ Client: 호스트가 아닌 모든 플레이어
    - Server로부터 데이터를 받음
    - 자신의 입력을 Server에 전송
    - 자율적(autonomous) 플레이어 컨트롤러 보유
    - 권한 있는 결정과 게임 상태 업데이트는 Server에 의존

언리얼에서 기본적으로 제공하는 네트워크 멀티플레이를 구현하는 방법에는 `Listen Server`와 `Dedicated Server`가 있습니다.  
이외에도, 언리얼에서 제공하는 방법 외에 `소켓 통신`, `Restful API` 방식 등이 있습니다.

## Dedicated Server

`Dedicated Server`(데디케이티드 서버)는 게임을 플레이하지 않는 전용 서버로, 클라이언트들이 접속하여 게임을 진행하는 방식입니다.

서버는 게임 화면이 없으며, 순수하게 네트워크 및 게임 로직만 처리합니다.

일반적으로 강력한 성능을 가진 별도의 서버 머신에서 실행됩니다.

클라이언트와는 별도로 실행되므로, 호스트 플레이어가 나가도 게임이 계속 유지됩니다.

+ 장점
    - 네트워크 안정성이 높아, 많은 플레이어가 접속해도 성능이 비교적 안정적
    - 클라이언트와 분리되어 있어, 호스트가 종료되더라도 게임이 지속
    - 치트 방어에 유리하며, 공정한 게임 환경을 유지 가능

+ 단점
    - 서버를 별도로 운영해야 하므로 비용이 발생
    - 서버를 호스팅하기 위한 추가적인 설정과 유지 관리가 필요

![Listen_Dedicated_Server-Dedicated_GameFrameWork]({{site.url}}/images/Unreal/ue5/2025-03-21-Listen_Dedicated_Server/Listen_Dedicated_Server-Dedicated_GameFrameWork.PNG)  
<cite class="small">
  <a href="https://www.reddit.com/r/UnrealEngine5/comments/1aiisow/i_tried_making_an_infographic_to_help_my/?utm_source=share&utm_medium=web3x&utm_name=web3xcss&utm_term=1&utm_content=share_button" target="_blank">
    Image from BadenNorthey
  </a>
</cite>

`GameMode`는 서버에 존재하며, 게임 규칙을 제어합니다.

`GameState`는 서버와 모든 클라이언트에 존재하며, 게임의 전체적인 정보(점수, 시간 등)을 동기화 합니다.

`PlayerController`는 서버와 자신의 클라이언트에 존재하며, 자신의 입력을 자신의 클라이언트와 서버에서 알 수 있도록 합니다.

`Pawn`은 서버와 모든 클라이언트에 존재하며, 자신의 `Pawn`을 모든 플레이어가 볼 수 있도록 서버에 `Replicate`합니다.

## Listen Server

`Listen Server`(리슨 서버)는 서버이자 클라이언트를 수행합니다.  
즉, 호스트 플레이어가 게임을 실행하면서 동시에 서버 역할을 수행하며, 서버와 클라이언트가 같은 프로세스에서 실행됩니다.

다른 플레이어들은 호스트가 실행한 게임에 접속하여 멀티플레이를 진행할 수 있습니다.

호스트 플레이어가 나가면 서버가 종료됩니다.

소규모 협동게임이나 테스트 환경에서 빠르게 멀티플레이를 개발 할 때 사용하기 좋습니다.

+ 장점
    - 별도의 서버 비용이 들지 않으며, 쉽게 설정할 수 있음
    - 개발 중 빠르게 멀티플레이를 테스트하기 용이
    - 개인 게임 서버를 쉽게 운영할 수 있음

+ 단점
    - 호스트의 네트워크 상태가 게임 전체에 영향을 미침 (핑이 높거나, 호스트가 나가면 게임이 종료됨)
    - 서버와 클라이언트가 같은 프로세스에서 실행되므로, 서버 역할을 하는 플레이어에게 높은 성능이 요구됨
    - 클라이언트가 서버 역할을 하므로, 보안이 취약할 수 있음 (치트 방지 어려움)