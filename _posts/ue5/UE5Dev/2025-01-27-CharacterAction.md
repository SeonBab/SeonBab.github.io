---
layout: single

title: "[UE5] 캐릭터 이동, 점프, 마우스 시점 회전, 달리기 구현"

categories:
    - UE5Dev
tag: [UE5, UE5Dev]

date: 2025-01-27
last_modified_at: 2025-02-03

order : 100150
---

# 캐릭터 기능 구현

[[UE5] 향상된 입력 시스템]({{ "/ue5/EnhancedInputSystem/" | relative_url }}){: target="_blank"}에서 구현한 입력 바인딩을 통해 캐릭터의 이동, 점프, 마우스 회전, 달리기를 구현해보도록 하겠습니다.

해당 글에서는 애니메이션을 포함하지 않은 로직을 구현합니다.

## 이동

이동 하는 함수는 다음과 같습니다.

소스 코드에서 캐릭터를 이동하는 로직을 구현합니다.

```cpp
void ASpartaCharacter::Move(const FInputActionValue& Value)
{
    if (!Controller) return;

    // Value는 Axis2D로 설정된 IA_Move의 입력값 (WASD)을 담고 있음
    // 예) (X=1, Y=0) → 전진 / (X=-1, Y=0) → 후진 / (X=0, Y=1) → 오른쪽 / (X=0, Y=-1) → 왼쪽
    const FVector2D MoveInput = Value.Get<FVector2D>();

    if (MoveInput.X != 0.f)
    {
        // 캐릭터가 바라보는 방향(정면)으로 X축 이동
        AddMovementInput(GetActorForwardVector(), MoveInput.X);
    }

    if (MoveInput.Y != 0.f)
    {
        // 캐릭터의 오른쪽 방향으로 Y축 이동
        AddMovementInput(GetActorRightVector(), MoveInput.Y);
    }
}
```

`FInputActionValue::Get<FVector2D>()`는 `IA_Move`가 Axis2D로 설정되어 있으므로, 2차원 벡터 형태로 입력이 들어옵니다.  
W(앞) / S(뒤) / D(오른쪽) / A(왼쪽)를 동시에 누를 수도 있으므로, (1,1) 같은 형태도 가능합니다.

`AddMovementInput(방향, 크기)`함수는 내부적으로 `CharacterMovementComponent`가 이 요청을 받아 속도를 계산하고, 실제 이동을 구현하며, 매개변수는 다음과 같습니다.  
- 첫 번째 파라미터: 월드 좌표 기준 이동 방향(Forward, Right 등)
- 두 번째 파라미터: 이동 스케일(속도)

## 점프

점프하는 함수는 입력 될 때와 입력이 끝났을 때로 분리했습니다.

소스 코드에서 캐릭터를 점프하는 로직을 구현합니다.

```cpp
void ASpartaCharacter::StartJump(const FInputActionValue& Value)
{
    // Jump 함수는 Character가 기본 제공
    if (Value.Get<bool>())
    {
        Jump();
    }
}

void ASpartaCharacter::StopJump(const FInputActionValue& Value)
{
    // StopJumping 함수도 Character가 기본 제공
    if (!Value.Get<bool>())
    {
        StopJumping();
    }
}
```

`const FInputActionValue& Value`는 `Enhanced Input System`에서 전달된 입력 값을 받습니다.

`Value.Get<bool>()`은 전달된 입력 값을 `bool`로 가져오는 코드입니다.  
`true`일 경우 키가 눌린 상태, `false`일 경우 키가 눌리지 않은 상태입니다.

`StopJumping()`, `Jump()`함수는 캐릭터 클래스에서 기본적으로 제공되는 함수입니다.  
캐릭터가 점프를 하거나 더이상의 점프를 멈추도록 만들어줍니다.

## 시점 회전

소스 코드에서 카메라를 회전하는 로직을 구현합니다.

```cpp
    // 마우스의 X, Y 움직임을 2D 축으로 가져옴
    FVector2D LookInput = Value.Get<FVector2D>();

    // X는 좌우 회전 (Yaw), Y는 상하 회전 (Pitch)
    // 좌우 회전
    AddControllerYawInput(LookInput.X);
    // 상하 회전
    AddControllerPitchInput(LookInput.Y);
```

`AddControllerYawInput()`은 카메라의 `Yaw축`(수평 회전)을 변경합니다.
`AddControllerPitchInput()`은 카메라의 `Pitch축`(수직 회전)을 변경합니다.

## 달리기

언리얼 엔진의 `CharacterMovementComponent`에 있는 `MaxWalkSpeed`라는 속성이 있습니다.  
`MaxWalkSpeed` 값을 변경하면, 캐릭터의 이동 속도가 즉시 바뀌는데, 이 기능을 사용해서 달리기를 구현해보겠습니다.

입력이 들어오면 이동속도를 증가하고, 입력이 없어질 경우 다시 원래 속도로 돌아오도록 설정합니다.

우선 달리기 관련 멤버 변수를 추가해야합니다.

헤더에 다음과 같은 변수를 추가하겠습니다.

```cpp
// 이동 속도 관련 프로퍼티들
// 기본 걷기 속도
UPROPERTY(EditAnywhere, BlueprintReadWrite, Category = "Movement")
float NormalSpeed;
// 기본 걷기 속도 대비 몇 배로 빠르게 달릴지 결정
UPROPERTY(EditAnywhere, BlueprintReadWrite, Category = "Movement")
float SprintSpeedMultiplier;
// 실제 달리기 속도
UPROPERTY(VisibleAnywhere, BlueprintReadOnly, Category = "Movement")
float SprintSpeed;
```

생성자나 블루프린트에서 해당 변수들에 값을 설정해줍니다.

저는 생성자에서 설정하겠습니다.  
생성자에 다음과 같은 코드를 추가합니다.

```cpp
NormalSpeed = 600.0f;
SprintSpeedMultiplier = 1.5f;
SprintSpeed = NormalSpeed * SprintSpeedMultiplier;

GetCharacterMovement()->MaxWalkSpeed = NormalSpeed;
```

`GetCharacterMovement()`함수는 `GameFramework/CharacterMovementComponent.h`헤더가 필요합니다.

달리는 키를 누른 순간과 뗀 순간 실행되는 함수에서 캐릭터의 이동속도를 변화시키는 로직은 다음과 같습니다.

```cpp
void ASpartaCharacter::StartSprint(const FInputActionValue& Value)
{
    if (GetCharacterMovement())
    {
        GetCharacterMovement()->MaxWalkSpeed = SprintSpeed;
    }
}

void ASpartaCharacter::StopSprint(const FInputActionValue& Value)
{
    if (GetCharacterMovement())
    {
        GetCharacterMovement()->MaxWalkSpeed = NormalSpeed;
    }
}
```

`StartSprint`함수는 키를 누른 순간 호출되며, `StopSprint`함수는 키를 뗀 순간 호출됩니다.

각 함수에서 `MaxWalkSpeed`를 설정해 최대 이동 속도를 결정합니다.