---
layout: single

title: "[UE5] 블루프린트 움직이는 액터"

categories:
    - UEPractice
tag: [UE5, UEPractice]

date: 2024-06-15
last_modified_at: 2025-01-24

order : 30
---

# 움직이는 액터

액터는 레벨에서 트랜스폼 값을 가질 수 있습니다.  
트랜스폼에는 위치, 회전, 스케일이 있고 위치와 회전을 게임 실행중에 변경하는 방법을 알아보겠습니다.

![MovableActor-PickActor]({{site.url}}/images/Unreal/UEPractice/2024-06-15-MovableActor/MovableActor-PickActor.PNG)

우선 움직이고자하는 액터를 하나 만들어줍니다.  
액터를 만드는 방법은 제가 전에 작성한 [[UE5] 언리얼 엔진 액터 제작](https://seonbab.github.io/UEPractice/MakeActor/)에서 알아 볼 수 있습니다.

액터를 이동하고 회전하는 것은 블루프린트의 이벤트그래프를 사용하겠습니다.  
이벤트는 `Tick` 이벤트를 사용해 매 프레임 움직이도록 만들겠습니다.

## 액터 모빌리티

액터의 트랜스폼에 모빌리티라는 프로퍼티가 있습니다.  
이 모빌리티는 액터가 게임 플레이 중 어떤 방식으로 이동하거나 변경될 수 있는지 제어하는 세팅입니다.

![MovableActor-SettingMobility]({{site.url}}/images/Unreal/UEPractice/2024-06-15-MovableActor/MovableActor-SettingMobility.PNG)

우리는 액터가 게임 플레이중에 움직이고자하므로 트랜스폼을 갖는 컴포넌트를 무버블로 변경하겠습니다.

액터 모빌리티는 [액터 모빌리티](https://dev.epicgames.com/documentation/ko-kr/unreal-engine/actor-mobility-in-unreal-engine?application_version=5.3)에서 자세하게 알아볼 수 있습니다.

## 액터 이동과 회전

![MovableActor-LocationAndRotationFunction]({{site.url}}/images/Unreal/UEPractice/2024-06-15-MovableActor/MovableActor-LocationAndRotationFunction.PNG)

`Target`핀에는 이동하거나 회전하려는 액터나 액터의 컴포넌트를 받습니다.
`Location`혹은 `Rotation`핀에는 이동과 회전을 얼만큼 할지 값을 받습니다.
`Sweep`핀은해당 액터가 움직이면서 충돌이 일어나면 더이상 목표 위치까지 가지 못하고 이동이 멈춥니다. `참`이 되면 활성화 됩니다.
`Teleport`혹은 `Teleport Physics`핀은 해당 액터가 아주 짧은 시간 아주 먼 거리를 이동한다면 문제가 생길 수 있는데, 이 문제를 벗어나기 위한 옵션입니다. 순간이동한 것 처럼 액터를 이동시킬 때 사용하는 옵션입니다. `참`이 되면 활성화 됩니다.

이 입력 핀들은 이동과 회전 함수들에서 동일합니다.

`Sweep`과 `Teleport`의 설명은 [물리적인 물체의 움직임 다루기!](https://www.unrealengine.com/ko/blog/moving-physical-objects)에서 좀 더 자세하게 알 수 있습니다.

![MovableActor-ExampleSetLocationAndRotation]({{site.url}}/images/Unreal/UEPractice/2024-06-15-MovableActor/MovableActor-ExampleSetLocationAndRotation.PNG)


액터의 이동과 회전을 동시에하는 것은 위의 사진과 같은 방법으로 할 수 있습니다.

이동하는 함수와 회전하는 함수 두 함수를 사용해 액터를 움직여도 무관합니다.

### Set Actor Location And Rotation

이 함수는 새로운 위치 값과 회전 값을 받아 액터의 위치와 회전 값을 바꿉니다.

### Set World Location And Rotation

이 함수는 새로운 위치 값과 회전 값을 받아 월드 스페이스로 액터 컴포넌트의 위치와 회전 값을 바꿉니다.

### Set Relative Location And Rotation

이 함수는 새로운 위치 값과 회전 값을 받아 부모 스페이스에 상대 좌표로 액터 컴포넌트의 위치와 회전 값을 바꿉니다.

## 액터 이동

![MovableActor-ExampleAddActorLocalOffset]({{site.url}}/images/Unreal/ue5/2024-06-15-MovableActor/MovableActor-ExampleAddActorLocalOffset.PNG)

액터의 이동은 위의 사진과 같은 방법으로 할 수 있습니다.

### Add Actor Local Offset

이 함수는 위치 값을 받아 액터의 기존 위치 값에 더합니다.

### Add Local Offset

이 함수는 위치 값을 받아 액터 컴포넌트의 기존 위치에 값을 더합니다.

### Add Actor World Offset

이 함수는 위치 값을 받아 월드 스페이스로 액터의 기존 위치에 값을 더합니다.

### Add World Offset

이 함수는 위치 값을 받아 월드 스페이스로 액터 컴포넌트의 기존 위치에 값을 더합니다.

### Add Relative Location

이 함수는 위치 값을 받아 부모 스페이스에 상대 좌표로 액터 컴포넌트의 기존 위치에 값을 더합니다.

### Set Actor Location

이 함수는 새로운 위치 값을 받아 액터의 위치 값을 바꿉니다.

### Set Actor Relative Location

이 함수는 위치 값을 받아 부모 스페이스에 상대 좌표로 액터의 위치를 바꿉니다.

### Set Relative Location

이 함수는 위치 값을 받아 부모 스페이스에 상대 좌표로 액터 컴포넌트의 위치를 바꿉니다.

### Set World Location

이 함수는 위치 값을 받아 월드 스페이스로 액터 컴포넌트의 위치를 바꿉니다.

## 액터 회전

![MovableActor-ExampleAddActorLocalRotation]({{site.url}}/images/Unreal/ue5/2024-06-15-MovableActor/MovableActor-ExampleAddActorLocalRotation.PNG)

액터의 회전은 위의 사진과 같은 방법으로 할 수 있습니다.

### Add Actor Local Rotation

이 함수는 회전 값을 받아 액터의 기존 회전 값에 더합니다.

### Add Local Rotation

이 함수는 회전 값을 받아 액터 컴포넌트의 기존 회전에 값을 더합니다.

### Add Actor World Rotation

이 함수는 회전 값을 받아 월드 스페이스로 액터의 기존 회전에 값을 더합니다.

### Add World Rotation

이 함수는 회전 값을 받아 월드 스페이스로 액터 컴포넌트의 기존 회전에 값을 더합니다.

### Add Relative Rotation

이 함수는 회전 값을 받아 부모 스페이스에 상대 좌표로 액터 컴포넌트의 기존 회전에 값을 더합니다.

### Set Actor Rotation

이 함수는 새로운 회전 값을 받아 액터의 회전 값을 바꿉니다.

### Set Actor Relative Rotation

이 함수는 회전 값을 받아 부모 스페이스에 상대 좌표로 액터의 회전을 바꿉니다.

### Set Relative Rotation

이 함수는 회전 값을 받아 부모 스페이스에 상대 좌표로 액터 컴포넌트의 회전을 바꿉니다.

### Set World Rotation

이 함수는 회전 값을 받아 월드 스페이스로 액터 컴포넌트의 회전을 바꿉니다.