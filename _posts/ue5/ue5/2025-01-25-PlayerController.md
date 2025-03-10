---
layout: single

title: "[UE5] 플레이어 컨트롤러"

categories:
    - UE5
tag: [Unreal Engine, UE5]

date: 2025-01-25
last_modified_at: 2025-01-25

order : 200120
---

# 플레이어 컨트롤러

플레이어 컨트롤러(Player Controller)란 폰과 그것을 제어하는 플레이어 사이의 인터페이스입니다.

사용자가 키보드, 마우스, 게임패드 등에서 입력을 받으면, 그 입력을 해석하여 캐릭터나 다른 오브젝트에게 동작을 명령하는 핵심 클래스입니다.

플레이어 컨트롤러는 게임동안 유지되며, 일반적인 게임 로직에서 캐릭터나 폰은 휘발성이기 때문에 사라질 수 있습니다.  
예를 들어 데스매치 게임이라면 폰이나 캐릭터가 죽을 경우 안에 있던 변수는 사라지지만 플레이어 컨트롤러에 저장했다면 변수의 값은 유지됩니다.  
이렇게 값을 유지할 필요가 있는 경우 플레이어 컨트롤러에 저장하기도 합니다.

## 주요 기능

### 입력 처리

언리얼 엔진의 중요한 철학 중 하나는 플레이어 입력은 플레이어 컨트롤러에서 처리한다는 것입니다.  
이를 통해 입력 처리 로직과 실제 캐릭터의 동작 로직을 분리할 수 있어, 코드를 구조적으로 관리하기 훨씬 수월해집니다.

언리얼 엔진 5에서 제공하는 향상된 입력(Enhanced Input) 시스템을 사용하면, 액션/축 매핑을 보다 체계적으로 설정할 수 있습니다.

C++에서는 `SetupInputComponent()` 함수를 오버라이드하여, 블루프린트에서는 이벤트 그래프를 통해 입력 로직을 구현합니다.

멀티플레이 환경에서는 각 플레이어마다 개별적인 플레이어 컨트롤러가 생성되므로, 여러 사용자의 입력을 충돌 없이 분리하고 관리할 수 있습니다.

입력이 처리되는 기본적인 흐름은 다음과 같습니다.

1. 키보드, 마우스, 게임패드 등의 입력 장치로부터 사용자 조작 신호가 들어옵니다.
2. 이 신호를 플레이어 컨트롤러가 받아서 해석합니다.
3. 플레이어 컨트롤러가 현재 소유(Possess)하고 있는 폰에게 이동, 회전, 공격 등의 구체적인 명령을 내립니다.

### 카메라 제어

마우스나 게임패드의 축 입력을 받아 캐릭터의 시점 회전이나 줌 인/아웃 같은 카메라 동작을 수행할 수 있습니다.

### HUD 및 UI와의 상호작용

언리얼의 UMG (언리얼 모션 그래픽) 기반 UI를 통해 버튼 클릭, 드래그, 터치 등의 이벤트를 플레이어 컨트롤러에서 받을 수 있습니다.

예를 들어 인벤토리 열기, 스킬 사용 등의 명령을 UI에서 트리거하면 플레이어 컨트롤러가 이를 해석해 폰 또는 게임모드 등 다른 시스템으로 전달할 수 있습니다.

### 빙의

플레이어 컨트롤러는 특정 폰에 빙의(Possess)하여 해당 폰을 제어합니다.

필요할 때 `UnPossess()` 함수를 호출하여 폰과의 연결을 해제한 뒤, 다른 폰으로 바꿔 탈 수도 있습니다.

멀티플레이 시 각 플레이어마다 고유의 플레이어 컨트롤러가 있고, 이 컨트롤러가 특정 폰을 소유함으로써 서로 다른 캐릭터 조작이 가능합니다.

# 참고

[언리얼 엔진 컨트롤러](https://dev.epicgames.com/documentation/ko-kr/unreal-engine/controllers-in-unreal-engine?application_version=5.5){: target="_blank"}

[언리얼 엔진 플레이어 컨트롤러](https://dev.epicgames.com/documentation/ko-kr/unreal-engine/player-controllers-in-unreal-engine?application_version=5.5){: target="_blank"}