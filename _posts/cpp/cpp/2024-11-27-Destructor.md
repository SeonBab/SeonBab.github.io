---
layout: single

title: "[C++] 소멸자"

categories:
    - Cpp
tag: [Cpp]

date: 2024-11-27
last_modified_at: 2024-11-27

order : 230
---

# 소멸자

소멸자(Destructor)는 클래스의 객체가 소멸될 때 자동으로 호출되는 클래스의 멤버함수입니다.

소멸자의 주요 목적은 객체의 리소스를 정리해 메모리를 해제하는 것입니다.  
이를 통해 자원 누수를 방지하는 중요한 기능을 수행합니다.

소멸자를 사용하는 경우의 예시는 다음과 같습니다.

+ `new`를 통해 동적 할당한 메모리를 해제할 때 사용합니다.
+ 파일을 여는 작업을 했다면, 파일을 닫아줄 때 사용합니다.
+ 네트워크 소켓을 여는 작업을 했다면, 소켓을 닫을 때 사용합니다.
+ 멀티 스레드 환경에서 락을 사용했다면, 락을 해제할 때 사용합니다.

## 소멸자 정의

소멸자의 정의에는 다음과 같은 규칙이 있습니다.

+ 소멸자 또한 접근제한자의 영향을 받습니다. 예시로 `protected`일 경우 상속 관계에서만 소멸자가 호출될 수 있습니다.
+ 소멸자의 이름은 클래스 이름과 동일해야 하지만, 앞에 `~`기호가 붙습니다.
+ 반환 타입이 없고, 명시하지도 않아 void를 지정하지도 않습니다. 따라서 값을 반환하지도 않습니다.
+ 소멸자는 인자를 받을 수 없습니다. 즉 매개변수를 가지지 않고, 오버로딩도 불가능합니다.
+ 클래스는 가상 소멸자를 가질 수 있습니다.

소멸자를 가진 클래스의 예시는 다음과 같습니다.

```cpp
#include <iostream>

class Dog
{
public:
    ~Dog()
    {
        std::cout << "Dog 소멸자 호출" << std::endl;
    }
};

int main()
{
    Dog Dog;
}
```

출력은 다음과 같습니다.

![Destructor-Call]({{site.url}}/images/cpp/cpp/2024-11-27-Destructor/Destructor-Call.PNG)

`Dog`클래스의 소멸자가 정상적으로 호출된 것을 알 수 있습니다.

만약 정의된 생성자가 없다면 컴파일러가 자동으로 기본 소멸자를 생성해 줍니다.  
이때 기본 소멸자는 동적으로 할당된 자료형, 네트워크 소켓 등을 정리해 주지 않으므로 메모리 누수가 발생할 수 있습니다.

## 가상 소멸자

기존의 소멸자는 부모 클래스 포인터에서 자식 클래스 객체를 삭제할 때 부모 클래스의 소멸자만 호출되고 자식 클래스의 소멸자는 호출되지 않는 문제가 있습니다.  
이 경우 메모리 누수나 리소스 관리 문제가 발생할 수 있습니다.

해당 문제에 대한 예시는 다음과 같습니다.

```cpp
#include <iostream>

class Animal
{
public:
    ~Animal()
    {
        std::cout << "Animal 소멸자 호출" << std::endl;
    }
};

class Dog : public Animal
{
public:
    ~Dog()
    {
        std::cout << "Dog 소멸자 호출" << std::endl;
    }
};

int main()
{
    Animal* Obj = new Dog();
    delete Obj;
}
```

출력은 다음과 같습니다.

![Destructor-CallOrderProblematicIssue]({{site.url}}/images/cpp/cpp/2024-11-27-Destructor/Destructor-CallOrderProblematicIssue.PNG)

출력을 보면 `Dog`클래스의 소멸자는 호출되지 않았고, `Animal`클래스의 소멸자만 호출된 것을 알 수 있습니다.

위의 문제를 해결하기 위해 `virtual`키워드를 소멸자 앞에 사용해 가상 함수를 만들어 줍니다.  
부모 클래스의 소멸자에 `virtual`키워드를 사용해 문제를 해결한 예시는 다음과 같습니다.

```cpp
#include <iostream>

class Animal
{
public:
    virtual ~Animal()
    {
        std::cout << "Animal 소멸자 호출" << std::endl;
    }
};

class Dog : public Animal
{
public:
    ~Dog()
    {
        std::cout << "Dog 소멸자 호출" << std::endl;
    }
};

int main()
{
    Animal* Obj = new Dog();
    delete Obj;
}
```

출력은 다음과 같습니다.

![Destructor-CallOrderProblemSolving]({{site.url}}/images/cpp/cpp/2024-11-27-Destructor/Destructor-CallOrderProblemSolving.PNG)

출력을 보면 소멸자가 정상적으로 호출된 것을 알 수 있습니다.

## 소멸자의 호출 시점

소멸자는 객체의 생명 주기가 끝날 때 자동으로 호출됩니다.  
주요 시점은 다음과 같습니다.

+ 로컬 객체가 지역 범위를 벗어날 때.
+ `delete` 연산자를 호출해 메모리를 해제할 때.
+ 객체가 포함된 클래스의 소멸자가 호출될 때.
+ 스마트 포인터가 범위를 벗어나거나 해제될 때.
+ 예외 발생으로 스택이 해제될 때.

클래스가 상속됐을 때 자식 클래스의 소멸자가 호출된 후 부모 클래스의 소멸자 순서로 호출됩니다.  
클래스의 자원을 해제하기 전에 하위 클래스의 자원을 먼저 해제해야 하기 때문에 그렇습니다.  
예시는 다음과 같습니다.

```cpp
#include <iostream>

class Animal
{
public:
    ~Animal()
    {
        std::cout << "Animal 소멸자 호출" << std::endl;
    }
};

class Dog : public Animal
{
public:
    ~Dog()
    {
        std::cout << "Dog 소멸자 호출" << std::endl;
    }
};

int main()
{
    Dog Dog;
}
```

출력은 다음과 같습니다.

![Destructor-CallOrder]({{site.url}}/images/cpp/cpp/2024-11-27-Destructor/Destructor-CallOrder.PNG)

출력을 보면 위에서 설명한 바와 같이 자식 클래스의 소멸자가 먼저 호출되고, 부모 클래스의 소멸자가 호출된다는 것을 알 수 있습니다.