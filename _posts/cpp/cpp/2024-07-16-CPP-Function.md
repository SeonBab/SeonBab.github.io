---
layout: single

title: "[C++] 함수"

categories:
    - Cpp
tag: [Cpp]

date: 2024-07-16
last_modified_at: 2024-12-24

mermaid: true

order : 130
---

# 함수

함수(Function)는 "특정한 목적의 작업을 수행하기 위해 독립적으로 작성된 코드의 묶음"으로 정의할 수 있습니다.  
함수는 서브루틴(Subroutine) 또는 프로시저(Procedure)라고도 불리고 객체지향 프로그래밍 언어 혹은 C++에서는 클래스가 가지는 함수를 메소드(Method)라고 합니다.

함수, 서브루틴, 프로시저는 구체적으로 다르지만 C++에서는 동일한 의미로 사용됩니다.

## 함수의 장점

함수는 다음과 같은 장점을 가지고 있어 모든 프로그래밍 언어에서 가장 중요한 요소입니다.

+ 복잡한 문제를 해결 가능한 작은 문제로 분해하고 분해된 작은 문제의 해결책을 함수로 처리하고 함수를 구조적으로 사용하여 복잡한 문제를 해결하게 됩니다.
+ 같은 코드를 반복적으로 작성해야하는 문제를 해결합니다. 함수로 만들어 사용하면 코드 작성의 중복을 최소화하고 프로그램의 크기를 줄일 수 있습니다. 또한 함수는 코드의 수정과 유지 보수를 용이하게 합니다.
+ 캡슐화(Capsulation)가 가능합니다. 함수의 내부 구현과 함수 외부와의 인터페이스를 구분하여 인터페이스가 정해지면 함수 내부 구현을 지속적으로 프로그래머의 실력 향상에 맞추어 진화시킬 수 있습니다. 이러한 특징 때문에 이미 만들어진 함수에 대해서 인터페이스의 모습만 알고 있으면 함수의 실제 구현 내용을 할지 못해도 함수의 기능을 이용할 수 있도록 합니다.

## 함수의 구성

함수에는 함수 정의, 함수 선언, 함수 호출이 있습니다.

+ 함수 정의(Function Definition): 함수의 기능을 수행하기 위한 코드를 작성하는 것입니다.
+ 함수 선언(Function Declaration): 함수가 어떤 형태인지를 알려주는 것(선언)입니다.
+ 함수 호출(Function Call): 정의된 함수를 실행하는 것으로 필요한 입력 데이터를 전달한 후 기능을 수행합니다.

이 세가지 작업은 컴파일러가 이해하고 해석해야하므로 표현 규칙(문법)이 서로 다르게 규정됩니다.

### 함수 정의

함수는 함수 헤더(Function Header)와 함수 몸체(Function Body)로 이루어져 있습니다.

함수 헤더는 **반환 자료형**, **함수 이름**, **매개변수 목록**에 해당됩니다.  
함수 몸체는 **중괄호{}**에 해당합니다.

![Function_definition]({{site.url}}/images/cpp/cpp/2024-07-16-CPP-Function/Function_definition.PNG)

반환 자료형(Return Type)은 함수가 기능을 수행한 후 반환하는 데이터의 자료형을 명시한 것입니다.  
함수가 반환하는 데이터가 없는 경우 반환 자료형을 `void`로 명시합니다.  
데이터의 값을 반환하기 위해서는 `return` 키워드를 사용하여 변수, 수식 등을 사용해 값을 반환해야 합니다.

함수 이름(Function Name)은 변수들을 구별할 때 사용한 이름과 같이 특정 함수를 구별하는 식별자 중 한 요소입니다.  
함수의 구분은 함수 이름과 매개변수의 자료형, 매개변수 목록의 개수로 이뤄져있습니다.

매개변수 목록(Parameter List)은 함수의 기능을 수행 할 때 필요한 입력 데이터의 표현에 사용됩니다.  
필요한 입력 데이터가 없는 경우 매개변수를 명시하지 않을 수 있습니다.  
매개 변수의 개수는 여러개가 될 수 있으며 쉼표로 구분합니다.

함수의 몸체에서 실질적인 작업이 이루어지므로 함수가 수행하는 기능을 구현하는데 필요한 프로그램 코드들이 들어갑니다.  
함수 몸체에 작성된 코드는 `{`에서 시작하여 `return` 키워드나 `}`에 도달할 때 까지 순차적으로 프로그램 명령어를 하나씩 실행합니다.  
함수 몸체의 코드가 종료되면 함수를 호출한 곳으로 되돌아가 다음 프로그램 코드를 시작합니다.

#### 매개 변수와 반환 자료형 모두 없는 함수 예시

콘솔창에 "Hello!"라는 글자를 출력하는 함수를 만들어 예시를 들어보겠습니다.

콘솔창에 특정 글자를 고정적으로 출력할 것이므로 매개변수는 필요 없을 것입니다.  
콘솔창에 글자만 출력하는 것은 반환 데이터가 필요 없을 것입니다.

```cpp
void PrintHello()
{
    std::cout << "Hello!" << std::endl;
}
```

함수 헤더:
```cpp
void PrintHello()
```

반환 자료형은 아무것도 반환하지 않을 것이므로 `void`입니다.
함수를 호출 할 때 `PrintHello`라는 함수 이름으로 호출 할 수 있습니다.  
함수를 호출 할 때 필요한 매개변수가 없으므로 매개변수 목록에 아무것도 작성하지 않습니다.

함수 몸체:
```cpp
{
    std::cout << "Hello!" << std::endl;
}
```

함수 몸체에서는 콘솔창에 `Hello!`라는 글자를 출력하고 반환하는 자료형은 없이 함수가 종료 될 것입니다.

이처럼 함수에 반환 자료형이 없다면 `void`키워드를 사용해 함수에서 반환하는 자료형이 없다고 명시해 정의 할 수 있습니다.  
함수에 매개변수도 필요 없다면 함수 이름 옆에`()`로 매개변수를 비워두면 함수에서 필요한 매개변수가 없다고 정의하는 것입니다.

#### 매개 변수와 반환 자료형 모두 있는 함수 예시

두 개의 정수 합을 구하고 결과 값을 반환하는 함수를 만들어 예시를 들어보겠습니다.

정수의 합을 구하려 하므로 매개변수로 정수 두 개가 필요할 것입니다.  
입력 데이터인 매개 변수가 정수일 것이므로 자료형은 정수형 자료형이 필요할 것입니다.  
두 정수의 합을 구한 후 결과 값을 반환해야 할 것이므로 반환 자료형을 명시해야 합니다.

```cpp
int Sum(int a, int b)
{
    return a + b;
}
```

함수 헤더:
```cpp
int Sum(int a, int b)
```

정수를 반환할 것이므로 함수 헤더의 반환 자료형을 int로 사용했습니다.
함수를 호출 할 때 `Sum`이라는 함수 이름으로 호출 할 수 있습니다.  
정수형 값을 더하고자 하므로 더하려는 값 2개를 매개변수로 받아올 수 있어야 합니다.

함수 몸체:
```cpp
{
    return a + b;
}
```

함수 몸체에서는 매개변수로 받은 값 두개를 더하고 `return` 지시문을 통해 더해진 값이 반환될 것입니다.

이처럼 함수에 반환 자료형이 있다면 반환하는 자료형을 명시해 정의하면 됩니다.  
함수에 매개변수가 필요하다면 함수 이름 옆의 `소괄호()`안에 매개변수를 작성해주면 됩니다.

### 함수 선언

함수 선언은 컴파일러에게 프로그램에서 사용하고자 하는 함수의 정보를 미리 알려주는 것입니다.

미리 알려주어야 할 함수의 정보를 함수 원형(Function Prototype)혹은 전방 선언(Forward Declaration)이라고 합니다.  
전방선언을 사용하는 이유는 순환 호출(Cyclic Calling)의 경우 서로의 함수를 함수 몸체에서 호출하는데 전방 선언이 없다면 다른 함수의 정보를 가지고있지 않아 컴파일이 되지않는 문제를 해결할 수 있어 사용합니다.

함수 선언은 함수의 헤더와 유사한 형태를 가지는데 함수 헤더 끝에 세미콜론(;)을 붙여 선언합니다.

```cpp
void PrintHello();
int Sum(int a, int b);
```

함수 선언은 컴파일러에게 미리 함수 원형에 대한 정보를 제공하여 함수 호출 시 함수의 반환 자료형과 매개변수의 자료형이 올바른 것인지 검사하도록 합니다.  
컴파일러가 함수 호출 문장을 해석 및 검사 할 때 컴파일러가 함수의 원형을 기반으로 함수 호출문이 올바른지 검사 할 수 있기 때문에 함수 원형을 알려 주는 함수 선언이 함수 호출보다 먼저 이루어져야 합니다.

함수의 정의가 함수 선언의 의미를 동시에 가지고 있습니다.

### 함수 호출

함수의 기능을 실행시키는 것을 함수 호출이라고 합니다.  
함수를 호출하려면 함수의 이름을 적고, 함수가 필요로 하는 매개변수가 있다면 매개변수를 나열하면 됩니다.  
함수 호출도 C++에서 하나의 명령문이기 때문에 문장의 끝에 세미콜론(;)을 붙입니다.  

함수를 호출하게 되면 함수 호출문 다음 코드의 실행은 잠시 중지되고, 호출된 함수의 몸체 내의 문장들이 순차적으로 실행된 후 함수의 실행이 종료되면 함수 호출문 다음 위치로 되돌아 옵니다.

![Function_Call]({{site.url}}/images/cpp/cpp/2024-07-16-CPP-Function/Function_Call.PNG)

10번 줄에 `int`자료형 `x`, `y`가 초기화 되고 `Sum`함수를 호출합니다.  
매개변수로 `x`와 `y`의 값인 `1`, `5`가 각각 `a`와 `b`에 값이 복사되고, 함수 몸체의 코드가 수행됩니다.  
`return` 지시문을 하기 전 `a + b`를 더한 값이 12번 줄에 반환되고 별다른 연산 없이 세미콜론(;)을 만나 13번줄로 이동합니다.

함수를 호출하며 데이터 값을 매개변수에 전달할 때 변수가 가지고 있는 값을 함수 내의 매개변수에 복사하는 방식을 "값에 의한 호출(call by value)"이라고 합니다.  
이 복사된 값으로 초기화된 매개변수는 별개의 변수가 되며, 매개변수의 인수로 전달되는 변수에 영향을 미치지 않습니다.

변수를 복사하지 않고, 데이터가 저장된 메모리 주소를 매개변수에 전달하는 것은 "참조에 의한 호출(call by reference)"라고 합니다.  
이런 작업은 포인터를 사용해 인수로 전달된 변수의 주소값을 전달하며, 참조자를 사용해 전달 할 수 도 있습니다.  
주소값으로 전달되었기 때문에 함수에서 값이 변경되면 인수로 전달된 변수에 영향을 미칩니다.