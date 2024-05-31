---
layout: single

title: "[UE5] 언리얼 엔진 콘텐츠 브라우저"

categories:
    - UE5
tag: [Unreal Engine, UE5]

date: 2024-06-01
last_modified_at: 2024-06-01
---

# 콘텐츠 브라우저

콘텐츠 브라우저(Content Browser)는 언리얼 프로젝트 내의 콘텐츠 에셋을 생성, 임포트, 구성, 확인 및 관리하고, 콘텐츠 폴더 또한 관리합니다.  
정리하자면 다음과 같습니다.

+ 프로젝트 내의 모든 에셋을 탐색하고 에셋과 상호작용합니다.
- 텍스트 필터를 사용해 에셋을 검색합니다.(고급 필터링을 조합할 수 있다.)
+ 에셋을 개인, 로컬, 공유 컬렉션으로 정리합니다.
- 문제가 있을 수 있는 에셋을 식별합니다.
+ 콘텐츠 폴더 간 또는 다른 프로젝트로 에셋을 마이그레이션합니다.

## 콘텐츠 브라우저 패널 열기

세 가지 방법으로 콘텐츠 브라우저를 열 수 있고, 4개까지 동시에 열 수 있습니다.

상단 메뉴 바의 창 메뉴에서 열 수 있습니다.
![ContentBrowser-AddOtherContentBrowser1]({{site.url}}/images/ue5/ue5/2024-06-01-ContentBrowser/ContentBrowser-AddOtherContentBrowser1.PNG)

메인 툴바의 생성 메뉴에서 열 수 있습니다.
![ContentBrowser-AddOtherContentBrowser2]({{site.url}}/images/ue5/ue5/2024-06-01-ContentBrowser/ContentBrowser-AddOtherContentBrowser2.PNG)

콘텐츠 드로어의 레이아웃에 고정 버튼을 누르면 열 수 있습니다.
고정 버튼을 눌렀을 경우 새 콘텐츠 브라우저 인스턴스가 생성되지만 새 콘텐츠 드로어가 계속 열려있습니다.

![ContentBrowser-FixContentDrawer]({{site.url}}/images/ue5/ue5/2024-06-01-ContentBrowser/ContentBrowser-FixContentDrawer.PNG)

## 콘텐츠 드로어

콘텐츠 드로어는 콘텐츠 브라우저의 특수 인스턴스입니다.

두 가지 방법으로 열 수 있수 있지만 포커스에서 벗어나면(다른 곳이 클릭되면) 자동으로 최소화 됩니다.  

첫 방법은 에디터 하단 툴바에서 콘텐츠 드로어 버튼을 클릭해 열 수 있습니다.

![ContentBrowser-OpenContentDrawer]({{site.url}}/images/ue5/ue5/2024-06-01-ContentBrowser/ContentBrowser-OpenContentDrawer.PNG)

다음 방법으로는 Ctrl + 스페이스바 단축키로 열 수 있습니다.

## 네비게이션 바

네비게이션 바에는 에셋을 추가, 임포트, 저장을 하고, 히스토리 뒤로 앞으로, 현재 열린 폴더의 경로를 표시하기 위한 컨트롤이 포함되어 있습니다.

![ContentBrowser-NavigationBar]({{site.url}}/images/ue5/ue5/2024-06-01-ContentBrowser/ContentBrowser-NavigationBar.PNG)

## 소스 패널

소스 패널에서는 프로젝트 내의 모든 폴더 목록이 포함되어 있습니다.

![ContentBrowser-Sources]({{site.url}}/images/ue5/ue5/2024-06-01-ContentBrowser/ContentBrowser-Sources.PNG)

자세한 정보는 [소스 패널 참고 자료](https://dev.epicgames.com/documentation/ko-kr/unreal-engine/sources-panel-reference-in-unreal-engine)에서 참조 할 수 있습니다.

## 컬렉션

컬렉션 패널에서는 액세스 가능한 모든 컬렉션의 목록을 표시하고, 새 컬렉션을 추가하거나 검색 할 수 있습니다.

컬렉션은 수동으로 추가된 에셋 레퍼런스를 포함합니다.  
에셋 뷰에서 한 번에 한 컬렉션의 콘텐츠만 볼 수 있습니다.  
컬렉션은 머신에 로컬로 저장할 수도 있고, 다른 사용자와 공유할 수도 있습니다.

자세한 정보는 [필터 및 컬렉션](https://dev.epicgames.com/documentation/ko-kr/unreal-engine/filters-and-collections-in-unreal-engine)에서 참조 할 수 있습니다.

![ContentBrowser-Collections]({{site.url}}/images/ue5/ue5/2024-06-01-ContentBrowser/ContentBrowser-Collections.PNG)

## 필터 열

필터 열에는 현재 로그인한 사용자의 커스텀 필터와 기본 제공 필터가 포함되어 있습니다.  

필터는 에셋 뷰에서 에셋을 자동으로 표시하거나 숨깁니다.  
다수의 필터를 동시에 활성화하고 개별적으로 토글하여 켜거나 끌 수 있습니다.  
필터는 로컬에 저장되며 프로젝트 전체에 공유됩니다.

![ContentBrowser-Filters]({{site.url}}/images/ue5/ue5/2024-06-01-ContentBrowser/ContentBrowser-Filters.PNG)

자세한 정보는 [필터 및 컬렉션](https://dev.epicgames.com/documentation/ko-kr/unreal-engine/filters-and-collections-in-unreal-engine)에서 참조 할 수 있습니다.

## 검색 바

이름과 유형을 기반으로 에셋을 빠르게 찾는 다양한 기능을 제공합니다.  
에셋 뷰는 소스 패널에서 선택한 폴더의 콘텐츠를 표시하고, 소스 패널에 입력하는 파라미터를 기반으로 동적으로 업데이트 됩니다.

![ContentBrowser-Search]({{site.url}}/images/ue5/ue5/2024-06-01-ContentBrowser/ContentBrowser-Search.PNG)

## 에셋 뷰

에셋 뷰 는 현재 선택된 폴더 및 컬렉션 내에서 사용 가능한 모든 에셋을 보여줍니다.

에셋 뷰에서 할 수 있는 작업은 다음과 같습니다.

+ 에셋을 레벨로 직접 드래그 앤 드롭합니다.
- 에셋 뷰 안에서 우클릭하면 열리는 컨텍스트 메뉴 를 통해 에셋을 생성 및 임포트합니다.
+ 새 폴더를 생성합니다.

![ContentBrowser-AssetView]({{site.url}}/images/ue5/ue5/2024-06-01-ContentBrowser/ContentBrowser-AssetView.PNG)

## 세팅

세팅 버튼은 브라우저 오른쪽 상단에 있습니다.

세팅 버튼을 클릭하면 콘텐츠 브라우저의 현재 인스턴스에 대해 세팅을 조정할 수 있는 메뉴가 열립니다.

![ContentBrowser-Setting]({{site.url}}/images/ue5/ue5/2024-06-01-ContentBrowser/ContentBrowser-Setting.PNG)