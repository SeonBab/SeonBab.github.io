---
layout: single

title: "[UE5] Nav Mesh Bounds Volume 동적 변화 및 Navigation Invoker"

categories:
    - UE5Dev
tag: [UE5, UE5Dev]

date: 2025-04-23
last_modified_at: 2025-04-23

order : 200000
---

# Nav Mesh Bounds Volume

NavMeshBoundsVolume은 내비게이션 메시 시스템을 사용하는 모든 오브젝트의 이동 영역을 정의하는 장치입니다.  
내비게이션 메시 또는 네비 메시라고도 부릅니다.

흔히 AI가 갈 수 있는 곳을 정해주는 기능으로 사용하며, 해석하기도 합니다.

`Nav Mesh Bounds Volume`은 `Scale`이 아니라 `Brush` 크기를 활용해 레벨 내 적용되는 지오메트리를 결정합니다.  
그런데 언리얼 엔진의 아키텍쳐 특성상 `Brush`는 런타임 중 동적 재생성이 불가능합니다.  
즉, 런타임 중 불륨을 새로 생성하여 레벨 내 네비메시 적용 범위를 늘리거나 줄이는 방식의 구현이 어렵습니다.

## Nav Mesh Bounds Volume 생성

Place Actors(액터 배치) 패널에서 NavMeshBoundsVolume을 검색해서 배치하거나, 이미지처럼 NavMeshBoundsVolume을 찾아 배치할 수 있습니다.

![Nav_Mesh_Bounds_Volume-Position]({{site.url}}/images/Unreal/UE5Dev/2025-04-23-Nav_Mesh_Bounds_Volume/Nav_Mesh_Bounds_Volume-Position.PNG)

맵 전체를 덮도록 스케일을 수정했습니다.

![Nav_Mesh_Bounds_Volume-Scale]({{site.url}}/images/Unreal/UE5Dev/2025-04-23-Nav_Mesh_Bounds_Volume/Nav_Mesh_Bounds_Volume-Scale.PNG)

네비 메시를 배치하고, 키보드의 `P`키를 누르면 내비게이션 데이터를 시각화해서 볼 수 있습니다.

![Nav_Mesh_Bounds_Volume-P]({{site.url}}/images/Unreal/UE5Dev/2025-04-23-Nav_Mesh_Bounds_Volume/Nav_Mesh_Bounds_Volume-P.PNG)

## Run Time Generation 변경

기본적으로 네비 메시는 한 번 세팅되면 그대로만 적용되는 정적 네비 메시입니다.  
이것을 동적으로 런타임에 변경되게 하고자 한다면 프로젝트 세팅에서 설정 값을 바꿔주어야 합니다.

![Nav_Mesh_Bounds_Volume-ProjectSetting_Dynamic]({{site.url}}/images/Unreal/UE5Dev/2025-04-23-Nav_Mesh_Bounds_Volume/Nav_Mesh_Bounds_Volume-ProjectSetting_Dynamic.PNG)

+ `static`
    - 내비게이션 메시가 오프라인(레벨 디자인 시)에 생성되어 레벨과 함께 저장된다.
    - 런타임에는 단순히 로드되며, 메시가 변경되지 않는다.
+ `Dynamic`
    - 내비게이션 메시가 오프라인에서 생성되어 저장되거나, 런타임에 새로 생성될 수 있다.
    - 런타임 중 내비게이션 관련 데이터가 변경되면, 변경된 타일에서 메시가 재생성되어 업데이트된다.
+ `Dynamic Modifiers Only`
    - 내비게이션 메시는 오프라인에서 생성되어 레벨과 함께 저장된다.
    - 런타임에서는 내비게이션 영역, 링크, 동적 오브젝트 등과 같이 메시의 일부만 수정하여 업데이트한다.
    - 새로운 메시 표면은 생성되지 않으며, 콜리전 데이터 캐시를 통해 타일 처리 비용을 최대 50% 절감한다.

설정이 `static`인 경우 다음과 같이 오브젝트가 움직여도 네비게이션 메시의 데이터가 변하지 않는 것을 알 수 있습니다.

![Nav_Mesh_Bounds_Volume-Static]({{site.url}}/images/Unreal/UE5Dev/2025-04-23-Nav_Mesh_Bounds_Volume/Nav_Mesh_Bounds_Volume-Static.gif)

설정이 `Dynamic`인 경우 다음과 같이 오브젝트가 움직임에 따라 네비게이션 메시의 데이터가 런타임 중에 변하는 것을 알 수 있습니다.

![Nav_Mesh_Bounds_Volume-Dynamic]({{site.url}}/images/Unreal/UE5Dev/2025-04-23-Nav_Mesh_Bounds_Volume/Nav_Mesh_Bounds_Volume-Dynamic.gif)

설정 `Dynamic Modifiers Only`인 경우 이름 그대로 `Modifiers`만을 체크해줍니다.  
움직이는 액터에 `Nav Modifier` 컴포넌트를 추가해주었고, 디테일의 프로퍼티 중 `Area Class`를 `NavArea_Null`로 지정해주었습니다.  
그 결과 네비게이션 메시의 데이터가 런타임 중에 변하기는 하지만, 기존 데이터에서 일부 영역이 제거만 되고, 움직이는 하얀색 액터 위에는 생성되지 않는다는 것을 알 수 있습니다.

![Nav_Mesh_Bounds_Volume-Dynamic_Modifiers_Only]({{site.url}}/images/Unreal/UE5Dev/2025-04-23-Nav_Mesh_Bounds_Volume/Nav_Mesh_Bounds_Volume-Dynamic_Modifiers_Only.gif)

결과적으로, `Static`은 레벨 내 동적변화에 대응이 불가능하지만 성능에 가장 좋으며, `Dynamic`은 런타임에서의 변화에 대응이 가능하지만 상대적으로 최적화에 좋지 않으며, `Dynamic Modifiers Only`은 런타임에서 영역 추가를 제외한 부분 수정만 가능합니다.

## Navigation Invoker

AI를 맵 전체에 풀어놓는데, 네비메시를 레벨 전체에 적용한다면 비효율적일 수 있습니다.  
이때, `Navigation Invoker`를 사용하면 좋습니다.

`Navigation Invoker`는 이 컴포넌트를 가진 액터의 주변 영역만 연산해서 리소스를 아끼면서도, 게임 플레이에는 문제가 없도록 해주는 기능입니다.

C++기준 사용 방법은 다음과 같습니다.

```cpp
// 헤더
#include "NavigationInvokerComponent.h"

// Invoker 세팅
UPROPERTY(BlueprintReadWrite, Category = "Navigation", meta = (AllowPrivateAccess = "true"))
TObjectPtr<UNavigationInvokerComponent> NavInvoker;

/** 네비게이션 메시 생성 반경 **/
float NavGenerationRadius;

/** 네비게이션 메시 제거 반경 **/
float NavRemovalRadius;

/** Returns NavInvoker subobject **/
FORCEINLINE class UNavigationInvokerComponent* GetNavInvoker() const { return NavInvoker; }

// 소스
// 활성화될 범위 설정
// 테스트를 위해 매우 작은 값 부여
NavGenerationRadius = 10.0f;
NavRemovalRadius = 15.0f;

// Navigation Invoker 컴포넌트 생성 및 초기값 셋업
NavInvoker = CreateDefaultSubobject<UNavigationInvokerComponent>(TEXT("NavInvoker"));
// SetGenerationRadii 함수를 사용하여 생성 반경과 제거 반경 설정
// Protected 멤버변수이므로 함수를 통해 수정 필요
NavInvoker->SetGenerationRadii(NavGenerationRadius, NavRemovalRadius); 
```

이외에 프로젝트의 `프로젝트.Build.cs`의 모듈에 `"NavigationSystem"`을 추가해주어야 합니다.

마지막으로 프로젝트 세팅의 다음 작업이 이루어져야 합니다.  
`Navigation Mesh`에 `Runtime Genertaion` 설정을 `Dynamic`으로 수정해야합니다.  
`Navigation System`의 `Generate Navigation Only Around Navigation Invokers` 설정을 `True`로 수정해야합니다.

![Nav_Mesh_Bounds_Volume-Invokers]({{site.url}}/images/Unreal/UE5Dev/2025-04-23-Nav_Mesh_Bounds_Volume/Nav_Mesh_Bounds_Volume-Invokers.PNG)

레벨에 컴포넌트가 적용된 액터를 테스트한다면 다음 사진처럼 위치에 따라 액터의 주변에만 네비메시가 활성화 되는 것을 알 수 있습니다.

![Nav_Mesh_Bounds_Volume-Invokers_Location_1]({{site.url}}/images/Unreal/UE5Dev/2025-04-23-Nav_Mesh_Bounds_Volume/Nav_Mesh_Bounds_Volume-Invokers_Location_1.PNG)

![Nav_Mesh_Bounds_Volume-Invokers_Location_2]({{site.url}}/images/Unreal/UE5Dev/2025-04-23-Nav_Mesh_Bounds_Volume/Nav_Mesh_Bounds_Volume-Invokers_Location_2.PNG)