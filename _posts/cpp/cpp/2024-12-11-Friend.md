---
layout: single

title: "[C++] 프렌드"

categories:
    - Cpp
tag: [Cpp]

date: 2024-12-11
last_modified_at: 2024-12-11

order : 240
---

# 프렌드

프렌드(friend)란 외부에서 접근할 수 없는 `private` 또는 `protected` 멤버에 접근할 수 있는 특수한 권한을 부여하는 데 사용됩니다.

프렌드로 선언된 함수나 클래스는 해당 클래스의 멤버가 아닙니다.  
즉, 객체의 범위 외부에서 호출됩니다.

코드의 모듈화와 재사용성을 높이는데 도움이됩니다.

프렌드는 캡슐화를 약화시킬 수 있으므로 신중히 사용해야합니다.  
또한 크래스 구조에서 의존성을 증가시킬 수 있습니다.

한 클래스가 다른 클래스의 프렌드라고 해서 반대의 경우도 프렌드가 되는 것은 아닙니다.  
한쪽이 프렌드를 선언한다고 해도, 양방향으로 접근이 가능하지 않습니다.

대표적으로 연산자 오버로딩을 통해 두 클래스 간 데이터를 직접 비교하거나 연산해야할 때 유용합니다.

## 프렌드 사용법

`friend`키워드는 접근 제어자에 영향을 받지 않습니다.  
하지만 일반적으로 가시성을 위해 `public`에서 프렌드를 선언합니다.

특정 클래스의 `friend`로 선언된 함수를 프렌드 함수라고 부릅니다.  
프렌드 함수를 선언하는 방법은 다음과 같습니다.

```cpp
// 전역 함수
friend 함수명(매개변수);
// 클래스 함수
friend 클래스명::함수명(매개변수);
```

전역 함수를 대상으로 프렌드 키워드를 사용할 경우 다음과 같습니다.

```cpp
#include <iostream>

// 전방선언
class A;

void Print(A classA);

class A
{
private:
    int x;

public:
    A(int n) : x(n) {}

    // 프렌드로 선언
    friend void Print(A classA);
};

void Print(A classA)
{
    // A클래스의 멤버 변수에 접근
    std::cout << classA.x << std::endl;
}

int main()
{
    A ClassA(3);

    Print(ClassA);
}
```

전역 함수의 경우 프렌드를 선언하는 `A`클래스보다 전역 함수의 원형이 먼저 선언되어 있어야 합니다.  
이 경우 `A`클래스의 정의가 없으므로 전역 함수보다 먼저 `A`클래스를 선언해주어야합니다.  
그 이유는 컴파일러가 `print`함수를 알 수 있도록 하기 위함입니다.

클래스 함수를 대상으로 프렌드 키워드를 사용할 경우 다음과 같습니다.

```cpp
#include <iostream>

// 전방선언
class A;

class B
{
public:
    // 함수 원형
    void Print(A classA);
};

class A
{
private:
    int x;

public:
    A(int n) : x(n) {}

    // 프렌드로 선언
    friend void B::Print(A classA);
};

void B::Print(A classA)
{
    // A클래스의 멤버 변수에 접근
    std::cout << classA.x << std::endl;
}

int main()
{
    A ClassA(3);
    B ClassB;

    ClassB.Print(ClassA);
}
```

프렌드 함수의 경우 프렌드를 선언하는 `A`클래스보다 `B` 클래스의 함수 원형이 먼저 선언되어 있어야 합니다.  
이 경우 `A`클래스의 정의가 없으므로 `B` 클래스보다 먼저 `A`클래스를 선언해주어야합니다.  
그 이유는 컴파일러가 `B::print`함수를 알 수 있도록 하기 위함입니다.

특정 클래스를 `friend`로 선언할 경우 프렌드 클래스라고 부릅니다.
프렌드 클래스를 선언하는 방법은 다음과 같습니다.

```cpp
friend class 클래스명;
```

```cpp
#include <iostream>

class A
{
private:
    int x;

public:
    A(int n) : x(n) {}
    friend class B;
};

class B
{
public:
    void Print(A classA)
    {
        std::cout << classA.x << std::endl;
    }
};

int main()
{
    A ClassA(3);
    B ClassB;

    ClassB.Print(ClassA);
}
```

## 연산자 오버로딩

프렌드 함수는 이진 연산자의 오버로딩에서 유용합니다.  
연산자 함수가 두 객체의 `private` 멤버를 비교하거나 조작할 때, 객체의 멤버 함수로만 처리하기 어려운 경우가 있기 때문입니다.


연산자를 오버로딩하고, 프렌드 함수를 사용하는 경우는 다음과 같습니다.

```cpp
#include <iostream>

class A
{
private:
    int x;

public:
    A(int n) : x(n) {}

    friend A operator+(const A& a1, const A & a2);

    void Print()
    {
        // A클래스의 멤버 변수에 접근
        std::cout << x << std::endl;
    }

    // 프렌드로 선언
    friend void Print(A classA);
};

A operator+(const A& a1, const A& a2)
{
    return A(a1.x + a2.x);
}

int main()
{
    A ClassA1(3);
    A ClassA2(5);

    A ClassA3 = ClassA1 + ClassA2;

    ClassA3.Print();
}
```