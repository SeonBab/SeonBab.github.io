---
layout: single

title: "[UE5] 비주얼 스튜디오 빌드"

categories:
    - UE5
tag: [Unreal Engine, UE5]

date: 2025-01-21
last_modified_at: 2025-01-24

order : 200001
---

# 비주얼 스튜디오 빌드

언리얼 엔진에서 C++ 코드를 수정했다면, 컴파일(Compile) + 링크(Link)해 동적 라이브러리(DLL)로 만드는 것이 빌드의 목적입니다.

이렇게 생성된 동적 라이브러리가 언리얼 에디터에 로드되어, 새로 작성한 로직(함수, 클래스 등)이 게임이나 에디터 내에 즉시 반영됩니다.

## 빌드 구성 및 플랫폼

비주얼 스튜디오 상단 툴바에는 빌드 구성(Configuration)과 플랫폼(Platform)을 선택하는 드롭다운이 있습니다.

![Configuration_Platform]({{site.url}}/images/Unreal/ue5/2025-01-21-VisualStudioBuild/VisualStudioBuild-Configuration_Platform.PNG)

왼쪽 - 빌드구성(DebugGame, Development Editor 등)

오른쪽 - 플랫폼(Win64)

빌드 구성의 종류는 다음과 같습니다.

+ DebugGame
    - 게임 로직만 디버그 정보를 포함하고, 엔진은 최적화된 상태로 빌드합니다.
    - 에디터가 아닌 독립 실행 파일 환경에서 디버깅이 가능합니다.

+ DebugGame Editor
    - 에디터 환경에서 게임 로직을 디버깅하기 편한 설정입니다.
    - 에디터 플레이 중에 C++로직을 추적하거나 브레이크포인트를 걸어볼 수 있습니다.

+ Development
    - 디버그 정보를 최소화해 실행 속도를 높인 개발용 빌드입니다.
    - 독립 실행 파일 환경 테스트 및 개발 단계에서 주로 쓰입니다.

+ Development Editor
    - 에디터에서도 개발 및 테스트를 원활히 할 수 있도록 구성된 빌드 모드입니다.
    - Live Coding 사용 시나리오와 궁합이 좋습니다.

+ Shipping
    - 최종 사용자에게 배포할 때 사용하는 릴리스 빌드입니다.
    - 디버그 정보를 제거하고, 성능 최적화가 극대화 됩니다.

플랫폼(Platform) 설정은 다음과 같습니다.

기본적으로 Win64가 선택되어 있으며, Windows 64비트 환경을 의미합니다.

모바일(Android, IOS) 및 콘솔(PS, Xbox 등)로 빌드하려면 해당 플랫폼용 SDK를 추가로 설치해야 합니다.

## 빌드

언리얼 C++ 프로젝트는 전체 솔루션 빌드와 부분 빌드로 나누어 빌드할 수 있습니다.

언리얼 에디터는 종료하고 빌드하는 편이 안전합니다.  
에디터가 실행 중이면 수정된 DLL을 교체하지 못해서 빌드 에러가 발생할 수 있습니다.

처음 빌드한 경우 엔진 모듈까지 모두 새로 컴파일하므로 오래 걸릴 수 있지만, 이후에는 변경된 소스만 컴파일 해서 빌드 시간이 크게 단축됩니다.

![Build_Completed]({{site.url}}/images/Unreal/ue5/2025-01-21-VisualStudioBuild/VisualStudioBuild-Build_Completed.PNG)

이미지 처럼 "Build completed"메시지가 나오면 정상 빌드된 것입니다.

![DLL]({{site.url}}/images/Unreal/ue5/2025-01-21-VisualStudioBuild/VisualStudioBuild-DLL.PNG)

빌드 후, 프로젝트 폴더의 `Binaries/Win64` 폴더 내에 `UnrealEditor-프로젝트명.dll` 등이 새로 생성됩니다.

이제 언리얼 에디터를 실행하면, 이 DLL을 로드해 수정된 로직이 적용됩니다.

### 전체 솔루션 빌드

엔진, 유틸리티, 게임 등 모든 모듈을 통째로 빌드합니다.

![Build_Solution]({{site.url}}/images/Unreal/ue5/2025-01-21-VisualStudioBuild/VisualStudioBuild-Build_Solution.PNG)

첫 빌드나 엔진 소스를 수정했을 때 또는 엔진 전체 파일이 필요한 경우에 사용합니다.

프로젝트의 규모가 크다면 시간이 오래 걸릴 수 있습니다.

### 부분 빌드

엔진이나 다른 모듈을 제외하고, 게임 프로젝트 코드만 빠르게 빌드합니다.

![Build]({{site.url}}/images/Unreal/ue5/2025-01-21-VisualStudioBuild/VisualStudioBuild-Build.PNG)

일반적으로 C++ 로직만 수정했다면 이 방법을 사용하는 것이 효율적입니다.