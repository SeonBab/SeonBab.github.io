---
layout: single

title: "[Design Pattern] 인터페이스"

categories:
    - DesignPattern
tag: [디자인 패턴]

date: 2025-03-09
last_modified_at: 2025-04-23

order : 1000
---

# 인터페이스

C++에서 인터페이스는 특정 기능을 제공하는 메서드들의 집합을 정의하지만, 실제 구현을 제공하지 않는 추상적인 개념입니다.

인터페이스에서 특정 동작을 정의만 하고, 동작의 구현은 해당 인터페이스를 구현한 클래스에서 하도록 강제합니다.  
이를 통해 다형성을 제공하고, 서로 다른 클래스들이 동일한 메서드 집합을 공유해 일관된 방식으로 동작하도록 만들어줍니다.

+ 장점
    + 인터페이스를 상속받은 클래스는 반드시 해당 메서드를 구현해야 하므로, 일관성 있는 메서드 집합을 제공할 수 있습니다.
    + 인터페이스를 사용하면 새로운 클래스만 추가하는 방식으로 시스템을 확장할 수 있습니다.

+ 단점
    + 코드가 더 복잡해지고, 클래스의 설계가 더 많아질 수 있습니다.

## 상속과의 차이점

상속과 비슷한 개념처럼 보이지만 차이가 존재합니다.  
상속은 하위 클래스가 상위 클래스의 모든 특성과 기능을 그대로 물려받아 사용하는 방식입니다.  
인터페이스는 특정 클래스가 수행해야 하는 역할 또는 기능을 정의하는 방식으로, 클래스의 행동을 약속하는 개념입니다.  
즉, 상속은 ~이다(is-a) 관계이고, 인터페이스는 ~할 수 있다(can-do) 관계입니다.

## C++ 인터페이스 구현 방법

C++에서는 Java나 C#처럼 명시적인 `interface`키워드가 없지만, 개념은 존재해 추상 클래스와 순수 가상함수를 조합해 구현할 수 있습니다.

순수 가상 함수는 함수 선언 뒤에 `= 0`을 붙여서 정의됩니다.  
순수 가상 함수는 구현을 가지지 않으며, 상속받은 클래스에서 반드시 구현해야하는 함수입니다.

추상 클래스는 순수 가상 함수만을 가진 클래스이며, 인터페이스처럼 동작합니다.  
이 클래스는 인스턴스를 생성할 수 없으며, 이를 상속받은 클래스들을 생성할 수 있습니다.  
추상 클래스의 포인터 변수는 생성할 수 있습니다.

```cpp
#include <iostream>

using namespace std;

class IVehicle
{
public:
	virtual void StartEngine() = 0;
	virtual void StopEngine() = 0;
};

class Car : public IVehicle
{
public:
	virtual void StartEngine() override
	{
		cout << "자동차 엔진 가동!" << endl;
	}

	virtual void StopEngine() override
	{
		cout << "자동차 엔진 정지!" << endl;
	}
};

class Bike : public IVehicle
{
public:
	virtual void StartEngine() override
	{
		cout << "오토바이 엔진 가동!" << endl;
	}

	virtual void StopEngine() override
	{
		cout << "오토바이 엔진 정지!" << endl;
	}
};

int main()
{
	IVehicle* vehicles[] = { new Car(), new Bike() };

	for (const auto& v : vehicles)
	{
		// 다형성 사용
		v->StartEngine();  
		v->StopEngine();
	}

	for (const auto& v : vehicles)
	{
		// 메모리 해제
		delete v;
	}
}
```

`IVehicle` 인터페이스(추상 클래스)에 `StartEngine`함수와 `StopEngine`함수를 정의합니다.

`Car`와 `Bike` 클래스가 `IVehicle` 추상 클래스를 상속하고, 각자 엔진을 다르게 동작하도록 구현합니다.  
`IVehicle*`를 사용해 동일한 방식으로 `Car`와 `Bike`를 조작 가능합니다.