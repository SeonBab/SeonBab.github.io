---
layout: single

title: "[C++] 함수의 오버로딩"

categories:
    - Cpp
tag: [Cpp]

date: 2024-07-27
last_modified_at: 2024-07-27

mermaid: true
---

# 함수 오버로딩

함수 오버로딩(overloading)이란 동일한 범위에서 같은 이름의 여러 함수를 만드는 기능입니다.  

함수를 오버로딩 하기 위해서는 함수의 이름이 같고, 매개변수의 개수나 형태를 다르게 설정해주면 됩니다.  
이처럼 오버로딩에 고려되는 사항들은 다음과 같습니다.

+ 인수의 수
+ 인수 자료형
+ 줄임표의 존재 여부
+ `const` 또는 `volatile`
+ 참조 한정자(`&`, `&&`)

만약 함수를 오버로딩하려할 때 함수의 반환 자료형을 다르게 정의한다면 해당 함수는 오버로딩 되지 않습니다.
이처럼 오버로딩에 고려되지 않는 사항들은 다음과 같습니다.

+ 함수 반환 자료형
+ typedef 사용
+ 지정하지 않은 배열 범위

오버로딩을 사용한 함수의 선언 예시입니다.

```cpp
int function(int a);
float function(float a);
void function(int a, ...);
void function(int a, int b);
void function(int a, double b);
void function(int& a);
void function(const int& a);
void function(int a) const; // 멤버 함수일 때 사용 가능한 오버로딩 방법
void function(int a) volatile; // 멤버 함수일 때 사용 가능한 오버로딩 방법
```

`int function(int a);`와 `float function(float a);`와 `void function(int a, ...);`은 반환 자료형이 다르지만 인수의 자료형이 다르므로 오버로딩 할 수 있습니다.  
만약 반환 자료형만 다르다면 오버로딩 할 수 없었을 것입니다.

예시의 다른 함수들도 매개변수의 갯수나 자료형이 다르며, 줄임표를 사용하거나 참조 한정자를 사용해 구분합니다.

`void function(const int& a);`처럼 `const` 또는 `volatile`는 참조 한정자나 포인터에서만 오버로딩 할 수 있습니다.  
만약 참조 한정자를 빼고 `void function(const int a);`로 선언할 경우 `int function(int a);`의 `int a`와 같은 인수로 보기 때문에 오버로딩 될 수 없습니다.

오버로딩을 사용한 함수 정의 예시는 다음과 같습니다.

```cpp
int function(int a)
{
	++a;
	std::cout << a << std::endl;
}
float function(float a)
{
	++a;
	std::cout << a << std::endl;
}
void function(int a, int b)
{
	std::cout << a + b << std::endl;
}
```

위의 예시와 같이 일반적인 함수의 정의와 같습니다.  

오버로딩된 함수를 호출할 경우 다음과 같은 결과 중 하나가 발생합니다.

+ 모호한 함수가 있다.
+ 일치하는 함수가 있다.
+ 일치하는 함수가 없다.

위에서 함수를 선언한 예시로 함수를 호출하는 경우 예시는 다음과 같습니다.

```cpp
function('1'); // 모호한 함수가 있다.
function(1); // 모호한 함수가 있다.
function(1.4f); // 일치하는 함수가 있다.
function("1.4f"); // 일치하는 함수가 없다.
```

`function('1');`의 경우 오버라이딩 된 함수들 중 인수의 개수와 자료형이 같은 함수가 없어 표준변환을 통해 일치하는 함수를 찾으려 할 것입니다.  
이 경우 `int function(int a)`, `void function(const int& a)` 등 다른 함수들 중 어떤 함수를 호출할지 컴파일러가 선택 할 수 없어 오류가 납니다.

`function(1);`의 경우 오버라이딩 된 함수들 중 인수의 개수와 자료형이 같은 함수가 많기 때문에 어떤 함수를 호출할지 컴파일러가 선택 할 수 없어 오류가 납니다.

정확히 일치하는 항목이 없으면 C++에서는 표준변환을 통해 일치하는 함수를 찾으려 합니다.

`function(1.4f);`의 경우 `float function(float a)` 함수와 인수의 개수, 자료형이 같아 호출됩니다.

`function("1.4f");`의 경우 오버라이딩 된 함수들에서 인수의 개수는 같아도 자료형이 같지 않아 호출 할 수 있는 함수가 없습니다.