---
layout: single

title: "[UE5] 왕복으로 이동하는 액터 구현"

categories:
    - UEPractice
tag: [UE5, UEPractice]

date: 2025-01-28
last_modified_at: 2025-01-28

order : 100000
---

# 이동 액터

시작 위치를 기준으로 왕복운동하는 액터를 구현해보겠습니다.

## 컴포넌트 추가

액터는 기본적으로 컴포넌트를 가지지 않습니다.  
그러므로 헤더에 씬 컴포넌트와 스태틱 메시 컴포넌트를 추가해줍니다.

```cpp
// 기본 컴포넌트
UPROPERTY(VisibleAnywhere, BlueprintReadOnly, Category = "Components")
TObjectPtr<USceneComponent> SceneRoot;
UPROPERTY(VisibleAnywhere, BlueprintReadOnly, Category = "Components")
TObjectPtr<UStaticMeshComponent> StaticMeshComp;
```

그 후 생성자에서 컴포넌트를 생성합니다.

```cpp
AMovingActor::AMovingActor()
{
	PrimaryActorTick.bCanEverTick = true;

	// 기본 컴포넌트 생성
	SceneRoot = CreateDefaultSubobject<USceneComponent>(TEXT("SceneRoot"));
	RootComponent = SceneRoot;

	StaticMeshComp = CreateDefaultSubobject<UStaticMeshComponent>(TEXT("StaticMesh"));
	StaticMeshComp->SetupAttachment(SceneRoot);
}
```

## 기능 구현

시작 위치와 움직이려는 방향을 저장하는 변수를 생성합니다.

```cpp
// 시작 위치
FVector StartLocation;
// 이동할 방향
FVector Direction;
```

게임 시작 시 시작 위치와 이동할 방향을 설정합니다.

```cpp
void AMovingActor::BeginPlay()
{
	Super::BeginPlay();

	StartLocation = GetActorLocation();
	Direction = FVector::One();
}
```

매 틱마다 액터를 이동 시킵니다.

`DeltaTime`으로 액터를 흐른 시간에 따라 이동시킵니다.

```cpp
void AMovingActor::Tick(float DeltaTime)
{
	Super::Tick(DeltaTime);

	// 현재 위치 기반으로 시간에 비례해 이동할 위치를 구함
	FVector NewLocation = GetActorLocation();
	NewLocation += MoveSpeed * DeltaTime * Direction;

	// 최대 이동 범위를 벗어난다면 이동할 방향을 반대로 설정
	if (NewLocation.X > StartLocation.X + MaxMoveRange.X || NewLocation.X < StartLocation.X - MaxMoveRange.X)
	{
		// 새로운 위치가 최대 범위를 넘지 않도록 값 재설정
		NewLocation.X = StartLocation.X + MaxMoveRange.X * Direction.X;

		Direction.X *= -1;
	}
	if (NewLocation.Y > StartLocation.Y + MaxMoveRange.Y || NewLocation.Y < StartLocation.Y - MaxMoveRange.Y)
	{
		// 새로운 위치가 최대 범위를 넘지 않도록 값 재설정
		NewLocation.Y = StartLocation.Y + MaxMoveRange.Y * Direction.Y;

		Direction.Y *= -1;
	}
	if (NewLocation.Z > StartLocation.Z + MaxMoveRange.Z || NewLocation.Z < StartLocation.Z - MaxMoveRange.Z)
	{
		// 새로운 위치가 최대 범위를 넘지 않도록 값 재설정
		NewLocation.Z = StartLocation.Z + MaxMoveRange.Z * Direction.Z;

		Direction.Z *= -1;
	}

	// 위치 값 변경
	SetActorLocation(NewLocation);
}
```

`X`, `Y`, `Z` 축으로 왕복 이동하는 코드입니다.