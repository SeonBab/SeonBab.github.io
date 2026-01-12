---
layout: single

title: "[UE5] 폰과 캐릭터 클래스"

categories:
    - UE5
tag: [Unreal Engine, UE5]

date: 2025-01-24
last_modified_at: 2026-01-12

order : 200100
---

# 폰과 캐릭터 클래스

## 폰

폰(Pawn)은 플레이어 혹은 AI가 소유(Possess)할 수 있는 가장 상위 클래스입니다.  
즉, 엔진에서 무언가를 조종할 때 기본이 되는형태가 폰입니다.

폰 자체에는 이동, 중력, 네트워크 이동 예측 혹은 보정 로직이 구현되어 있지 않습니다.  

폰을 위한 기본 이동 컴포넌트는 제공되지 않으며, 이동을 직접 구현해 자유롭게 이동 방식을 구성할 수 있습니다.  
예를 들어 비행기, 드론처럼 기본 캐릭터의 이동 방식을 벗어난 특수한 로직을 완전히 자유롭게 구현할 때 유용합니다.

실제로 이동하지 않고 이동할 수 있는 길을 찾는 길찾기는 이용 가능합니다.  
이는 네비게이션 시스템으로 경로 계산이 이루어지기 때문에 가능합니다.

사용자의 입력을 실제로 처리하거나 사용자의 화면에 대응되는 카메라를 설정할 수 있습니다.

기믹이나 다른 폰과 상호작용 할 수 있도록 할 수 있습니다.

상태에 적합한 애니메이션을 재생하도록 할 수 있습니다.

## 캐릭터

캐릭터(Character)는 폰을 상속받아 만들어진 자식 클래스 중 하나입니다.

캐릭터를 구성하는 전형적인 요소들이 표준화되어 있어, 일반적인 인간형 캐릭터를 만드는 데 최적화되어 있습니다.

자동차나 비행기처럼 완전히 다른 이동 방식을 구현할 때는 캐릭터 내부에 탑재된 기능들이 오히려 방해가 될 수 있습니다.  
이런 경우에는 폰을 직접 확장해서 사용하는 것을 고려해야 합니다.

### 캐릭터 클래스 구조

캐릭터 클래스를 C++로 생성하고, 블루프린트가 상속받도록 해 블루프린트 클래스에서 살펴보겠습니다.

블루프린트 클래스를 열고 왼쪽 컴포넌트 패널에서 컴포넌트 트리를 보면 여러가지 컴포넌트가 기본적으로 포함되어있는 것을 알 수 있습니다.

![Pawn_Character-Components]({{site.url}}/images/Unreal/ue5/2025-01-24-Pawn_Character/Pawn_Character-Components.PNG)

`캡슐 컴포넌트`는 캐릭터의 루트 컴포넌트로 캐릭터가 벽이나 지형에 충돌하는 범위를 정의하는 콜리전 컴포넌트로서 있습니다.  
캡슐 형태로, 반지름(Radius)과 절반 높이(Half Height)를 조정해 캐릭터의 물리적 크기를 설정할 수 있습니다.

`화살표 컴포넌트`는 캐릭터가 어느 방향을 바라보고 있는지를 표시하기 위해 씬에 화살표를 띄워주는 컴포넌트입니다.  
게임 플레이 로직에는 직접적인 영향을 주지 않고, 주로 편집기에서 시작적 디버깅용으로 사용됩니다.

`스켈레탈 메시 컴포넌트`는 캐릭터의 3D모델과 애니메이션을 적용하는 컴포넌트입니다.  
스켈레탈 메시와 애님 블루프린트 등을 여기로 할당해 캐릭터의 외형과 동작을 제어합니다.

`캐릭터 무브먼트 컴포넌트`는 캐릭터의 이동, 회전, 점프, 중력, 지형 따라가기, 네트워크 동기화 등 물리적 이동 로직을 담당하는 핵심 컴포넌트입니다.

### 카메라 세팅하기

헤더의 선언은 다음과 같습니다.

```cpp
#pragma once

#include "CoreMinimal.h"
#include "GameFramework/Character.h"
#include "SpartaCharacter.generated.h"

class USpringArmComponent; // 스프링 암 관련 클래스 전방 선언
class UCameraComponent; // 카메라 관련 클래스 전방 선언

UCLASS()
class SPARTAPROJECT_API ASpartaCharacter : public ACharacter
{
	GENERATED_BODY()

public:
	ASpartaCharacter();

protected:
	virtual void BeginPlay() override;

	virtual void SetupPlayerInputComponent(class UInputComponent* PlayerInputComponent) override;

protected:
	// 스프링 암 컴포넌트
	UPROPERTY(VisibleAnywhere, BlueprintReadOnly, Category = "Camera")
	USpringArmComponent* SpringArmComp;
	// 카메라 컴포넌트
	UPROPERTY(VisibleAnywhere, BlueprintReadOnly, Category = "Camera")
	UCameraComponent* CameraComp;
};
```

스프링 암(Spring Arm)과 카메라(Camera) 클래스를 전방 선언하고 프로퍼티를 추가합니다.

`VisibleAnywhere`, `BlueprintReadOnly` 지정자를 사용했는데, 블루프린트에서 보기만 가능하고, C++코드에서만 수정이 가능하도록 하는 속성입니다.

전방 선언(Forward Declaration)은 헤더 파일의 의존성을 줄이는 좋은 습관입니다.  
필요할 때만 `#include`를 추가하는 것이 좋습니다.

컴포넌트를 정의하기 위한 소스 코드는 다음과 같습니다.

```cpp
#include "SpartaCharacter.h"
// 카메라, 스프링 암 실제 구현이 필요한 경우라서 include
#include "Camera/CameraComponent.h"
#include "GameFramework/SpringArmComponent.h"

ASpartaCharacter::ASpartaCharacter()
{
	PrimaryActorTick.bCanEverTick = false;

	// 스프링 암 생성
	SpringArmComp = CreateDefaultSubobject<USpringArmComponent>(TEXT("SpringArm"));
	// 루트 컴포넌트에 스프링 암 부착
	SpringArmComp->SetupAttachment(RootComponent);
	// 캐릭터와 카메라 사이의 기본 거리값 300으로 설정
	SpringArmComp->TargetArmLength = 300.f;
	// 컨트롤러 회전에 따라 스프링 암도 회전하도록 설정
	SpringArmComp->bUsePawnControlRotation = true;

	// 카메라 생성
	CameraComp = CreateDefaultSubobject<UCameraComponent>(TEXT("Camera"));
	// 스프링 암의 소켓 위치에 카메라 부착
	CameraComp->SetupAttachment(SpringArmComp, USpringArmComponent::SocketName);
	// 카메라는 스프링 암의 회전을 따르므로 회전하지 않도록 설정
	CameraComp->bUsePawnControlRotation = false;
}
```

해당 코드를 작성하고 빌드를 마친 후 언리얼 에디터를 확인해보면 컴포넌트 트리에 스프링 암과 카메라 컴포넌트가 추가된 것을 확인할 수 있습니다.

![Pawn_Character-AddComponents]({{site.url}}/images/Unreal/ue5/2025-01-24-Pawn_Character/Pawn_Character-AddComponents.PNG)

### 확인해보기

이후 레벨에 적용된 게임모드의 디폴트폰 클래스를 변경한 후 에디터에서 실행하면 다음과 같이 직접 구현해본 캐릭터가 생성되는 것을 볼 수 있습니다.

![Pawn_Character-PlayTest]({{site.url}}/images/Unreal/ue5/2025-01-24-Pawn_Character/Pawn_Character-PlayTest.PNG)