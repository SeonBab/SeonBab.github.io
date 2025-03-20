---
layout: single

title: "[Design Pattern] Facade Pattern(파사드 패턴)"

categories:
    - DesignPattern
tag: [디자인 패턴]

date: 2025-03-20
last_modified_at: 2025-03-20

order : 1090
---

# Facade Pattern

Facade Pattern(파사드 패턴)은 여러 개의 복잡한 서브시스템(하위 모듈, 클래스 집합)의 인터페이스를 하나의 간결한 인터페이스로 통합하여 제공하는 구조(Structural) 디자인 패턴입니다.

파사드라는 용어는 건물의 정면을 의미하는데, 이는 클라이언트가 복잡한 내부 시스템과 직접 상호작용하지 않고 단순한 인터페이스(파사드)를 통해 여러 기능을 쉽게 사용할 수 있도록 돕는다는 의미를 내포합니다.

이를 통해 복잡한 시스템에 대한 접근을 단순화하여 클라이언트 코드의 사용성을 높이고, 내부 구현 변경 시 클라이언트에 미치는 영향을 최소화할 수 있습니다.  
즉, 캡슐화를 통해 추상화를 실현하는 디자인 패턴이라고 할 수 있습니다.

+ 구조
    - Facade (파사드): 클라이언트가 직접 사용하게 되는 단순한 인터페이스를 제공합니다. 클라이언트의 요청을 받아 적절한 서브시스템으로 전달하고 처리합니다.  
    - Subsystem Classes (서브시스템 클래스): 실제로 기능을 수행하는 여러 클래스로, 파사드가 이들을 내부적으로 호출하여 작업을 처리합니다.  
    - Client (클라이언트): 파사드 인터페이스를 통해 서브시스템의 기능을 사용합니다.

+ 주요 목적
    - 복잡한 서브시스템을 캡슐화하여 단순한 API 제공
    - 클라이언트 코드의 의존성을 줄이고 유지보수를 쉽게 함
    - 하위 시스템의 변경이 있어도 클라이언트 코드에 영향을 최소화

+ 장점
    - 클라이언트는 복잡한 내부 서브시스템을 직접 알 필요 없이, 단일 인터페이스를 통해 모든 기능을 사용할 수 있습니다.
    - 클라이언트와 서브시스템 간의 직접적인 의존성이 줄어들어, 내부 시스템 변경 시 클라이언트 코드에 미치는 영향을 최소화합니다.
    - 내부 서브시스템이 변경되어도 파사드 클래스만 수정하면 되므로, 전체 시스템의 유지보수가 용이해집니다.
    - 내부의 복잡한 로직을 감추어 클라이언트에 단순한 인터페이스만 제공함으로써, 시스템의 복잡성을 효과적으로 관리할 수 있습니다.

+ 단점
    - 서브 시스템의 모든 기능을 감출 수는 없습니다.
    - 파사드가 모든 기능을 캡슐화하다 보면, 서브시스템의 특정 기능에 대해 세부적인 제어가 어려워질 수 있어 유연성 제한됩니다.
    - 단순화된 인터페이스를 제공하기 위해 별도의 클래스 계층이 추가되면서 시스템 구조가 다소 복잡해질 수 있습니다.
    - 파사드가 너무 커질 경우 단일 책임 원칙(SRP)을 위반할 수 있습니다.

Facade는 복잡한 서브시스템을 단순화하는 역할을 하고, Adapter는 서로 다른 인터페이스를 변환하는 역할을 합니다.

## 예시

다음과 같은 상황에서 사용합니다.

1. 대규모 라이브러리/프레임워크에서 사용하여 복잡한 API를 단순한 인터페이스로 감싸서 개발자가 쉽게 사용할 수 있도록 합니다.
2. 복잡하거나 낡은 기존 시스템(레거시 시스템)을 파사드로 감싸서 새로운 시스템과 연동하기 쉽게 만듭니다.
3. 전투, 미션, 이벤트 처리 등 여러 시스템이 복합적으로 작동하는 게임 로직에서 파사드 패턴을 활용하여 코드의 가독성과 유지보수를 개선할 수 있습니다.

<details>
<summary><h5 style="display: inline;">게임에서의 전투 시스템</h5></summary>
<div markdown="1">

```cpp
#include <iostream>
#include <random>
#include <thread>
#include <chrono>

// 적의 행동을 담당하는 클래스
class EnemySystem
{
private:
    int health;
    std::random_device rd;
    std::mt19937 gen;

public:
    EnemySystem(int hp) : health(hp), gen(rd()) {}

    // 공격을 수행하며, 데미지를 반환
    int Attack()
    {
        std::uniform_int_distribution<int> dist(10, 20);
        int damage = dist(gen);
        std::cout << "[EnemySystem] 적이 " << damage << "의 데미지를 입힙니다.\n";
        return damage;
    }

    // 데미지를 받을 때 체력을 감소시키는 메서드
    void ReceiveDamage(int damage)
    {
        health -= damage;
        if (health < 0) health = 0;
        std::cout << "[EnemySystem] 적의 체력이 " << damage << "만큼 감소하여 현재 체력: " << health << "\n";
    }

    // 현재 체력을 반환
    int GetHealth() const { return health; }
};

// 플레이어의 행동을 담당하는 클래스
class PlayerSystem
{
private:
    int health;
    int mana;
    std::random_device rd;
    std::mt19937 gen;

public:
    PlayerSystem(int hp, int mp) : health(hp), mana(mp), gen(rd()) {}

    // 플레이어가 공격을 수행하여 데미지를 반환
    int Attack()
    {
        std::uniform_int_distribution<int> dist(15, 25);
        int damage = dist(gen);
        std::cout << "[PlayerSystem] 플레이어가 " << damage << "의 데미지를 입힙니다.\n";
        return damage;
    }

    // 플레이어가 데미지를 받을 때 체력을 감소시키는 메서드
    void ReceiveDamage(int damage)
    {
        health -= damage;
        if (health < 0) health = 0;
        std::cout << "[PlayerSystem] 플레이어의 체력이 " << damage << "만큼 감소하여 현재 체력: " << health << "\n";
    }

    // 플레이어의 현재 체력을 반환
    int GetHealth() const { return health; }
};

// 데미지를 계산하는 클래스
class DamageCalculator
{
public:
    // 기본 데미지에 추가적인 수정치를 적용하여 최종 데미지를 계산
    int CalculateDamage(int baseDamage, int modifier)
    {
        int totalDamage = baseDamage + modifier;
        std::cout << "[DamageCalculator] 기본 데미지 " << baseDamage << "에 수정치 " << modifier << "를 더해 총 " << totalDamage << "의 데미지.\n";
        return totalDamage;
    }
};

// 공격 애니메이션을 처리하는 클래스
class AnimationSystem
{
public:
    void PlayAttackAnimation(const std::string& actor)
    {
        std::cout << "[AnimationSystem] " << actor << "의 공격 애니메이션을 재생합니다...\n";
        std::this_thread::sleep_for(std::chrono::milliseconds(300));
        std::cout << "[AnimationSystem] " << actor << "의 공격 애니메이션 재생 완료.\n";
    }
};

// 사운드 효과를 처리하는 클래스
class SoundSystem
{
public:
    void PlaySoundEffect(const std::string& effectName)
    {
        std::cout << "[SoundSystem] '" << effectName << "' 사운드 효과를 재생합니다...\n";
        std::this_thread::sleep_for(std::chrono::milliseconds(200));
        std::cout << "[SoundSystem] '" << effectName << "' 사운드 효과 재생 완료.\n";
    }
};

// 전투 시스템의 파사드 역할을 하는 클래스
class BattleFacade
{
private:
    EnemySystem* enemy;
    PlayerSystem* player;
    DamageCalculator damageCalculator;
    AnimationSystem animationSystem;
    SoundSystem soundSystem;

public:
    BattleFacade(EnemySystem* e, PlayerSystem* p) : enemy(e), player(p) {}

    // 전투 턴을 실행하는 메서드 (플레이어와 적이 서로 공격)
    void ExecuteBattleTurn()
    {
        std::cout << "\n--- 전투 턴 시작 ---\n";

        // 플레이어 공격
        animationSystem.PlayAttackAnimation("플레이어");
        soundSystem.PlaySoundEffect("플레이어 공격 효과음");
        int playerBaseDamage = player->Attack();
        int playerDamage = damageCalculator.CalculateDamage(playerBaseDamage, 5); // 공격력 보정치 적용
        enemy->ReceiveDamage(playerDamage);

        // 적이 살아있다면 반격
        if (enemy->GetHealth() > 0)
        {
            animationSystem.PlayAttackAnimation("적");
            soundSystem.PlaySoundEffect("적 공격 효과음");
            int enemyBaseDamage = enemy->Attack();
            int enemyDamage = damageCalculator.CalculateDamage(enemyBaseDamage, -3); // 방어 효과 적용
            player->ReceiveDamage(enemyDamage);
        }
        else
        {
            std::cout << "[BattleFacade] 적이 이미 쓰러졌습니다.\n";
        }

        std::cout << "--- 전투 턴 종료 ---\n\n";
    }
};

int main()
{
    // 적과 플레이어 객체 생성 (초기 체력 설정)
    EnemySystem enemy(100);
    PlayerSystem player(120, 50);

    // 전투 파사드 객체 생성
    BattleFacade battleFacade(&enemy, &player);

    // 전투 진행 (한 명이 쓰러질 때까지 반복)
    while (enemy.GetHealth() > 0 && player.GetHealth() > 0)
    {
        battleFacade.ExecuteBattleTurn();

        // 전투 턴 간 대기 시간
        std::this_thread::sleep_for(std::chrono::seconds(1));
    }

    std::cout << "전투가 종료되었습니다." << std::endl;
}
```

</div>
</details>