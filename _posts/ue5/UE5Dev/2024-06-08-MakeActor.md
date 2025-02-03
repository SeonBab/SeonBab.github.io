---
layout: single

title: "[UE5] 블루프린트로 액터 제작"

categories:
    - UE5Dev
tag: [UE5, UE5Dev]

date: 2024-06-08
last_modified_at: 2025-01-24

order : 10
---

# 액터

액터(Actor)는 레벨에 배치할 수 있는 오브젝트를 말합니다.  
이동, 회전, 스케일과 같은 3D트랜스폼을 지원하는 범용 클래스입니다.

액터는 여러가지 유형이 있습니다.  
StaticMeshActor, CameraActor, PlayerStartActor 등이 있습니다.

액터는 위치와 회전, 스케일 같은 트랜스폼 데이터를 직접 저장하지 않습니다.  
루트 컴포넌트에서 트랜스폼 데이터를 가지고 있는경우 이것을 사용합니다.

## 블루프린트로 액터 만들기

블루프린트 클래스를 사용해 레벨에 배치할 동그란 구 형태의 액터를 만들어보겠습니다.

![MakeActor-AddActor]({{site.url}}/images/Unreal/ue5/2024-06-08-MakeActor/MakeActor-AddActor.PNG)

우선 콘텐츠 브라우저에서 `+추가`버튼을 클릭해 블루프린트 클래스를 생성하겠습니다.

![MakeActor-PickParentClass]({{site.url}}/images/Unreal/ue5/2024-06-08-MakeActor/MakeActor-PickParentClass.PNG)

블루프린트 클래스를 생성하려고 한다면 부모 클래스를 선택해주어야 합니다.  
우리는 액터를 만들고자 하므로 액터를 선택하겠습니다.

![MakeActor-ActorName]({{site.url}}/images/Unreal/ue5/2024-06-08-MakeActor/MakeActor-ActorName.PNG)

액터의 이름은 언리얼엔진의 명명규칙에 따라 `BP_`로 시작하는 이름을 지어주었습니다.

![MakeActor-Viewport1]({{site.url}}/images/Unreal/ue5/2024-06-08-MakeActor/MakeActor-Viewport1.PNG)

액터를 클릭해 블루프린트 클래스 창을 열어주고, 뷰포트를 살펴보면 우리가 만들고자하는 동그란 구 형태의 모습을 찾아볼 수 없습니다.  
이는 스태틱 메시가 없기 때문입니다.

![MakeActor-PickComponent]({{site.url}}/images/Unreal/ue5/2024-06-08-MakeActor/MakeActor-PickComponent.PNG)

좌측 상단에 컴포넌트 탭에서 `+추가`버튼을 클릭해 스태틱 메시 컴포넌트를 추가하겠습니다.

스태틱 메시란 비디오 메모리에 캐시되며 그래픽 카드에서 렌더링할 수 있는 폴리곤 세트로 구성되는 지오메트리 조각을 말합니다.  
스태틱 메시를 추가하면, 개임 내에서 유저가 해당 오브젝트를 시작적으로 확인 할 수 있습니다.

![MakeActor-StaticMeshDetails]({{site.url}}/images/Unreal/ue5/2024-06-08-MakeActor/MakeActor-StaticMeshDetails.PNG)

스태틱 메시 컴포넌트를 선택하고 디테일 패널에서 프로퍼티를 수정하겠습니다.  

![MakeActor-Viewport2]({{site.url}}/images/Unreal/ue5/2024-06-08-MakeActor/MakeActor-Viewport2.PNG)

스태틱 메시를 부여해준 후 뷰포트를 확인해보면 처음과 다르게 동그란 구 형태의 모습을 볼 수 있습니다.  
하지만 구에 사각형 모양의 무늬와 색상이 마음에 들지 않아 머티리얼을 변경하겠습니다.

머티리얼은 오브젝트에 색상, 재질 등을 정의할 수 있는 애셋입니다.

![MakeActor-MaterialDetails]({{site.url}}/images/Unreal/ue5/2024-06-08-MakeActor/MakeActor-MaterialDetails.PNG)

액터의 스태틱 메시 컴포넌트의 디테일에서 머티리얼을 선택하겠습니다.

![MakeActor-Viewport3]({{site.url}}/images/Unreal/ue5/2024-06-08-MakeActor/MakeActor-Viewport3.PNG)

머티리얼을 부여해준 후 뷰포트를 확인해보면 깔끔한 흰색을 가진 구 모양의 액터가 만들어졌습니다.

## 액터 배치하기

![MakeActor-ArrangeActor]({{site.url}}/images/Unreal/ue5/2024-06-08-MakeActor/MakeActor-ArrangeActor.PNG)

지금까지 만든 액터 클래스를 레벨에 배치하기 위해서는 에셋 브라우저에서 뷰포트로 클래스를 드래그해서 배치할 수 있습니다.