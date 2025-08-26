---
layout: single

title: "[UE5] AI 이동 명령 및 Nav Modifier Volume로 경로 바꾸기"

categories:
    - UEPractice
tag: [UE5, UEPractice]

date: 2025-04-23
last_modified_at: 2025-04-24

order : 200050
---

# AI 이동 명령

언리얼 엔진의 경로 탐색 알고리즘에 대한 세부적인 로직은 공개되지 않았습니다.  
하지만, A*(A-Star)나 다익스트라(Djikstra)알고리즘과 같이 특정 경로로 이동하기 위한 비용최적화 경로 탐색 공식을 바탕으로 경로를 탐색합니다.

AI의 이동 명령은 `AIController`에 내장된 함수인 `MoveToLocation`이나 `MoveToActor`함수로 이동시킬 수 있습니다.

```cpp
// 원형 함수
EPathFollowingRequestResult::Type MoveToLocation(
const FVector& Dest,          // 이동할 목적지 위치 벡터
float AcceptanceRadius = -1,  //  AI가 목적지에 도달했다고 판단할 거리 (값이 클수록 더 멀리서 도착 처리)
bool bStopOnOverlap = true,   // 목적지와 캐릭터의 충돌 영역이 겹치면 도착으로 간주할지 여부
bool bUsePathfinding = true,  // 경로 탐색 알고리즘을 사용할지 여부 (false면 직선으로 이동 시도)
bool bProjectDestinationToNavigation = true, // 목적지를 네비게이션 메시에 투영할지 여부
bool bCanStrafe = false,      // AI가 측면 이동을 할 수 있는지 여부
TSubclassOf<UNavigationQueryFilter> FilterClass = nullptr, // 네비게이션 쿼리에 사용할 필터 클래스
bool bAllowPartialPath = true // 완전한 경로를 찾지 못할 경우 부분 경로라도 허용할지 여부
);
```

`bProjectDestinationToNavigation`는 네비게이션 메시 밖에 있는 위치나 액터에게 이동할 경우, 일반적으로 이동할 수 없는데, 해당 값을 `true`로 설정할 경우 네비게이션 메시가 있는 최대 범위까지 이동할 것이냐에 대한 여부입니다.  
즉, 갈 수 있는 위치까지 가도록 합니다.

`bAllowPartialPath`는 네비게이션 메시 안에서 찾을 수 있는 길까지 이동할 것인지에 대한 여부입니다.

`MoveToActor`의 경우 목표 위치 대신에 목표하는 액터를 매개변수로 넘겨주면, 해당 액터의 위치로 찾아갑니다.  
이때, 대상 액터의 위치가 계속 변경되더라도 위치 변화를 일정 주기로 계속 추적합니다.  
만약 대상 액터가 너무 빠르게 이동하거나 순간 이동을 할 경우 경로가 끊어지며 다시 계산될 필요가 생깁니다.

`MoveToActor`는 `MoveToLocation`보다 연산 비용이 조금 더 발생합니다.

## Nav Modifier Volume로 경로 바꾸기

언리얼 엔진의 `Nav Modifier Volume`을 사용해 레벨 내에 영역에 이동 비용을 주변 대비 높게 설정해 경로를 바꾸어보겠습니다.

우선 Place Actors(액터 배치) `패널에서 Nav Modifier Volume`을 검색해서 배치하거나, 이미지처럼 `Nav Modifier Volume`을 찾아 배치할 수 있습니다.

![AI_Pathfinding-Nav_Modifier_Volume_Place]({{site.url}}/images/Unreal/UEPractice/2025-04-23-AI_Pathfinding/AI_Pathfinding-Nav_Modifier_Volume_Place.PNG)

`Nav Modifier Volume`의 `Details` 내에 `Area Class`라는 프로퍼티를 찾아 클래스를 `NavArea_Obstacle`로 설정해 해당 불륨의 영역은 이동 비용을 주변 대비 높게 설정해줍니다.  
올바른 클래스를 선택했다면, 이동 비용이 높아졌다는 의미에서 영역이 주황색으로 나타납니다.

![AI_Pathfinding-Nav_Modifier_Volume_Area_Class]({{site.url}}/images/Unreal/UEPractice/2025-04-23-AI_Pathfinding/AI_Pathfinding-Nav_Modifier_Volume_Area_Class.PNG)

저는 `MoveToLocation`함수를 사용하고, `Target Pointer`를 사용해 두 지점을 설정한 뒤 C++에서 해당 지점을 찾아 번갈아가며 이동할 수 있도록 구현했습니다.  
결과적으로 `Nav Modifier Volume`과 `Target Pointer`를 배치한 모습은 다음과 같습니다.

![AI_Pathfinding-Nav_Modifier_Volume_Target_Pointer]({{site.url}}/images/Unreal/UEPractice/2025-04-23-AI_Pathfinding/AI_Pathfinding-Nav_Modifier_Volume_Target_Pointer.PNG)

다음 유튜브 영상은 `Nav_Modifier_Volume`을 사용하기 전과 후를 비교한 영상입니다.

{% include video id="sKN822Cg2lg" provider="youtube" %}

영상을 보면 `Nav_Modifier_Volume`에 의해 특정 영역의 이동 비용이 올라가고, 해당 영역만 피해서 최단 경로로 이동하는 것을 볼 수 있습니다.

만약, 현재 상태에서 `Nav_Modifier_Volume`와 맵의 길이를 늘려 AI가 훨씬 더 먼 거리를 돌아가야한다면, 높은 비용을 뚫고 갈지 궁금해져 테스트해보았습니다.

![AI_Pathfinding-Nav_Modifier_Volume_Long]({{site.url}}/images/Unreal/UEPractice/2025-04-23-AI_Pathfinding/AI_Pathfinding-Nav_Modifier_Volume_Long.PNG)

결과적으로는, 거리가 멀어져도 불륨이 배치되어있는 위치는 지나가지 않은 상태에서의 최단 경로로 이동했습니다.  
이미지의 경로보다 훨씬 더 거리가 멀어지고 크게 돌아가야하는 경우를 테스트해보았는데, 똑같이 크게 돌아가는 최단 경로가 나왔습니다.

만약, 돌아갈 수 있는 위치가 없이 무조건 불륨을 지나가야하는 경우에는 기존의 최단 경로로 이동했습니다.