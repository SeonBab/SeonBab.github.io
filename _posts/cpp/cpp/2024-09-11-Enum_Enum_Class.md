---
layout: single

title: "[C++] 열거형과 열거형 클래스"

categories:
    - Cpp
tag: [Cpp]

date: 2024-09-11
last_modified_at: 2024-09-11

order : 170
---

# 열거형

열거형은 정수 값에 고유한 이름을 붙인 것들을 모아놓은 사용자 정의 자료형입니다.

특정 정수 값에 이름을 붙였기 때문에 특정 정수가 사용된 의미에 대해 직관적으로 이해할 수 있습니다.  
즉 코드 문서화 및 가독성 목적으로 유용합니다.

정수 값에 이름을 붙인 것만 사용 할 수 있으므로 표현할 수 있는 값의 범위가 제한된 자료형입니다.

## 열거형 정의

열거형은 다음과 같이 정의됩니다.

```cpp
enum 열거형이름
{
    열거자1,
    열거자2,
    ...
}

enum Week
{
    Sunday,   // 0
    Monday,   // 1
    Tuesday,  // 2
    Wednesday,// 3
    Thursday, // 4
    Friday,   // 5
    Saturday  // 6
};
```

열거형은 우선 `enum`키워드를 사용해 정의합니다.

각 열거자는 쉼표(,)를 사용해 구분합니다.

전체 열거는 세미콜론(;)으로 끝납니다.

위의 경우 첫 번째 열거자인 `Sunday`는 정수 값 0이 할당되며, 그 이후 열거자부터는 이전 열거자보다 1이 더 큰 값이 할당됩니다.  
그렇기 때문에 `Monday`는 1이고, `Tuesday`는 2가 됩니다.

```cpp
enum Week
{
    Sunday,       // 0
    Monday,       // 1
    Tuesday = 5,  // 5
    Wednesday,    // 6
    Thursday,     // 7
    Friday = 10,  // 10
    Saturday      // 11
};
```

이런식으로 직접 열거자의 정수값이 몇이 될지 정해줄 수 있습니다.

## 사용법

```cpp
Week LastWeek = Friday;
Week ThisWeek(Friday);
Week NextWeek{Friday};
```

열거형은 사용자가 직접 자료형을 만든것이기 때문에 직접 만든 자료형의 이름을 써주고 변수의 이름을 써주면 됩니다.  
그 후 기본 자료형의 변수를 선언이나 초기화했던 것처럼 작성합니다.  
다만 값에는 정수값이 아닌 사용자가 정한 이름을 작성해주어야 합니다.

## 주의점

열거형을 사용 할 때 주의할 사항들이 몇가지 있습니다.

이전 열거자보다 1이 더 큰 값이 할당되므로 다음과 같은 상황이 있을 수 있습니다.

```cpp
#include <iostream>

enum Week {
    Sunday = 2,   // 2
    Monday = 1,   // 1
    Tuesday,  // 2
    Wednesday,// 3
    Thursday, // 4
    Friday,   // 5
    Saturday  // 6
};
```

`Sunday`는 2를 `Monday`는 1이라는 값을 할당받았는데, 이전 열거자보다 1이 더 큰 값이 할당되게 되므로 `Tuesday`는 2를 할당받습니다.  
이 경우 `Sunday`와 `Tuesday`는 정수값으로는 2를 가지므로 사용에 주의가 필요합니다.

열거자의 이름은 같은 네임스페이스에 배치되기 때문에 같은 이름을 가진 열거자가 있어서는 안됩니다.

```cpp
enum Week
{
    Sunday,
    Monday,
    Tuesday,
    Wednesday,
    Thursday,
    Friday,
    Saturday
};

enum Weekend
{
    Sunday,   //ERROR
    Saturday, //ERROR
}
```

열거형을 초기화 하거나 값을 대입할 때 정수를 암시적으로 변환하지 않습니다.  

```cpp
Week CurWeek = 1; // ERROR
Week CurWeek = static_cast<Week>(1);
```

이 경우 명시적으로 형변환을 할 경우 오류가 발생하지 않습니다.

암시적으로 형변환을 허용하지 않는 것은 열거형 변수에 정수를 암시적으로 형변환하지 않을 뿐이고, 정수형 변수에 열거자를 사용할 경우 형변환이 됩니다.
```
int a = Monday;
```

# 열거형 클래스

열거형 클래스의 기본적인 개념은 열거형과 같습니다.  
열거형의 문제점들과 단점들을 보완하기 위해 만들어진 것이 열거형 클래스입니다.

C++11에서부터 추가되어 사용할 수 있습니다.

기본적인 열거형과 다르게 열거자의 이름이 충돌하지 않고, 전방 선언을 지원합니다.

열거형의 기반 타입을 지정할 수 있게 되었는데, 기본적으로 `int`를 사용하며 명시적으로 변경해 `char`자료형 등 다른 자료형으로 변경 할 수 있습니다.

이 외의 기본적인 사용법은 열거형과 같습니다.

## 열거형 클래스 정의

```cpp
enum class 열거형이름
{
    열거자1,
    열거자2,
    ...
}

enum class Week : char
{
    Sunday,   // 0
    Monday,   // 1
    Tuesday,  // 2
    Wednesday,// 3
    Thursday, // 4
    Friday,   // 5
    Saturday  // 6
};
```

열거형과 같게 정의하지만 키워드는 `enum`에서 `enum class`가 됩니다.

## 주의점

열거형에서 사용하던 방법대로 소속 없이 이름만 사용할 수 없습니다.

```cpp
Week a = Sunday; //ERROR
Week b = Week::Monday;
```

어떤 열거형인지 열거형의 이름을 함께 명시해주어야 합니다.  
이 경우 코드가 너무 지저분하거나 불편하다면 `using enum`을 사용할 수 있습니다.

정수형 변수에 열거자를 사용해 암시적 형변환으로 초기화하거나 값을 변경 할 수 있었지만 `enum class`에서는 이 암시적 형변환을 사용할 수 없습니다.

```cpp
int a = Week::Sunday; //ERROR
int b = static_cast<int>(Week::Monday);
```

정적 형변환을 사용하면 사용할 수 있습니다.


