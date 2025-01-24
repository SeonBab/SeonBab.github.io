---
layout: single

title: "[UE C++] 언리얼 엔진 트랜스폼 변경"

categories:
    - UECpp
tag: [Unreal Engine, UE5, UECpp]

date: 2025-01-23
last_modified_at: 2025-01-24

order : 70
---

# 트랜스폼 변경

트랜스폼(Transform)에 대한 기본적인 정보는 다른 글에 정리해 두었습니다.  
[언리얼 엔진 트랜스폼]({{ "/ue5/Transform/" | relative_url }}){: target="_blank"}

C++에서 트랜스폼은 효율적으로 관리하기 위해 위치, 회전, 스케일을 하나로 묶어 구조체로 만든 `FTransform`이 있습니다.

`FTransform`은 내부적으로 세 요소를 보관합니다.  
+ Translation: 위치를 표현하는 `FVector`
+ Rotation: 회전을 표현하는 `FRotator`
+ Scale3D: 스케일을 표현하는 `FVector`

회전의 경우 `FQuat`를 사용할 수 도 있습니다.

## 트랜스폼 관련 함수

다음 함수들은 월드 기준의 액터의 트랜스폼의 값을 변경하거나 가져올 수 있는 대표적인 함수입니다.

+ `SetActorLocation(FVector NewLocation)`
+ `SetActorRotation(FRotator NewRotation)`
+ `SetActorScale3D(FVector NewScale)`
+ `SetActorTransform(FTransform NewTransform)`

`Set`함수 이외에 `Add`함수도 있습니다.

+ `AddActorWorldOffset(FVector DeltaLocation)`
+ `AddActorWorldRotation(FRotator DeltaRotation)`
+ `AddActorWorldRotation(const FQuat& DeltaRotation)`
+ `AddActorWorldTransform(const FTransform& DeltaTransform)`

각 함수에서 매개변수로 위치, 회전, 스케일 값을 변경합니다.

+ `GetTransform()`
+ `GetActorLocation()`, `GetActorRotation()`, `GetActorScale3D()`

각 함수에서 월드를 기준으로 현재 위치, 회전, 스케일의 트랜스폼 정보 정보를 가져옵니다.

---

부모 기준의 상대 위치, 회전, 스케일의 값을 수정하거나 가져오는 함수도 있습니다.  

+ `SetRelativeLocation(FVector NewLocation)`
+ `SetRelativeRotation(FRotator NewRotation)`
+ `SetActorRelativeScale3D(FVector NewRelativeScale)`

부모 기준의 트랜스폼에서도 `Set`함수 이외에 `Add`함수도 있습니다.

+ `AddActorLocalOffset(FVector DeltaLocation)`
+ `AddActorLocalRotation(FRotator DeltaRotation)`
+ `AddActorLocalRotation(const FQuat& DeltaRotation)`
+ `AddActorLocalTransform(const FTransform& NewTransform)`

`Set`함수의 경우 월드 기준으로 이동하는 함수들에 `Relative`라는 단어가 추가된 함수로 존재합니다.  
`Add`함수의 경우 월드 기준으로 이동하는 함수들에 `Local`이라는 단어로 변경된 함수로 존재합니다.

이외에도 트랜스폼의 값을 수정하거나 가져오는 함수가 많습니다.

## 함수 사용

트랜스폼의 값을 변경하는 함수를 사용해보며 예시를 들어보겠습니다.  

게임이 시작했을 때 액터의 위치가 이동되도록 `BeginPlay()`함수에서 구현해보겠습니다.

```cpp
void AItem::BeginPlay()
{
    Super::BeginPlay();
        
    // 위치, 회전, 스케일 설정하기
    // (300, 200, 100) 위치로 이동
    SetActorLocation(FVector(300.0f, 200.0f, 100.0f));
    // Yaw 방향으로 45도 회전
    SetActorRotation(FRotator(0.0f, 45.0f, 0.0f));
    // 모든 축을 2배로 스케일
    SetActorScale3D(FVector(2.0f));
}
```

`SetActorScale3D(FVector(2.0f))`와 같이 스케일 값을 통일하면, 모든 축 (x, y, z)이 같은 비율로 확대 및 축소됩니다.

시작하기 전과 후의 트랜스폼 값이 달라진 것을 알 수 있습니다.

![ModifyTransform-Begin]({{site.url}}/images/Unreal/uecpp/2025-01-23-ModifyTransform/ModifyTransform-Begin.PNG)

다음은 `Tick`함수를 통해 매 프레임마다 이동, 회전, 스케일 값을 변경하도록 구현해보겠습니다.

```cpp
void AItem::Tick(float DeltaTime)
{
	Super::Tick(DeltaTime);

	// 기본 이동 속도 (1초에 200유닛 이동)
	float MoveSpeedZ = 200.f;

	// 초당 MoveSpeedZ만큼, 한 프레임당 (MoveSpeedZ * DeltaTime)만큼 Z축으로 이동
	FVector NewLocation = GetActorLocation();
	NewLocation.Z += MoveSpeedZ * DeltaTime;

	// 위치 값 변경
	SetActorLocation(NewLocation);

	// 기본 회전 속도 (1초에 90도 회전)
	float RotationSpeed = 90.f;

	// 초당 RotationSpeed만큼, 한 프레임당 (RotationSpeed * DeltaTime)만큼 회전
	AddActorLocalRotation(FRotator(0.f, RotationSpeed * DeltaTime, 0.f));

	// 시작 기준이 되는 크기
	float BaseScale = 1.f;
	// 변경되는 스케일 값 ( -1 ~ 1 * 0.5f)
	float ScaleRange = 0.5f;
	// 스케일 변경 속도 (2초 마다)
	float ScaleSpeed = 0.5f;
	// 게임이 시작된 후의 총 시간
	float Time = GetWorld()->GetTimeSeconds();

	// 초당 ScaleSpeed만큼, 한 프레임당 ScaleRange * FMath::Sin(PI * 2 * Time * ScaleSpeed)만큼 스케일 변경
	FVector NewScale = FVector(BaseScale + ScaleRange * FMath::Sin(PI * 2 * Time * ScaleSpeed));

	SetActorScale3D(NewScale);
}
```

해당 `Tick`함수의 코드가 작동되며 트랜스폼 값이 달라진 것을 알 수 있습니다.

![ModifyTransform-Tick]({{site.url}}/images/Unreal/uecpp/2025-01-23-ModifyTransform/ModifyTransform-Tick.PNG)