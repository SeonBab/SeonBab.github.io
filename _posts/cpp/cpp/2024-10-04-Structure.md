---
layout: single

title: "[C++] 구조체"

categories:
    - Cpp
tag: [Cpp]

date: 2024-10-04
last_modified_at: 2024-10-04
---

# 구조체

구조체(struct)는 여러 데이터들을 묶어서 새로운 자료형을 만들어 내는 사용자 정의 자료형입니다.

멤버의 접근제한과 상속 시 기본 public으로 되는 것 외, 구조체와 클래스는 대부분 같습니다.

변수들의 메모리 저장공간을 동시에 할당받고, 프로그램 내에서 연관성을 유지하며 저장공간을 사용하기 위해서 사용됩니다.  
구조체의 메모리 배치는 멤버의 순서와 데이터 타입에 따라 달리지며, 메모리 패딩이 추가되기도 합니다.

## 구조체 정의

구조체는 기본적으로 다음과같이 정의합니다.

```cpp
struct MyStruct
{
    int myint;
    float myfloat = 1.f;
    std::string mystring;
};
```

구조체를 정의 할 때 끝에 세미콜론(;)을 잊지 않아야합니다.

`MyStruct`의 변수들 즉 `myint`, `myfloat`등은 멤버(member) 또는 필드(field)라고 부릅니다.

구조체에서도 생성자와 멤버 함수를 정의할 수 있습니다.

```cpp
struct MyStruct
{
    int myint;
    float myfloat;

    MyStruct(int mint, float mfloat) : myint(mint), myfloat(mfloat) {}

    void PrintMyValue()
    {
        std::cout << "myint : " << myint << "myfloat : " << myfloat << std::endl;
    }
};
```

구조체에서도 상속을 받을 수 있습니다.

```cpp
struct A
{
    int AData;
};

struct B : public A
{
    void PrintAData()
    {
        std::cout << "AData : " << AData << std::endl;
    }
};
```

구조체끼리의 포함도 할 수 있습니다.  
중첩된 구조체(Nested structs)라고도 합니다.

```cpp
struct A
{
    int AData = 3;
};

struct B
{
    A AStruct;

    void PrintAData()
    {
        std::cout << "AStruct.AData : " << AStruct.AData << std::endl;
    }
};
```

## 사용법

위에서 알아본 방법으로 만든 구조체를 사용하기 위해서는 다음과 같습니다.

```cpp
struct A
{
    int AData = 3;
};

struct B
{
    A AStruct;

    void PrintAData()
    {
        std::cout << "AStruct.AData : " << AStruct.AData << std::endl;
    }
};

int main()
{
    A Astruct1;
    Astruct1.AData = 1;

    A Astruct2{15};

    B Bstruct;
    Bstruct.AStruct.AData = 5;

    Bstruct.PrintAData();
}
```

이처럼 해당 구조체의 변수를 선언한 뒤 멤버에 접근 할 때에는 멤버 선택 연산자(.)를 사용하면 됩니다.  
혹은 구조체 변수를 선언하면서 유니폼 초기화를 사용할 수 있습니다.

중첩된 구조체의 경우 `Bstruct.AStruct.AData`와 같이 멤버 선택 연산자를 2번 사용하면 됩니다.