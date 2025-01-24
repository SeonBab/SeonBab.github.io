---
layout: single

title: "[UE5] 콜리전과 충돌"

categories:
    - UE5
tag: [Unreal Engine, UE5]

date: 2024-06-20
last_modified_at: 2024-06-20

order : 110
---

# 콜리전

콜리전 반응(Collision)및 트레이스 반응(Trace Responses)은 충돌및 레이 캐스팅의 실시간 처리를 위한 바탕을 이룹니다.  
충돌 가능한 모든 오브젝트는 오브젝트 유형(Object Type)과 일련의 반응들을 통해 다른 모든 오브젝트 유형과 어떻게 상호작용할 수 있을지 정의합니다.  
콜리전(충돌) 또는 오버랩(겹침) 이벤트가 발생하면, 그에 관련된 오브젝트는 서로 막을지, 겹칠지, 무시할지 영향을 주고받도록 설정할 수 있습니다.

트레이스 반응 작동방식도 기본적으로 같으나 트레이스(레이 캐스트) 자체를 하나의 트레이스 반응 유형으로 설정해, 액터가 그 트레이스 반응에 따라 트레이스를 막거나 무시하도록 할 수 있습니다.

## 블록

콜리전이 블록(Block)으로 설정되어있는 경우 서로 겹치거나 뚫고 지나가지 못하게 됩니다.

예시로 언리얼에서 제공하는 삼인칭 템플릿의 게임을 시작해보면 캐릭터가 바닥을 뚫지않고 바닥 위에 서있는 것을 볼 수 있습니다.

![Collision-CharacterontheFloor]({{site.url}}/images/Unreal/ue5/2024-06-20-Collision/Collision-CharacterontheFloor.PNG)

플레이어 캐릭터와 바닥에 있는 스태틱메시 액터의 콜리전을 각각 살펴보겠습니다.

![Collision-FloorCollision]({{site.url}}/images/Unreal/ue5/2024-06-20-Collision/Collision-FloorCollision.PNG)

우선 바닥에 있는 스태틱 메시 액터의 콜리전입니다.

`BlockAll`이라는 콜리전 프리셋이며, 오브젝트 타입은 `WorldStatic`입니다.  
모든 타입에 대해서 블록 처리를 하게 설정이 되있는 것을 알 수 있습니다.

![Collision-CharacterCollision]({{site.url}}/images/Unreal/ue5/2024-06-20-Collision/Collision-CharacterCollision.PNG)

플레이어 캐릭터의 콜리전은 `Mesh`와 `CapsuleComponent` 두 개가 있는 것을 볼 수 있습니다.  
두 개가 각각 콜리전을 가지는 것인데, `Mesh`의 콜리전 프리셋은 `CharacterMesh`이고, 오브젝트 타입은`Pawn`입니다.  `CapsuleComponent`의 콜리전 프리셋은 `Pawn`이고, 오브젝트 타입도`Pawn`입니다.

`WorldStatic`에 대해서는 모두 블록 처리를 하게 돼있으므로 두 콜리전 모두 바닥에 있는 스태틱메시 액터와 겹쳐지거나 뚫리지 않는다는 것을 알 수 있습니다.

### 히트 이벤트 생성

히트 이벤트란 무언가와 충돌한다면 이 사실을 보고해 블루프린트나 코드가 발동되게 할 수 있는 이벤트를 말합니다.

충돌 사실을 보고하도록 하기 위해서는 서로 블록(block)으로 설정되어 있어야 합니다.  
그리고 시뮬레이션 히트 이벤트 생성(Simulation Generates Hit Events)옵션을 활성화해야 무언가와 충돌 할 때마다 자체적으로 이벤트를 발동시킬 수 있습니다.

![Collision-EventHit]({{site.url}}/images/Unreal/ue5/2024-06-20-Collision/Collision-EventHit.PNG)

서로 블록으로 반응하지 않는다면 히트 이벤트는 발동되지 않습니다.

## 오버랩, 무시

콜리전이 오버랩(Overlap)혹은 무시(Ignore)로 설정되어있는 경우 블록과 다르게 겹쳐있거나 뚫고 지나갈 수 있게 됩니다.

예시로 액터를 하나 만들고 플레이어 캐릭터와 오버랩을 설정해보면 캐릭터가 해당 액터와 겹쳐지고 뚫고 지나갈 수 있습니다.

![Collision-ActorandCharacterOverlap]({{site.url}}/images/Unreal/ue5/2024-06-20-Collision/Collision-ActorandCharacterOverlap.PNG)

플레이어 캐릭터와 만들어낸 액터의 콜리전을 각각 살펴보겠습니다.

![Collision-ActorCollision]({{site.url}}/images/Unreal/ue5/2024-06-20-Collision/Collision-ActorCollision.PNG)

액터는 `StaticMesh` 컴포넌트 한개만을 가지고 있어 콜리전은 `StaticMesh`에서만 가지고있는 것을 알 수 있습니다.
`StaticMesh`의 콜리전 프리셋은 `OverlapAll`이고, 오브젝트 타입은`WorldStatic`입니다.  

콜리전 프리셋에서 모든 콜리전 반응을 오버랩으로 반응하게 설정되어있다는 것을 알 수 있습니다.

![Collision-CharacterCollision]({{site.url}}/images/Unreal/ue5/2024-06-20-Collision/Collision-CharacterCollision.PNG)

플레이어 캐릭터의 콜리전은 변경하지 않았으므로 블록에서 알아보았을 때와 같습니다.  
그러므로 분명 `WorldStatic`은 블록 처리로 돼있어 만든 액터와 겹쳐지거나 뚫을 수 없어야 하겠지만, 콜리전 반응이 한 쪽이라도 블록으로 되어있지 않다면 지금의 경우와 같이 겹쳐지거나 뚫고 지나갈 수 있다는 것을 알 수 있습니다.  
이때 플레이어 캐릭터에서 히트 이벤트를 발동되게 했다고 하더라도 충돌이 되지 않아 히트 이벤트가 발동 할 수 없습니다.

### 오버랩 이벤트 생성

오버랩 이벤트란 무언가와 겹치면 이 사실을 보고해 블루프린트나 코드가 발동되게 할 수 있는 이벤트를 말합니다.

오버랩 사실을 보고하도록 하기 위해서는 서로 오버랩(Overlap)으로 설정되어 있어야 합니다.  
그리고 오버랩 이벤트 생성(Generate Overlap Events)옵션을 활성화해야 무언가와 겹침이 발생할 때마다 자체적으로 이벤트를 발동시킬 수 있습니다.

히트 이벤트에서는 충돌을 수신 할 액터에서만 이벤트 생성을 켜면 됐지만 오버랩은 퍼포먼스를 위해 겹침을 확인 할 액터들은 모두 옵션을 활성화해야 합니다.

![Collision-EventOverlap]({{site.url}}/images/Unreal/ue5/2024-06-20-Collision/Collision-EventOverlap.PNG)

오버랩 이벤트는 히트 이벤트와 다르게 겹침이 시작 될 때와 겹침이 끝날 때의 발동이 나뉘어 있어 각각 경우에 발동합니다.

오버랩 이벤트 생성이 켜져있지 않다면 사실상 콜리전 반응의 무시와 같다고 볼 수 있습니다.

## 오브젝트 채널 추가

언리얼에는 기본적으로 오브젝트 반응 채널(Object Response Channel) 6개와 트레이스 반응 채널(Trace Response Channel) 2개가 존재합니다.  
이 채널들로 만들고자하는 것을 만들기에 부족 할 수 있는데, 이를 추가 할 수 있습니다.

메뉴의 편집 >> 프로젝트 세팅 >> 콜리전 으로 접근시 반응 채널들을 추가 할 수 있습니다.

![Collision-ProjectSettingCollision]({{site.url}}/images/Unreal/ue5/2024-06-20-Collision/Collision-ProjectSettingCollision.PNG)

버튼을 클릭하면 창이 열리는데, 채널의 이름을 적은 후 기본 반응(Default Response)을 선택한 뒤 수락을 클릭하면 됩니다.

![Collision-NewObjectChannel]({{site.url}}/images/Unreal/ue5/2024-06-20-Collision/Collision-NewObjectChannel.PNG)

오브젝트와 트레이스 채널을 포함해서 커스텀 채널은 18개까지 가능합니다.

만약 사용하던 오브젝트 채널을 삭제할 경우 해당 타입의 모든 사용이 월드 스태틱으로 변경됩니다.

## 프리셋 추가 및 편집

오브젝트 채널과 트레이스 채널을 추가한 곳 아래쪽에 프리셋을 추가하거나 편집 할 수 있는 콜리전 프로파일에 접근 할 수 있습니다.

![Collision-CollisionPreset]({{site.url}}/images/Unreal/ue5/2024-06-20-Collision/Collision-CollisionPreset.PNG)

이곳에서 편집하거나 생성하고자 하면 창이 열리는데, 이름과 다양한 설정 값을 수정 할 수 있습니다.  
콜리전을 켜거나 끄고, 오브젝트 유형을 선택하고, 선택된 오브젝트 유형에 대한 각각의 콜리전 반응을 설정 할 수 있습니다.

![Collision-NewCollsionPreset]({{site.url}}/images/Unreal/ue5/2024-06-20-Collision/Collision-NewCollsionPreset.PNG)

디테일에서 콜리전 프리셋에서 콜리전 반응을 수정한 값은 디폴트 값으로 리셋하는 버튼이 생겨 특정 콜리전에 대한 반응을 수정한 것을 알 수 있습니다.

### 콜리전 프리셋 Custom...

블루프린트 클래스나 뷰포트에 배치된 인스턴스의 콜리전을 개별적으로 설정 할 수 있습니다.

콜리전의 프리셋을 `Custom...`으로 선택하면 변경 할 수 없던 콜리전 프리셋의 설정을 변경 할 수 있게 됩니다.  
이렇게 설정한 콜리전 프리셋은 콜리전 프로파일을 수정하지 않고, 해당 블루프린트 클래스나 인스턴스에서만 콜리전을 수정하게 됩니다.

![Collision-CollisionPresetCustom]({{site.url}}/images/Unreal/ue5/2024-06-20-Collision/Collision-CollisionPresetCustom.PNG)

## 콜리전 메시 설정

오브젝트가 콜리전 반응을 하기 위해서는 콜리전의 범위나 모양이 있어야 할것입니다.  
이 범위나 모양을 만드는 방식은 크게 3가지가 있습니다.

첫번째로 스태틱메시 에셋에 콜리전 영역을 심는 방법입니다.  
스태틱메시 에셋을 열고 콜리전 그리기 옵션을 선택하면 콜리전을 추가하거나 제거하는 등의 작업을 할 수 있습니다.  

![Collision-StaticMeshAsset]({{site.url}}/images/Unreal/ue5/2024-06-20-Collision/Collision-StaticMeshAsset.PNG)

두번째로 구체, 박스, 캡슐 모양의 콜리전 컴포넌트를 사용하는 방법입니다.
이는 해당 액터의 블루프린트 클래스에서 원하는 모양의 콜리전 컴포넌트를 추가하면 사용 할 수 있습니다.

![Collision-CollisionComponent]({{site.url}}/images/Unreal/ue5/2024-06-20-Collision/Collision-CollisionComponent.PNG)

세번째는 피직스 애셋을 사용하는 방법입니다.
피직스 에셋은 스켈레탈 메시에 사용하는 콜리전과 피직스를 정의하는 데 사용되는 에셋입니다.
그러므로 스켈레탈 메시를 사용하는 애셋에서 사용 할 수 있는 방법입니다.

피직스 애셋에 자세한 설명은 [언리얼엔진 공식 문서의 피직스 에셋 에디터](https://dev.epicgames.com/documentation/ko-kr/unreal-engine/physics-asset-editor-in-unreal-engine?application_version=5.3)에서 확인 할 수 있습니다.

![Collision-PhysicsAsset]({{site.url}}/images/Unreal/ue5/2024-06-20-Collision/Collision-PhysicsAsset.PNG)


## 콜리전 처리 방식 규칙

해당 규칙은 [언리얼 엔진 공식 문서의 콜리전 개요](https://dev.epicgames.com/documentation/ko-kr/unreal-engine/collision-in-unreal-engine---overview?application_version=5.3)의 규칙을 가지고 온 것입니다.

+ 막음(Blocking)은 막음(Block)으로 설정된 두 (또는 그 이상의) 액터 사이에 자연적으로 발생합니다. 하지만 시뮬레이션에서 히트 이벤트 생성(Simulation Generates Hit Events) 옵션을 켜 줘야 Event Hit 를 실행할 수 있는데, 이는 블루프린트, 디스트럭터블 액터, 트리거 등에서 사용됩니다.
- 액터를 겹침(Overlap)으로 설정하면 마치 서로 무시(Ignore)하는 것처럼 보이며, 실제로 오버랩 이벤트 생성(Generate Overlap Events) 옵션이 없으면 기본적으로 같습니다.
+ 둘 이상의 시뮬레이션 오브젝트가 서로를 막도록 하려면, 그 각각의 유형에 대해 막음 설정되어 있어야 합니다.
- 둘 이상의 시뮬레이션 오브젝트에 대해, 하나가 오브젝트 겹침 설정되어 있고, 두 번째 오브젝트가 다른 것을 막음 설정되어 있는 경우, 겹침은 발생하나 막음은 발생하지 않습니다.
+ 오버랩 이벤트는 한 오브젝트가 다른 것을 막는 경우에도 발생 가능한데, 특히나 고속 이동인 경우 좋습니다.
    + 한 오브젝트가 충돌 및 겹침 이벤트 둘 다 갖는 것은 좋지 않습니다. 가능은 하지만, 수동 처리가 필요한 부분이 많습니다.
- 한 오브젝트가 무시 설정되어 있고, 다른 것은 겹침 설정된 경우, 겹침 이벤트는 발생하지 않습니다.
+ 기본 에디터에서 플레이 카메라는 폰입니다. 그래서 폰을 막는 것으로 설정된 것이면 막힙니다.
- 에디터에서 시뮬레이트 카메라는, 빙의하기 전이면 폰은 아닙니다. 자유롭게 모든 것을 뚫고 들어갈 수 있으며, 충돌이나 겹침 이벤트가 생성되지 않습니다.