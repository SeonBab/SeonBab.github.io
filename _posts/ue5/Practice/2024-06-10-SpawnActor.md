---
layout: single

title: "[UE5] 블루프린트 액터 스폰"

categories:
    - UEPractice
tag: [UE5, UEPractice]

date: 2024-06-10
last_modified_at: 2025-01-24

order : 20
---

# 액터 스폰

액터의 새 인스턴스를 생성하는 과정을 스폰(Spawn)이라고 합니다.

플레이어의 캐릭터에 특정 키가 입력이 되면 액터가 스폰되도록 하겠습니다.

![SpawnActor-PickBPClass]({{site.url}}/images/Unreal/UEPractice/2024-06-10-SpawnActor/SpawnActor-PickBPClass.PNG)

플레이어 캐릭터로 정해진 클래스의 블루프린트를 편집하겠습니다.  
해당 블루프린트 클래스를 실행해줍니다.

저는 언리얼에서 제공하는 삼인칭 템플릿의 캐릭터를 수정하겠습니다.

![SpawnActor-AddEvent]({{site.url}}/images/Unreal/UEPractice/2024-06-10-SpawnActor/SpawnActor-AddEvent.PNG)

블루프린트의 이벤트 그래프에서 특정 키에 대한 이벤트를 추가하겠습니다.

`Pressed`는 해당 키가 눌려질 때 1번 호출됩니다.  
`Released`는 해당 키가 눌려진 후 땠을 때 1번 호출됩니다.

![SpawnActor-FunctionCall]({{site.url}}/images/Unreal/UEPractice/2024-06-10-SpawnActor/SpawnActor-FunctionCall.PNG)

`E`키가 `Pressed` 됐을 때 액터를 스폰하겠습니다.

이벤트에서 `Pressed`실행핀을 드래그해 새 노드를 만들어줍니다.  
이 노드는 액터를 생성해주는 노드입니다.

![SpawnActor-PickActorClass]({{site.url}}/images/Unreal/UEPractice/2024-06-10-SpawnActor/SpawnActor-PickActorClass.PNG)

노드의 파라미터에 값을 입력해줘야 합니다.  
우선 액터 클래스를 지정해주겠습니다.  
우리가 스폰하고자 하는 액터의 클래스를 찾아 지정해주면 됩니다.

![SpawnActor-SettingSpawnTransform]({{site.url}}/images/Unreal/UEPractice/2024-06-10-SpawnActor/SpawnActor-SettingSpawnTransform.PNG)

다음으로 생성할 액터의 위치를 지정해줘야 합니다.  
저는 캐릭터의 위치에 액터를 스폰하겠습니다.

`Get Actor Transform`은 현재 액터의 트랜스폼 정보를 가져옵니다.  
현재는 타깃이 수정중인 캐릭터기 때문에 캐릭터의 트랜스폼 정보를 가져옵니다.

![SpawnActor-SettingCollisionHandling]({{site.url}}/images/Unreal/UEPractice/2024-06-10-SpawnActor/SpawnActor-SettingCollisionHandling.PNG)

다음은 액터가 스폰 될 때 설정해준 위치에 다른 액터와 충돌(Collision)이 일어날 수 있는데, 이 때 처리를 어떻게 할지도 설정 할 수 있습니다.

저는 항상 지정한 위치에 스폰되게 해보겠습니다.

![SpawnActor-PlayViewport]({{site.url}}/images/Unreal/UEPractice/2024-06-10-SpawnActor/SpawnActor-PlayViewport.PNG)

게임을 실행시키고 `E`키를 누른다면 액터가 스폰되는걸 확인 할 수 있습니다.  
하지만 액터가 스폰 될 때 캐릭터가 밀려나는걸 알 수 있습니다.

이 경우 플레이어와 액터가 서로 충돌하지 않게 설정해주거나 액터의 스폰 위치를 변경하는 등 다양한 방법으로 해결 할 수 있습니다.