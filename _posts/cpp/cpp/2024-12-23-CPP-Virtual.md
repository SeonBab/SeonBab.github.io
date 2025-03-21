---
layout: single

title: "[C++] 가상 함수와 순수 가상 함수"

categories:
    - Cpp
tag: [Cpp]

date: 2024-12-23
last_modified_at: 2024-12-23

order : 260
---

# 가상 함수

가상 함수(Virtual Function)은 자식 클래스에서 재정의될 것을 기대하는 멤버 함수입니다.

가상 함수는 일반적인 함수와 다르게 정적 바인딩을 하지 않고, 프로그램이 실행 될 때 객체를 결정하므로, 자신을 호출하는 객체의 동적 타입에 따라 실제 호출할 함수가 결정됩니다.  
동적 바인딩(Dynamic Binding)또는 지연 바인딩(Late Binding)이라고 합니다.
가상 함수 테이블(Virtual Table, V-Table)이라는 메커니즘으로 구현됩니다.

가상 함수 테이블이란 각 클래스의 가상 함수를 관리해주며, 가상 함수의 주소가 저장되는 곳입니다.  
객체가 생성되면 가상 함수 테이블에 대한 포인터(VPTR)가 객체에 포함되어, 이를 통해 동적 바인딩이 가능합니다.  
그러므로 정적 바인딩보다 약간의 오버헤드가 발생하지만, 컴파일러가 최적화 해주므로 실제 성능 차이가 크지 않습니다.

가상 함수도 타입이 분명할 경우 일반 함수와 같이 정적 바인딩을 합니다.  
부모 클래스 타입의 포인터나 참조를 통해 호출될 경우 동적 바인딩을 합니다.

## 가상 함수를 사용하는 이유

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

이 예제를 보면 각 타입에 각 객체의 주소를 넣고 함수를 호출 시키면 각 클래스에서 정의된 `Print`함수를 호출합니다.  
하지만 `pPointer`에 `cPointer`의 주소를 넣고 똑같이 함수를 호출하면, `cPointer`의 `Print`함수가 아닌 `pPointer`의 함수가 호출되는 것을 알 수 있습니다.

이 문제는 원하는 함수를 호출하지 못한다는 문제가 생길 수 도 있지만, 원하는 소멸자가 호출되지 않을 수 있다는 문제가 있습니다.

## 가상 함수 사용법

가상 함수는 `virtual`키워드를 사용하여 선언합니다.

```cpp
virtual 반환자료형 함수명;
virtual void Parent Print();
```

```cpp
#include <iostream>

class Parent
{
public:
	virtual void Print()
	{
		std::cout << "Parent" << std::endl;
	}
};

class Child : public Parent
{
public:
	virtual void Print() override
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

![Virtual-VirtualFunctionCall]({{site.url}}/images/cpp/cpp/2024-12-23-CPP-Virtual/Virtual-VirtualFunctionCall.PNG)

사용하는 이유의 코드와 똑같지만 함수 앞에 `virtual`키워드를 사용해 가상함수로 만들어 주었습니다.

그 결과 `pPointer`에서 `Parent`클래스의 `Print`가 아닌 `Child`클래스의 `Print`함수를 호출하는 것을 알 수 있습니다.

부모 클래스에서 가상 함수를 선언하면, 자식 클래스에서 재정의된 함수도 자동으로 가상 함수가 됩니다.  
하지만 자식 클래스의 함수에도 `virtual`키워드를 명시적으로 사용하여 가상 함수라는 것을 명확히 해주는 것이 좋습니다.

`override`키워드는 명시적으로 가상 함수를 재정의 한다는 것을 나타내는 키워드입니다.  
이 키워드를 사용할 경우 컴파일러가 오버라이딩이 가능한 함수인지, 함수 이름과 매개 변수등이 잘 맞는지 등을 확인해줍니다.  
만약 재정의 하지 않는다면 오류를 띄워줍니다.

![Virtual-OverrideError]({{site.url}}/images/cpp/cpp/2024-12-23-CPP-Virtual/Virtual-OverrideError.PNG)

# 순수 가상 함수

가상 함수 뒤에 `= 0`을 붙이면 순수 가상 함수(Pure Virtual Function)가 됩니다.

```cpp
virtual 반환자료형 함수명 = 0;
virtual void Parent Print() = 0;
```

가상 함수와 다르게 자식 클래스에서 재정의를 강제합니다.  
만약 재정의를 하지 않을 경우 오류를 띄웁니다.

순수 가상 함수를 하나라도 포함한 클래스는 추상 클래스(Abstract Class)가 됩니다.  
추상 클래스의 인스턴스를 직접 생성할 수 없고, 상속받은 클래스에서 모든 순수 가상 함수를 재정의해야 객체를 생성할 수 있습니다.

C++에는 별도의 `interface`키워드가 없습니다.  
그래서 C++에서 인터페이스는 순수 가상 함수로만 구성된 추상 클래스로 구현합니다.

# final 키워드

클래스에 사용할 경우 해당 클래스를 더 이상 상속하지 못하도록 막습니다.  
가상 함수에 사용할 경우 더이상 파생 클래스에서 오버라이딩하지 못하도록 막습니다.

상속이나 재정의로 인한 의도하지 않은 동작을 방지합니다.  
해당 클래스나 함수가 더 이상 상속, 재정의 되지 않음을 명시적으로 보여줍니다.

클래스에 사용할 경우 다음과 같습니다.

```cpp
class 클래스명 final : 접근제어자 상속클래스
class Child final : public Parent
```

가상 함수에 사용할 경우 다음과 같습니다.

```cpp
virtual 반환자료형 함수명 final;
virtual void Parent Print() final;
virtual void Parent Print() override final;
```