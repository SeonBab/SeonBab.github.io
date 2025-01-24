---
layout: single

title: "[UE5] 리플렉션"

categories:
    - UE5
tag: [Unreal Engine, UE5]

date: 2025-01-24
last_modified_at: 2025-01-24

order : 200010
---

# 리플렉션

리플렉션(Reflection)이란 C++클래스, 멤버 변수, 함수, 열거형, 구조체 등의 정보를 엔진 내부의 메타데이터 형태로 저장하고, 이를 에디터나 블루프린트에서 활용할 수 있게 만들어주는 기술입니다.  
언리얼 엔진의 블루프린트, 자동 시리얼라이제이션, 네트워크 리플리케이션, 에디터의 디테일 패널, 직렬화 및 저장 등 다양한 기능에서 활용됩니다.

Unreal Header Tool(UHT)이 C++ 코드를 분석하고 메타데이터와 여러 맞춤형 코드를 생성해줍니다.  
이 코드들을 통해 리플렉션을 사용할 수 있습니다.

C++클래스에 있는 여러 멤버(변수, 함수 등)를 리플렉션 해, 에디터와 블루프린트에서 직접 설정 및 호출이 가능하도록 합니다.

리플렉션을 통해 엔진은 객체가 다른 객체에 의해 참조되고 있는지 여부를 결정할 수 있습니다.  
해당 기능 덕분에 가비지 컬렉션을 실행 가능할 수 있게 합니다.

매개변수를 코드에서만 변경하는 것이 아니라, 에디터에서 바로 조정하여 반복 테스트를 빠르게 진행할 수 있습니다.

프로그래머가 만든 C++로직의 뼈대를 디자이너나 다른 팀원들이 에디터에서 직관적으로 조정할 수 있습니다.  
리플렉션 시스템을 제대로 이해하고 활용하면, 큰 프로젝트에서도 개발 효율과 협업 효과를 극대화할 수 있습니다.

리플렉션은 런타임에서 메타데이터를 조회하기 때문에 약간의 오버헤드가 발생합니다.  
반복 호출되거나 대규모 데이터에 적용할 경우 오버헤드를 고려해야 합니다.  
필요하지 않은 경우에는 리플렉션을 최소화하여 최적화를 유지하는 것이 좋습니다.

## C++ 클래스 리플렉션 등록

다음과 같은 클래스를 살펴보겠습니다.  
언리얼 에디터를 통해 클래스를 생성한다면 클래스 선언에 리플렉션 관련 매크로가 포함되어 생성됩니다.

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
	USceneComponent* SceneRoot;
	UStaticMeshComponent* StaticMeshComp;

	float RotationSpeed;

	virtual void BeginPlay() override;
	virtual void Tick(float DeltaTime) override;
};
```

`#include "Item.generated.h"`는 언리얼 엔진이 자동 생성하는 헤더 파일로 클래스의 리플렉션 및 엔진 통합에 필요한 코드가 들어있습니다.  
반드시 헤더 파일의 가장 마지막 `#include`구문 아래에 위치해야 합니다.  
다른 `#include`보다 아래에 위치하지 않으면 빌드 에러 발생 위험이 있습니다.

`UCLASS()`는 해당 클래스를 언리얼 엔진의 리플렉션 시스템에 등록한다는 의미입니다.  
이 매크로가 있어야만 블루프린트 등 에디터에서 이 클래스를 인식하고 사용할 수 있습니다.

`GENERATED_BODY()`는 언리얼의 코드 생성 도구가 사용하는 코드를 삽입하는 역할을 합니다.  
클래스 내부에 필요한 리플렉션 정보를 자동으로 생성해 줍니다.

### UCLASS 주요 지정자

`UCLASS()`는 리플렉션 시스템에 등록하면서, 추가적으로 몇 가지 옵션을 설정할 수 있습니다.  

다음과 같은 주요 지정자가 있습니다.

+ `Blueprintable`: 블루프린트에서 상속이 가능한 클래스로 만듭니다.
+ `NotBlueprintable`: 블루프린트에서 이 클래스를 상속할 수 없도록 합니다.
+ `BlueprintType`: 블루프린트에서 변수나 참조로 사용할 수 있게 합니다. 이 옵션이 있을 경우 상속은 허용되지 않습니다.
+ `Abstract` : 클래스를 추상 클래스로 만들어 액터로서 레벨에 배치할 수 없게 합니다.

만약 옵션을 지정하지 않은 상태라면, 블루프린트에서 상속이 가능하며 변수로 참조할 수 있는 형태로 등록됩니다.  
내부적으로 `Blueprintable`, `BlueprintType`과 동일한 효과를 가지게 됩니다.

### C++ 클래스를 상속 받는 블루프린트 클래스 생성

C++ 클래스로부터 블루프린트 클래스를 상속받아 생성하는 방법은 다음과 같습니다.

콘텐츠 브라우저에서 C++클래스인 `AItem` 클래스를 우클릭 > `Item` 기반 블루프린트 클래스 생성

이후 저장할 폴더를 지정하거나, 블루프린트의 이름을 정해주면 됩니다.

![Reflection-CppInheritance1]({{site.url}}/images/Unreal/ue5/2025-01-24-Reflection/Reflection-CppInheritance1.PNG)

콘텐츠 브라우저에서 컨텍스트 메뉴 > 블루프린트 클래스 선택 > `Item`클래스 검색 및 선택

이후 저장할 폴더를 지정하거나, 블루프린트의 이름을 정해주면 됩니다.

![Reflection-CppInheritance2.PNG]({{site.url}}/images/Unreal/ue5/2025-01-24-Reflection/Reflection-CppInheritance2.PNG)

## 변수에 리플렉션 등록

변수에 리플렉션을 등록하려면 `UPROPERTY()`매크로를 사용합니다.

멤버 변수에 리플렉션을 적용한 예시입니다.

```cpp
UPROPERTY(VisibleAnywhere, BlueprintReadOnly, Category="Item|Components")
USceneComponent* SceneRoot;	

UPROPERTY(EditAnywhere, BlueprintReadWrite, Category="Item|Components")
UStaticMeshComponent* StaticMeshComp;

UPROPERTY(EditDefaultsOnly, BlueprintReadOnly, Category="Item|Properties")
float RotationSpeed;
```

### UPROPERTY() 주요 지정자

`UPROPERTY()`에는 여러 지정자를 작성해, 에디터에서의 표시 여부나 블루프린트 접근성, 읽기/쓰기 권한 등을 자세하게 설정할 수 있습니다.  
아래는 자주 쓰이는 대표적인 지정자들입니다.

편집 가능 범위 지정자

+ `VisibleAnywhere`: 변수를 클래스 에디터 창의 값, 인스턴스 모두에서 읽기 전용으로만 표시하며, 수정이 불가능 하도록 합니다.
+ `VisibleDefaultsOnly`: 변수를 클래스 에디터 창에서 값이 읽기 전용으로만 표시되며, 수정이 불가능하도록 합니다.
+ `VisibleInstanceOnly`: 변수를 인스턴스의 디테일 패널에서 값이 읽기 전용으로만 표시되며, 수정이 불가능하도록 합니다.
+ `EditAnywhere`: 변수를 클래스 에디터 창의 값, 인스턴스의 디테일 패널 모두에서 수정 가능이 가능하도록 합니다.
+ `EditDefaultsOnly`: 변수를 클래스 에디터 창에서만 값의 수정 가능하도록 합니다.
+ `EditInstanceOnly`: 변수를 인스턴스의 디테일 패널에서만 값의 수정 가능하도록 합니다.

블루프린트 접근성 지정자

+ `BlueprintReadWrite`: 블루프린트 그래프에서 Getter/Setter로 값을 읽거나 쓸 수 있습니다.
+ `BlueprintReadOnly`: 블루프린트 그래프에서 Getter 핀만 노출되어, 읽기만 가능합니다.

Category 지정자

여러 변수를 비슷한 카테고리에 묶으면, 세부 정보 패널에서 깔끔하게 정리되어 보입니다.

+ `Category=""`: Details 패널에서 지정한 이름의 범주(폴더) 아래에 표시합니다.

메타 옵션 지정자

+ `meta=(ClampMin="0.0")`: 에디터에서 변수 입력 시 최소값을 제한할 수 있습니다.
+ `meta=(AllowPrivateAccess="true")`: 해당 멤버가 `private`로 선언되어 있어도, 에디터나 블루프린트에서 접근할 수 있도록 허용합니다.

만약 `UPROPERTY()`만 있고, 추가 지정자를 하나도 주지 않는다면 다음과 같습니다.

+ 엔진 리플렉션 시스템에는 등록되지만, 에디터나 블루프린트에 노출되지 않습니다.
+ 엔진이 변수의 존재는 알고 있지만, 외부에서는 보이지 않게 숨겨둔 상태입니다.
+ 리플렉션에 등록만 되어 있어도 가비지 컬렉션(메모리 관리)과 직렬화(세이브/로드) 같은 엔진 내부 기능이 작동할 수 있습니다.

### 에디터에서 C++ 변수 조정

블루프린트 클래스를 통해 해당 변수들을 에디터 내에서 조정할 수 있게 됩니다.

![Reflection-Variable1]({{site.url}}/images/Unreal/ue5/2025-01-24-Reflection/Reflection-Variable1.PNG)

블루프린트 클래스 인스턴스들의 디테일 패널을 통해 해당 변수들을 조정할 수 있게 됩니다.

![Reflection-Variable2]({{site.url}}/images/Unreal/ue5/2025-01-24-Reflection/Reflection-Variable2.PNG)

## 함수에 리플렉션 등록

함수 또한 블루프린트에서 직접 호출할 수 있도록 등록할 수 있습니다.

복잡한 C++로직을 블루프린트에서 간단히 노드로 불러와 제어할 수 있으므로 작업 효율이 높아집니다.

함수에 리플렉션을 등록하려면 `UFUNCTION()`매크로를 사용합니다.

멤버 함수에 리플렉션을 적용한 예시입니다.

```cpp
// 함수를 블루프린트에서 호출 가능하도록 설정
UFUNCTION(BlueprintCallable, Category="Item|Actions")
void ResetActorPosition();

// 블루프린트에서 값만 반환하도록 설정
UFUNCTION(BlueprintPure, Category = "Item|Properties")
float GetRotationSpeed() const;

// C++에서 호출되지만 구현은 블루프린트에서 수행
UFUNCTION(BlueprintImplementableEvent, Category = "Item|Event")
void OnItemPickedUp();
```

### UFUNCTION() 주요 지정자

`UFUNCTION`에는 여러 지정자를 작성해, 블루프린트 그래프의 호출 여부나 구현을 블루프린트에서 하도록 설정할 수 있습니다.

아래는 자주 쓰이는 대표적인 지정자들입니다.

블루프린트 관련 지정자

+ `BlueprintCallable`: 블루프린트 이벤트 그래프(노드)에서 호출(Execute) 가능한 함수로 만듭니다.
+ `BlueprintPure`: Getter 역할만 수행합니다. (Exec 핀 없이 Return Value만 노출)
+ `BlueprintImplementableEvent`: 함수의 선언만 C++에 있고, 구현은 블루프린트**에서 하도록 합니다.
    - C++ 코드에서는 함수 이름만 정의하고, 실제 동작은 블루프린트 이벤트 그래프 안에서 이벤트 노드처럼 구현됩니다.
    
만약 `UFUNCTION()`만 있고, 추가 지정자를 하나도 주지 않는다면 다음과 같습니다.

+ `UPROPERTY()`와 마찬가지로, 함수가 언리얼 리플렉션에 등록되긴 하지만, 블루프린트에 노출되지 않습니다.
+ 엔진이 함수의 존재는 알고 있지만, 블루프린트에서 직접 호출할 수 없게 숨겨둔 상태입니다.

### 블루프린트에서 함수 노드 사용

블루프린트의 이벤트 그래프에서 우클릭해 컨텍스트 메뉴를 열고, 함수 이름을 검색하면 아래와 같은 노드를 얻을 수 있습니다.

![Reflection-FunctionNodes]({{site.url}}/images/Unreal/ue5/2025-01-24-Reflection/Reflection-FunctionNodes.PNG)

+ `ResetActorPosition`: `BlueprintCallable`로 선언했으므로, 노드로 실행(Exec)할 수 있습니다.
+ `GetRotationSpeed`: `BlueprintPure`로 선언했으므로, 단순히 값만 반환하는 Getter 노드로 사용됩니다.
+ `OnItemPickedUp`: `BlueprintImplementableEvent`로 선언했으므로, 이벤트 그래프 안에서 구현한 내용을 C++에서 `OnItemPickedUp()`를 호출함으로써 실행할 수 있습니다.

블루프린트에서 이벤트처럼 구현한 `OnItemPickedUp()`은 C++에선 함수 이름만 존재하고 실제 코드는 없습니다.  
대신, Blueprint에서 이벤트 그래프를 통해 시각적 로직으로 구현해둔 후, C++에서 `OnItemPickedUp()`를 부르는 순간 그 이벤트가 실행됩니다.

# 참고

[언리얼 엔진 리플렉션 시스템](https://dev.epicgames.com/documentation/ko-kr/unreal-engine/reflection-system-in-unreal-engine)