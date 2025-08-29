---
layout: single

title: "[Unity6] 입력"

categories:
    - UnityPractice
tag: [Unity6, UnityPractice]

date: 2025-08-29
last_modified_at: 2025-08-29

order : 100
---

# 입력

## UnityEngine.Input 클래스

Unity의 전통적인 입력 시스템으로, `Input`클래스를 통해 키보드, 마우스, 조이스틱 입력 등을 확인합니다.

```C#
if (Input.anyKeyDown)
    // 아무 키를 누른 경우

if (Input.anyKey)
    // 아무 키를 누르고 있는 경우
```

입력 함수는 키 다운, 스테이, 업 3가지 상태로 구분합니다.  
해당 함수는 다음과 같습니다.

```C#
if (Input.GetKeyDown(KeyCode.Return))
    // 엔터 키를 누를 때

if (Input.GetKey(KeyCode.LeftArrow))
    // 왼쪽 화살표 키를 누르는 중일 때

if (Input.GetKeyUp(KeyCode.Space))
    // 스페이스바 키를 뗄 때

if (Input.GetMouseButtonDown(0))
    // 마우스 왼쪽 버튼을 누를 때

if (Input.GetMouseButtonDown(1))
    // 마우스 오른쪽 버튼을 누를 때
```

### InputManager 활용하는 방법

InputManager을 통해 버튼을 추가하여 이름이나 대응하는 키, 축을 수정할 수 있습니다.

수정한 입력에 대해 입력값을 가져오는 방법은 다음과 같습니다.  
인자값인 문자열은 버튼의 이름을 넘겨주어야 합니다.

```C#
if (Input.GetButtonDown("Jump"))
    // 점프 키를 누를 때

if (Input.GetButton("Jump"))
    // 점프 키를 누르는 중일 때

if (Input.GetButtonUp("Jump"))
    // 점프 키를 뗄 때

// 보간 처리한 입력값
Input.GetAxis("Horizontal");

// 보간 처리하지 않은 입력값(가공하지 않은 값)
Input.GetAxisRaw("Horizontal");
```