---
layout: single

title: "[UE5] 이동 액터"

categories:
    - UE5Dev
tag: [UE5, UE5Dev]

date: 2025-01-28
last_modified_at: 2025-01-28

order : 100000
---

# 이동 액터

헤더

```cpp
#pragma once

#include "CoreMinimal.h"
#include "BaseActor.h"
#include "MovingActor.generated.h"

UCLASS()
class SPARTAPUZZLE_API AMovingActor : public ABaseActor
{
	GENERATED_BODY()

protected:
	virtual void BeginPlay() override;

	virtual void Tick(float DeltaTime) override;
	
private:
	// 이동 함수
	void Move(float DeltaTime);

private:
	// 기본 컴포넌트
	UPROPERTY(VisibleAnywhere, BlueprintReadOnly, Category = "Components")
	TObjectPtr<USceneComponent> SceneRoot;
	UPROPERTY(VisibleAnywhere, BlueprintReadOnly, Category = "Components")
	TObjectPtr<UStaticMeshComponent> StaticMeshComp;

	// 시작 위치
	FVector StartLocation;
	// 이동할 방향
	FVector Direction;
};
```

소스 코드

```cpp
#include "MovingActor.h"

AMovingActor::AMovingActor()
{
	PrimaryActorTick.bCanEverTick = true;

	// 기본 컴포넌트 생성
	SceneRoot = CreateDefaultSubobject<USceneComponent>(TEXT("SceneRoot"));
	RootComponent = SceneRoot;

	StaticMeshComp = CreateDefaultSubobject<UStaticMeshComponent>(TEXT("StaticMesh"));
	StaticMeshComp->SetupAttachment(SceneRoot);
}

void AMovingActor::BeginPlay()
{
	Super::BeginPlay();

	StartLocation = GetActorLocation();
	Direction = FVector::One();
}

void AMovingActor::Tick(float DeltaTime)
{
	Super::Tick(DeltaTime);

	Move(DeltaTime);
}

void AMovingActor::Move(float DeltaTime)
{
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