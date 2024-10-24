---
layout: single

title: "[C++] 접근 제어자"

categories:
    - Cpp
tag: [Cpp]

date: 2024-10-25
last_modified_at: 2024-10-25
---

# 접근 제어자

접근 제어자(Access Modifire)는 클래스의 멤버변수와 멤버함수에 대한 접근 권한을 제어하는 역할을 합니다.  
외부 코드가 내부 구현에 직접 접근하지 못하게 해 데이터를 안전하게 보호하는 캡슐화의 개념 중 하나인 정보 은닉을 위해 사용합니다.  
이런 방식은 코드의 안정성을 높이고 유지보수를 쉽게 해줍니다.

접근 제어자에는 `public`, `private`, `protected` 이렇게 세 가지가 있습니다.

`public` 접근 제어자를 사용해 선언된 클래스 멤버는 외부에 공개되며, 모든 접근을 허용합니다.

`private` 접근 제어자를 사용해 선언된 클래스 멤버는 외부에 공개되지 않으며, 클래스 외부에 대한 모든 접근을 제한합니다.  
해당 멤버의 접근은 클래스의 멤버 함수만이 해당 멤버에 접근할 수 있습니다.

`private`는 일반적으로 멤버 변수에 사용해 데이터를 보호하고, `pulbic` 접근 제어자가 사용된 함수를 제공해 객체의 멤버 변수 값을 수정하거나 가져오는 등의 처리를 제공합니다.

`protected` 접근 제어자를 사용해 선언된 클래스 멤버는 외부에 공개되지 않으며, `private`와 비슷하게 클래스 외부의 접근을 차단하지만, 해당 클래스를 상속받은 자식 클래스 내부에서는 접근이 가능합니다.

## 사용법

예시는 다음과 같습니다.

```cpp
#include <iostream>
#include <string>

struct Human
{
    // 기본 접근 제어자 public

    // 멤버 변수
    std::string Name;
    int Age;
};

class Animal
{
    // 멤버 변수
public:
    float Height;
private:
    float Weight;
protected:
    std::string Name;
    int Age;
};

class Dog : public Animal
{
    // 기본 접근 제어자 private

    // 멤버 변수
public:
    Dog() {};

    // Setter
    void SetHeight(float DogHeight){ Height = DogHeight; }
    
    void SetName(std::string DogName){ Name = DogName; }

    void SetAge(int DogAge) { Age = DogAge; }

    // Getter
    std::string GetName() { return Name; }

    int GetAge() { return Age; }

    void Bark()
    {
        std::cout << Name << ": Bark!" << std::endl;
    }

    void Bark()
    {
        std::cout << Name << ": Bark!" << std::endl;

        Weight = 3.f; // ERROR
    }
};

int main()
{
    Human MyHuman;
    MyHuman.Name = "스트링";
    MyHuman.Age = 5;

    Dog MyDog;
    MyDog.SetHeight(85);
    MyDog.SetName("리트리버");
    MyDog.SetAge(4);
    MyDog.Bark();

    std::cout << MyDog.GetName() << std::endl;
    std::cout << MyDog.GetAge() << std::endl;

    MyDog.Height = 50;
    MyDog.Weight = 3.f; // ERROR
    MyDog.GetAge = 10;  // ERROR
}
```

`Human`은 접근 제어자를 설정해주지 않았지만 구조체의 기본 접근 제어자가 `public`이기 때문에 모든 멤버 변수에 접근 할 수 있습니다.  
클래스의 기본 접근 제어자는 `private`입니다.

`Dog`클래스와 같이 접근 제어자를 적어주면 그 이후의 모든 멤버 변수와 멤버 함수에 지정됩니다.

`Dog`의 기본 생성자는 `public` 접근 제어자를 사용해야 외부에서 인스턴스를 생성 할 수 있습니다.  
기본 생성자는 항상 `public`만 사용하지는 않고 싱글톤 패턴 또는 정적 팩토리 메서드에서 `private`생성자를 사용하기도 합니다.  
혹은 상속 관계에서 클래스를 사용한다면 `protected`를 사용 할 수 도 있습니다.

`SetHeight`, `SetName`, `SetAge`와 `GetName`,`GetAge`는 캡슐화의 일환으로 Setter와 Getter 함수라고 부릅니다.  
외부에서 멤버 변수를 안전하게 접근할 수 있도록 해줍니다.

`public`은 외부에서 접근이 가능하기 때문에 `SetHeight`함수와 `MyDog.Height = 50;`는 문제가 없습니다.
`protected`인 `Name`과 `Age`는 상속을 받은 클래스의 경우 접근이 가능하므로 `Dog`클래스 내부에서 접근이 가능합니다.

`Bark`함수의 `Weight = 3.f;`는 클래스 외부에서 `private`인 멤버 변수에 접근하려하기 때문에 오류가 발생합니다.  
`main`함수의 `MyDog.Weight = 3.f;`는 클래스 외부에서 `private`인 멤버 변수에 접근하려하기 때문에 오류가 발생합니다.
`main`함수의 `MyDog.Age = 10;`는 클래스 외부에서 `protected`인 멤버 변수에 접근하려하기 때문에 오류가 발생합니다.