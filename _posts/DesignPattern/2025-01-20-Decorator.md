---
layout: single

title: "[Design Pattern] 데코레이터 패턴"

categories:
    - DesignPattern
tag: [디자인 패턴]

date: 2025-01-20
last_modified_at: 2025-01-20

order : 20
---

# 데코레이터 패턴

데코레이터(Decorator) 패턴은 객체에 동적으로 새로운 기능을 추가하거나 수정할 때 사용하는 디자인 패턴입니다.

객체를 다른 객체로 감사(Decorate) 추가 기능을 제공하는 방식입니다.

게임에서는 캐릭터의 능력치 강화, 아이템 효과 추가 등에 적용해볼 수 있을 것 같습니다.

단일 책임 원칙(SRP)으로 클래스를 단일 기능 단위로 분리합니다.  
개방-폐쇄 원칙(OCP)으로 객체에 새로운 기능 추가 혹은 수정할 때, 기존 코드를 수정 할 필요가 없습니다.

자식 클래스를 만들지 않고, 객체의 기능을 확장할 수 있습니다.

객체의 계층이 많아질수록 코드 가독성이 떨어질 수 있습니다.

중첩된 데코레이터가 많을 경우 디버깅이 어려울 수 있습니다.  
예를 들어, 여러 강화 옵션이 적용된 무기를 디버깅할 때, 특정 기능이 어느 데코레이터에서 추가되었는지 확인하기 복잡해질 수 있습니다.

데코레이터 패턴은 아래와 같은 단계로 동작합니다.

1. 데코레이션할 핵심되는 기본 객체를 생성합니다.
2. 데코레이터를 통해 핵심 객체에 기능을 동적으로 추가합니다.
3. 여러 데코레이터를 중첩해 필요한 기능을 조합합니다.
4. 데코레이션된 객체를 사용하여 원하는 기능을 수행합니다.

## 데코레이터 패턴 예시

게임에서는 캐릭터의 능력치 강화, 아이템 효과 추가 등을 예시로 한 구현입니다.

```cpp
#include <iostream>
#include <string>

using namespace std;

// 추상 컴포넌트 (Component): Weapon
// - 무기 객체의 기본 구조를 정의하는 인터페이스입니다.
// - 모든 무기는 이름(`getName`)과 공격력(`getDamage`)을 가져야 합니다.
class Weapon {
public:
    virtual ~Weapon() {}
    virtual string getName() const = 0;  // 무기의 이름 반환
    virtual int getDamage() const = 0;  // 무기의 기본 공격력 반환
};

// 구체 컴포넌트 (Concrete Component): BasicWeapon
// - 기본 무기 클래스입니다.
// - 무기의 기본 이름과 공격력을 구현합니다.
class BasicWeapon : public Weapon {
public:
    string getName() const {
        return "Basic Sword"; // 기본 무기의 이름
    }

    int getDamage() const {
        return 10; // 기본 무기의 공격력
    }
};

// 데코레이터 추상 클래스 (Decorator): WeaponDecorator
// - 기존 무기의 기능을 확장하기 위한 데코레이터의 기본 구조를 정의합니다.
// - 내부적으로 `Weapon` 객체를 감싸며, 이름과 공격력에 추가적인 기능을 제공합니다.
class WeaponDecorator : public Weapon {
protected:
    Weapon* weapon; // 기존의 무기 객체를 참조합니다.
public:
    WeaponDecorator(Weapon* w) : weapon(w) {}
    virtual ~WeaponDecorator() {
        delete weapon;
    }
};

// 구체 데코레이터 (Concrete Decorators): DamageBoost, CriticalBoost, SpeedBoost
// - 각각의 강화 옵션은 `WeaponDecorator`를 상속받아 이름과 공격력을 확장합니다.

// 데미지 증가 강화
class DamageBoost : public WeaponDecorator {
public:
    DamageBoost(Weapon* w) : WeaponDecorator(w) {}

    string getName() const {
        return weapon->getName() + " + Damage Boost";
    }

    int getDamage() const {
        return weapon->getDamage() + 5; // 데미지 +5
    }
};

// 크리티컬 확률 증가 강화
class CriticalBoost : public WeaponDecorator {
public:
    CriticalBoost(Weapon* w) : WeaponDecorator(w) {}

    string getName() const {
        return weapon->getName() + " + Critical Boost";
    }

    int getDamage() const {
        return weapon->getDamage() + 3; // 크리티컬 데미지 증가 +3
    }
};

// 속도 증가 강화
class SpeedBoost : public WeaponDecorator {
public:
    SpeedBoost(Weapon* w) : WeaponDecorator(w) {}

    string getName() const {
        return weapon->getName() + " + Speed Boost";
    }

    int getDamage() const {
        return weapon->getDamage() + 2; // 공격 속도 증가 (간접적으로 데미지 증가로 가정)
    }
};

// 무기와 데코레이터를 조합하여 최종 무기를 생성하고, 정보를 출력합니다.
int main() {
    // 1. 기본 무기를 생성합니다.
    Weapon* weapon = new BasicWeapon();

    // 2. 데미지 강화 추가
    weapon = new DamageBoost(weapon);

    // 3. 크리티컬 확률 강화 추가
    weapon = new CriticalBoost(weapon);

    // 4. 속도 강화 추가
    weapon = new SpeedBoost(weapon);

    // 5. 최종 무기 정보 출력
    cout << "Weapon: " << weapon->getName() << endl; // 무기의 이름 출력
    cout << "Damage: " << weapon->getDamage() << endl; // 무기의 데미지 출력

    // 6. 메모리 해제
    delete weapon;

    return 0;
}
```