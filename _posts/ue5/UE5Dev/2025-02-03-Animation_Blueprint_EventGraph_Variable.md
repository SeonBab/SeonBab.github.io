---
layout: single

title: "[UE5] 애니메이션 블루프린트에서 필요한 변수 세팅하기"

categories:
    - UE5Dev
tag: [UE5, UE5Dev]

date: 2025-02-03
last_modified_at: 2025-02-03

order : 101000
---

# 애니메이션 블루프린트에서 필요한 변수 세팅하기

애니메이션의 동적인 변화를 제어하고, 캐릭터의 움직임을 게임의 상황과 동기화하여 자연스럽게 표현하기 위한 필수 과정입니다.

대표적으로 블렌드 스페이스나 상태 머신 등에 사용됩니다.

## 캐릭터 Movement Component 변수화

애니메이션 블루프린트와 캐릭터를 연동해, 캐릭터의 이동 상태에 따라 다른 애니메이션을 재생하기 위해서는 이 `Movement Component`의 정보가 필수적입니다.

`Movement Component`는 캐릭터의 이동 속도, 가속도, 점프, 낙하 여부 등 다양한 이동 관련 정보를 제공합니다.

애니메이션 블루프린트의 `Event Graph`에서 두 개의 변수를 만듭니다.

![Animation_Blueprint_EventGraph_Variable-Character_Variable]({{site.url}}/images/Unreal/UE5Dev/2025-02-03-Animation_Blueprint_EventGraph_Variable/Animation_Blueprint_EventGraph_Variable-Character_Variable.PNG)

1. `Character` : 타입 - Character Object Reference
2. `CharacterMovement` : 타입 Character Movement Component Object Reference

이 두 변수로 캐릭터와 캐릭터의 무브먼트 컴포넌트를 쉽게 참조합니다.

![Animation_Blueprint_EventGraph_Variable-Character_Variable_Init]({{site.url}}/images/Unreal/UE5Dev/2025-02-03-Animation_Blueprint_EventGraph_Variable/Animation_Blueprint_EventGraph_Variable-Character_Variable_Init.PNG)

`Blueprint Initialize Animation` 이벤트 노드는 애니메이션 블루프린트가 런타임 중에 처음 초기화될 때 실행됩니다.  
이 노드를 사용하면 애니메이션 블루프린트 시작 시 한 번 활성화되는 로직을 빌드할 수 있습니다.

지금 노드에서는 `Get Owning Actor`노드로 현재 애니메이션을 재생 중인 액터(캐릭터)를 가져옵니다.  
캐스팅을 통해 우리가 사용 중인 캐릭터가 맞는지 확인하고, 성공하면 해당 객체를 `Character`변수에 저장합니다.  
`Character`에서 `Get Character Movement` 노드를 사용해 `Movement Component`를 얻어, 이를 `CharacterMovement` 변수에 저장합니다.

## 매 프레임 캐릭터의 수평 속도

`Event BlueprintUpdateAnimation`이벤트 노드는 프레임마다 호출되어 애니메이션 로직을 업데이트하는 핵심 이벤트입니다.  
해당 이벤트에서 수평 속도를 가져오겠습니다.

먼저 `Character`가 유효한지(Null이 아닌지) `Is Valid?` 노드 등을 통해 확인합니다.  
캐릭터가 제대로 설정되지 않았다면, 이후 로직을 수행하지 않도록 합니다.

![Animation_Blueprint_EventGraph_Variable-Convert_to_Validated_Get]({{site.url}}/images/Unreal/UE5Dev/2025-02-03-Animation_Blueprint_EventGraph_Variable/Animation_Blueprint_EventGraph_Variable-Convert_to_Validated_Get.PNG)

![Animation_Blueprint_EventGraph_Variable-Valid_Check]({{site.url}}/images/Unreal/UE5Dev/2025-02-03-Animation_Blueprint_EventGraph_Variable/Animation_Blueprint_EventGraph_Variable-Valid_Check.PNG)

`CharacterMovement`의 `Get Velocity` 노드로 캐릭터의 현재 속도 벡터(X, Y, Z)를 가져옵니다.  
`Vector Length XY` 노드를 사용해 X, Y 성분의 길이(즉, 2D 평면상의 속도)만 구합니다.  
이렇게 하면 캐릭터가 수평으로 어느 정도 빠르게 움직이고 있는지 알 수 있게 됩니다.

![Animation_Blueprint_EventGraph_Variable-GoundSpeed_Init]({{site.url}}/images/Unreal/UE5Dev/2025-02-03-Animation_Blueprint_EventGraph_Variable/Animation_Blueprint_EventGraph_Variable-GoundSpeed_Init.PNG)

값을 `Ground Speed`라는 `float` 변수에 저장합니다.  
전체 속도 벡터는 `Velocity`라는 `Vector` 변수에 저장해둡니다.  
점프 중인 경우 Z축 속도를 확인해야 하는 상황 등이 있을 수 있기 때문입니다.

![Animation_Blueprint_EventGraph_Variable-GoundSpeed_Variable]({{site.url}}/images/Unreal/UE5Dev/2025-02-03-Animation_Blueprint_EventGraph_Variable/Animation_Blueprint_EventGraph_Variable-GoundSpeed_Variable.PNG)

## 매 프레임 캐릭터의 움직임 여부

캐릭터가 정지 상태(Idle)인지 이동 중(Walk/Run)인지를 구분하는 기본적인 로직을 구현해보겠습니다.

매 프레임마다 확인하므로 `Event BlueprintUpdateAnimation`이벤트 노드에서 값을 갱신해줍니다.

![Animation_Blueprint_EventGraph_Variable-bShouldMove_Init]({{site.url}}/images/Unreal/UE5Dev/2025-02-03-Animation_Blueprint_EventGraph_Variable/Animation_Blueprint_EventGraph_Variable-bShouldMove_Init.PNG)

`Ground Speed`가 특정 값(3.0f) 이상이면, 캐릭터가 이동 중이라고 판단할 수 있습니다.

`CharacterMovement`의 `Get Current Acceleration` 노드를 사용해 현재 가속도 값을 가져올 수도 있습니다.  
가속도 벡터가(0, 0, 0)에 가깝다면, 입력 중이 아닌 상태일 수 있습니다.

이동 여부와 사용자 입력(가속도 여부)을 `AND`조건 등으로 조합해, "캐릭터가 실제로 이동 중인지”를 판별해 값을 저장합니다.

![Animation_Blueprint_EventGraph_Variable-bShouldMove_Variable]({{site.url}}/images/Unreal/UE5Dev/2025-02-03-Animation_Blueprint_EventGraph_Variable/Animation_Blueprint_EventGraph_Variable-bShouldMove_Variable.PNG)

## 매 프레임 캐릭터의 낙하 여부

캐릭터가 점프 중이거나 공중에 있는 상태를 구분하는 기본적인 로직을 구현해보겠습니다.

![Animation_Blueprint_EventGraph_Variable-bIsFalling_Init]({{site.url}}/images/Unreal/UE5Dev/2025-02-03-Animation_Blueprint_EventGraph_Variable/Animation_Blueprint_EventGraph_Variable-bIsFalling_Init.PNG)

`CharacterMovement`의 `Is Falling` 함수를 사용해 캐릭터가 공중에 떠 있는지 확인할 수 있습니다.  
점프를 했거나 플랫폼에서 떨어졌을 때 등, 캐릭터가 지면에 붙어있지 않으면 `True`가 반환됩니다.

![Animation_Blueprint_EventGraph_Variable-bIsFalling_Variable]({{site.url}}/images/Unreal/UE5Dev/2025-02-03-Animation_Blueprint_EventGraph_Variable/Animation_Blueprint_EventGraph_Variable-bIsFalling_Variable.PNG)

이를 `bool`변수에 저장하면, Anim Graph 내에서 캐릭터가 점프나 낙하 관련 애니메이션으로 전환하도록 제어할 수 있습니다.

# 참고

[애니메이션 블루프린트 이벤트 노드](https://dev.epicgames.com/documentation/ko-kr/unreal-engine/animation-blueprint-event-nodes-in-unreal-engine){: target="_blank"}