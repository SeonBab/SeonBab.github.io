---
layout: single

title: "비주얼 스튜디오 디버깅"

categories:
    - etc
tag: [etc]

date: 2025-01-21
last_modified_at: 2025-01-21

order : 4
---

# 비주얼 스튜디오 디버깅

디버깅(Debugging)이란 프로그램에서 발생하는 코드의 오류나 버그를 찾아 수정하는 과정을 이야기 합니다.

## 중단점

중단점(Breakpoint)은 실행을 특정 지점에서 멈추고 상태를 확인할 수 있게 해줍니다.

디버깅 중 특정 코드가 실행되기 전이나 후에 중단하고, 변수 값을 확인할 수 있습니다.

![Breakpoint]({{site.url}}/images/etc/2025-01-21-Visual_Studio_Debugging/Visual_Studio_Debugging-Breakpoint.PNG)

설정 방법은 다음과 같습니다.

+ 코드 줄 번호 왼쪽을 클릭해 중단점을 만들거나 지웁니다.
+ 키보드 단축키의 `F9`를 누르면 현재 줄에 중단점을 만들거나 지웁니다.
+ 키보드 단축키의 `Ctrl + F9`를 누르면 현재 줄의 중단점을 활성화하거나 비활성화 합니다.
+ 키보드 단축키의 `Ctrl + Alt + B`를 누르면 모든 중단점을 확인하거나 제어할 수 있는 창을 띄웁니다.

### 조건부 중단점

조건부 중단점(Conditional Breakpoint)은 디버깅 시 코드가 특정 조건을 만족할 때 중단점을 활성화 되도록 합니다.

불필요한 중단을 피하고, 특정 상황에서만 디버거를 멈추도록 제어할 수 있습니다.

예를 들면 ``x > 10``일 때 중단하도록 설정 할 수 있습니다.

![Conditional_Breakpoint]({{site.url}}/images/etc/2025-01-21-Visual_Studio_Debugging/Visual_Studio_Debugging-Conditional_Breakpoint.PNG)

중단점 우클릭 > 조건(Conditions)을 클릭해서 설정할 수 있습니다.

## 실행 제어

`F5`를 눌러 디버깅을 시작하거나 계속합니다.

`Shift + F5` / `Ctrl + break`를 눌러 디버깅을 중지할 수 있습니다.

`F10`을 눌러 코드를 한 줄 실행합니다. (Step Over)

`F11`을 눌러 함수 내부로 진입할 수 있습니다. (Step Into)

## 간략한 조사식

간략한 조사식(Quick Watch)는 변수나 식의 값을 즉시 확인하는 방법입니다.  
빠른 보기라고도 불립니다.

변수의 값을 실시간으로 확인할 수 있습니다.

변수의 값 말고도 복잡한 표현식을 사용할 수 있습니다.  
예를 들면``x + y``의 연산 등 실시간으로 평가하고, 값을 확인 할 수 있습니다.

![Quick_Watch]({{site.url}}/images/etc/2025-01-21-Visual_Studio_Debugging/Visual_Studio_Debugging-Quick_Watch.PNG)

디버깅 중 `Shift + F9`또는 마우스 오른쪽 클릭 후 간략한 조사식을 선택합니다.

## 출력 창

출력 창(Output Window)은 개발 중에 발생하는 다양한 정보와 메시지를 확인할 수 있는 창입니다.

프로젝트 빌드/컴파일 과정에서 발생한 메시지(성공, 실패, 경고 등)을 보여줍니다.

디버깅 중에 출력된 메시지를 표시합니다.

파일 복사, 배포 또는 확장 도구에서 출력한 로그를 확인할 수 있습니다.

![Output_Window]({{site.url}}/images/etc/2025-01-21-Visual_Studio_Debugging/Visual_Studio_Debugging-Output_Window.PNG)

기본적으로 하단에 표시되며, 메뉴에서 보기(View) > 출력(Output)으로 열 수 있습니다.

## 데이터 기록

데이터 기록(Logpoints)은 실행 중 코드에 중단점을 설정하지 않고, 특정 위치에서 로그 메시지를 기록하도록 설정할 수 있는 기능입니다.

프로그램의 흐름과 데이터를 디버깅하면서 실행 중단 없이 확인하고 싶을 때 유용합니다.

![Log_Points]({{site.url}}/images/etc/2025-01-21-Visual_Studio_Debugging/Visual_Studio_Debugging-Log_Points.PNG)

중단점 우클릭 > 작업(Actions)을 클릭해서 설정할 수 있습니다.

## 호출 스택

호출 스택(Call Stack)은 프로그램이 실행되는 동안 호출된 함수들의 순서를 추적하는 도구입니다.

디버깅 시 특정 시점에서 프로그램이 어떤 함수에서 실행중인지, 해당 함수가 어떤 경로를 통해 호출되었는지 쉽게 파악 할 수 있게 해줍니다.

![Call_Stack]({{site.url}}/images/etc/2025-01-21-Visual_Studio_Debugging/Visual_Studio_Debugging-Call_Stack.PNG)

기본적으로 하단에 표시되며, 디버깅 모드 > 디버그 > 창 > 호출 스택으로 열 수 있습니다.