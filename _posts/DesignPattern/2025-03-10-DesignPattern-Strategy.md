---
layout: single

title: "[Design Pattern] 전략 패턴"

categories:
    - DesignPattern
tag: [디자인 패턴]

date: 2025-03-10
last_modified_at: 2025-03-10

order : 1010
---

# 전략 패턴

전략(Strategy) 패턴은 특정한 작업(알고리즘)을 독립적으로 정의하고 캡슐화 하여, 해당 작업을 동적으로 교체할 수 있도록 하는 패턴을 의미합니다.  
즉, 특정 기능을 인터페이스로 분리하고, 다양한 구현체를 만들어서 필요할 때 변경할 수 있도록 합니다.

세 가지 주요 구성 요소로 이루어집니다.

1. 전략 인터페이스(Strategy): 특정 알고리즘의 인터페이스를 정의합니다.
2. 구체적인 전략 클래스(ConcreteStrategy): 전략 인터페이스를 구현하여 알고리즘을 구체화하는 클래스입니다.
3. 전략을 사용하는 클래스(Context): 전략 객체를 포함하며, 실행 시점에서 전략을 선택하여 실행합니다.

자주 바뀌는 로직(할인 정책, 무기 시스템, 정렬 알고리즘 등)에 대해서 전략 패턴을 적용하면 유리합니다.  
복잡한 `if-else` 또는 `switch문`으로 분기 처리하던 로직을 행동 객체로 분리함으로써 코드 가독성과 유지보수성을 높일 수 있습니다.

+ 장점
    + 새로운 행동 클래스만 추가하면 되므로, 기존 코드 영향이 적는 유연성을 가집니다.
    + 인터페이스만 알면 되므로, 구체 구현 변경 시 수정 범위가 최소화되어 결합도가 감소됩니다.
    + 프로그램 실행 중에 행동을 교체할 수 있습니다.
    + 여러 클래스에서 동일한 전략을 재사용할 수 있습니다.

+ 단점
    + 여러 전략 인터페이스 및 클래스를 생성해야 하므로 클래스의 개수가 많아집니다.
    + 생성자 혹은 팩토리에서 어떤 행동을 쓸지 결정하는 과정이 필요합니다.
    
## 예시

```cpp
// 행동 인터페이스
class IFlyBehavior
{
public:
    virtual void Fly() = 0;
};

class IQuackBehavior
{
public:
    virtual void Quack() = 0;
};
```

`IFlyBehavior`와 `IQuackBehavior`는 오리의 행동(날기, 울기)을 추상화한 인터페이스입니다.  

`Fly`함수와 `Quack`함수는 순수 가상 함수로 선언되어 있으며, 이를 상속받는 클래스에서 반드시 구현해야 합니다.  
오리의 비행 및 울음소리 행동을 개별적으로 변경할 수 있도록 만듭니다.

```cpp
// 인터페이스 기반 구체적인 행동 구현
class FlyWithWings : public IFlyBehavior
{
    virtual void Fly() override
    {
        cout << "날개로 납니다." << endl;
    }
};

class FlyNoWay : public IFlyBehavior
{
    virtual void Fly() override
    {
        cout << "날지 못합니다." << endl;
    }
};

class FlyRocketPowered : public IFlyBehavior
{
    virtual void Fly() override
    {
        cout << "로켓 추진으로 엄청나게 날아갑니다!!!" << endl;
    }
};

class QuackQuack : public IQuackBehavior
{
public:
    virtual void Quack() override
    {
        cout << "꽥꽥 소리를 냅니다." << endl;
    }
};

class MuteQuack : public IQuackBehavior
{
public:
    virtual void Quack() override
    {
        cout << "... (침묵)" << endl;
    }
};

class SqueakQuack : public IQuackBehavior
{
public:
    virtual void Quack() override
    {
        cout << "삑삑 소리를 냅니다." << endl;
    }
};
```

+ FlyWithWings: 날개를 사용하여 나는 행동을 구현.
+ FlyNoWay: 날지 못하는 행동을 구현.
+ FlyRocketPowered: 로켓을 이용하여 빠르게 나는 행동을 구현.

+ QuackQuack: 기본적으로 오리가 "꽥꽥" 소리를 냄.
+ MuteQuack: 소리를 내지 않는 행동.
+ SqueakQuack: 장난감 오리처럼 "삑삑" 소리를 내는 행동.

```cpp
// 인터페이스를 포함하는 클래스 정의
class Duck
{
public:
    virtual ~Duck()
    {
        delete flyBehavior;
        delete quackBehavior;
    }

    virtual void Display() {}

    virtual void PerformFly()
    {
        flyBehavior->Fly();
    }

    void PerformQuack()
    {
        quackBehavior->Quack();
    }

    void SetFlyBehavior(IFlyBehavior* fb)
    {
        delete flyBehavior;
        flyBehavior = fb;
    }

    void SetQuackBehavior(IQuackBehavior* qb)
    {
        delete quackBehavior;
        quackBehavior = qb;
    }

protected:
    IFlyBehavior* flyBehavior;
    IQuackBehavior* quackBehavior;
};
```

`Duck`은 오리의 기본 클래스 이며, 행동을 인터페이스(`IFlyBehavior`, `IQuackBehavior`)로 정의했습니다.

`PerformFly`함수와 `PerformQuack`함수를 통해 현재 설정된 비행 및 울음 행동을 실행합니다.  
`SetFlyBehavior`함수와 `SetQuackBehavior`함수를 통해 런타임 중에 행동을 동적으로 변경할 수 있습니다.  
소멸자에서 `flyBehavior`와 `quackBehavior`를 `delete`하여 메모리 해제합니다.

```cpp
// 구체적인 클래스 정의
class MallardDuck : public Duck
{
public:
    MallardDuck()
    {
        flyBehavior = new FlyWithWings();
        quackBehavior = new QuackQuack();
    }

    virtual void Display() override
    {
        cout << "Mallard 오리" << endl;
    }
};

class RubberDuck : public Duck
{
public:
    RubberDuck()
    {
        flyBehavior = new FlyNoWay();
        quackBehavior = new SqueakQuack();
    }

    virtual void Display() override
    {
        cout << "Rubber 오리" << endl;
    }
};
```

`MallardDuck`(청둥오리)는 기본적으로 날개로 날고(`FlyWithWings`), "꽥꽥" 우는(`QuackQuack`) 행동을 가집니다.  
`RubberDuck`(고무오리)는 날지 못하며(`FlyNoWay`), "삑삑" 소리(`SqueakQuack`)를 냅니다.

```cpp
// 실제 사용
int main()
{
    Duck* mallardDuck = new MallardDuck;
    Duck* rubberDuck = new RubberDuck;

    mallardDuck->Display();         // Mallard 오리
    mallardDuck->PerformFly();      // 날개로 납니다.
    mallardDuck->PerformQuack();    // 꽥꽥 소리를 냅니다.

    rubberDuck->Display();          // Rubber 오리
    rubberDuck->PerformFly();       // 날지 못합니다.
    rubberDuck->PerformQuack();     // 삑삑 소리를 냅니다.
    rubberDuck->SetFlyBehavior(new FlyRocketPowered); // 인터페이스 변경
    rubberDuck->PerformFly();       // 로켓 추진으로 엄청나게 날아갑니다!!!

    delete mallardDuck;
    delete rubberDuck;
}
```

`MallardDuck`과 `RubberDuck` 객체를 생성하고 행동을 실행합니다.

`RubberDuck`의 비행 행동을 `FlyRocketPowered`로 변경한 후 다시 `PerformFly`함수를 실행하면 "로켓 추진으로 엄청나게 날아갑니다!!!"가 출력됩니다.

동적 할당된 객체를 `delete`하여 메모리 누수를 방지합니다.