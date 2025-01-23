---
layout: single

title: "[UE5] 언리얼 엔진 머티리얼 개요"

categories:
    - UE5
tag: [Unreal Engine, UE5]

date: 2024-12-17
last_modified_at: 2024-12-19

order : 140
---

# 머티리얼

머티리얼(Materials)은 씬에서 오브젝트의 표면 프로퍼티를 정의합니다.  
큰 개념에서 머티리얼이란 메시에 적용되어 표면의 색감, 질감, 반사율, 투명도 등 시각적인 형태를 제어하는 '페인트'라고 할 수 있습니다.

머티리얼은 표면이 씬의 라이트와 어떻게 상호작용해야 하는지를 엔진에 알립니다.

언리얼 엔진의 셰이더는 HLSL(High Level Shading Language)로 작성됩니다.  
그 다음 셰이더 코드는 GPU 하드웨어가 실행할 수 있는 어셈블리 언어로 변환됩니다.  
이런 과정을 거쳐 최종 픽셀의 색이 디스플레이에 출력됩니다.

머티리얼은 노드 기반 워크플로입니다.  
이 노드들이 보이지 않는 곳에서 HLSL로 변환됩니다.  
사용자는 HLSL 코드를 볼 수 있지만 편집할 수 없습니다.

## 새 머티리얼 생성하기

콘텐츠 브라우저에서 우클릭하거나 추가 버튼을 눌러줍니다.  
그 후 컨텍스트 메뉴의 기본 에셋 생성 카테고리에서 머티리얼을 선택합니다.

![MaterialOutline-SelectMaterial]({{site.url}}/images/Unreal/ue5/2024-12-17-MaterialOutline/MaterialOutline-SelectMaterial.PNG)

콘텐츠 브라우저에 머티리얼이 생성되며, 해당 머티리얼을 설명할 수 있는 고유한 이름을 지정해줍니다.  
머티리얼에 권장하는 에셋 접두사는 `M_`입니다.

![MaterialOutline-ReName]({{site.url}}/images/Unreal/ue5/2024-12-17-MaterialOutline/MaterialOutline-ReName.PNG)

## 머티리얼 그래프

![MaterialOutline-MaterialGraph]({{site.url}}/images/Unreal/ue5/2024-12-17-MaterialOutline/MaterialOutline-MaterialGraph.PNG)

이미지의 강조된 영역은 머티리얼 그래프(Material Graph)입니다.  
머티리얼 그래프는 메인 머티리얼 노드(Main Material Node)와 머티리얼 표현식 노드(Material Expression Nodes)들로 이루어집니다.  

머티리얼 그래프에서 데이터는 왼쪽에서 오른쪽으로 흐릅니다.

![MaterialOutline-MainAndExpression]({{site.url}}/images/Unreal/ue5/2024-12-17-MaterialOutline/MaterialOutline-MainAndExpression.PNG)

빨간색 박스에 있는 메인 머티리얼 노드는 모든 머티리얼 네트워크가 종료되는 지점입니다.  
메인 머티리얼 노드 입력에 어떤 머티리얼 표현식 노드 조합이 연결되는지에 따라, 레벨에서 컴파일 및 사용하면 최종 머티리얼의 전반적인 외관이 결정됩니다.

파란색 박스에 있는 머티리얼 표현식은 머티리얼 그래프에서 프로그래밍된 특정 작업들을 수행합니다.

예시로 `Multiply`노드는 두 값을 곱하고, `Texture Coordinates`는 머티리얼의 UV 텍스처 좌표를 2채널 벡터 값으로 출력합니다.

머티리얼 표현식을 삽입하는 방법은 4가지가 있습니다.

+ 팔레트에서 드래그 앤 드롭하기  
+ 우클릭 컨텍스트 메뉴에서 선택하기  
+ 입력 또는 출력 핀에서 드래그하여 선택하기  
+ 키보드 단축키를 누른채로 그래프의 아무 곳이나 좌클릭 하기



## 머티리얼 프로퍼티

메인 머티리얼 노드를 선택하면 디테일(Details) 패널에 글로벌 머티리얼 프로퍼티 및 세팅이 표시됩니다.  
머티리얼 프로퍼티의 빈 공간을 아무 곳이나 클릭하면 머티리얼 프로퍼티도 표시할 수 있습니다.

![MaterialOutline-Properties]({{site.url}}/images/Unreal/ue5/2024-12-17-MaterialOutline/MaterialOutline-Properties.PNG)

머티리얼 도메인, 블렌드 모드, 셰이딩 모델 이 세 가지 설정은 머티리얼의 토대를 형성하고 사용 방법을 결정하며, 머티리얼 제작 과정 초기 단계에서 중요합니다.  
이외에도 나나이트나 반투명에 관한 옵션도 존재하며, 프리뷰 뷰포트에서 머리티얼을 미리 볼 때 사용할 스태틱 메시도 설정할 수 있습니다.

좀 더 자세한 설명과 다른 프로퍼티에 대한 설명은 아래 링크에서 볼 수 있습니다.

[언리얼 엔진 머티리얼 프로퍼티](https://dev.epicgames.com/documentation/ko-kr/unreal-engine/unreal-engine-material-properties){: target="_blank"}

## 머티리얼 인스턴스

머티리얼 인스턴스는 부모 머티리얼 하나로 여러개의 자식 머티리얼 즉, 여러개의 인스턴스를 빠르게 만들 수 있습니다.  
원본 머티리얼을 상속받아 일부 파라미터 값만 수정할 수 있습니다.

필요한 기본 머티리얼이 동일하고, 표면 특성이 다를 때 사용합니다.  
아래 이미지는 가구 하나를 여러 색으로 표현하는 경우입니다.

![머티리얼 인스턴스 사용 예시](https://d1iv7db44yhgxn.cloudfront.net/documentation/images/622dc3fd-a2e2-46c3-ac0a-ed41934993f4/instances-example.png)

머티리얼 인스턴스는 기존 머티리얼의 속성을 실시간으로 수정하거나 사용할 수 있습니다.   
즉, 인스턴스에 적용한 변경사항을 모든 뷰포트에서 즉시 볼 수 있습니다.

머티리얼 인스턴스 에디터에서 파라미터를 노출시킬 수 있습니다.  
이로인해 복잡한 노드 그래프를 편집하지 않고, 머티리얼의 베리에이션을 만들 수 있습니다.

## 머티리얼 함수

머티리얼 노드 로직을 모듈화하여 다른 머티리얼에서 재사용할 수 있게 만든 것입니다.  
공통 라이브러리에 공유가 가능하고 다른 머티리얼에 쉽게 삽입할 수 있습니다.

머티리얼 표현식과 머티리얼 함수의 차이점은 표현식은 언리얼 엔진의 만들어진 노드로 존재하지만, 머티리얼 함수는 콘텐츠 브라우저에 사용자가 편집 할 수 있는 에셋이라는 점입니다.

함수는 모듈화의 장점인 반복 작업이 줄고, 일관된 결과를 얻으며, 유지보수에 효율적이고, 노드 그래프의 복잡도를 줄입니다.

언리얼 에디터에는 사전 제작된 머티리얼 함수가 많습니다.  
머티리얼 함수를 편집하여 행동을 변경하거나 에디터에서 함수를 직접 만들 수 있습니다.

# 참고

[머티리얼](https://dev.epicgames.com/documentation/ko-kr/unreal-engine/unreal-engine-materials){: target="_blank"}