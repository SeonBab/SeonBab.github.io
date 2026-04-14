---
layout: single

title: "[C++] 빈 객체의 크기"

categories:
    - Cpp
tag: [Cpp]

date: 2026-04-14
last_modified_at: 2026-04-14

order : 255
---

# 빈 객체의 크기

C++에서 객체는 `struct`, `class`, `union` 등이 있는데, 해당 객체들이 비어있을 때, 상속 될 때 등 다양한 상황에서의 메모리 구조를 이해하는 것이 좋습니다.

예를 들어, 구조체나 클래스 등 객체에 아무 멤버 변수가 없다면 해당 객체가 할당받는 메모리의 크기가 어떻게 될지 알아야합니다.

예시 코드는 다음과 같습니다.

```cpp
#include <iostream>

class MyClass { };

struct MyStruct { };

union MyUnion { };

int main()
{
    std::cout << sizeof(MyClass) << std::endl;  // 1 출력
    std::cout << sizeof(MyStruct) << std::endl; // 1 출력
    std::cout << sizeof(MyUnion) << std::endl;  // 1 출력

    return 0;
}
```

해당 코드를 실행해 보면, 모두 사이즈의 크기가 1이 출력됩니다.  
즉, 멤버 변수와 데이터가 없는 비어있는 객체지만 최소 크기가 1이라는 것을 알 수 있습니다.

그 이유는 메모리에서 두 객체를 구분하려면 서로 다른 주소를 가져야 하는데, 크기가 0일 경우 동일한 주소를 가지게 되는 경우가 생겨 최소 1바이트 이상의 크기를 가지도록 정의되어 있습니다.

그렇다면, 다음과 같은 코드처럼 비어있는 객체를 기저(부모) 객체로서 활용한다면 어떻게 될지 예측해 볼 수 있습니다.

```cpp
#include <iostream>

class MyClass { };
class MyClass2 : public MyClass { };

struct MyStruct { };
struct MyStruct2 : public MyStruct { };

int main()
{
    std::cout << sizeof(MyClass) << std::endl;  // 1 출력
    std::cout << sizeof(MyClass2) << std::endl; // 1 출력

    std::cout << sizeof(MyStruct) << std::endl;  // 1 출력
    std::cout << sizeof(MyStruct2) << std::endl; // 1 출력

    return 0;
}
```

파생 클레스에 추가적인 멤버가 없다면, 결과적으로 비어있는 클래스와 동일하게 취급되어 최소 크기인 1바이트를 가집니다.

빈 기저 클래스는 컴파일러 최적화에 의해 EBO(Empty Base Optimization)에 의해 실제 객체의 메모리에서 공간을 차지하지 않을 수 있지만 항상 보장되는 것은 아닙니다.

다음으로, 자식 객체에 멤버 변수가 있을 경우에 대해 알아보겠습니다.

```cpp
#include <iostream>

class MyClass { };
class MyClass2 : public MyClass
{
    int a;
};

struct MyStruct { };
struct MyStruct2 : public MyStruct
{
    int a;
};

int main()
{
    std::cout << sizeof(MyClass) << std::endl;  // 1 출력
    std::cout << sizeof(MyClass2) << std::endl; // 4 출력

    MyClass2 MyClassInstance;
    std::cout << sizeof(MyClassInstance) << std::endl;  // 4 출력

    std::cout << std::endl;

    std::cout << sizeof(MyStruct) << std::endl;     // 1 출력
    std::cout << sizeof(MyStruct2) << std::endl;    // 4 출력

    MyStruct2 MyStructInstance;
    std::cout << sizeof(MyStructInstance) << std::endl; // 4 출력

    return 0;
}
```

멤버 변수를 가지게 되는 경우 더이상 객체가 필요한 메모리 크기가 0이 아니기 때문에 가지고 있는 멤버 변수 `int`에 대한 4바이트를 할당 받아 객체의 크기가 4바이트가 됩니다.