---
layout: single

title: "[UE5] 컴포넌트 멤버 변수 추가 및 초기화"

categories:
    - UE5
tag: [Unreal Engine, UE5]

date: 2025-01-22
last_modified_at: 2025-01-24

order : 200060
---

# 컴포넌트란

해당 글에서 `AActor`를 상속받은 클래스에 컴포넌트를 추가하고, 초기화하는 예시를 작성해보겠습니다.

모든 액터는 최상위 컴포넌트인 루트 컴포넌트(Root Component)를 가져야 합니다.

루트 컴포넌트는 액터의 트랜스폼을 정의하며, 모든 하위 컴포넌트가 이를 기준으로 동작합니다.

일반적으로 `Scene Component`를 루트로 설정하여 액터의 트랜스폼을 관리하고, 이에 속하도록 다양한 컴포넌트를 계층적으로 붙입니다.

컴포넌트에 대한 더 자세한 설명은 다른 게시글에 정리해 두었습니다.

[언리얼 엔진 컴포넌트]({{ "/ue5/Component/" | relative_url }}){: target="_blank"}

# 컴포넌트 멤버 변수 추가 및 초기화

그럼 이 `Scene Component`와 `Static Mesh Component`를 추가해보도록 하겠습니다.

`Item.h`에서 다음과 같이 포인터 멤버 변수를 추가합니다.

```cpp
#pragma once

#include "CoreMinimal.h"
#include "GameFramework/Actor.h"
#include "Item.generated.h"

UCLASS()
class SPARTAPROJECT_API AItem : public AActor
{
	GENERATED_BODY()
	
public:	
	AItem();

protected:
	// 루트 컴포넌트를 나타내는 Scene Component 포인터
	USceneComponent* SceneRoot;
	// Static Mesh Component 포인터
	UStaticMeshComponent* StaticMeshComp;
};
```

생성자에서 컴포넌트를 생성하고 포인터에 값을 저장해줍니다.

```cpp
AItem::AItem()
{
	// Scene Component를 생성하고 루트로 설정
	SceneRoot = CreateDefaultSubobject<USceneComponent>(TEXT("SceneRoot"));
	SetRootComponent(SceneRoot);

	// Static Mesh Component를 생성하고 Scene Component에 Attach
	StaticMeshComp = CreateDefaultSubobject<UStaticMeshComponent>(TEXT("StaticMesh"));
	StaticMeshComp->SetupAttachment(SceneRoot);
}
```

`USceneComponent* SceneRoot`는 눈에 보이지 않는 논리적 컴포넌트로, 실제 3D 모델이나 시각적 요소를 가지지 않습니다.

`UStaticMeshComponent* StaticMeshComp`는 정적 메시(Static Mesh)를 렌더링하는 역할을 합니다.

`CreateDefaultSubobject<T>(TEXT(""))`이 함수는 컴포넌트를 생성하고, 초기화할 때 사용하는 함수입니다.  
`<T>`는 템플릿 타입을 의미하며, 생성할 컴포넌트의 유형을 지정합니다.  
`"SceneRoot"`와 `"StaticMesh"`는 각 컴포넌트의 식별 이름이 됩니다.  
`TEXT()`매크로는 문자열을 유니코드로 처리하기 위한 것으로, 언리얼 엔진 코드 표준에서 권장합니다.

`SetRootComponent(SceneRoot)`는 루트 컴포넌트를 `SceneRoot`로 설정한다는 의미입니다.

`StaticMeshComp->SetupAttachment(SceneRoot)`는 `StaticMeshComp`를 `SceneRoot`에 부착(Attach)하며, `SceneRoot`의 하위 컴포넌트로서 동작해 루트 컴포넌트의 트랜스폼을 기준으로 동작하게 됩니다.

여기까지 작업을 해보고, 해당 클래스를 레벨 뷰포트에 배치해보면 디테일 창에서 컴포넌트가 보이는 것을 알 수 있습니다.  

![ActorClassAddComponent-Detail]({{site.url}}/images/Unreal/ue5/2025-01-22-ActorClassAddComponent/ActorClassAddComponent-Detail.PNG)

하지만 스태틱 매시 컴포넌트가 보이지 않는데, 이는 에디터 상에 컴포넌트 들이 노출되도록 리플렉션 시스템에 등록하지 않았기 때문입니다.  
그런데 루트 컴포넌트는 설정하지 않았는데도 컴포넌트가 보이는데, 이는 루트 컴포넌트일 경우 설정하지 않아도 기본적으로 리플렉션 시스템에 등록이 되기 때문입니다.

## 메시 및 머티리얼 할당

스태틱 메시 컴포넌트의 스태틱 메시와 머티리얼을 설정해보도록 하겠습니다.

```cpp
AItem::AItem()
{
	// Scene Component를 생성하고 루트로 설정
	SceneRoot = CreateDefaultSubobject<USceneComponent>(TEXT("SceneRoot"));
	SetRootComponent(SceneRoot);

	// Static Mesh Component를 생성하고 Scene Component에 Attach
	StaticMeshComp = CreateDefaultSubobject<UStaticMeshComponent>(TEXT("StaticMesh"));
	StaticMeshComp->SetupAttachment(SceneRoot);
	
	// Static Mesh를 코드에서 설정
	static ConstructorHelpers::FObjectFinder<UStaticMesh> MeshAsset(TEXT("/Game/Resources/Props/SM_Chair.SM_Chair"));
	if (MeshAsset.Succeeded())
	{
		StaticMeshComp->SetStaticMesh(MeshAsset.Object);
	}

	// Material을 코드에서 설정
	static ConstructorHelpers::FObjectFinder<UMaterial> MaterialAsset(TEXT("/Game/Resources/Materials/M_Metal_Gold.M_Metal_Gold"));
	if (MaterialAsset.Succeeded())
	{
		StaticMeshComp->SetMaterial(0, MaterialAsset.Object);
	}
}
```

`ConstructorHelpers::FObjectFinder<T>`은 특정 리소스를 경로 기반으로 로드하는 클래스입니다.

`TEXT("/Game/Resources/Props/SM_Chair.SM_Chair")`는 리소스의 경로를 나타내는데, 리소스의 경로를 가져오기 위해서는 에셋을 우클릭 하고, 레퍼런스 복사를 클릭하면 경로를 얻을 수 있습니다.  
단 경로는 `/Game`부터 시작하면 되므로 앞 부분의 불필요한 경로는 삭제해 줍니다.  
`/Game`은 프로젝트의 `Content`폴더를 나타냅니다.

![ActorClassAddComponent-CopyReference]({{site.url}}/images/Unreal/ue5/2025-01-22-ActorClassAddComponent/ActorClassAddComponent-CopyReference.PNG)

조건문의 `.Succeeded()`는 지정된 경로에서 리소스를 성공적으로 찾았는지 값을 반환해줍니다.  
경로가 잘못되었거나 리소스 파일이 누락된 경우 실패하며, 이후 설정 함수가 호출되지 않도록 해줍니다.

`SetStaticMesh()` 함수와, `SetMaterial()`는 에셋을 설정해주는 함수입니다.  
`SetStaticMesh()`는 로드된 메시를 세팅하게 됩니다.  
`SetMaterial()`는 특정 머티리얼 슬롯에 머티리얼을 적용하며, 여기서는 0 번째 슬롯에 머티리얼이 설정됩니다.

이렇게 설정을 마치면 다음과 같이 할당이 잘 된것을 확인해 볼 수 있습니다.

![ActorClassAddComponent-Viewport]({{site.url}}/images/Unreal/ue5/2025-01-22-ActorClassAddComponent/ActorClassAddComponent-Viewport.PNG)

리플렉션을 설정하지 않아 디테일 창에서 컴포넌트가 보이지 않고, 에디터 상에서 할당이 불가능 하지만 기본적으로 컴포넌트를 추가하는 방법에 대한 설명이었습니다.