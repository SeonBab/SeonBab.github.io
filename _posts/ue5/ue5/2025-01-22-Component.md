---
layout: single

title: "[UE5] 컴포넌트"

categories:
    - UE5
tag: [Unreal Engine, UE5]

date: 2025-01-22
last_modified_at: 2026-01-14

order : 250
---

# 컴포넌트

컴포넌트(Component)란 언리얼 엔진에서 Actor가 어떤 역할을 하거나 특정 속성을 갖도록 만들어주는 부품의 개념입니다.  
하나의 액터에서 여러 종류의 컴포넌트를 조합해 다양한 기능을 구현 할 수 있습니다.

예를 들어 `StaticMeshComponent` + `AudioComponent` + `CollisionComponent`를 사용해, 충돌 시 소리가 나는 아이템을 만들 수 있습니다.

액터는 일반적으로 최상위 컴포넌트인 루트 컴포넌트(Root Component)를 가집니다.  
루트 컴포넌트는 액터의 트랜스폼을 정의하며, 모든 하위 컴포넌트가 이를 기준으로 동작합니다.

일반적으로 `Scene Component` 계열을 루트로 설정하여 액터의 트랜스폼을 관리하고, 이에 속하도록 다양한 컴포넌트를 계층적으로 붙입니다.

컴포넌트는 언리얼 오브젝트이기 때문에 C++로 작업할 때는 일반적으로 `UPROPERTY`를 사용해 관리하며, `TObjectPtr`로 포인터를 선언합니다.

CDO에서 생성한 컴포넌트는 액터가 스폰될 때 자동으로 월드에 생성과 등록이되며, `NewObject`로 생성한 컴포넌트는 `RegisterComponent`와 같은 함수로 반드시 등록 절차를 거쳐야 합니다.  
이렇게 등록된 컴포넌트는 월드의 기능을 사용할 수 있으며, 물리와 렌더링 처리에 합류됩니다.

## Scene Component

모든 트랜스폼 속성을 가지는 기본 컴포넌트입니다.

직접적인 시각적 출력(3D 모델, 빛 등)을 가지지 않지만, 다른 컴포넌트들의 계층적 트랜스폼을 정의하는 기준점 역할을 합니다.

Scene Component를 루트로 설정하면, 다른 시각적 컴포넌트를 아래에 부착(Attach)하여 관리할 수 있습니다.

## Static Mesh Component

애니메이션 없이 움직임이 없거나 단순 이동 및 회전만 하는 고정된(Static) 3D 모델을 표시하는 컴포넌트입니다.

건물, 바위, 아이템, 환경 오브젝트 등 움직임이 없거나 단순한 오브젝트에 주로 사용됩니다.

3D 모델을 표현하고, 물리 충돌과 관련된 기능도 제공합니다.

## Capsule Component

![Component-Capsule1]({{site.url}}/images/Unreal/ue5/2025-01-22-Component/Component-Capsule1.PNG)

일반적으로 캐릭터의 루트 컴포넌트로 캐릭터가 벽이나 지형에 충돌하는 범위를 정의하는 콜리전 컴포넌트로서 사용합니다.

캡슐 형태로, 반경/반지름(Radius)과 절반 높이(Half Height)를 조정해 물리적 크기를 설정할 수 있습니다.

![Component-Capsule2]({{site.url}}/images/Unreal/ue5/2025-01-22-Component/Component-Capsule2.PNG)

## Character Movement Component

이동, 회전, 점프, 중력, 지형 따라가기, 네트워크 동기화 등 보행형 캐릭터에게 필요한 기능이 이미 구현되어 있어, 사람이 달리고 점프하는 형태의 캐릭터를 쉽게 만들 수 있습니다.

여기에 미리 정의된 대표적인 함수들(`MoveForward`, `MoveRight`, `Jump`)이 존재하므로, 몇 줄의 코드만 추가해도 금방 캐릭터 움직임을 테스트할 수 있습니다.

## Skeletal Mesh Component

캐릭터의 3D모델과 애니메이션을 적용하는 컴포넌트입니다.

스켈레탈 메시와 애님 블루프린트 등을 여기로 할당해 캐릭터의 외형과 동작을 제어합니다.

### 스켈레탈 메시 설정하기

스켈레탈 메시(Skeletal Mesh)란 내부에 뼈대(Skeleton)을 가진 3D 모델을 의미합니다.  
팔, 다리, 머리 등 신체 부위별 본이 존재합니다.

이 뼈(본, Bone)가 부모와 자식 관계로 연결되어 있으며, 본이 움직이면 외형(Mesh)도 함께 움직이게 됩니다.

본과 메시가 연동되기 때문에, 애니메이션(Bone 움직임)에 맞춰 캐릭터가 뛰거나 걷는 동작을 구현할 수 있습니다.

언리얼 엔진은 물리 엔진과도 연결할 수 있어, Ragdoll(피격 후 쓰러지는) 효과 등 물리 기반 애니메이션 구현도 쉽게 가능합니다.

이런 스켈레탈 메시를 설정하기 위해서는 다음과 같습니다.

우선 스켈레탈 메시를 적용하려는 클래스의 스켈레탈 메시 컴포넌트를 선택합니다.  
그 후 디테일 패널의 스켈레탈 메시 항목에 스켈레탈 메시를 할당합니다.

![Component-SetSkeletalMesh1]({{site.url}}/images/Unreal/ue5/2025-01-22-Component/Component-SetSkeletalMesh1.PNG)

할당 후 다음과 같은 모습을 가집니다.

![Component-SetSkeletalMesh2]({{site.url}}/images/Unreal/ue5/2025-01-22-Component/Component-SetSkeletalMesh2.PNG)

언리얼에서 일반적으로 캐릭터의 전방 방향은 X축이지만 스켈레탈 메시의 전방 축이 다를 경우 캐릭터가 옆을 바라보는 것처럼 보입니다.  
이 경우 트랜스폼의 값을 수정해주어 전방(X축)을 바라보게 해주면 됩니다.  
다른 트랜스폼 또한 필요하다면 수정해야합니다.

![Component-SetSkeletalMesh3]({{site.url}}/images/Unreal/ue5/2025-01-22-Component/Component-SetSkeletalMesh3.PNG)

## Spring Arm Component

캐릭터와 카메라 간의 거리를 유지하고, 충돌 시 카메라가 벽 등에 박히지 않도록 위치를 자동 조정해줍니다.

## Camera Component

실제로 화면에 표시되는 카메라 컴포넌트입니다.

위치와 회전을 제어하면 게임 뷰가 변경됩니다.