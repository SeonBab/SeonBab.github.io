---
layout: single

title: "[UE5] 언리얼 엔진 블루프린트"

categories:
    - UE5
tag: [Unreal Engine, UE5]

date: 2024-06-03
last_modified_at: 2024-06-03
---

# 블루프린트

블루프린트(Blueprint)는 복잡한 C++ 문법 대신 사용할 수 있는 노드 기반 비주얼 스크립트 입니다.  

프로토타입같은 간단한 타입들을 만들 때 유용합니다.

언리얼을 사용하다 보면, 블루프린트를 사용하여 정의된 오브젝트를 그냥 일상적으로 "블루프린트" 라 하는 경우가 많습니다.

## 블루프린트 유형

가장 흔히 작업하게 되는 블루프린트 유형은 레벨 플루프린트와 블루프린트 클래스입니다.  
이 외에도 블루프린트 매크로, 블루프린트 인터페이스가 있습니다.

### 레벨 블루프린트

레벨 블루프린트는 언리얼 엔진에서 키즈멧의 역할을 대체하는 것으로, 기능 역시 똑같습니다.  
레벨에 있는 액터를 가리키고 조작하며, 마티네 액터를 사용해서 시네마틱을 제어하고, 레벨 스트리밍, 체크포인트, 기타 레벨 관련 시스템같은 것들을 관리합니다.

각 레벨마다 레벨 블루프린트가 존재하며, 다른 레벨에선 동작하지 않고 해당 레벨에서만 동작하게됩니다.

![Blueprint-OpenLevelBlueprint]({{site.url}}/images/ue5/ue5/2024-06-03-Blueprint/Blueprint-OpenLevelBlueprint.PNG)

### 블루프린트 클래스

블루프린트 클래스는 문, 스위치, 수집가능 아이템, 파괴가능 배경과 같은 상호작용형 애셋을 만드는 데 좋습니다.

블루프린트 클래스로 작성한 에셋을 뷰포트에 드래그해 레벨에 배치할 수 있고, 레벨 블루프린트와 다르게 여러 갯수를 생성하고 배치할 수 있습니다.  
이때 배치된 블루프린트 클래스는 오브젝트별로 동작할 수 있습니다.

![Blueprint-BlueprintClass]({{site.url}}/images/ue5/ue5/2024-06-03-Blueprint/Blueprint-BlueprintClass.PNG)

1. 파일을 저장하거나 컴파일하고, 가장 최근에 사용된 에셋을 콘텐츠 브라우저에서 선택할 수 있는 버튼입니다.
2. 클래스의 세팅에선 부모 클래스를 변경하거나 블루프린트의 옵션을 변경 할 수 있고, 클래스 디폴트에선 클래스의 초기값을 편집 할 수 있습니다.
3. 뷰포트의 레벨을 플레이 하거나 디버깅할 오브젝트를 선택 할 수 있습니다.
4. 컴포넌트를 추가, 선택, 붙일 수 있습니다.
5. 그래프 에디터에서는 뷰포트로 컴포넌트를 확인 및 조작 할 수 있고, 컨스트럭션 스크립트에서는 해당 클래스의 인스턴스에서 초기화 작업을 할 수 있도록 노드 작업을 할 수 있습니다. 이벤트 그래프에서는 게임플에이에 반응하는 동작을 수행하기 위한 노드 작업을 할 수 있습니다.
6. 블루프린트 클래스에 속하는 엘리먼트(변수, 함수, 그래프 등)를 표시하고, 추가 할 수 있습니다.
7. 클래스 세팅이나 클래스 디폴트 선택시 값을 편집할 수 있는 디테일 탭입니다. 이 값은 오브젝트 단위로 편집 됩니다.

### 블루프린트 클래스 예제

블루프린트 클래스를 생성해보고 레벨을 플레이할 때 Hello!를 출력해보겠습니다.

![Blueprint-ClickCreateBlueprintClass]({{site.url}}/images/ue5/ue5/2024-06-03-Blueprint/Blueprint-ClickCreateBlueprintClass.PNG)

콘텐츠 브라우저의 에셋 뷰를 우클릭해 컨텍스트 메뉴를 열고 블루프린트 클래스를 선택합니다.

![Blueprint-ParentClassesSelect]({{site.url}}/images/ue5/ue5/2024-06-03-Blueprint/Blueprint-ParentClassesSelect.PNG)

블루프린트 클래스를 선택하고나면 부모 클래스를 선택해야합니다.  
액터를 선택해 부모 클래스를 액터로 설정하겠습니다.

![Blueprint-AssetViewBlueprintClass]({{site.url}}/images/ue5/ue5/2024-06-03-Blueprint/Blueprint-AssetViewBlueprintClass.PNG)

설정하고나면 에셋 뷰에서 블루프린트 클래스가 만들어진 것을 볼 수 있고 우클릭하고 컨텍스트 메뉴에서 에셋의 이름을 변경했습니다.

![Blueprint-DataOnlyBlueprint]({{site.url}}/images/ue5/ue5/2024-06-03-Blueprint/Blueprint-DataOnlyBlueprint.PNG)

해당 블루프린트 클래스 에셋을 열면 이런 창이 열리는데 스크립트나 변수가 없어 데이터 전용 블루프린트로 열렸기 때문입니다.  
풀 블루프린트 열기를 클릭하면 위에서 살펴본 블루프린트 클래스로 창이 변경됩니다.

![Blueprint-BeginPlay]({{site.url}}/images/ue5/ue5/2024-06-03-Blueprint/Blueprint-BeginPlay.PNG)

레벨을 플레이 할 때 출력을 하려고 하므로 `Event BeginPlay`를 사용하겠습니다.  
해당 노드는 이벤트 노드로 플레이중에 이 액터가 인스턴스로 생성되면 이벤트가 발생합니다.

![Blueprint-EventAdd]({{site.url}}/images/ue5/ue5/2024-06-03-Blueprint/Blueprint-EventAdd.PNG)

해당 노드가 존재하지 않거나 보이지 않는다면 이벤트 그래프에서 우클릭해 컨텍스트 메뉴를 열고 검색에 `BeginPlay`라고 텍스트를 적어주면 이벤트를 찾을 수 있습니다.  
해당 이벤트를 선택해 추가해주면 됩니다.

![Blueprint-AddPrintString]({{site.url}}/images/ue5/ue5/2024-06-03-Blueprint/Blueprint-AddPrintString.PNG)

이벤트 노드에서 오른쪽 방향의 삼각형 버튼을 클릭하고 바깥쪽으로 드래그하면 이미지와 같은 컨텍스트 창이 열립니다.  
검색창에서 `Print String`을 검색하면 스트링을 로그 및 화면에 출력하는 함수를 찾을 수 있습니다.

![Blueprint-SettingPrintString]({{site.url}}/images/ue5/ue5/2024-06-03-Blueprint/Blueprint-SettingPrintString.PNG)

해당 함수에서 `In String`에 출력하고자하는 문자열인 `Hello!`를 적어주고 컴파일해주고 저장하겠습니다.

![Blueprint-MyBlueprintArrangement]({{site.url}}/images/ue5/ue5/2024-06-03-Blueprint/Blueprint-MyBlueprintArrangement.PNG)

레벨에 해당 액터를 배치해줘야합니다.  
에셋 뷰에서 액터를 클릭하고 뷰포트에 드래그해주면 액터가 레벨에 배치됩니다.  

![Blueprint-PlayPrint]({{site.url}}/images/ue5/ue5/2024-06-03-Blueprint/Blueprint-PlayPrint.PNG)

그 후 레벨을 플레이해주면 좌측 상단에 우리가 출력하려는 문자가 보이는 것을 알 수 있습니다.

이 외에도 이벤트와 함수, 컴포넌트를 활용해 다양한 기능을 만들 수 있습니다.