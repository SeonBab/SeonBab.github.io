---
layout: single

title: "[Design Pattern] Adapter Pattern(어댑터 패턴)"

categories:
    - DesignPattern
tag: [디자인 패턴]

date: 2025-03-19
last_modified_at: 2025-03-19

order : 1070
---

# Adapter Pattern

Adapter Pattern(어댑터 패턴)은 호환되지 않는 인터페이스를 가진 클래스들 간의 호환성을 맞춰주는 구조(Structural) 디자인 패턴입니다.  
즉, 기존 클래스의 인터페이스를 클라이언트가 기대하는 인터페이스로 변환하여 기존 코드를 변경하지 않고 재사용할 수 있도록 도와줍니다.

예를 들어 220V 전압 전기기기를 110V 콘센트에 연결하기 위해, 전압 변환 어댑터를 사용하는 것과 비슷한 개념입니다.

어댑터는 클라이언트가 사용하는 인터페이스(ITarget)를 구현하고, 내부적으로 Adaptee(기존 기능)를 참조하여 필요한 메서드를 호출하거나, 필요한 변환 로직을 수행합니다.

예를 들어, 다음과 같은 상황이 있을 수 있습니다.  
기존 인터페이스와 새로운 요구사항을 충족하기위해 새로 구현한 인터페이스 사이에 함수의 시그니처가 (반환 자료형, 함수 이름, 파라미터 등) 달라서 직접 사용하기 어려운 경우가 있을 수 있습니다.  
이 경우, 새로운 인터페이스에 맞추어 아예 새 클래스를 만드는 것은 비용이나 리스크 면에서 부담스러울 수 있습니다.  
이미 잘 만들어진 레거시(기존) 클래스(또는 외부 라이브러리)를 완전히 폐기하고 새로 작성하기에는 비효율적일 수 있습니다.  
직접 레거시 클래스 안에 if문이나 오버로드 등을 추가하여 억지로 요구사항을 맞추면, 코드가 복잡해지고 결합도가 높아집니다.

필요 이상으로 복잡한 매핑/로직이 들어가면, 어댑터 클래스가 비대해질 수 있습니다.  
경우에 따라 Mapper(예: Automapper)를 별도로 두거나, DTO 전환 로직을 분리하여 유지보수성을 높일 수 있습니다.

코드 중복이 발생하지 않도록 다양한 들을 단일 어댑터로 처리하거나, 인터페이스별로 작은 어댑터 계층을 둬서 중복을 최소화할 수 있습니다.

다단계 어댑터가 중첩되어 사용되면 호출 오버헤드가 쌓일 수 있습니다.  
실제 현업에서 성능 병목이 발생하지 않도록 주의해야 합니다.

어댑터 자체를 단위 테스트(Unit Test)하여, 입력/출력 변환이 의도대로 이루어지는지 확인하는 과정이 필수입니다.

이외에 어댑터 패턴이 사용되는 경우는 다음과 같습니다.

+ 라이브러리 또는 API 연동: 외부 라이브러리의 인터페이스를 변환하여 사용해야 할 때
+ 호환성 문제 해결: 서로 다른 인터페이스를 가진 클래스 간의 통신을 가능하게 할 때

+ 장점
    - 기존 클래스를 수정하지 않고도, 필요한 인터페이스에 맞춰서 사용할 수 있기 때문에 코드 재사용성을 높입니다.
    - 어댑터 클래스를 만들어 두면, 여러 종류의 인스턴스를 간단히 교체하여 사용할 수 있어 유지보수가 용이하게 해줍니다.
    - 변환 로직(인터페이스 호환성)에 대한 책임을 어댑터에만 집중시켜, 클라이언트 코드가 복잡해지지 않습니다.

+ 단점
    - 요구사항이 많아질수록, 클래스가 여러 개 생길 수 있습니다.
    - 단순한 상황이라면, 어댑터를 만드는 것보다 기존 코드를 직접 수정하는 편이 더 간단할 수 있습니다.
    - 어댑터가 여러 단계로 중첩되어 사용되면(어댑터 안의 어댑터), 호출 오버헤드가 누적될 수 있습니다.

## 어댑터 패턴의 종류

Object Adapter(객체 어댑터)와 Class Adapter(클래스 어댑터) 두 가지 방식으로 구현할 수 있습니다.

1. Object Adapter(객체 어댑터)
    - 어댑터가 기존 클래스의 인스턴스를 멤버 변수로 포함하고, 해당 인스턴스의 메서드를 호출하여 동작을 위임(Delegation)방식으로 연결합니다.
    
2. 클래스 어댑터 방식(Class Adapter)
    - 어댑터가 기존 클래스와 타겟 인터페이스를 다중 상속을 받아 어댑터 클래스를 구현하는 방식입니다.
    - C++에서는 다중 상속이 가능하지만, 다중 상속으로 인해 복잡성이 증가할 수 있습니다.

즉, 객체 어댑터는 구성(Composition)을 사용하며, 클래스 어댑터는 다중 상속을 사용합니다.

### Object Adapter

+ 장점
    - 구현이 간편할 뿐만 아니라, 유지보수성이 뛰어납니다.
    - 객체 어댑터는 상속보다 유연성이 높습니다.

+ 단점
    - 객체 어댑터 방식은 기존 클래스의 객체를 포함하므로, 메모리 사용량이 증가할 수 있습니다.

#### 예시

<details>
<summary><h5 style="display: inline;">1. Object Adapter Duck/Turkey</h5></summary>
<div markdown="1">

`Duck` 인터페이스에는 `Quack()`, `Fly()` 같은 메서드가 있습니다.  
`Turkey` 인터페이스에는 `Gobble()`, `FlyShortDistance()` 같은 메서드가 있습니다.  
`Duck` 인터페이스를 쓰는 코드는 `Turkey` 인터페이스만 제공되는 객체와 직접 호환되지 않으므로, 어댑터가 중간에서 변환 역할을 해주어야 합니다.

```cpp
#include <iostream>
#include <memory>

// 인터페이스
class IDuck
{
public:
    virtual void Quack() const = 0;
    virtual void Fly() const = 0;
    virtual ~IDuck() = default;
};

class ITurkey
{
public:
    virtual void Gobble() const = 0;
    virtual void FlyShortDistance() const = 0;
    virtual ~ITurkey() = default;
};

// 구상 클래스
class MallardDuck : public IDuck
{
public:
    void Quack() const override
    {
        std::cout << "Duck: Quack!" << std::endl;
    }

    void Fly() const override
    {
        std::cout << "Duck: I'm flying far..." << std::endl;
    }
};

class WildTurkey : public ITurkey
{
public:
    void Gobble() const override
    {
        std::cout << "Turkey: Gobble gobble!" << std::endl;
    }

    void FlyShortDistance() const override
    {
        std::cout << "Turkey: I'm flying a short distance..." << std::endl;
    }
};

// TurkeyAdapter는 IDuck(Duck 인터페이스)을 구현하고, 내부에서 ITurkey를 참조합니다.
// Quack()를 호출하면 Gobble()로, Fly()를 호출하면 FlyShortDistance()를 여러 번 호출하도록 바꿔주는 식입니다.
class TurkeyAdapter : public IDuck
{
private:
    std::shared_ptr<ITurkey> turkey;

public:
    explicit TurkeyAdapter(std::shared_ptr<ITurkey> t) : turkey(std::move(t)) {}

    void Quack() const override
    {
        // Duck의 Quack()을 Turkey의 Gobble()로 변환합니다.
        turkey->Gobble();
    }

    void Fly() const override
    {
        // 오리는 멀리 날 수 있지만, 칠면조는 짧은 거리만 날 수 있으므로
        // 여러 번 호출해 보정하는 방식으로 처리 가능합니다.
        for (int i = 0; i < 5; ++i)
        {
            turkey->FlyShortDistance();
        }
    }
};

int main()
{
    // 1) 오리 객체는 그대로 사용 가능합니다.
    std::unique_ptr<IDuck> duck = std::make_unique<MallardDuck>();
    duck->Quack(); // "Duck: Quack!"
    duck->Fly();   // "Duck: I'm flying far..."

    // 2) 칠면조 객체(ITurkey)는 오리 인터페이스(IDuck)와 호환되지 않습니다.
    std::shared_ptr<ITurkey> turkey = std::make_shared<WildTurkey>();

    // 3) 칠면조를 오리처럼 사용하고 싶다면, TurkeyAdapter를 이용합니다.
    std::unique_ptr<IDuck> turkeyAdapter = std::make_unique<TurkeyAdapter>(turkey);
    turkeyAdapter->Quack(); // 내부적으로 turkey->Gobble() 호출
    turkeyAdapter->Fly();   // 내부적으로 여러 번 FlyShortDistance() 호출
}
```

실행을 통해 출력되는 결과는 다음과 같습니다.

```
Duck: Quack!
Duck: I'm flying far...
Turkey: Gobble gobble!
Turkey: I'm flying a short distance...
Turkey: I'm flying a short distance...
Turkey: I'm flying a short distance...
Turkey: I'm flying a short distance...
Turkey: I'm flying a short distance...
```

</div>
</details>

### Class Adapter

Class Adapter(클래스 어댑터)는 기존 클래스(Adaptee)를 새로운 인터페이스(Target)에 맞게 변환하는 역할을 합니다.

+ 장점
    - 어댑터가 기존 클래스를 직접 상속받아 변환하기 때문에, 오버헤드 없이 빠른 성능을 제공합니다.
    - 객체 어댑터보다 코드가 간결하며, 직접 상속을 사용하여 구현이 쉽습니다.

+ 단점
    - 클래스 어댑터 방식은 C++의 다중 상속을 사용하기 때문에 유연성이 낮고, 구조가 복잡해질 수 있습니다.
    - 다이아몬드 상속 문제(다이아몬드 문제)를 유발할 수도 있습니다.

#### 예시

<details>
<summary><h5 style="display: inline;">1. Class Adapter Duck/Turkey</h5></summary>
<div markdown="1">

`Duck` 인터페이스에는 `Quack()`, `Fly()` 같은 메서드가 있습니다.  
`Turkey` 인터페이스에는 `Gobble()`, `FlyShortDistance()` 같은 메서드가 있습니다.  
`Duck` 인터페이스를 쓰는 코드는 `Turkey` 인터페이스만 제공되는 객체와 직접 호환되지 않으므로, 어댑터가 중간에서 변환 역할을 해주어야 합니다.

```cpp
#include <iostream>
#include <memory>

// 인터페이스
class IDuck
{
public:
    virtual void Quack() const = 0;
    virtual void Fly() const = 0;
    virtual ~IDuck() = default;
};

class ITurkey
{
public:
    virtual void Gobble() const = 0;
    virtual void FlyShortDistance() const = 0;
    virtual ~ITurkey() = default;
};

// 구상 클래스
class MallardDuck : public IDuck
{
public:
    void Quack() const override
    {
        std::cout << "Duck: Quack!" << std::endl;
    }

    void Fly() const override
    {
        std::cout << "Duck: I'm flying far..." << std::endl;
    }
};

class WildTurkey : public ITurkey
{
public:
    void Gobble() const override
    {
        std::cout << "Turkey: Gobble gobble!" << std::endl;
    }

    void FlyShortDistance() const override
    {
        std::cout << "Turkey: I'm flying a short distance..." << std::endl;
    }
};

// 클래스 어댑터: 다중 상속을 사용하여 ITurkey → IDuck 변환
class TurkeyAdapter : public IDuck, private WildTurkey
{
public:
    void Quack() const override
    {
        // Duck의 Quack()을 Turkey의 Gobble()로 변환
        Gobble(); // WildTurkey의 Gobble()을 직접 호출
    }

    void Fly() const override
    {
        // 오리는 멀리 날지만, 칠면조는 짧게 날 수 있으므로 보정
        for (int i = 0; i < 5; ++i)
        {
            FlyShortDistance(); // WildTurkey의 FlyShortDistance() 호출
        }
    }
};

int main()
{
    // 1) 오리 객체
    std::unique_ptr<IDuck> duck = std::make_unique<MallardDuck>();
    duck->Quack();  // "Duck: Quack!"
    duck->Fly();    // "Duck: I'm flying far..."

    // 2) 칠면조 객체 (원래 IDuck과 호환되지 않음)
    std::unique_ptr<ITurkey> turkey = std::make_unique<WildTurkey>();
    //turkey->Gobble();
    //turkey->FlyShortDistance();

    // 3) 클래스를 이용한 어댑터 (WildTurkey → IDuck)
    std::unique_ptr<IDuck> turkeyAdapter = std::make_unique<TurkeyAdapter>();
    turkeyAdapter->Quack(); // 내부적으로 Gobble() 호출
    turkeyAdapter->Fly();   // 내부적으로 여러 번 FlyShortDistance() 호출
}
```

실행을 통해 출력되는 결과는 다음과 같습니다.

```
Duck: Quack!
Duck: I'm flying far...
Turkey: Gobble gobble!
Turkey: I'm flying a short distance...
Turkey: I'm flying a short distance...
Turkey: I'm flying a short distance...
Turkey: I'm flying a short distance...
Turkey: I'm flying a short distance...
```

</div>
</details>