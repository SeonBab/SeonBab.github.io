---
layout: single

title: "[UE5] State Machine을 사용해 이동, 점프 등 애니메이션 구현하기"

categories:
    - UEPractice
tag: [UE5, UEPractice]

date: 2025-02-04
last_modified_at: 2025-02-04

order : 101010
---

# 상태 기계

상태 기계(State Machine)란 언리얼 엔진의 애니메이션 시스템에서, 캐릭터의 상태(Idle, Walk, Run, Jump 등)에 따라 어떤 애니메이션을 재생하고, 어떻게 전환할지를 결정하기 위한 논리적 구조입니다.

가장 쉽게 이해하자면, “Idle 상태이면 Idle 애니메이션을 재생하고, 캐릭터가 움직이기 시작하면 Walking 애니메이션으로 전환한다.” 같은 로직을 직관적으로 구성하게 해줍니다.

`State Machine`의 핵심 개념은 다음과 같습니다.

1. State(상태)
    - 캐릭터가 현재 어떤 동작을 하고 있는지 나타냅니다.
    - 예) `Idle`, `Walking`, `Running`, `Jumping` 등이 각각 하나의 상태가 될 수 있습니다.
2. Transition(전환)
    - 한 상태에서 다른 상태로 언제 전환되는지 조건(Condition)을 정의합니다.
    - 예) `Idle` → `Walking` 전환 조건: “속도 > 0” (즉, 캐릭터가 이동을 시작)
3. Animation Graph와의 연결
    - State Machine 내부의 상태 (State)는 실제로 재생할 애니메이션을 배치하는 장소입니다.
    - State 간 전환 조건을 만족하면 다른 애니메이션이 재생되도록 합니다.

## 상태 기계 생성

`State Machine`을 생성하는 방법은 다음과 같습니다.  
Anim Graph 창에서 우클릭 > State Machine > 이름 설정

![StateMachine-Create]({{site.url}}/images/Unreal/UEPractice/2025-02-04-StateMachine/StateMachine-Create.PNG)

이 `State Machine` 노드를 `Final Output Pose` 쪽에 연결하면, 해당 `State Machine`에서 나온 결과가 캐릭터 최종 포즈가 됩니다.

![StateMachine-Locomotion_Output]({{site.url}}/images/Unreal/UEPractice/2025-02-04-StateMachine/StateMachine-Locomotion_Output.PNG)

이 글에서 사용할 상태 기계에 필요한 변수는 모두 [[UE5] 애니메이션 블루프린트에서 필요한 변수 세팅하기]({{ "/UEPractice/Animation_Blueprint_EventGraph_Variable/" | relative_url }}){: target="_blank"}에서 준비했습니다.

### Locomotion State Machine

`Locomotion`이라는 이름을 붙인 `State Machine`을 만들어 캐릭터의 기본 이동(Idle, Walk, Run)관련 애니메이션들을 전환하는 기능을 구현해보겠습니다.

다음과 같은 스테이트들을 생성합니다.

![StateMachine-Locomotion_State_Machine]({{site.url}}/images/Unreal/UEPractice/2025-02-04-StateMachine/StateMachine-Locomotion_State_Machine.PNG)

해당 스테이트를 사용해 플레이해보면 다음과 같습니다.

![StateMachine-Locomotion_Result]({{site.url}}/images/Unreal/UEPractice/2025-02-04-StateMachine/StateMachine-Locomotion_Result.gif)

#### Idle State

`State Machine`을 더블 클릭해 들어가면 `Entry` 노드가 보입니다.  
이 `Entry` 노드에서 드래그 드롭하여 스테이트 추가(Add State)를 선택하면 스테이트를 추가할 수 있습니다.

![StateMachine-Locomotion_AddState]({{site.url}}/images/Unreal/UEPractice/2025-02-04-StateMachine/StateMachine-Locomotion_AddState.PNG)

서있을 경우 애니메이션을 재생시키려 하므로 이름을 `Idle`이라고 정하겠습니다.

![StateMachine-Locomotion_AddIdleState]({{site.url}}/images/Unreal/UEPractice/2025-02-04-StateMachine/StateMachine-Locomotion_AddIdleState.PNG)

`Entry` → `Idle`로 가는 선은 `State Machine`이 시작되면 `Idle` 상태로 진입한다는 의미입니다.

`Idle State`를 더블 클릭해서 내부로 들어간 뒤, `Output Animation Pose`에 재생하고자하는 애니메이션 시퀀스를 연결합니다.

![StateMachine-Idle_Anim_Sequence]({{site.url}}/images/Unreal/UEPractice/2025-02-04-StateMachine/StateMachine-Idle_Anim_Sequence.PNG)

저는 반복 재생을 하고자 하므로, 애니메이션 루프(Loop Animation)를 활성화해주었습니다.

#### Walk/Run State

또 다른 스테이트를 생성하고자 하므로, `State Machine` 창으로 돌아와 `Add State`를 선택해 스테이트를 생성하겠습니다.

![StateMachine-Locomotion_AddWalkRunState]({{site.url}}/images/Unreal/UEPractice/2025-02-04-StateMachine/StateMachine-Locomotion_AddWalkRunState.PNG)

해당 스테이트에서는 걷거나 뛰는 애니메이션을 가진 블렌드 스페이스를 재생하고자 하므로, 해당 에셋을 연결했습니다.

![StateMachine-WalkRun_BlendSpace]({{site.url}}/images/Unreal/UEPractice/2025-02-04-StateMachine/StateMachine-WalkRun_BlendSpace.PNG)

#### State 전환 조건 설정

`Idle State`의 테두리를 클릭하고, `Walk/Run State`으로 드래그하면 화살표가 하나 생깁니다.  
해당 방향으로 애니메이션이 전환 될 수 있음을 의미합니다.

![StateMachine-Create_TransitionRule]({{site.url}}/images/Unreal/UEPractice/2025-02-04-StateMachine/StateMachine-Create_TransitionRule.PNG)

애니메이션을 전환하고자 할때 조건을 설정할 수 있습니다.

![StateMachine-Transition]({{site.url}}/images/Unreal/UEPractice/2025-02-04-StateMachine/StateMachine-Transition.PNG)

`Idle State`에서 `Walk/Run State`으로 가는 화살표(Transition)를 더블 클릭하면 트렌지션 룰(Transition Rule)설정 창이 열립니다.

![StateMachine-IdleToWalkRun]({{site.url}}/images/Unreal/UEPractice/2025-02-04-StateMachine/StateMachine-IdleToWalkRun.PNG)

해당 설정 창은 `Idle`에서 `Walk/Run` 전환 될 조건을 설정할 수 있습니다.  
저는 캐릭터가 이동 입력을 받고 있을 때 전환하도록 설정했습니다.

![StateMachine-WalkRunToIdle]({{site.url}}/images/Unreal/UEPractice/2025-02-04-StateMachine/StateMachine-WalkRunToIdle.PNG)

반대로 `Walk/Run`에서 `Idle`로 가는 조건 또한 설정해주어야 합니다.

여기에서는 캐릭터가 이동 입력을 받고 있지 않을 때 전환하도록 설정했습니다.

### Main State Machine

`Main State Machine`에서는 점프, 낙하, 착지와 같은 추가 상태들을 포함한 상위 `State Machine`을 구현해보겠습니다.

`Locomotion State Machine`을 상위 `State Machine`에서 사용하기 위해 캐시 포즈(Cached Pose)로 저장해줍니다.

![StateMachine-Create_MainStates]({{site.url}}/images/Unreal/UEPractice/2025-02-04-StateMachine/StateMachine-Create_MainStates.PNG)

다음과 같은 스테이트들을 생성합니다.

![StateMachine-Main_State_Machine]({{site.url}}/images/Unreal/UEPractice/2025-02-04-StateMachine/StateMachine-Main_State_Machine.PNG)

해당 스테이트를 사용해 플레이해보면 다음과 같습니다.

![StateMachine-MainStates_Result]({{site.url}}/images/Unreal/UEPractice/2025-02-04-StateMachine/StateMachine-MainStates_Result.gif)

### Locomotion State

`Locomotion State`에 들어가서, 앞서 캐시 포즈로 저장한 `Locomotion` 포즈를 연결해줍니다.

![StateMachine-Add_Cached_Locomotion]({{site.url}}/images/Unreal/UEPractice/2025-02-04-StateMachine/StateMachine-Add_Cached_Locomotion.PNG)

![StateMachine-Cached_Locomotion]({{site.url}}/images/Unreal/UEPractice/2025-02-04-StateMachine/StateMachine-Cached_Locomotion.PNG)

이러면 해당 스테이트에서는 `Locomotion State Machine`이 작동하게 됩니다.

### Land State

`Land State`에서는 착지 애니메이션을 사용하고, 자연스러운 애니메이션을 위해 `Locomotion` 상태에서 넘어온 포즈를 적절히 섞어줍니다.  
착지 애니메이션은 애니메이션 루프(Loop Animation)를 켜주는게 좀 더 자연스러울 수 있습니다.

![StateMachine-LandState]({{site.url}}/images/Unreal/UEPractice/2025-02-04-StateMachine/StateMachine-LandState.PNG)

`Locomotion State`로 전환하는 조건은 2개를 걸겠습니다.  
`Land State`에서 전환선을 두 개 생성해 두 가지 조건을 걸 수 있습니다.

![StateMachine-LandState_Transition]({{site.url}}/images/Unreal/UEPractice/2025-02-04-StateMachine/StateMachine-LandState_Transition.PNG)

애니메이션 시퀀스 재생을 완료하면 자동으로 스테이트를 전환시켜주는 조건과 캐릭터가 움직이고 있다는 조건을 저장한 변수를 사용하여 전환이 이루어지도록 합니다.

스테이트의 시퀀스 플레이어에 따른 자동 규칙(Automatic Rule Based on Sequence Player in State)을 활성화 해주어 자동으로 전환되게 해줍니다.

![StateMachine-LandState_Transition_Rule1]({{site.url}}/images/Unreal/UEPractice/2025-02-04-StateMachine/StateMachine-LandState_Transition_Rule1.PNG)

움직이고 있다는 조건은 다음과 같이 설정합니다.

![StateMachine-LandState_Transition_Rule2]({{site.url}}/images/Unreal/UEPractice/2025-02-04-StateMachine/StateMachine-LandState_Transition_Rule2.PNG)

### Fall Loop State

추락하고 있는 애니메이션을 적용하고 애니메이션 루프(Loop Animation)를 활성화합니다.

![StateMachine-FallLoopState]({{site.url}}/images/Unreal/UEPractice/2025-02-04-StateMachine/StateMachine-FallLoopState.PNG)

### Jump State

해당 스테이트에서는 점프하는 애니메이션을 적용하고, 해당 애니메이션 시퀀스가 끝나면 자동으로 `Fall Loop`으로 전환되게 해주어야 합니다.  

![StateMachine-JumpState]({{site.url}}/images/Unreal/UEPractice/2025-02-04-StateMachine/StateMachine-JumpState.PNG)

스테이트의 시퀀스 플레이어에 따른 자동 규칙(Automatic Rule Based on Sequence Player in State)을 활성화 해주어 자동으로 전환되게 해줍니다.

![StateMachine-Jump_Transition_Rule]({{site.url}}/images/Unreal/UEPractice/2025-02-04-StateMachine/StateMachine-Jump_Transition_Rule.PNG)

### To Land(State Alias)

`To Land`는 스테이트 에일리어스(State Alias)입니다.  
스테이트 에일리어스는 특정 스테이트들을 그룹지어 별칭을 주는 기능입니다.

해당 기능을 추가하는 방법은 다음과 같습니다.

![StateMachine-Create_StateAlias]({{site.url}}/images/Unreal/UEPractice/2025-02-04-StateMachine/StateMachine-Create_StateAlias.PNG)

공중에 있다가 지면으로 향할 때 전환되는 스테이트들의 그룹입니다.  
`Jump`와 `Fall Loop`를 `To Land` 그룹으로 묶어준 것입니다.

![StateMachine-ToLand_StateAlias]({{site.url}}/images/Unreal/UEPractice/2025-02-04-StateMachine/StateMachine-ToLand_StateAlias.PNG)

`To Land`에서 `Land`로 전환하는 조건은 떨어지고 있는지 값을 저장한 변수가 `false`, 즉 떨어지지 않고있다면 전환하도록 설정합니다.

![StateMachine-ToLand_Transition_Rule]({{site.url}}/images/Unreal/UEPractice/2025-02-04-StateMachine/StateMachine-ToLand_Transition_Rule.PNG)

## To Falling(State Alias)

`To Falling`또한 `State Alias`입니다.

지면에 있다가 공중에 떠있는 상태로 전환되는 스테이트들의 그룹입니다.  
`Locomotion`과 `Land`를 `To Falling` 그룹으로 묶어준 것입니다.

![StateMachine-ToFalling_StateAlias]({{site.url}}/images/Unreal/UEPractice/2025-02-04-StateMachine/StateMachine-ToFalling_StateAlias.PNG)

`To Falling`에서 `Jump`로 전환하는 조건은 떨어지고 있는지 값을 저장한 변수가 `true`이며, Z 축의 속도가 일정 값 이상이라면 전환합니다.  

![StateMachine-ToFalling_Jump_TransitionRule]({{site.url}}/images/Unreal/UEPractice/2025-02-04-StateMachine/StateMachine-ToFalling_Jump_TransitionRule.PNG)

`To Falling`에서 `Fall Loop`로 전환하는 조건은 떨어지고 있는지 값을 저장한 변수가 `true`이기만 한다면 전환합니다.  
해당 상황은 걷다가 갑자기 바닥이 꺼져서 낙하를 한다거나, 높은 곳에서 미끄러져 떨어지는 등의 상황이 있습니다.

![StateMachine-ToFalling_FallLoop_TransitionRule]({{site.url}}/images/Unreal/UEPractice/2025-02-04-StateMachine/StateMachine-ToFalling_FallLoop_TransitionRule.PNG)