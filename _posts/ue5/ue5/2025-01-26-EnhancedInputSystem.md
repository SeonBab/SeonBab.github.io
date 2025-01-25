---
layout: single

title: "[UE5] 향상된 입력 시스템"

categories:
    - UE5
tag: [Unreal Engine, UE5]

date: 2025-01-26
last_modified_at: 2025-01-26

order : 200130
---

# 향상된 입력 시스템

향상된 입력 시스템(Enhanced Input System)이란 언리얼 엔진5 이전 버전(UE4 등)에서 사용하던 전역 `Project Settings` > `Input` 시스템을 대체하거나 확장하기 위한 시스템입니다.

크게 4가지 주요 콘셉트로 이루어져 있습니다.

입력 설정을 입력 매핑(Input Mapping Context, IMC)  
입력 액션(Input Action, IA)  
입력 모디파이어(Input Modifier)
입력 트리거(Input Trigger)

이렇게 4가지 개념으로 나누어 관리합니다.

## 입력 액션

입력 액션(Input Action, IA)은 캐릭터의 이동, 점프, 발사, 줌 등과 같이 특정 동작을 추상화한 단위입니다.

예를 들어 W, A, S, D 등의 이동을 담당하는 `IA_Move`, 스페이스바 점프를 담당하는 `IA_Jump`, 마우스 회전을 담당하는 `IA_Look` 등을 만들 수 있습니다.

### 입력 액션 생성 및 설정

입력 액션을 만드는 방법은 다음과 같습니다.  
컨텐츠 브라우저(Content Browser) > 우클릭(컨텍스트 메뉴) 혹은 추가(Add) > 입력(Input) > 입력 액션(Input Action)을 선택합니다.

![EnhancedInputSystem-IACreate]({{site.url}}/images/Unreal/ue5/2025-01-26-EnhancedInputSystem/EnhancedInputSystem-IACreate.PNG)

인풋 액션 애셋을 더블 클릭해 패널을 열면 아래와 같은 주요 속성을 설정할 수 있습니다.

![EnhancedInputSystem-IAOpen]({{site.url}}/images/Unreal/ue5/2025-01-26-EnhancedInputSystem/EnhancedInputSystem-IAOpen.PNG)

값 타입(Value Type)은 인풋 액션이 입력 동작을 발생시킬 때, 어떤 유형의 값을 제공할지 결정하는 옵션입니다.
+ `Bool` (참/거짓)
    - 단순 On/Off 토글 입력에 사용됩니다.
    - 대표적으로 점프(스페이스바), 공격(마우스 왼쪽 버튼)에 사용됩니다.
- `Axis1D` (1차원 축 값)
    - 단일 축 (-1~1 범위)의 입력에 사용됩니다.
    - 예) 게임패드 트리거(가속 페달), 전진/후진(W/S)
- `Axis2D` (2차원 축 값)
    - X, Y 두 축을 동시에 처리할 때 사용됩니다.
    - 예) 캐릭터 이동(WASD), 마우스 이동(가로+세로)
- `Axis3D` (3차원 축 값)
    - X, Y, Z 세 축을 동시에 처리합니다.
    - 예) 비행 시뮬레이션에서 3축 제어

---

예시로 이동을 구현한다면 다음과 같습니다.

![EnhancedInputSystem-IA_Move]({{site.url}}/images/Unreal/ue5/2025-01-26-EnhancedInputSystem/EnhancedInputSystem-IA_Move.PNG)

이동은 일반적으로 앞/뒤와 왼/오른쪽 두 방향을 동시에 처리하므로, 값 타입은 `Axis2D`로 설정합니다.

---

예시로 점프를 구현한다면 다음과 같습니다.

![EnhancedInputSystem-IA_Jump]({{site.url}}/images/Unreal/ue5/2025-01-26-EnhancedInputSystem/EnhancedInputSystem-IA_Jump.PNG)

스페이스바를 누르면 점프하는 간단한 방식이므로, 값 타입을 `Bool`로 설정합니다.  
점프 동작은 단순히 On/Off로 동작하기 때문에 키가 눌렸는지 여부만 중요하며, 추가적인 수치나 축 값이 필요하지 않습니다.

---

예시로 시점 회전을 구현한다면 다음과 같습니다.

![EnhancedInputSystem-IA_Look]({{site.url}}/images/Unreal/ue5/2025-01-26-EnhancedInputSystem/EnhancedInputSystem-IA_Look.PNG)

마우스 입력은 항상 가로(X축, 좌/우)와 세로(Y축, 위/아래) 움직임을 동시에 포함합니다.  
따라서 값 타입을 `Axis2D`로 설정해줍니다.

---

예시로 달리기를 구현한다면 다음과 같습니다.

![EnhancedInputSystem-IA_Sprint]({{site.url}}/images/Unreal/ue5/2025-01-26-EnhancedInputSystem/EnhancedInputSystem-IA_Sprint.PNG)

달리기 또한 단순히 특정 키를 값으로 속도를 늘리거나 줄이고를 설정해줍니다.  
따라서 값 타입을 `Bool`로 설정해줍니다.

## 트리거

트리거(Trigger)는 입력이 활성화되는 특정 조건을 말합니다.

- `Pressed Trigger`: 키를 누르는 순간에만 작동합니다.
- `Hold Trigger`: 키를 일정 시간 눌렀을 때 작동합니다.
- `Released Trigger`: 키를 뗄 때 작동합니다.

## 모디파이어

모디파이어(Modifier)는 입력 값을 수정하거나 변환하기 위한 설정입니다.

- FOV 스케일링(FOV Scaling): 카메라의 시야각에 따라 입력 값의 스케일을 조정합니다.
    + 시야각에 따라 마우스 감도를 조정하는 기능을 구현할 때 사용할 수 있습니다.
- 스칼라(Scalar): 입력 값에 지정된 값을 곱해줍니다.
    + 마우스, 조이스틱, 터치 패드 등의 감도를 조절 할 때 사용합니다.
- 부정(Negate): 축의 입력값을 반전합니다.
    + ($\times -1$)
- 데드 존(Deadzone): 일정 임계값보다 작은 입력은 무시합니다.
    + 게임패드 조이스틱 미세 떨림 방지를 할 때 사용합니다.
- 스위즐 입력 축 값(Swizzle Input Axis Values): 입력 축 (Axis)을 변환하거나 재구성하는 기능입니다.
    + 언리얼 엔진의 입력 시스템에서는 입력 데이터를 X, Y, Z 축 중 하나로 매핑합니다. 스위즐은 이 입력 값이 올바른 축에 맞지 않을 경우, 특정 축으로 재배치하거나 변환할 수 있게 도와줍니다.

## 입력 매핑

입력 매핑(Input Mapping Context, IMC)은 여러 개의 입력 액션들을 한데 모아놓은 매핑 설정 파일입니다.

게임 진행 중 특정 상황에서 입력 매핑을 활성(Enable)하거나 비활성(Disable)하여 입력을 제어할 수도 있습니다.

예를 들어, 플레이어 기본 이동, 점프, 시점 전환을 하나의 입력 매핑에 넣고, UI 전용 입력을 다른 입력 매핑으로 분리하며 관리할 수 있습니다.

### 입력 매핑 생성 및 설정

입력 매핑 컨텍스트를 생성하는 방법은 다음과 같습니다.  
컨텐츠 브라우저(Content Browser) > 우클릭(컨텍스트 메뉴) 혹은 추가(Add) > 입력(Input) > 입력 매핑 컨텍스트(Input Mapping Context)을 선택합니다.

![EnhancedInputSystem-IMCCreate]({{site.url}}/images/Unreal/ue5/2025-01-26-EnhancedInputSystem/EnhancedInputSystem-IMCCreate.PNG)

입력매핑 컨텍스트 애셋을 더블 클릭해 패널을 열면 입력을 매핑할 수 있습니다.

![EnhancedInputSystem-IMCOpen]({{site.url}}/images/Unreal/ue5/2025-01-26-EnhancedInputSystem/EnhancedInputSystem-IMCOpen.PNG)

---

이동 인풋 액션(IA_Move)은 다음과 같이 매핑했습니다.

![EnhancedInputSystem-IMC_Move]({{site.url}}/images/Unreal/ue5/2025-01-26-EnhancedInputSystem/EnhancedInputSystem-IMC_Move.PNG)

이동을 위한 W, A, S, D 키를 각각 축 값으로 매핑했습니다.

- W 키(전진)
    - W 키를 누르면 입력 값이 `X축`(앞뒤 방향)에 맞춰 정렬됩니다.
    - 전진은 `X축` +1 방향이므로 추가적인 변환은 필요 없습니다.
- S 키(후진)
    - S 키의 입력 값도 스위즐 입력 축 값을 통해 `X축`으로 정렬됩니다.
    - 후진은 전진 (W)의 반대 방향이므로 부정을 사용해 입력 값을 반전합니다. (X축 +1 → X축 -1)
- A 키 (왼쪽 이동)
    - A 키를 누르면 입력 값이 `Y축`(좌우 방향)에 맞춰 정렬됩니다.
    - A키는 오른쪽 이동(D)의 반대 방향이므로 부정을 사용해 입력 값을 반전합니다. (Y축 +1 → Y축 -1)
- D 키 (오른쪽 이동)
    - D 키의 입력값도 스위즐 입력 축 값을 통해 `Y축`으로 정렬됩니다.
    - D 키는 `Y축` +1 방향이므로 추가적인 변환은 필요 없습니다.

---

점프 인풋 액션(IA_Jump)은 다음과 같이 매핑했습니다.

![EnhancedInputSystem-IMC_Jump]({{site.url}}/images/Unreal/ue5/2025-01-26-EnhancedInputSystem/EnhancedInputSystem-IMC_Jump.PNG)

점프를 위한 스페이스바 키를 값으로 매핑했습니다.

On/Off로 동작하며, 별도의 트리거나 모디파이어가 필요 없으므로, 기본 상태로 둡니다.

---

시점 회전 인풋 액션(IA_Look)은 다음과 같이 매핑했습니다.

![EnhancedInputSystem-IMC_Look]({{site.url}}/images/Unreal/ue5/2025-01-26-EnhancedInputSystem/EnhancedInputSystem-IMC_Look.PNG)

시점 회전을 위한 마우스 움직임을 키 값으로 매핑했습니다.  

Yaw(좌우 회전)과 Pitch(상하 회전) 값을 동시에 전달받습니다.  
`Axis2D`로 지정한 `X축`, `Y축` 값을 게임 내 좌표계에 맞춰 회전에 반영합니다.  

기본적으로 마우스 Y축의 움직임은 `위로 움직임 = 양수`, `아래로 움직임 = 음수`로 전달됩니다.  
하지만, 카메라의 상하 회전(Pitch)은 엔진의 좌표계에서 `위로 = 음수`, `아래로 = 양수`로 작동하는 경우가 많습니다.  
따라서, `Y축` 값을 모디파이어의 부정을 통해 반전해야 합니다.
+ 위로 움직임 → 음수(-) (카메라 위로).
+ 아래로 움직임 → 양수(+) (카메라 아래로).
`X축` (좌우 회전, Yaw)의 경우, 기본적인 엔진 좌표계와 마우스 움직임의 방향이 일치합니다.
+ 오른쪽 이동 → 양수(+).
+ 왼쪽 이동 → 음수(-).

---

달리기 인풋 액션(IA_Sprint)는 다음과 같이 매핑했습니다.

![EnhancedInputSystem-IMC_Sprint]({{site.url}}/images/Unreal/ue5/2025-01-26-EnhancedInputSystem/EnhancedInputSystem-IMC_Sprint.PNG)

달라기를 위한 Left Shift 키를 값으로 매핑했습니다.

On/Off로 동작하며, 별도의 트리거나 모디파이어가 필요 없으므로, 기본 상태로 둡니다.

## 플레이어 컨트롤러에서 입력 매핑 활성화

플레이어 컨트롤러에서 입력 매핑 컨텍스트(IMC)를 활성화하겠습니다.


활성화하기 위해서는 우리가 만든 `IA`와 `IMC`를 C++ 멤버 변수로 선언하고, 언리얼 에디터에서 이를 지정할 수 있도록 리플렉션 처리합니다.

```cpp
#pragma once

#include "CoreMinimal.h"
#include "GameFramework/PlayerController.h"
#include "SpartaPlayerController.generated.h"

// 전방 선언
class UInputMappingContext;
class UInputAction;

UCLASS()
class SPARTAPROJECT_API ASpartaPlayerController : public APlayerController
{
	GENERATED_BODY()
	
public:
	ASpartaPlayerController();

	// 에디터에서 세팅할 IMC
	UPROPERTY(EditAnywhere, BlueprintReadWrite, Category = "Input")
	TSoftObjectPtr<UInputMappingContext> InputMappingContext;
	// IA_Move를 지정할 변수
	UPROPERTY(EditAnywhere, BlueprintReadWrite, Category = "Input")
	TSoftObjectPtr<UInputAction> MoveAction;
	// IA_Jump를 지정할 변수
	UPROPERTY(EditAnywhere, BlueprintReadWrite, Category = "Input")
	TSoftObjectPtr<UInputAction> JumpAction;
	// IA_Look를 지정할 변수
	UPROPERTY(EditAnywhere, BlueprintReadWrite, Category = "Input")
	TSoftObjectPtr<UInputAction> LookAction;
	// IA_Sprint를 지정할 변수
	UPROPERTY(EditAnywhere, BlueprintReadWrite, Category = "Input")
	TSoftObjectPtr<UInputAction> SprintAction;
};
```

빌드한 뒤, 언리얼 에디터를 재시작하고, 생성한 클래스를 상속받는 블루프린트 클래스를 생성 후 열어 디테일 패널을 확인해봅니다.  
위에서 선언한 변수가 노출되어 있는 것을 볼 수 있습니다.

![EnhancedInputSystem-PlayerController]({{site.url}}/images/Unreal/ue5/2025-01-26-EnhancedInputSystem/EnhancedInputSystem-PlayerController.PNG)

여기서 우리가 만든 입력 매핑 컨텍스트와 인풋 액션을 멤버 변수에 각각 할당합니다.

이제 C++에서 입력 매핑 컨텍스트를 활성화 해보겠습니다.

언리얼 5의 향상된 입력 시스템은 `Local Player Subsystem`을 통해 입력 매핑 컨텍스트를 활성화하거나 비활성화합니다.

헤더에 `BeginPlay`함수를 선언합니다.

```cpp
virtual void BeginPlay() override;
```

```cpp
// Enhanced Input System의 Local Player Subsystem을 사용하기 위해 포함
#include "EnhancedInputSubsystems.h"
#include "InputMappingContext.h"

void ASpartaPlayerController::BeginPlay()
{
	Super::BeginPlay();

    // 현재 PlayerController에 연결된 Local Player 객체를 가져옴
    ULocalPlayer* LocalPlayer = GetLocalPlayer();
    if (LocalPlayer)
    {
        // Local Player에서 EnhancedInputLocalPlayerSubsystem을 획득
        UEnhancedInputLocalPlayerSubsystem* Subsystem = LocalPlayer->GetSubsystem<UEnhancedInputLocalPlayerSubsystem>();
        if (Subsystem)
        {
            if (!InputMappingContext.IsNull())
            {
                // Subsystem을 통해 우리가 할당한 IMC를 활성화
                // 우선순위(Priority)는 0이 가장 높은 우선순위
                Subsystem->AddMappingContext(InputMappingContext.LoadSynchronous(), 0);
            }
        }
    }
}
```

`GetLocalPlayer()`
+ 현재 `PlayerController`가 관리하는 `Local Player`를 반환합니다.

`GetSubsystem<UEnhancedInputLocalPlayerSubsystem>()`
+ 해당 `Local Player`에 부착된 `Enhanced Input Subsystem`을 반환합니다.
+ 이를 통해 `AddMappingContext`나 `RemoveMappingContext` 등을 호출하여 입력 매핑을 동적으로 제어할 수 있습니다.

`AddMappingContext()`
- 주어진 `IMC`를 `Subsystem`에 추가하여 입력 매핑을 활성화합니다.
- `InputMappingContext`는 활성화하고자 하는 IMC 애셋입니다.
- `0`은 우선순위를 의미하며, 낮을수록 높은 우선순위를 가집니다.
- 이 함수를 여러 번 호출해 여러 `IMC`를 활성화할 수도 있습니다.
    + 우선순위를 달리 부여해, 특정 `IMC`가 다른 `IMC`보다 우선순위가 높도록 설정할 수도 있습니다.

이 로직이 실행되면, `IMC`에 정의된 모든 `IA`와 키 매핑이 `PlayerController`에 적용됩니다.  
이 경우 각 입력 액션을 활성화해줄 뿐이며, 어떤 함수가 호출될지 바인딩해주어야 합니다.

캐릭터 클래스에서 액션 바인딩을 추가해보도록 하겠습니다.

헤더 파일에서 바인딩에 필요한 함수들을 선언합니다.

```cpp
// 전방 선언
// Enhanced Input에서 액션 값을 받을 때 사용하는 구조체
struct FInputActionValue;

virtual void SetupPlayerInputComponent(class UInputComponent* PlayerInputComponent) override;

// IA_Move와 IA_Jump 등을 처리할 함수 원형
// Enhanced Input에서 액션 값은 FInputActionValue로 전달됩니다.
UFUNCTION()
void Move(const FInputActionValue& value);
UFUNCTION()
void StartJump(const FInputActionValue& value);
UFUNCTION()
void StopJump(const FInputActionValue& value);
UFUNCTION()
void Look(const FInputActionValue& value);
UFUNCTION()
void StartSprint(const FInputActionValue& value);
UFUNCTION()
void StopSprint(const FInputActionValue& value);
```

소스 코드에서 입력 액션을 바인딩하는 로직을 구현합니다.

```cpp
// 입력 바인드에 필요한 헤더
#include "EnhancedInputComponent.h"
// 플레이어 컨트롤러 헤더
#include "SpartaPlayerController.h"

void ASpartaCharacter::SetupPlayerInputComponent(UInputComponent* PlayerInputComponent)
{
	Super::SetupPlayerInputComponent(PlayerInputComponent);

    // Enhanced InputComponent로 캐스팅
    if (UEnhancedInputComponent* EnhancedInput = Cast<UEnhancedInputComponent>(PlayerInputComponent))
    {
        // IA를 가져오기 위해 현재 소유 중인 Controller를 ASpartaPlayerController로 캐스팅
        if (ASpartaPlayerController* PlayerController = Cast<ASpartaPlayerController>(GetController()))
        {
            if (!PlayerController->MoveAction.IsNull())
            {
                // IA_Move 액션 키를 "키를 누르고 있는 동안" Move() 호출
                EnhancedInput->BindAction(PlayerController->MoveAction.LoadSynchronous(), ETriggerEvent::Triggered, this, &ASpartaCharacter::Move);
            }

            if (!PlayerController->JumpAction.IsNull())
            {
                // IA_Jump 액션 키를 "키를 누르고 있는 동안" StartJump() 호출
                EnhancedInput->BindAction(PlayerController->JumpAction.LoadSynchronous(), ETriggerEvent::Triggered, this, &ASpartaCharacter::StartJump);

                // IA_Jump 액션 키에서 "손을 뗀 순간" StopJump() 호출
                EnhancedInput->BindAction(PlayerController->JumpAction.LoadSynchronous(), ETriggerEvent::Completed, this, &ASpartaCharacter::StopJump);
            }

            if (!PlayerController->LookAction.IsNull())
            {
                // IA_Look 액션 마우스가 "움직일 때" Look() 호출
                EnhancedInput->BindAction(PlayerController->LookAction.LoadSynchronous(), ETriggerEvent::Triggered, this, &ASpartaCharacter::Look);
            }

            if (!PlayerController->SprintAction.IsNull())
            {
                // IA_Sprint 액션 키를 "누르고 있는 동안" StartSprint() 호출
                EnhancedInput->BindAction(PlayerController->SprintAction.LoadSynchronous(), ETriggerEvent::Triggered, this, &ASpartaCharacter::StartSprint);

                // IA_Sprint 액션 키에서 "손을 뗀 순간" StopSprint() 호출
                EnhancedInput->BindAction(PlayerController->SprintAction.LoadSynchronous(), ETriggerEvent::Completed, this, &ASpartaCharacter::StopSprint);
            }
        }
    }
}
```

`FInputActionValue`는 Enhanced Input에서 액션 값(축 이동값, 마우스 이동량 등)을 전달할 때 사용하는 구조체로, IA에서 설정한 Value Type입니다.

`UFUNCTION()`은 입력 바인딩 함수는 언리얼 엔진 리플렉션 시스템과 연동되어야 합니다.  
`UFUNCTION()`을 붙이지 않으면 바인딩에 실패할 수 있습니다.
+ 블루프린트 접근성을 설정하지 않았더라도, 기본적으로 메타데이터가 생성됩니다.
+ 언리얼 엔진의 입력 처리 시스템은 바인딩된 함수가 리플렉션 시스템을 통해 접근 가능한지 확인합니다.

+ `BindAction` 함수
    - 첫 번째 인자: 어떤 `UInputAction`과 연결할지. (예:`MoveAction`)
    - 두 번째 인자: 액션이 발생하는 트리거 이벤트 (`Triggered`, `Ongoing`, `Completed` 등).
    - 세 번째/네 번째 인자: 액션 발생 시 실행할 객체(`this`)와 함수 포인터.

+ 점프와 스프린트 함수 분리
    - 키를 누를 때와 뗄 때가 다르게 처리될 수 있으므로 두 함수로 분리했습니다.

# 참고

[향상된 입력](https://dev.epicgames.com/documentation/ko-kr/unreal-engine/enhanced-input-in-unreal-engine){: target="_blank"}