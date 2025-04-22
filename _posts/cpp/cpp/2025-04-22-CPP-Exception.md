---
layout: single

title: "[C++] 예외"

categories:
    - Cpp
tag: [Cpp]

date: 2025-04-22
last_modified_at: 2025-04-22

order : 300
---

# 예외

예외(Exception)이란 코드를 실행하는 중에 발생할 수 있는 예상되거나 예상하지 못한 문제 상황을 알려주는 안전장치입니다.  
즉, 프로그래머가 의도한 대로 실행되지 않는 문제 상황을 의미합니다.

예외는 다음과 같은 목적으로 사용합니다.

+ 프로그램이 비정상적으로 종료되지 않도록 방지합니다.
+ 에러를 추적하고 복구할 수 있는 구조를 제공합니다.
+ 코드 흐름을 명확하게 분리합니다.

예외와 버그(bug)는 비슷해 보이지만, 다른 개념입니다.  
버그는 프로그래머가 의도하지 않은 오류이며, 프로그램이 잘못된 동작을 하는 경우를 의미합니다.  
예외는 버그가 발생할 가능성을 미리 예측하고 의도적으로 만들어 처리할 수 있는 안전장치입니다.  
즉, 예외는 버그가 발생했을 때 피해를 최소화하고 프로그램의 안정성을 높이기 위해 사용하는 중요한 기능입니다.

프로그래밍 언어에서 흔히 정의된 대표적인 예외는 다음과 같습니다.

+ NullReferenceException: null 포인터에 접근한 경우
+ DivideByZeroException: 숫자를 0으로 나누는 경우
+ FileNotFoundException: 존재하지 않는 파일에 접근한 경우
+ IndexOutOfRangeException: 배열의 범위를 벗어난 위치에 접근하는 경우

이러한 문제는 발생 가능성이 높으며, 방치할 경우 프로그램이 의도치 않은 결과를 낳을 수 있기 때문에 예외로 처리합니다.  
예를 들어, 배열의 크기가 5인데 6번째 요소에 접근하려고 하면 즉시 예외를 발생시켜 문제가 확산되는 것을 방지합니다.

## C++의 예외

C++에서 예외 처리 메커니즘은 try~catch, throw라고 불리며 세 가지 키워드로 사용할 수 있습니다.

+ `try`: 예외가 발생할 가능성이 있는 코드를 감쌉니다.
+ `throw`: 예외를 발생시킵니다.
+ `catch`: 발생한 예외를 잡아서 처리합니다.

이것을 사용하는 기본 형식은 다음과 같습니다.

```cpp
try // 예외가 발생하는 영역
{
    // 예외가 발생하면 예외를 던지는 영역
   if (예외 조건)
   {
        throw 예외 객체;
   }
}
catch (예외 객체) // 던져진 예외를 잡는 영역
{
   // 예외 처리 영역
}
```

예외가 발생하지 않으면 모든 `try` 블록 내 모든 코드가 실행되며, 이후 `catch` 블록은 건너뛰고 `try~catch` 구문 이후의 코드가 계속 실행됩니다.  
만약, `try` 블록 내부에서 예외가 발생하면, 해당 예외를 처리할 수 있는 `catch` 블록이 실행되며, 이후 `try~catch` 블록 다음의 코드가 계속 실행됩니다.

다음과 같이 예외 타입에 따라 다른 처리를 할 수 있습니다.

```cpp
try
{
    // 예외 발생 가능 코드
}
catch (예외 타입1)
{
    // 예외 처리1
}
catch (예외 타입2)
{
    // 예외 처리2
}
```

## 주의사항

기본적으로 throw는 어떤 타입이든 던질 수 있습니다.  
예를 들어, `int`, `std::string`, 사용자 정의 클래스 등

모든 예외는 `std::exception` 클래스를 상속받습니다.  
이 클래스를 상속받은 객체를던지는 것이 권장됩니다.

예외를 처리할 때, 예외를 다음과 같이 모든 예외를 뭉뚱그려 잡으면 안됩니다.

```cpp
#include <iostream>
#include <exception>

int main()
{
    try
    {
        // 코드 실행 중 예시로 throw std::runtime_error("예외 발생");
    }
    catch (const std::exception& e)
    {
        // 표준 에러 스트림을 사용한 에러/경고 메시지 출력
        std::cerr << "예외 발생: " << e.what() << std::endl;
    }
}
```

다음과 같이 예외를 개별적으로 처리하면 어떤 문제가 발생했는지 명확하게 알 수 있습니다.  
구체적인 예외에 따라 맞춤형 처리또한 가능해집니다.

```cpp
#include <iostream>
#include <stdexcept>

int main()
{
    try
    {
        // 코드 실행 중 예시로 다음과 같은 예외 발생
        // throw std::logic_error("null 포인터 참조");
        // throw std::runtime_error("잘못된 연산");
    }
    catch (const std::logic_error& e)
    {
        std::cerr << "논리 오류(예: null 참조): " << e.what() << std::endl;
    }
    catch (const std::runtime_error& e)
    {
        std::cerr << "실행 오류(예: 잘못된 작업): " << e.what() << std::endl;
    }
}
```

## 예시

```cpp
#include <iostream>

using namespace std;

int divide(int a, int b)
{
    if (b == 0)
    {
        // 문자열 예외 발생
        throw "0으로 나눌 수 없습니다.";
    }

    return a / b;
}

int main()
{
    try
    {
        int result = divide(10, 0);
        cout << "결과: " << result << endl;
    }
    catch (const char* e)
    {
        cout << "예외 발생: " << e << endl;
    }
}
```