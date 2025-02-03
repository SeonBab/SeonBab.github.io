---
layout: single

title: "[UE5] 애니메이션 블루프린트"

categories:
    - UE5
tag: [Unreal Engine, UE5]

date: 2025-02-03
last_modified_at: 2025-02-03

order : 100010
---

# 애니메이션 블루프린트

애니메이션 블루프린트(Animation Blueprint)는 언리얼 엔진에서 캐릭터의 스켈레톤 기반 애니메이션을 시각적으로 설계하는 데 특화된 전용 블루프린트입니다.

일반 블루프린트가 게임 로직(캐릭터의 이동, 공격, 상호작용 등)을 시각적으로 구현하는 것과 유사하게, 애니메이션 블루프린트는 캐릭터의 골격 움직임과 모션 전이(Transition) 등을 그래프나 State Machine을 통해 손쉽게 구현하도록 돕습니다.

보통은 스켈레탈 메시, 애니메이션 시퀀스, State Machine 등을 연결하는 “중간 다리” 역할을 하며, 애니메이션을 매끄럽게 전환시키는 다양한 로직을 담게 됩니다.

## 애니메이션 블루프린트 생성

컨텐츠 브라우저(Content Browser) > 우클릭(컨텍스트 메뉴) 혹은 추가(Add) > 애니메이션(Animation) > 애니메이션 블루프린트(Animation Blueprint)를 선택합니다.

![AnimationBlueprint-Create]({{site.url}}/images/Unreal/ue5/2025-02-03-AnimationBlueprint/AnimationBlueprint-Create.PNG)

이후 스켈레톤과 부모 클래스를 선택해야합니다.  
저는 `SK_Mannequin`스켈레톤과 `AnimInstance`를 부모 클래스로 선택하겠습니다.

![AnimationBlueprint-Choose_SK_ParentClass]({{site.url}}/images/Unreal/ue5/2025-02-03-AnimationBlueprint/AnimationBlueprint-Choose_SK_ParentClass.PNG)

`UAnimInstance`는 언리얼 엔진의 애니메이션 시스템에서 핵심 역할을 하는 클래스이며, 이 클래스를 통해 캐릭터 애니메이션 상태를 전환하거나 제어할 수 있습니다.

## 애니메이션 블루프린트 에디터

![AnimationBlueprint-Editor]({{site.url}}/images/Unreal/ue5/2025-02-03-AnimationBlueprint/AnimationBlueprint-Editor.PNG)

1. 툴바에는 애니메이션 블루프린트 관리 및 에디터 타입 전환을 위한 버튼이 있습니다.
2. 뷰포트(Viewport)에서는 캐릭터의 애니메이션 블루프린트 로직의 행동을 프리뷰할 수 있습니다.
3. 내 블루프린트(My Blueprint)는 블루프린트 에디터에서도 찾아볼 수 있으며 그래프, 함수, 변수 및 기타 애니메이션 블루프린트 내의 관련된 프로퍼티 목록을 포함합니다.
4. 애니메이션 블루프린트에는 크게 Anim Graph와 Event Graph 두 가지 그래프가 있습니다.
    + 애니메이션 그래프(Anim Graph)는 입력 노드 → 블렌딩(Blend) → 출력 노드(Output Pose) 순으로, 최종적으로 캐릭터가 어떤 포즈와 애니메이션을 취할지 결정합니다.
    + 이벤트 그래프(Event Graph)는 일반 블루프린트와 유사한 이벤트 그래프로서 Tick이나 BlueprintImplementableEvent 등을 활용해 C++ 혹은 게임 로직과 애니메이션을 연동할 수 있습니다.
5. 디테일(Details) 에는 선택된 항목의 프로퍼티가 표시됩니다.
6. 애님 프리뷰 에디터(Anim Preview Editor)에서는 변수 및 클래스 디폴트를 변경할 수 있습니다. 별도의 탭으로 도킹되어 있는 에셋 브라우저(Asset Browser) 에서 프로젝트에 들어있는 이 스켈레톤과 연관된 애니메이션 에셋을 보고 열 수 있습니다. 

## 애니메이션 연결

에셋 브라우저에서 프로젝트에 들어있는 애니메이션 시퀀스 중에서 재생시키고 싶은 애셋을 하나 선택하고, 그래프로 드래그해 추가해보겠습니다.

![AnimationBlueprint-Choose_Asset]({{site.url}}/images/Unreal/ue5/2025-02-03-AnimationBlueprint/AnimationBlueprint-Choose_Asset.PNG)

애니메이션 시퀀스를 아웃풋 포즈에 연결합니다.  
반복 재생을 하고싶으므로 디테일 패널에서 애니메이션 루프(Loop Animation)를 활성화합니다.

![AnimationBlueprint-AssetDetails]({{site.url}}/images/Unreal/ue5/2025-02-03-AnimationBlueprint/AnimationBlueprint-AssetDetails.PNG)

이렇게 연결하면 캐릭터가 연결된 애니메이션을 출력하게 됩니다.

## 애니메이션 블루프린트 적용

캐릭터 블루프린트를 열고, 애니메이션을 재생하고싶은 스켈레탈 메시를 선택합니다.  
그 후 애니메이션 모드(AnimationMode)를 Use Animation Blueprint로 설정하고, 애님 클래스(Anim Class)에 만든 애니메이션 블루프린트를 할당합니다.

![AnimationBlueprint-Apply]({{site.url}}/images/Unreal/ue5/2025-02-03-AnimationBlueprint/AnimationBlueprint-Apply.PNG)

플레이 시켜보면 캐릭터가 Idle 애니메이션을 계속 수행하는 것을 확인할 수 있습니다.

![AnimationBlueprint-Idle]({{site.url}}/images/Unreal/ue5/2025-02-03-AnimationBlueprint/AnimationBlueprint-Idle.gif)

# 참고

[애니메이션 블루프린트 에디터](https://dev.epicgames.com/documentation/ko-kr/unreal-engine/animation-blueprint-editor-in-unreal-engine)