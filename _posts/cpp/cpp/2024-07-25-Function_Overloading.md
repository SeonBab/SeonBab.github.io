---
layout: single

title: "[C++] 함수의 오버로딩"

categories:
    - Cpp
tag: [Cpp]

date: 2024-07-25
last_modified_at: 2024-12-31

mermaid: true

order : 160
---

# 함수 오버로딩

함수 오버로딩(overloading)이란 동일한 범위에서 같은 이름을 가진 여러 개의 함수를 만드는 기능입니다.

함수를 오버로딩 하기 위해서는 함수의 이름이 같고, 매개변수의 개수나 자료형을 다르게 설정해주면 됩니다.  
매개변수의 개수나 자료형처럼 오버로딩에 고려되는 사항들은 다음과 같습니다.

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

반환 자료형은 컴파일러가 함수 호출을 식별하는 기준이 아니기 때문에 반환 자료형만으로는 오버로딩 조건이 성립하지 않습니다.

`typedef`는 기존 자료형에 대한 별칭이므로, 실제 자료형을 변경하지 않습니다.

## 함수 선언 및 정의

오버로딩을 사용한 함수의 선언 예시입니다.

```cpp
int function(int a);
float function(float a);
void function(int a, ...);
void function(int a, int b);
void function(int a, double b);
void function(float a, float b);
void function(int& a);
void function(const int& a);
void function(int a) const; // 멤버 함수일 때 사용 가능한 오버로딩 방법
void function(int a) volatile; // 멤버 함수일 때 사용 가능한 오버로딩 방법
```

`int function(int a);`와 `float function(float a);`와 `void function(int a, ...);`은 매개변수의 자료형이 다르므로 오버로딩 할 수 있습니다.  
만약 반환 자료형만 다르다면 오버로딩 할 수 없었을 것입니다.

예시의 다른 함수들도 매개변수의 개수나 자료형이 다르며, 줄임표를 사용하거나 참조 한정자를 사용해 구분합니다.

`void function(const int& a);`처럼 `const` 또는 `volatile`는 참조 한정자나 포인터에서만 오버로딩 할 수 있습니다.  
만약 참조 한정자를 빼고 `void function(const int a);`로 선언할 경우 `int function(int a);`의 `int a`와 같은 인수로 보기 때문에 오버로딩 될 수 없습니다.

오버로딩을 사용한 함수 정의 예시는 다음과 같습니다.

```cpp
void function(int a)
{
	++a;
	std::cout << "int: " << a << std::endl;
}
void function(float a)
{
	++a;
	std::cout << "float: " << a << std::endl;
}
void function(int a, int b)
{
	std::cout << "int, int: " << a + b << std::endl;
}
void function(float a, float b)
{
	std::cout << "float, float: " << a + b << std::endl;
}
```

위의 예시와 같이 일반적인 함수의 정의와 같습니다.  

## 함수 호출

오버로딩된 함수를 호출할 경우 다음과 같은 결과 중 하나가 발생합니다.

+ 모호한 함수가 있다.
+ 일치하는 함수가 있다.
+ 일치하는 함수가 없다.

정확히 일치하는 함수가 없으면 C++에서는 표준변환을 통해 일치하는 함수를 찾으려 합니다.  
자세한 설명은 링크로 대체하겠습니다.  
[C++ 형변환]({{ "cpp/TypeConversion/" | relative_url }}){: target="_blank"}

이때 표준변환 등으로 일치하는 함수가 2개 이상이라면 컴파일 오류가 발생합니다.

위에서 정의한 함수를 사용해 호출하는 경우 예시는 다음과 같습니다.

```cpp
function(1, 1.4f); // 모호한 함수가 있다.
function("1.4f"); // 일치하는 함수가 없다.
function('1'); // 일치하는 함수가 있다.
function(1); // 일치하는 함수가 있다.
function(1.4f); // 일치하는 함수가 있다.
function(1, 1.4); // 일치하는 함수가 있다.
```

`function(1, 1.4f);`의 경우 오버로딩 된 함수들 중 인수의 개수와 자료형이 같은 함수를 찾습니다.  
하지만 일치하는 함수가 없으므로 표준변환을 통해 일치하는 함수를 찾는데, 이때 매개변수를 `int`로 캐스팅할지 `float`로 캐스팅할지 모호해 오류가 발생합니다.

`function("1.4f");`의 경우 오버로딩 된 함수들에서 인수의 개수는 같아도 자료형이 같지 않아 호출 할 수 있는 함수가 없습니다.

`function('1');`의 경우 오버로딩 된 함수들 중 인수의 개수와 자료형이 같은 함수가 없어 표준변환을 통해 `int function(int a)`함수가 호출됩니다.

`function(1);`의 경우 오버로딩 된 함수들 중 인수의 개수와 자료형이 같은 함수가 `int function(int a)`이므로 해당 함수가 호출됩니다.

`function(1.4f);`의 경우 `float function(float a)` 함수와 인수의 개수, 자료형이 같아 호출됩니다.

`function(1, 1.4);`의 경우 첫 매개변수는 `int`자료형이고, 두번째 매개변수는`double`이므로 표준변환을 통해 `void function(float a, float b)`가 호출됩니다.