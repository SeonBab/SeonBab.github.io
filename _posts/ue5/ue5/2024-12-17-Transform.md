---
layout: single

title: "[UE5] 언리얼 엔진 트랜스폼"

categories:
    - UE5
tag: [Unreal Engine, UE5]

date: 2024-12-17
last_modified_at: 2024-12-17

order : 130
---

# 트랜스폼

모든 액터에는 트랜스폼 정보가 들어있습니다.  
트랜스폼이란 액터를 위치(Location), 회전(Rotation), 스케일(Scale) 조절하는 것입니다.

위치, 회전, 스케일은 모두X, Y, Z 축을 기반으로 값을 가집니다.  
+ 위치: X, Y, Z 좌표를 기준으로 액터의 공간적 위치를 결정합니다.
+ 회전: Roll(X축), Pitch(Y축), Yaw(Z축)을 기준으로 액터의 방향을 설정합니다.
+ 스케일: X, Y, Z를 축으로 액터의 크기를 일정 비율로 조절합니다.

X의 경우 빨강색, Y의 경우 초록색, Z의 경우 파란색으로 표시됩니다.


언리얼 에디터에서 액터 트랜스폼 방법은 수동 트랜스포메이션과 인터랙티브 트랜스포메이션 두 가지입니다.  
둘 다 현재 선택된 액터의 트랜스폼을 변경할 수 있습니다.

## 수동 트랜스포메이션

디테일 패널의 트랜스폼 카테고리 통해 수동 트렌스포메이션(Manual Transformation)을 수행 할 수 있습니다.

![Transform-Details]({{site.url}}/images/ue5/ue5/2024-12-17-Transform/Transform-Details.PNG)

레벨 뷰포트혹은 월드 아웃라이너에서 하나 이상의 액터를 선택하면 이 카테고리에서 해당 액터의 위치, 회전, 스케일을 편집 할 수 있습니다.  
액터에 따라 가능한 경우 액터 모빌리티 세팅도 포함됩니다.

![Transform-TransformAndMobility]({{site.url}}/images/ue5/ue5/2024-12-17-Transform/Transform-TransformAndMobility.PNG)

각 트랜스폼 프로퍼티에는 X, Y, Z축에 대한 숫자 입력 필드가 있습니다.  
특정 값을 이 필드에 직접 입력하여 선택된 액터를 조정하거나 필드 안을 클릭하고 마우스를 위아래로 드래그하여 필드 값을 조정할 수 있습니다.

하나 이상의 액터가 선택된 경우 위치 또는 회전에 여러 값이 있으며 관련 필드는 다수의 값을 표시합니다.  
이 경우 숫자를 입력하면 해당 값을 선택한 모든 액터에 값을 덮어씁니다.  
이 경우 액터가 같은 값을 가지므로 겹칠 수 있습니다.

![Transform-MultipleValues]({{site.url}}/images/ue5/ue5/2024-12-17-Transform/Transform-MultipleValues.PNG)

스케일 고정(Lock Scale) 버튼을 클릭하여 스케일 필드를 고정할 수 있습니다. 고정되면 각 축(X, Y, Z)의 값이 함께 변하므로 균일한 스케일 조정이 이뤄지며 왜곡을 방지할 수 있습니다.

![Transform-LockScale]({{site.url}}/images/ue5/ue5/2024-12-17-Transform/Transform-LockScale.PNG)

트랜스폼 프로퍼티의 기본값은 상대적 좌표 공간이며, 이는 액터의 부모 혹은 컴포넌트의 부모를 기준으로 트랜스폼이 발생한다는 것을 뜻합니다.  

![Transform-RelativeCoordinate]({{site.url}}/images/ue5/ue5/2024-12-17-Transform/Transform-RelativeCoordinate.PNG)

위 이미지를 예시로 다음과 같습니다.

`BP_MyActor2`는 `BP_MyCator`의 자식으로 속해있이므로, `BP_MyCator`의 상대적 좌표입니다.

`Follow Camera`는 `Camera Boom`의 자식으로 속해있으므로, `Camera Boom`의 상대적 좌표를 가집니다.  
`Camera Boom`은 `메시`에 상대적 좌표를 가집니다.  
`메시`는 `캡슐 컴포넌트`에 상대적 좌표를 가집니다.

프로퍼티 라벨 옆의 드롭다운 화살표를 클릭하여 상대적 및 월드 트랜스폼을 토글할 수 있습니다.  
월드 트랜스폼은 액터의 부모가 아닌 월드 좌표를 기준으로 발생합니다.

![Transform-DefaultCoordinateSpace]({{site.url}}/images/ue5/ue5/2024-12-17-Transform/Transform-DefaultCoordinateSpace.PNG)

## 인터랙티브 트랜스포메이션

인터랙티브 트랜스포메이션(Interactive transformation)은 뷰포트에서 기즈모라는 시각적 툴로 직접 수행할 수 있습니다.  때로는 기즈모를 위젯(widget)으로 부르기도 하는데, 언리얼 엔진에서는 두 단어의 뜻이 같습니다.

기즈모는 어떤 축에 영향을 미치는지에 따라 색상이 할당된 여러 부분으로 구성되어 있습니다.  
빨강은 X축을 나타냅니다.  
녹색은 Y축을 나타냅니다.  
파랑은 Z축을 나타냅니다.

![Transform-GizmoColor]({{site.url}}/images/ue5/ue5/2024-12-17-Transform/Transform-GizmoColor.PNG)

### 이동 기즈모

이동(Translation)기즈모는 월드 내 각 축에서 양의 방향을 가리키는 여러 색상의 화살표 세트입니다.  
이 기즈모를 사용하면 액터를 축이나 평면을 따라서 또는 자유로운 방향으로 움직일 수 있습니다.

![1개 축 이동](https://d1iv7db44yhgxn.cloudfront.net/documentation/images/80754b06-a32d-47de-9de7-a8c62d684e7e/translate-single-axis.gif)

액터가 동시에 두 개의 축을 따라 움직이게 하려면 두 축이 만나는 점의 사각형을 클릭한 다음, 액터를 두 축(XY, XZ, YZ)으로 정의된 평면을 따라 드래그하여 움직입니다.

![2개 축 이동](https://d1iv7db44yhgxn.cloudfront.net/documentation/images/6cb67da6-5761-4d03-adf4-73b051184aa6/translate-two-axes.gif)

세 축을 따라 액터를 자유롭게 움직이려면 세 축이 교차하는 지점의 하얀색 구체를 클릭하여 드래그합니다. 액터를 마우스 휠로 가까이 또는 멀리 움직일 수도 있습니다.

![모든 축 이동](https://d1iv7db44yhgxn.cloudfront.net/documentation/images/c0ad1c81-7ce1-48ed-8e32-2930b42ce68d/translate-three-axes.gif)

### 회전 기즈모

회전 기즈모는 세 가지 색상의 호로 이루어진 세트로, 각 호는 하나의 축과 연관됩니다.  
호 가운데 하나를 드래그하면 선택된 액터가 해당 축을 중심으로 회전합니다.  
회전 기즈모의 경우 관련 호에 의해 영향을 받는 축은 해당 호에 직각인 축입니다.

예를 들어 XY 평면과 직각인 Z축을 중심으로 회전합니다.
예를 들어 XY 평면에 정렬된 호는 Z축을 중심으로 액터를 회전시킵니다.

![회전 기즈모](https://d1iv7db44yhgxn.cloudfront.net/documentation/images/0248bff5-772a-40f3-b6a4-7b7c7a5626de/rotate-actor.gif)

커서를 특정 호로 가져가면 해당 호가 노란색으로 바뀝니다.  
그러면 드래그하여 액터를 회전시킬 수 있습니다.  
액터를 회전시키면 기즈모의 모양이 변하며 액터가 회전하는 중심 축만 표시됩니다.  
진행상황을 수치로 알 수 있도록 회전 수가 실시간으로 표시됩니다.

### 스케일 기즈모

스케일 기즈모의 핸들에는 끝에 큐브가 달려 있습니다.  
이 핸들 가운데 하나를 잡고 기즈모를 드래그하면 연결된 축만을 따라서 선택한 액터의 스케일을 조절합니다.  
이 핸들에는 이동 및 회전 기즈모와 마찬가지로 축마다 색이 부여됩니다.

![1개 축 스케일](https://d1iv7db44yhgxn.cloudfront.net/documentation/images/071c8a43-c079-4c1a-91bc-7bc6867f6295/scale-single-axis.gif)

스케일 기즈모의 경우 이동 기즈모처럼 한번에 두 축을 동시에 조절하거나 한번에 세 축을 동시에 조절하는 것이 가능하고 방법 또한 같습니다.

### 월드 및 로컬 트랜스포메이션 모드

인터랙티브 트랜스포메이션를 사용하는 경우, 어떤 레퍼런스 좌표계를 사용할지 선택할 수 있습니다.  

월드 스페이스의 경우 이동 기즈모의 XYZ축과 월드의 XYZ축이 같습니다.  
Z축을 따라 드래그하면 큐브가 바닥을 기준으로 위아래로 움직입니다.

![월드 축 트랜스폼](https://d1iv7db44yhgxn.cloudfront.net/documentation/images/e2acce6f-8ed1-4f15-9051-1778100e0edf/coordinates-world-space.gif)

로컬 스페이스의 경우 이동 기즈모의 XYZ축이 큐브의 로컬 좌표를 사용합니다.  
Z축을 따라 드래그하면 마찬가지로 큐브가 위아래로 움직이지만 해당 각도에 맞춰 움직입니다.

![로컬 스페이스 트랜스폼](https://d1iv7db44yhgxn.cloudfront.net/documentation/images/24453b9c-ea8c-428a-b95b-983111782e4a/coordinates-local-space.gif)

월드 스페이스는 객체를 전체 씬 기준으로 정렬할 때 사용합니다.  
로컬 스페이스는 부모-자식 관계의 컴포넌트들의 회전이나 이동을 로컬 기준으로 수행하고 싶을 때 사용합니다.

# 참고

[액터 트랜스폼하기](https://dev.epicgames.com/documentation/ko-kr/unreal-engine/transforming-actors-in-unreal-engine){: target="_blank"}  
[액터 트랜스폼](https://dev.epicgames.com/documentation/ko-kr/unreal-engine/transforming-actors?application_version=4.27){: target="_blank"}