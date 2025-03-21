---
layout: single

title: "[UE5] FastArraySerializer"

categories:
    - UE5
tag: [Unreal Engine, UE5]

date: 2025-03-21
last_modified_at: 2025-03-21

order : 500100
---

# FastArraySerializer

`FastArraySerializer`은 빠른 복제(Replication) 및 동기화(Synchronization)를 위해 제공하는 특수한 배열 직렬화 시스템입니다.  
특히 네트워크 멀티플레이어 환경에서 자주 변경되는 배열 데이터를 효율적으로 동기화 할 수 있도록 설계되었습니다.

블루프린트에서 사용할 수 없고 C++에서만 사용 가능한 자료구조입니다.

일반적인 `TArray`를 복제하려면 배열 전체를 전송해야 하지만, 전체 배열에서 변경된 데이터만 전송 시킬 수 있게 해줍니다.  
즉, 큰 `Array`를 `Replication`할 때의 CPU 부하와 `Bandwidth`를 크게 줄일 수 있습니다.

네트워크 트래픽을 비교적 적게 사용합니다.

모듈("NetCore")을 추가하고, 상속 받아서 사용할 수 있습니다.  
각 배열 요소(아이템)는 `FFastArraySerializerItem`을 상속해야 하며, 이를 포함하는 배열 구조체는 `FFastArraySerializer`를 상속해야 합니다.

이렇게 구성을 사용하면 엔진이 추가/제거/수정 사항을 추적하고, `delta`형태로만 전송됩니다.

복제를 수행할 때, 특정 이벤트가 발생하면 콜백 함수가 호출됩니다.  
콜백 함수는 다음과 같습니다.

```cpp
// 배열에서 아이템이 삭제되기 직전에 호출됩니다.
// 삭제된 아이템을 정리하거나 UI를 업데이트하는 용도로 사용됩니다.
void PreReplicatedRemove(const struct FInventoryArray& InArraySerializer);
// 배열에 아이템이 추가된 직후에 호출됩니다.
// 클라이언트에서 새로운 아이템이 추가되었을 때 알림을 주는 역할을 합니다.
void PostReplicatedAdd(const struct FInventoryArray& InArraySerializer);
// 배열에 있는 아이템이 변경된 후 호출됩니다.
// 예를 들어, 아이템의 개수가 변했거나 속성이 수정된 경우 이 함수가 실행됩니다.
void PostReplicatedChange(const struct FInventoryArray& InArraySerializer);
```

콜백 함수에서는 배열을 직접 수정하면 안됩니다.

+ 장점
    - 변경된 항목만 네트워크로 전송하므로 대역폭 절약 가능
    - 별도의 변경 감지 로직을 구현할 필요 없음

+ 단점
    - 클라이언트는 데이터를 직접 변경할 수 없으며, 반드시 서버에서 변경 후 복제해야 합니다.

인벤토리 목록이나 플레이어 명단같이 아이템이 들어갔다 나왔다 하거나 일부만 변하는 경우에 유용합니다.

## 예시

<details>
<summary><h5 style="display: inline;">FFastArraySerializerItem - InventoryItem.h</h5></summary>
<div markdown="1">

```cpp
#pragma once

#include "CoreMinimal.h"
#include "Net/Serialization/FastArraySerializer.h"
#include "InventoryItem.generated.h"

struct FInventoryArray;

USTRUCT(BlueprintType)
struct FInventoryItem : public FFastArraySerializerItem
{
    GENERATED_USTRUCT_BODY()

    // Your data:
    UPROPERTY()
    int32   ExampleIntProperty;

    UPROPERTY()
    float   ExampleFloatProperty;

    void PreReplicatedRemove(const struct FInventoryArray& InArraySerializer);
    void PostReplicatedAdd(const struct FInventoryArray& InArraySerializer);
    void PostReplicatedChange(const struct FInventoryArray& InArraySerializer);

    // Optional: debug string used with LogNetFastTArray logging
    FString GetDebugString();
};
```

</div>
</details>

<details>
<summary><h5 style="display: inline;">FFastArraySerializerItem - InventoryItem.cpp</h5></summary>
<div markdown="1">

```cpp
#include "InventoryItem.h"
#include "InventoryArray.h"

void FInventoryItem::PreReplicatedRemove(const FInventoryArray& InArraySerializer)
{
    UE_LOG(LogTemp, Warning, TEXT("Item Removed!"));
}

void FInventoryItem::PostReplicatedAdd(const FInventoryArray& InArraySerializer)
{
    UE_LOG(LogTemp, Warning, TEXT("New Item Added!"));
}

void FInventoryItem::PostReplicatedChange(const FInventoryArray& InArraySerializer)
{
    UE_LOG(LogTemp, Warning, TEXT("Item Updated!"));
}
```

</div>
</details>

<details>
<summary><h5 style="display: inline;">FFastArraySerializer</h5></summary>
<div markdown="1">

```cpp
#pragma once

#include "CoreMinimal.h"
#include "Net/Serialization/FastArraySerializer.h"
#include "InventoryItem.h"
#include "InventoryArray.generated.h"

// #include했으므로, 전방선언 해줄 필요는 없음
struct FInventoryItem;

USTRUCT(BlueprintType)
struct FInventoryArray : public FFastArraySerializer
{
	GENERATED_USTRUCT_BODY()

	UPROPERTY()
	TArray<FInventoryItem>	Items;	// FFastArraySerializerItem 구조체의 TArray 배열을 반드시 포함해야 합니다.

	bool NetDeltaSerialize(FNetDeltaSerializeInfo& DeltaParms)
	{
		return FFastArraySerializer::FastArrayDeltaSerialize<FInventoryItem, FInventoryArray>(Items, DeltaParms, *this);
	}
};
```

</div>
</details>