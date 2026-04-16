---
layout: single

title: "[C++] 가상 함수와 순수 가상 함수"

categories:
    - Cpp
tag: [Cpp]

date: 2024-12-23
last_modified_at: 2026-04-15

order : 260
---

# 가상 함수

가상 함수(Virtual Function)은 부모 클래스에서 선언되어 자식 클래스에서 재정의될 것을 의도하는 멤버 함수입니다.

일반적인 멤버 함수는 컴파일 시점에 호출 대상이 결정되는 정적 바인딩을 사용합니다.  
반면 가상 함수는 실행 시점에 객체의 실제 타입을 기준으로 호출 대상이 결정되는 동적 바인딩을 사용합니다.  
다만, 호출 대상이 컴파일 시점에 명확한 경우에는 컴파일러가 동적 바인딩을 생략하고 정적으로 처리할 수 도 있습니다.

부모 클래스 타입의 포인터나 참조를 통해 호출될 경우 동적 바인딩을 합니다.

이러한 메커니즘을 통해 다형성이 구현됩니다.

이 동작은 가상 함수 테이블(V-Table/Virtual Table)을 통해 구현됩니다.

## 가상 함수 테이블

가상 함수 테이블(V-Table/Virtual Table)이란 클래스에 선언된 가상 함수들의 주소를 저장하고 관리하기 위한 테이블입니다.

가상 함수는 일반 함수와 달리 실행 시점에 호출 대상이 결정되기 때문에, 이를 위해 내부적으로 가상 함수 테이블을 사용하여 함수 주소를 관리합니다.

동작 방식은 다음과 같습니다.

- 각 클래스는 자신이 가진 가상 함수들의 주소를 저장하는 가상 함수 테이블을 가집니다.
- 객체가 생성되면, 해당 객체는 자신의 클래스에 대응되는 가상 함수 테이블을 가리키는 포인터(VPTR)를 포함합니다.
- 가상 함수가 호출되면, 객체의 VPTR을 통해 가상 함수 테이블에 접근하고, 해당 함수의 주소를 찾아 호출합니다.

특징은 다음과 같습니다.

- 각 클래스는 자신의 가상 함수 목록에 대응되는 V-Table을 가집니다.
	+ 그로 인한 오버헤드가 적용되지만, 컴파일러의 최적화로 인해 일반적인 상황에서는 성능 차이가 크지 않습니다.
- 일반적인 단일 상속 환경에서는 객체가 자신의 클래스에 대응되는 VPTR을 통해 V-Table에 접근합니다.
- VPTR은 일반적으로 객체의 메모리 시작 부분에 위치합니다.
- 생성 중에는 현재 생성이 완료된 클래스 단계에 맞는 V-Table이 설정됩니다.
- 소멸 중에는 아직 유효한 클래스 단계에 맞는 V-Table이 다시 변경됩니다.

다만, VPTR의 위치나 V-Table의 구체적인 구조는 C++ 표준이 보장하지 않으며, 컴파일러 및 ABI에 따라 달라질 수 있습니다.

객체의 VPTR은 생성 시 한 번만 설정되는 것이 아니라, 생성 과정과 소멸 과정에 따라 단계적으로 변경될 수 있습니다.

### 가삼 함수 테이블의 동작 과정

다음 코드는 일반적인 구현에서 VPTR을 관찰하기 위한 예제입니다.

```cpp
#include <iostream>

class BaseClass
{
public:
    BaseClass()
    {
        PrintVPtr("BaseClass Constructor");
    }

    virtual ~BaseClass()
    {
        PrintVPtr("BaseClass Destructor");
    }

    virtual void Print() const
    {
        PrintVPtr("BaseClass::Print");
    }

protected:
    void PrintVPtr(const char* label) const
    {
        // 일반적인 구현을 관찰하기 위한 실험용 코드
        void* const* vptr = reinterpret_cast<void* const*>(this);

        std::cout << label
            << " | this: " << this
            << " | vtable: " << *vptr
            << std::endl;
    }
};

class DerivedClass : public BaseClass
{
public:
    DerivedClass()
    {
        PrintVPtr("DerivedClass Constructor");
    }

    ~DerivedClass() override
    {
        PrintVPtr("DerivedClass Destructor");
    }

    void Print() const override
    {
        BaseClass::Print();
        PrintVPtr("DerivedClass::Print");
    }
};

int main()
{
    DerivedClass derived;

    std::cout << std::endl;

    derived.Print();

    std::cout << std::endl;

    return 0;
}
```

이 코드에서는 객체의 시작 주소를 통해 VPTR이 가리키는 V-Table 주소를 출력하여, 생성 및 소멸 과정에서의 변화를 확인합니다.

![Virtual-VPTR]({{site.url}}/images/cpp/cpp/2024-12-23-CPP-Virtual/Virtual-VPTR.png)

즉, 생성 과정에서는 기반 클래스에서 파생 클래스로 올라가며 VPTR이 변경되고, 소멸 과정에서는 반대로 파생 클래스에서 기반 클래스로 내려가며 VPTR이 다시 변경됩니다.

이는 생성되지 않았거나 이미 소멸된 파생 클래스 영역에 접근하는 것을 방지하기 위한 동작입니다.

## 가상 함수를 사용하는 이유

다음 코드를 통해 정적 바인딩의 한계를 확인할 수 있습니다.

```cpp
#include <iostream>

class Parent
{
public:
	void Print()
	{
		std::cout << "Parent" << std::endl;
	}
};

class Child : public Parent
{
public:
	void Print()
	{
		std::cout << "Child" << std::endl;
	}
};

int main()
{
	Parent* pPointer = new Parent;
	Child* cPointer = new Child;

	pPointer->Print();
	cPointer->Print();

	pPointer = cPointer;

	pPointer->Print();
}
```

출력은 다음과 같습니다.

![Virtual-Prablem]({{site.url}}/images/cpp/cpp/2024-12-23-CPP-Virtual/Virtual-Prablem.PNG)

이 예제를 마지막 호출에서 `pPointer`는 `Child` 객체를 가리키고 있음에도 불구하고, `Parent::Print()`가 호출됩니다.

이는 함수 호출이 포인터의 타입(`Parent*`)을 기준으로 컴파일 시점에 결졍되기 때문입니다.

이러한 문제는 단순히 함수 호출의 문제를 넘어서, 소멸자 호출이 올바르게 이루어지지 않는 심각한 문제로 이어질 수 있습니다.

## 가상 함수 사용법

가상 함수는 `virtual`키워드를 반환 자료형 앞에 명시하여 선언합니다.

```cpp
class Parent
{
public:
	virtual void Print()
	{
		std::cout << "Parent" << std::endl;
	}
};
```

이렇게 선언된 함수는 파생 클래스에서 재정의될 경우, 객체의 실제 타입에 따라 호출 대상이 결정됩니다.

부모 클래스에서 가상 함수를 선언하면, 자식 클래스에서 재정의된 함수도 자동으로 가상 함수가 됩니다.  
그렇기 때문에 자식 클래스에서는 `virtual`키워드를 사용하지 않아도 되지만, 명시적으로 사용하여 가상 함수라는 것을 명확하게 해주는 것이 좋습니다.

## 오버라이딩

오버라이딩(Overriding)은 기반 클래스의 가상 함수를 파생 클래스에서 동일한 형태로 재정의하는 것을 의미합니다.

오버라이딩이 성립하기 위해서는 다음 조건을 만족해야합니다.

- 함수 이름이 동일해야 한다.
- 매개변수 목록이 동일해야 한다.
- 반환형이 동일해야 한다.
	+ 공변 반환형은 예외적으로 허용합니다.

```cpp
class Child : public Parent
{
public:
	void Print() override
	{
		std::cout << "Child" << std::endl;
	}
};
```

이 경우 `Child::Print()`는 `Parent::Print()`를 오버라이딩한 함수가 됩니다.

오버라이딩 한 후 출력은 다음과 같습니다.

![Virtual-VirtualFunctionCall]({{site.url}}/images/cpp/cpp/2024-12-23-CPP-Virtual/Virtual-VirtualFunctionCall.PNG)

`pPointer`에서 `Parent::Print()`가 아닌 `Child::Print()`를 호출하는 것을 알 수 있습니다.

## override 키워드

`override`키워드는 해당 함수가 기반 클래스의 가상 함수를 재정의하고 있음을 명시적으로 나타냅니다.

`void Print() override;` 이 키워드를 사용하면 컴파일러가 다음 사항을 검사합니다.

- 실제로 재정의 가능한 가상 함수인지
- 함수 시그니처가 정확히 일치하는지

조건을 만족하지 않을 경우 컴파일 오류가 발생하므로, 오버라이딩 과정에서 발생할 수 있는 실수를 예방할 수 있습니다.

![Virtual-OverrideError]({{site.url}}/images/cpp/cpp/2024-12-23-CPP-Virtual/Virtual-OverrideError.PNG)

# 순수 가상 함수

가상 함수 뒤에 `= 0`을 붙이면 순수 가상 함수(Pure Virtual Function)가 됩니다.

```cpp
class Parent
{
public:
	virtual void Print() = 0;
};
```

이 함수는 구현을 가지지 않으며, 파생 클래스에서 반드시 재정의 해야 합니다.

하나 이상의 순수 가상 함수를 포함하는 클래스는 추상 클래스(Abstract Class)가 됩니다.  
추상 클래스는 객체를 직접 생성할 수 없고, 모든 순수 가상 함수를 재정의한 파생 클래스만 인스턴스 생성이 가능합니다.

C++에는 별도의 `interface`키워드가 없기 때문에 순수 가상 함수로만 구성된 추상 클래스로 구현합니다.

# final 키워드

`final` 키워드는 클래스의 상속 또는 가상 함수의 오버라이딩을 더 이상 허용하지 않도록 제한하는 키워드입니다.

상속 구조를 더 이상 확장하지 못하도록 제한하고, 의도하지 않은 오버라이딩을 방지하기 위해 사용합니다.  
또한, 설계 의도를 명확하게 표현하여 클래스 또는 함수의 동작을 고정합니다.

이 키워드는 다음과 같이 적용할 수 있습니다.

1. 클래스에 사용

```cpp
class Child final : public Parent
```

클래스에 `final`을 붙이면, 해당 클래스는 더 이상 상속될 수 없다.

즉, 다음과 같은 코드는 컴파일 오류가 발생합니다.

```cpp
class GrandCHild : public Child
{
};
```

2. 가상 함수에 사용

```cpp
class Parent
{
public:
	virtual void Print() final;
};
```

가상 함수에 `final`을 붙이면, 해당 함수는 파생 클래스에서 더 이상 오버라이딩할 수 없습니다.

그래서 다음과 같은 코드는 컴파일 오류가 발생합니다.

```cpp
class Child : public Parent
{
public:
	void Print() override;
};
```