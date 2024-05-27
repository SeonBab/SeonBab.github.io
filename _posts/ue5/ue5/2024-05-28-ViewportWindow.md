---
layout: single

title: "[UE5] 언리얼 엔진 뷰포트"

categories:
    - UE5
tag: [Unreal Engine, UE5]

date: 2024-05-28
last_modified_at: 2024-05-28
---

# 뷰포트

뷰포트(Viewport)에서 레벨을 살펴보고 편집 할 수 있으며, 오브젝트를 관리 할 수 있습니다.

![ViewportWindow-Viewport]({{site.url}}/images/ue5/ue5/2024-05-28-ViewportWindow/ViewportWindow-Viewport.png)

## 뷰포트 카메라 조작법

뷰포트의 유형과 상관없이 모두 동일한 조작법은 다음과 같습니다.

**F**: 선택한 오브젝트에게 이동하며, 포커싱됩니다.

### 원근

원근에서 뷰포트 카메라의 움직이는 방식은 1인칭 게임과 비슷합니다.

뷰포트 유형이 원근일 때의 조작법은 다음과 같습니다.

<span style="color:#ffe599">**마우스 좌클릭 + 드래그**</span>: 마우스를 좌우로 움직이면 카메라가 회전하고, 앞뒤로 움직이면 카메라의 Z축은 고정된 채로 앞 뒤로 이동합니다.  
<span style="color:#ffe599">**마우스 우클릭 + 드래그**</span>: 마우스를 움직인 방향으로 카메라가 회전합니다.  
<span style="color:#ffe599">**마우스 좌클릭 + 우클릭 + 드래그**</span>: 마우스를 좌우로 움직이면 카메라가 양옆으로 움직이고, 앞뒤로 움직이면 위아래로(Z축) 움직입니다.  
<span style="color:#ffe599">**마우스 휠**</span>: 휠을 돌린 방향에 따라 앞 또는 뒤로 이동합니다.

<span style="color:#ffe599">**마우스 좌클릭 또는 우클릭 + 드래그**</span>: 마우스를 좌우로 움직이면 카메라가 좌우로 움직이고, 앞뒤로 움직이면 카메라의 Z축으로 위 또는 아래로 이동합니다.

<span style="color:#ffe599">**마우스 좌클릭 또는 우클릭 + W, A, S, D**</span>: 버튼을 누르면 앞, 뒤, 좌, 우로 카메라가 이동합니다.  
<span style="color:#ffe599">**마우스 좌클릭 또는 우클릭 + Q, E**</span>: 버튼을 누르면 카메라의 방향과 상관없이 Z축으로 위 또는 아래로 이동합니다.  
<span style="color:#ffe599">**마우스 좌클릭 또는 우클릭 + 마우스 휠**</span>: 마우스 휠을 돌린 방향에 따라 카메라 이동속도가 증가하거나 감소합니다.  
<span style="color:#ffe599">**마우스 좌클릭 또는 우클릭 + Z, C**</span>: 버튼을 누르면 카메라를 줌 인, 줌 아웃합니다.(FOV 증감 및 감소)

포커싱을 한 후 조작하는 키는 다음과 같습니다.  
<span style="color:#ffe599">**Alt + 마우스 좌클릭 + 드래그**</span>: 포커싱 대상의 주위로 회전합니다. (텀블)  
<span style="color:#ffe599">**Alt + 마우스 우클릭 + 드래그**</span>: 포커싱 대상과 가까워지거나 멀어집니다. (돌리)  
<span style="color:#ffe599">**Alt + 마우스 휠클릭 + 드래그**</span>: 상하좌우로 움직입니다. (트래킹)

W, A, S, D, Q, E, Z, C 대신 사용 가능한 키는 다음과 같습니다.  
<span style="color:#ffe599">**W**</span>: Numpad8, 위쪽 방향키  
<span style="color:#ffe599">**A**</span>: Numpad4, 왼쪽 방향키  
<span style="color:#ffe599">**S**</span>: Numpad2, 아래쪽 방향키  
<span style="color:#ffe599">**D**</span>: Numpad6, 오른쪽 방향키  
<span style="color:#ffe599">**Q**</span>: Numpad7, Page Dn  
<span style="color:#ffe599">**E**</span>: Numpad9, Page Up  
<span style="color:#ffe599">**Z**</span>: Numpad1  
<span style="color:#ffe599">**C**</span>: Numpad3

### 직교

<span style="color:#ffe599">**마우스 좌클릭 + 드래그**</span>: 범위 선택 박스가 생깁니다.  
<span style="color:#ffe599">**마우스 우클릭 + 드래그**</span>: 카메라가 드래그한 방향으로 평행 이동합니다.  
<span style="color:#ffe599">**마우스 좌클릭 + 우클릭 + 드래그**</span>: 마우스를 위아래로 움직이면 카메라를 확대 및 축소합니다.  

## 뷰포트 옵션

뷰포트 탭의 좌측 상단에 있는 버튼은 뷰포트의 옵션들을 볼 수 있는 버튼입니다.  
이 버튼을 누를 시 작업중인 뷰포트의 옵션이 표시됩니다.

![ViewportWindow-ViewportOption]({{site.url}}/images/ue5/ue5/2024-05-28-ViewportWindow/ViewportWindow-ViewportOption.png)

## 뷰포트 유형

뷰포트 유형은 옵션 우측에 있는 버튼으로 관리 할 수 있습니다.

유형은 대표적으로 3D로 보는 원근과 직교 투영한 화면인 직교로 나뉩니다.  
직교는 바라보는 방향에 따라 6가지로 나뉩니다.

![ViewportWindow-ViewportCategory]({{site.url}}/images/ue5/ue5/2024-05-28-ViewportWindow/ViewportWindow-ViewportCategory.png)

뷰포트 유형을 변경하는 단축키는 다음과 같습니다.

<span style="color:#ffe599">**Alt + G**</span>: 원근  
<span style="color:#ffe599">**Alt + H**</span>: 정면  
<span style="color:#ffe599">**Alt + J**</span>: 측면  
<span style="color:#ffe599">**Alt + K**</span>: 상단

## 뷰 모드

뷰 모드에서는 시각화 모드를 변경 할 수 있습니다.

시각화 모드는 [언리얼 엔진의 뷰포트 문서](https://docs.github.com/ko/get-started) 에서 볼 수 있습니다.

![ViewportWindow-ViewportMod]({{site.url}}/images/ue5/ue5/2024-05-28-ViewportWindow/ViewportWindow-ViewportMod.png)

## 플래그 옵션

플래그 옵션에서는 뷰포트에서 보이는 플러그들에 대해 표시할지 안할지 설정값을 변경 할 수 있습니다.

![ViewportWindow-ViewportShowFlag]({{site.url}}/images/ue5/ue5/2024-05-28-ViewportWindow/ViewportWindow-ViewportShowFlag.png)

## 뷰포트 최대화

뷰포트 탭의 우측 상단에 있는 버튼은 뷰포트의 레이아웃 패널을 최대화 하거나 원래 크기로 복원해줍니다.

![ViewportWindow-ViewportMaximize]({{site.url}}/images/ue5/ue5/2024-05-28-ViewportWindow/ViewportWindow-ViewportMaximize.png)

원래 크기로 복원 할 시 4개의 패널이 보이는데 이것은 뷰포트 옵션의 레이아웃이 4패널로 설정돼있기 때문입니다.

![ViewportWindow-ViewportLayout]({{site.url}}/images/ue5/ue5/2024-05-28-ViewportWindow/ViewportWindow-ViewportLayout.png)

4패널에서는 기본적으로 후면, 오른쪽, 원근, 상단 이렇게 4가지 보기로 설정 되어있습니다.

## 뷰포트 탭 열기

뷰포트는 기본적으로 하나만 표시되고, 뷰포트 탭의 갯수를 늘릴 수 있습니다.

![ViewportWindow-OtherViewport]({{site.url}}/images/ue5/ue5/2024-05-28-ViewportWindow/ViewportWindow-OtherViewport.png)

창 >> 뷰포트 >> 뷰포트 선택시 뷰포트탭이 열립니다.

![ViewportWindow-AddOtherViewport]({{site.url}}/images/ue5/ue5/2024-05-28-ViewportWindow/ViewportWindow-AddOtherViewport.png)

## 선택 컨트롤

뷰포트에서 오브젝트를 클릭해서 선택할 수 도 있고, 직교뷰에서 박스식 선택으로 여러 액터를 선택할 수 있습니다.  
이런 선택에 대한 조작법을 알아보겠습니다.

<span style="color:#ffe599">마우스 좌클릭</span>: 커서 위치의 오브젝트를 선택하고, 이전에 선택되어 있던 오브젝트는 취소됩니다.  
<span style="color:#ffe599">Ctrl + 마우스 좌클릭</span>: 현재 선택된 오브젝트를 취소하지 않고, 커서 위치의 오브젝트를 선택에 추가합니다.

### 직교뷰 선택 컨트롤

직교뷰에서만 가능한 선택 컨트롤입니다.

<span style="color:#ffe599">마우스 좌클릭 + 드래그</span>: 선택 박스에 포함된 오브젝트를 선택하고, 이전에 선택되어 있던 오브젝트는 취소됩니다.  
<span style="color:#ffe599">Shift + 마우스 좌클릭 + 드래그</span>: 현재 선택된 오브젝트를 취소하지 않고, 선택 박스에 포함된 오브젝트를 선택에 추가합니다.  
<span style="color:#ffe599">Ctrl + 마우스 우클릭 + 드래그</span>: 현재 선택된 오브젝트에서 선택 박스에 포함된 오브젝트를 취소합니다.

## 우측 상단 메뉴

우측 상단의 메뉴는 대부분 트랜스폼 컨트롤에 관한 메뉴입니다.  
좌측 상단에 있던 메뉴들과 다르게 토글 메뉴가 많으므로 한번에 설명하겠습니다.

![ViewportWindow-TopRightMenu]({{site.url}}/images/ue5/ue5/2024-05-28-ViewportWindow/ViewportWindow-TopRightMenu.png)

1. 오브젝트를 선택할 수 있게 됩니다.
2. 오브젝틀를 선택하거나 이동할 수 있는 툴을 선택합니다.
3. 오브젝트를 선택하거나 회전할 수 있는 툴을 선택합니다.
4. 오브젝트를 선택하거나 스케일을 변경 할 수 있는 툴을 선택합니다.
5. 월드 좌표계, 포컬 좌표계 중 한가지를 선택합니다.
6. 오브젝트를 이동 시 스냅되는 방식을 설정합니다.
7. 이동 스냅을 켜거나 끌 수 있습니다.
8. 이동 스냅의 단위를 변경 합니다.
9. 회전 스냅을 켜거나 끌 수 있습니다.
10. 회전 스냅의 단위를 변경 합니다.
11. 스케일 스냅을 켜거나 끌 수 있습니다.
12. 스케일 스냅의 단위를 변경합니다.
13. 카메라의 속도를 변경 할 수 있습니다.

단축키는 다음과 같습니다.

<span style="color:#ffe599">**W**</span>: 이동 툴을 선택합니다.  
<span style="color:#ffe599">**E**</span>: 회전 툴을 선택합니다.  
<span style="color:#ffe599">**R**</span>: 스케일 툴을 선택합니다.  
<span style="color:#ffe599">**V**</span>: 다른 지오메트리의 버텍스에 스냅하도록 해주는 버텍스 스냅을 토글합니다.  
<span style="color:#ffe599">**마우스 좌클릭 + 드래그(트랜스폼 툴에서)**</span>: 현재 활성화된 트랜스폼 기즈모에 따라 이동, 회전, 스케일을 조절합니다.  
<span style="color:#ffe599">**마우스 휠클릭 + 드래그(피벗에서)**</span>: 현재 오브젝트의 피벗을 일시적으로 이동합니다.  
<span style="color:#ffe599">**Alt + 마우스 좌클릭 + 드래그(트랜스폼 툴에서)**</span>: 현재 선택한 오브젝트의 복제본을 생성하고, 원본은 그대로 둔 후 복제본만 트랜스폼 툴로 값을 변경합니다.

<span style="color:#ffe599">**Ctrl + 마우스 좌클릭 + 드래그**</span>: 스케일 툴 컨트롤을 선택한 상태에서 선택한 오브젝트의 스케일을 모든 축으로 균등하게 조절합니다.

### 원근 뷰

뷰포트 유형이 원근일 때의 트랜스폼 컨트롤을 사용한 조작법은 다음과 같습니다.

<span style="color:#ffe599">**Ctrl + 마우스 좌클릭 + 드래그**</span>: 이동 툴과 회전 툴에서 X축으로 값을 변경합니다.  
<span style="color:#ffe599">**Ctrl + 마우스 우클릭 + 드래그**</span>: 이동 툴과 회전 툴에서 Y축으로 값을 변경합니다.  
<span style="color:#ffe599">**Ctrl + 마우스 좌클릭 + 마우스 우클릭 + 드래그**</span>: 이동 툴과 회전 툴에서 Z축으로 값을 변경합니다.

### 직교 뷰

뷰포트 유형이 직교일 때의 트랜스폼 컨트롤을 사용한 조작법은 다음과 같습니다.

<span style="color:#ffe599">**Ctrl + 마우스 좌클릭 + 드래그**</span>: 선택한 오브젝트를 화면에 보이는 두 축으로 정의된 평면을 따라 이동합니다.  
<span style="color:#ffe599">**Ctrl + 마우스 우클릭 + 드래그**</span>: 선택한 오브젝트를 화면에 보이는 축을 따라 회전합니다.