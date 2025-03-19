---
layout: single

title: "[C++] 생성자"

categories:
    - Cpp
tag: [Cpp]

date: 2024-10-29
last_modified_at: 2024-11-09

order : 220
---

# 생성자

생성자(Constructor)는 클래스의 객체가 생성될 때 자동으로 호출되는 함수입니다.

생성자의 주요 목적은 객체의 멤버 변수 초기화를 하는것입니다.  
예시로 클래스의 모든 멤버 변수가 `public`인 경우 직접 접근해서 초기화 할 수 있지만, 멤버 변수가 `private`인 경우 변수에 직접 접근할 수 없어 초기화 할 수 없습니다.  
이때 생성자를 사용해 멤버 변수를 기본값 또는 사용자 제공 값으로 초기화할 수 있습니다.

생성자는 클래스와 구조체에서 사용됩니다.

생성자의 정의에는 다음과 같은 규칙이 있습니다.

+ 생성자 또한 접근제한자의 영향을 받습니다. 예시로 `protected`일 경우 상속 관계에서만 생성자가 호출될 수 있습니다.
+ 생성자의 이름은 클래스 이름과 동일해야합니다.
+ 생성자는 반환 타입이 없고, 명시하지도 않아 void를 지정하지도 않습니다. 따라서 값을 반환하지도 않습니다.
+ 생성자는 오버로딩(overloading)을 통해 중복정의가 가능합니다.
+ 클래스의 멤버 변수에 기본값을 준 상태에서 생성자를 사용할 경우 생성자에 있는 값으로 할당되거나 초기화됩니다.

## 생성자의 호출 시점

생성자는 객체 생성시 메모리가 할당되고난 후 자동으로 호출됩니다.

클래스가 상속 됐을 때의 생성자는 부모 클래스의 생성자가 먼저 불리고, 자식 클래스의 생성자가 그 다음으로 불립니다.

예시는 다음과 같습니다.

```cpp
#include <iostream>

class Animal
{
public:
    Animal()
    {
        std::cout << "Animal 생성자 호출" << std::endl;
    }
};

class Dog : public Animal
{
public:
    Dog()
    {
        std::cout << "Dog 생성자 호출" << std::endl;
    }
};

int main()
{
    Dog Dog;
}
```

출력은 다음과 같습니다.

![Constructor-CallOrder]({{site.url}}/images/cpp/cpp/2024-10-29-CPP-Constructor/Constructor-CallOrder.PNG)

위와 같이 상속 관계에서 `Animal`클래스의 생성자가 먼저 호출되고 `Dog`클래스의 생성자가 호출된다는 것을 알 수 있습니다.

클래스의 멤버 변수로 다른 클래스를 포함하고 있을 경우 상속 관계시의 호출 순서처럼 멤버 변수로 있는 클래스의 생성자가 먼저 호출됩니다.

## 기본 생성자

기본 생성자(Default Constructor)는 디폴트 생성자라고도 불립니다.

매개변수가 없거나 모든 멤버 변수가 기본값을 가지는 생성자입니다.

정의된 생성자가 없다면 컴파일러가 자동으로 디폴트 생성자를 만듭니다.  
이때 기본 자료형인 멤버 변수는 초기화되지 않기 때문에 쓰레기값을 가지고 있습니다.

예시는 다음과 같습니다.

```cpp
#include <iostream>
#include <string>

class Dog
{
private:
    std::string Name;
    int Age;

public:
    Dog()   // 기본 생성자
    {
        std::cout << "기본 생성자 호출" << std::endl;

        Name = "리트리버";
        Age = 3;
    }

    std::string GetName() { return Name; }
    int GetAge() { return Age; }
};

int main()
{
    Dog MyDog;  // 기본 생성자 호출

    std::cout << "Dog Name : " << MyDog.GetName() << std::endl;
    std::cout << "Dog Age : " << MyDog.GetAge() << std::endl;
}
```

출력은 다음과 같습니다.

![Constructor-DefaultConstructor]({{site.url}}/images/cpp/cpp/2024-10-29-CPP-Constructor/Constructor-DefaultConstructor.PNG)

`MyDog`라는 인스턴스가 생성되면서 기본 생성자가 호출됩니다.  
그 후 생성자에서 각 변수에 값이 할당됩니다.

## 매개 변수를 받는 생성자

매개 변수를 받는 생성자(Parameterized Constructor)는 사용자가 특정한 값을 전달해 값을 할당할 수 있도록 하는 생성자입니다.  
기본 생성자와 다르게 원하는 값으로 멤버 변수의 값을 할당 할 수 있습니다.

예시는 다음과 같습니다.

```cpp
#include <iostream>
#include <string>

class Dog
{
private:
    std::string Name;
    int Age;

public:
    Dog()   // 기본 생성자
    {
        std::cout << "기본 생성자 호출" << std::endl;

        Name = "리트리버";
        Age = 3;
    }

    Dog(std::string DogName, int DogAge)    // 매개 변수를 받는 생성자
    {
        std::cout << "매개 변수를 받는 생성자 호출" << std::endl;

        Name = DogName;
        Age = DogAge;
    }

    std::string GetName() { return Name; }
    int GetAge() { return Age; }
};

int main()
{
    Dog MyDog("시추", 5); // 매개 변수를 받는 생성자 호출

    std::cout << "Dog Name : " << MyDog.GetName() << std::endl;
    std::cout << "Dog Age : " << MyDog.GetAge() << std::endl;
}
```

출력은 다음과 같습니다.

![Constructor-ParameterizedConstructor]({{site.url}}/images/cpp/cpp/2024-10-29-CPP-Constructor/Constructor-ParameterizedConstructor.PNG)

`MyDog`라는 인스턴스가 생성되면서 생성자를 호출해야하는데, 매개변수 2개를 가진 함수를 호출하므로 매개 변수를 받는 생성자를 호출합니다.  
그 후 생성자에서 각 변수에 값이 할당된 것을 알 수 있습니다.

기본 생성자와 매개 변수를 받는 생성자가 한 클래스 안에 있을 수 있는 것은 함수 오버로딩이 가능하기 때문입니다.

예시에서 매개 변수를 받는 생성자를 호출 할 때 직접 초기화를 사용했는데, 유니폼 초기화도 사용 할 수 있습니다.

```cpp
Dog MyDog("시추", 5);
Dog MyDog{"시추", 5};
```


## 생성자 멤버 초기화 리스트

생성자 멤버 초기화 리스트(Constructor Member Initializer List)는 생성자 함수 내부에서 대입 연산자(=)를 사용하는 것보다 효율적으로 멤버 변수를 초기화할 수 있습니다.  
기본 생성자와 매개 변수를 받는 생성자에서는 대입 연산자를 사용했는데, 이 경우 할당을 한 것이지 초기화를 한 것이 아닙니다.

특히 선언과 동시에 초기화를 해주어야하는 const 및 참조 변수의 경우 선언과 동시에 초기화를 해주어야합니다.  
이 경우 기본 생성자와 매개 변수를 받는 생성자만으로는 변수를 초기화 할 수 없고, 생성자 멤버 초기화 리스트를 사용하면 가능합니다.

변수에 기본값이 지정되어 있더라도 생성자 멤버 초기화 리스트의 값으로 설정됩니다.

기본 생성자와 매개 변수를 받는 생성자 둘 다 사용이 가능하며, 생성자의 매개 변수 뒤에 `:`이 삽입되어 시작합니다.

예시는 다음과 같습니다.

```cpp
#include <iostream>
#include <string>

class Dog
{
private:
    const std::string Name;
    int Age;

public:
    Dog() : Name("리트리버"), Age(3)     // 기본 생성자
    {
        std::cout << "기본 생성자 호출" << std::endl;
    }

    Dog(std::string DogName, int DogAge) : Name(DogName), Age(DogAge)    // 매개 변수를 받는 생성자
    {
        std::cout << "매개 변수를 받는 생성자 호출" << std::endl;
    }

    std::string GetName() { return Name; }
    int GetAge() { return Age; }
};

int main()
{
    Dog ADog; // 기본 생성자 호출

    std::cout << "Dog Name : " << ADog.GetName() << std::endl;
    std::cout << "Dog Age : " << ADog.GetAge() << std::endl;

    Dog BDog("시추", 5); // 매개 변수를 받는 생성자 호출

    std::cout << "Dog Name : " << BDog.GetName() << std::endl;
    std::cout << "Dog Age : " << BDog.GetAge() << std::endl;
}
```

출력은 다음과 같습니다.

![Constructor-ConstructorMemberInitializerList]({{site.url}}/images/cpp/cpp/2024-10-29-CPP-Constructor/Constructor-ConstructorMemberInitializerList.PNG)

`ADog`라는 인스턴스가 생성되면서 매개 변수가 없으므로 기본 생성자를 호출합니다.  
`BDog`라는 인스턴스가 생성되면서 매개 변수를 2개 넘기므로 매개 변수를 받는 생성자를 호출합니다.  
각각 생성자에 멤버 초기화 리스트가 적용되어있어 const 변수인 `Name`변수를 초기화한 것을 알 수 있습니다.

## 생성자 위임

생성자 위임(Constructor Delegation)은 같은 클래스의 생성자가 다른 생성자를 호출하는 기능입니다.

생성자 위임을 사용해 코드 중복을 줄이고, 여러 생성자에서 동일한 초기화 로직을 간결하게 구현할 수 있습니다.

C++11부터 사용할 수 있는 기능입니다.

초기화 리스트를 사용했던 것 처럼 생성자의 매개변수 뒤에 `:`이 삽입되어 시작합니다.

예시는 다음과 같습니다.

```cpp
#include <iostream>
#include <string>

class Dog
{
private:
    const std::string Name;
    int Age;

public:
    Dog() : Dog("", 0)  // 기본 생성자
    {
        std::cout << "기본 생성자 호출" << std::endl;
    }

    Dog(std::string DogName, int DogAge) : Name(DogName), Age(DogAge)    // 매개 변수를 받는 생성자
    {
        std::cout << "매개 변수를 받는 생성자 호출" << std::endl;
    }

    std::string GetName() { return Name; }
    int GetAge() { return Age; }
};

int main()
{
    Dog ADog; // 기본 생성자 호출

    std::cout << "Dog Name : " << ADog.GetName() << std::endl;
    std::cout << "Dog Age : " << ADog.GetAge() << std::endl;
}
```

출력은 다음과 같습니다.

![Constructor-ConstructorDelegation]({{site.url}}/images/cpp/cpp/2024-10-29-CPP-Constructor/Constructor-ConstructorDelegation.PNG)

`ADog`라는 인스턴스가 생성되기 위해 생성자를 호출하는데, 매개 변수가 없으므로 기본 생성자를 호출합니다.  
이때 기본 생성자에서 생성자 위임을 했기 때문에 매개 변수를 받는 생성자를 호출합니다.  
이 경우 함수의 실행 순서는 위임을 받은 함수인 매개 변수를 받는 생성자의 기능을 수행하고 기본 생성자의 기능을 수행합니다.

반대로 매개 변수를 받는 생성자에서 기본 생성자를 호출 하는 것도 가능합니다.

## 복사 생성자

복사 생성자(Copy Constructor)는 한 클래스 객체의 정보를 다른 클래스 객체로 복사하는 생성자입니다.

사용자가 정의하지 않을 경우 컴파일러가 자동으로 디폴트 복사 생성자(Default Copy Constructor)를 생성합니다.  
이 경우 복사 생성자는 얕은 복사를 수행합니다.

const 참조 타입으로 동일 클래스의 객체를 매개 변수로 받아와야 합니다.

예시는 다음과 같습니다.

```cpp
#include <iostream>
#include <string>

class Dog
{
private:
    std::string Name;
    int* Age;

public:
    Dog() // 기본 생성자
    {
        std::cout << "기본 생성자 호출" << std::endl;
        Age = new int;
    }

    Dog(const Dog& Other)   // 복사 생성자
    {
        std::cout << "복사 생성자 호출" << std::endl;

        Name = std::string(Other.Name);
        Age = new int(*Other.Age);
    }

    std::string GetName() { return Name; }
    int* GetAge() { return Age; }
};

int main()
{
    Dog ADog; // 인스턴스 생성 및 생성자 호출

    std::cout << "Dog Name : " << ADog.GetName() << std::endl;
    std::cout << "Dog Age : " << ADog.GetAge() << std::endl;
    std::cout << "Dog Age : " << *ADog.GetAge() << std::endl;

    Dog BDog = ADog; // 복사 생성자 호출
    
    std::cout << "Dog Name : " << BDog.GetName() << std::endl;
    std::cout << "Dog Age : " << BDog.GetAge() << std::endl;
    std::cout << "Dog Age : " << *BDog.GetAge() << std::endl;
}
```

출력은 다음과 같습니다.

![Constructor-CopyConstructorShallowCopy]({{site.url}}/images/cpp/cpp/2024-10-29-CPP-Constructor/Constructor-CopyConstructorShallowCopy.PNG)

`BDog`라는 인스턴스가 생성되기 위해 생성자를 호출하는데, 이때 복사 생성자를 호출합니다.  
복사 생성자에서 `int* Age`의 할당을 얕은 복사로 했기 때문에 `ADog`의 멤버 변수에 값을 가리키는 포인터(주소 값)를 복사합니다.  
이 경우 `BDog`에서 값을 변경하면 `ADog`의 멤버 변수에서도 값이 변하거나 둘 중 한 인스턴스가 삭제된 후 다른 인스턴스에서 접근 하려 하면 오류가 발생할 수 있기 때문에 주의해야합니다.

만약 깊은 복사를 하고자 한다면 예시는 다음과 같습니다.

```cpp
    Dog(const Dog& Other)   // 복사 생성자
    {
        std::cout << "복사 생성자 호출" << std::endl;

        Name = std::string(Other.Name);
        Age = new int(*Other.Age);
    }
```

예시의 함수를 사용한 출력은 다음과 같습니다.

![Constructor-CopyConstructorDeepCopy]({{site.url}}/images/cpp/cpp/2024-10-29-CPP-Constructor/Constructor-CopyConstructorDeepCopy.PNG)

깊은 복사가 수행되어 출력에서 변수의 주소가 다른 것을 알 수 있습니다.

위의 예시를 포함한 복사 생성자를 호출하는 경우는 다음과 같습니다.

`새로운 객체를 이미 존재하는 객체로 초기화 할 때:`
```cpp
Dog ADog;
Dog BDog = ADog;
```

`객체가 함수에 매개 변수로 전달 될 때:`
```cpp
void SomeFunction(Dog SomeDog);
```

`함수에서 객체를 반환할 때:`
```cpp
Dog SomeFunction()
{
    Dog SomeDog;
    return SomeDog;
}
```

## 이동 생성자

이동 생성자(Move Constructor)는 객체가 복사되는 대신 이동 될 수 있도록 도와주는 생성자입니다.  
복사 생성자를 통해 다른 객체로 복사되는것이 일반적이지만, 효율적인 리소스 관리를 위해 생겨난 생성자입니다.

이동 생성자를 사용할 경우 새로운 객체에 소유권을 이전하고, 기존 객체를 초기 상태로 남겨두는 방식으로 사용합니다.

이동 생성자도 사용자가 정의하지 않을 경우 컴파일러가 자동으로 이동 생성자를 생성합니다.

RValue로 동일 클래스의 객체를 매개 변수로 받아와야 합니다.

예시는 다음과 같습니다.

```cpp
#include <iostream>
#include <string>

class Dog
{
private:
    std::string Name;
    int* Age;

public:
    Dog() // 기본 생성자
    {
        std::cout << "기본 생성자 호출" << std::endl;
        Name = "리트리버";
        Age = new int(3);
    }

    Dog(Dog&& Other) noexcept   // 이동 생성자
    {
        std::cout << "이동 생성자 호출" << std::endl;

        Name = std::move(Other.Name);
        Age = Other.Age;
        Other.Age = nullptr;
    }

    std::string GetName() { return Name; }
    int* GetAge() { return Age; }
};

int main()
{
    Dog ADog; // 기본 생성자 호출
    Dog BDog = std::move(ADog); // 이동 생성자 호출
    
    std::cout << "Dog Name : " << ADog.GetName() << std::endl;
    std::cout << "Dog Age : " << ADog.GetAge() << std::endl;

    std::cout << "Dog Name : " << BDog.GetName() << std::endl;
    std::cout << "Dog Age : " << BDog.GetAge() << std::endl;

}
```

출력은 다음과 같습니다.

![Constructor-MoveConstructor]({{site.url}}/images/cpp/cpp/2024-10-29-CPP-Constructor/Constructor-MoveConstructor.PNG)

`BDog`라는 인스턴스를 `ADog`라는 인스턴스를 이용해 이동 생성자를 호출합니다.  
이때 `std::move()`는 이동을 시켜주는 것이 아닌 객체를 RValue로 변환해주는 역할을 합니다.

예시를 포함한 이동 생성자를 호출하는 대표적인 경우는 다음과 같습니다.

`RVlalue 참조를 통해 객체가 초기화될 때:`
```cpp
    Dog ADog;
    Dog BDog = std::move(ADog);
    Dog CDog(std::move(BDog));
```

`RValue 객체가 함수에 매개 변수로 전달 될 때:`
```cpp
void SomeFunction(Dog SomeDog);

SomeFunction(std::move(DDog));
```

`함수에서 RValue 객체를 반환 할 때:`
```cpp
Dog SomeFunction()
{
    Dog SomeDog;
    return SomeDog;
}

Dog DDog = SomeFunction();
```