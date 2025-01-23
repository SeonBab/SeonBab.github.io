---
layout: single

title: "[UE5] 언리얼 엔진 라이브 코딩"

categories:
    - UE5
tag: [Unreal Engine, UE5]

date: 2025-01-21
last_modified_at: 2025-01-21

order : 70
---

# 언리얼 엔진 라이브 코딩

라이브 코딩(Live Coding)을 사용하면 에디터를 일일이 끄고 켜는 번거로움이 줄어듭니다.

함수 내부 로직, 변수 값 변경, 로그 출력 변경 등의 간단한 코드 변경은 라이브코딩으로 즉시 반영됩니다.

기존 방식의 비주얼 스튜디오에서 수정한 코드를 반영 할 때 다음과 같은 과정을 거쳐야 했습니다.

+ Shift + F5(에디터 연결 종료)
+ C++ 코드 수정
+ Visual Studio에서 빌드
+ F5 (에디터 재연결)
+ 결과 확인

라이브 코딩 기능을 사용한 다면 다음과 같은 과정이 됩니다.

+ (에디터 연결 종료 안한채) C++ 코드 수정
+ Live Coding으로 변경 사항만 컴파일
+ 에디터에 즉시 로직 반영

## 제약사항

라이브 코딩은 특정 상황에 적용이 되지 않는 문제가 있습니다.

- **UCLASS, USTRUCT, UENUM 매크로**의 추가·삭제·수정
- **새로운 C++ 클래스** (.h/.cpp) 생성
- **엔진 코어** 영역 수정
- **함수 시그니처 (인자, 반환값)**나 **클래스 상속 구조** 변경
- **생성자** 수정

이 경우 전통적인 빌드 프로세스를 진행해야합니다.

## 기능 활성화

언리얼 에디터 하단의 라이브 코딩 아이콘 옆 세로 점 3개에서 라이브 코딩 활성화(Enable Live Coding)를 체크합니다.

![EnableLiveCoding]({{site.url}}/images/Unreal/ue5/2025-01-21-LiveCoding/LiveCoding-EnableLiveCoding.PNG)

## 사용 방법

우선 비주얼 스튜디어에서 수정한 코드의 파일을 저장해야합니다.

에디터 하단에 있는 라이브 코딩 아이콘을 클릭하거나, 단축키 `Ctrl + Alt + F11`을 눌러 라이브 코딩 빌드를 시작합니다.

![Icon]({{site.url}}/images/Unreal/ue5/2025-01-21-LiveCoding/LiveCoding-Icon.PNG)

컴파일이 완료되면 라이브 코딩 창에 "Fiished" 메시지가 뜨고, 에디터가 즉시 수정된 로직을 반영합니다.

![Finished]({{site.url}}/images/Unreal/ue5/2025-01-21-LiveCoding/LiveCoding-Finished.PNG)