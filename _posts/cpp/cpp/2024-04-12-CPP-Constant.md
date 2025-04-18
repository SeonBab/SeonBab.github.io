---
layout: single

title: "[C++] 상수"

categories:
    - Cpp
tag: [Cpp]

date: 2024-04-12
last_modified_at: 2024-04-26

order : 30
---

# 상수

상수(constant)는 변하지 않는 값을 뜻하고, 값을 변경시키지 못하도록 제한하고 싶을때 사용합니다.

상수는 값을 바꾸는 것을 방지하고 코드의 의도를 명확하게 만들어줍니다.

처음 선언할 때 값을 할당받게되면서 그 다음부터는 값을 바꿀 수가 없습니다. 그렇기에 상수를 선언할 때 초기화(Initialization)해야 합니다.

상수에는 리터럴 상수와 심볼릭 상수가 있습니다.

## 리터럴 상수

이름이 없는 상수를 가리켜 리터럴(Literal) 상수 또는 리터럴이라고 합니다.

### 숫자,부울 및 포인터 리터럴
```
int a = 30 + 40;        // 정수 리터럴(integer literal)
double d = 108.87;      // 부동 소수점 리터럴(floating point literal)
bool c = true;          // 부울 리터럴(boolean literal)
MyClass* mc = nullptr;  // 포인터 리터럴(pointer literal)
auto x = 0B001101;      // 이진 리터럴(binary literal)
```

### 문자 리터럴
char의 일반 문자 리터럴 'a'
char8_t의 UTF-8 문자 리터럴 u8'a';
char16_t의 UTF-16 문자 리터럴 u'a';
char32_t의 UTF-32 문자 리터럴 U'a';
wchar_t의 와이드 문자 리터럴 L'a';

### 매직넘버

```
int maxStudents = numClassrooms * 30;
```

위 코드에서 30과 같은 숫자를 매직 넘버(Magic number)라고 부릅니다. 매직 넘버란 컨텍스트(context)가 없는 코드 중간에서 하드 코딩된 리터럴(일반적으로 숫자)을 말합니다. 여기서 30은 교실당 최대 학생 수를 의미한다는 것을 유추해야 하며, 30을 설명하기 위한 주석이 없으면 나중에 추론하는게 어려울 수 있습니다.

매직 넘버가 무엇에 사용되는지에 대한 컨텍스트(context)가 없을뿐더러 나중에 값을 바꿔야 할 경우 문제가 발생하기 때문에 사용하지 않는 것이 좋습니다.

## 심볼릭 상수

심볼릭(Symbolic) 상수란 변수와 마찬가지로 이름을 지니는 상수를 말하며, const 상수와 매크로가 있습니다.

### const 상수

변수 선언시 const 키워드를 추가하면 됩니다.

자료형 이전에 const 키워드를 사용하는 것이 관습이고, 자료형 이후에도 const를 사용할 수 있습니다.

컴파일 에러시 변수 이름으로 추적이 가능하며, 한번만 선언해 사용하면 되므로 많은 사본이 생기지 않고, 클래스 내부에 상수 선언이 가능해 객체지향 프로그래밍이 가능하게 됩니다.

```
const double a{ 1.3 };
int const b{ 4 };
```

### constexpr

const와 같은 방법으로 사용할 수 있으며, 해당 객체나 함수의 리턴값을 컴파일 타임에 값을 알 수 있다라는 의미를 전달합니다.

```
constexpr int a = 20;
int constexpr b = 10;
```

컴파일러가 컴파일 타임에 어떠한 식의 값을 결정할 수 있다면 해당 식을 상수식(Constant expression)이라고 표현합니다. 그리고 이러한 상수식들 중에서 값이 정수인 것을 정수 상수식(Inrtegral constant expression)이라고 하게 되는데, 쓰임새가 많습니다.

```
int a = 1;
constexpr int b = a;    // ERROR : 런타임 상수
```
constexpr의 경우 오른쪽에 반드시 다른 상수식이 와야 합니다. 하지만 컴파일 타임 시에 a가 뭐가 올 지 알 수 없습니다. 따라서 위 코드는 컴파일 오류가 됩니다.

const 객체가 상수식으로 초기화 되었다 하더라도 컴파일러에 따라 이를 런타임에 초기화 할지, 컴파일에 초기화할지 다를 수 있습니다. 따라서 컴파일 상수를 확실히 사용하고 싶다면 constexpr 키워드를 꼭 사용해야 합니다.