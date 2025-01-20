---
layout: single

title: "[Design Pattern] 옵저버 패턴"

categories:
    - DesignPattern
tag: [디자인 패턴]

date: 2025-01-20
last_modified_at: 2025-01-20

order : 30
---

# 옵저버 패턴

옵저버(Observer) 패턴은 한 객체의 상태가 변경되면, 이 객체에 의존하는 다른 객체들에게 자동으로 알림이 전달되도록 하는 디자인 패턴입니다.  
주로 이벤트 기반 시스템이나 데이터의 변경 사항을 실시간으로 반영해야 하는 경우에 사용됩니다.

여기서 한 객체는 주제(Subject)라고 하며, 이 객체에 의존하는 다른 객체를 옵저버(Observers)라고 합니다.

느슨한 결합을 가능하게 하며, 주제와 옵저버간의 의존성을 최소화 하고, 인터페이스를 통해 소통하기 때문에 객체 간의 독립성을 유지할 수 있습니다.  
주제와 옵저버를 수정, 교체하거나 새로운 옵저버를 추가, 제거해도 주제의 코드에 영향을 미치지 않는 독립적인 구조이므로, 유지보수를 용이하게 한다는 장점이 있습니다.

객체 간의 1:N 의존성을 정의하는 패턴이라고 할 수 있습니다.  
예를 들어 게임 상태 변경 알림과 관련해서 적용해볼 수 있습니다.  
게임에서 플레이어가 데미지를 입거나 회복되면 체력 상태가 변경되고, 이를 기반으로 체력바 표시 UI가 업데이트 되고, 데미지를 입거나 체력이 회복되는 사운드 효과가 재생되고, 특정 경우 게임 오버 처리 등이 실행될 수 있습니다.

옵저버가 많아질수록 알림 비용이 증가해 성능에 영향을 줄 수 있습니다.

순환 참조가 발생하지 않도록 주의해야합니다.

## 옵저버 패턴 예시

```cpp
#include <iostream>
#include <vector>
#include <memory>
#include <string>
#include <algorithm> // std::remove_if

using namespace std;

// 추상 옵저버 (Observer): 체력 변경 이벤트를 관찰하는 인터페이스
class IObserver {
public:
    virtual ~IObserver() = default;
    virtual void Update(int health) = 0; // 상태 변경 시 호출
};

// 주제 (Subject): 체력을 관리하고 옵저버들에게 상태 변화를 알림
class Player {
private:
    vector<shared_ptr<IObserver>> observers; // 옵저버 목록
    int health;                              // 플레이어 체력

public:
    Player(int initialHealth) : health(initialHealth) {}

    // 옵저버 등록
    void Attach(const shared_ptr<IObserver>& observer) {
        observers.push_back(observer);
    }

    // 옵저버 제거
    void Detach(const shared_ptr<IObserver>& observer) {
        observers.erase(remove(observers.begin(), observers.end(), observer), observers.end());
    }

    // 상태 변경 시 모든 옵저버에게 알림
    void Notify() {
        for (const auto& observer : observers) {
            observer->Update(health);
        }
    }

    // 체력 감소
    void TakeDamage(int damage) {
        health -= damage;
        if (health < 0) health = 0; // 체력이 음수로 내려가지 않도록 제한
        cout << "Player took " << damage << " damage. Current health: " << health << endl;
        Notify(); // 옵저버들에게 상태 변경 알림
    }

    // 체력 회복
    void Heal(int amount) {
        health += amount;
        cout << "Player healed " << amount << ". Current health: " << health << endl;
        Notify(); // 옵저버들에게 상태 변경 알림
    }
};

// 구체 옵저버 (Concrete Observers)

// UI 업데이트 옵저버
class HealthBar : public IObserver {
public:
    void Update(int health) override {
        cout << "[UI] Health bar updated: " << health << endl;
    }
};

// 사운드 효과 옵저버
class SoundEffect : public IObserver {
public:
    void Update(int health) override {
        if (health > 0) {
            cout << "[Sound] Damage sound played." << endl;
        } else {
            cout << "[Sound] Game over sound played." << endl;
        }
    }
};

// 게임 오버 처리 옵저버
class GameOverHandler : public IObserver {
public:
    void Update(int health) override {
        if (health <= 0) {
            cout << "[GameOver] Player is dead. Game Over!" << endl;
        }
    }
};

int main() {
    // 1. 주제(Subject) 생성
    auto player = make_shared<Player>(100); // 초기 체력 100

    // 2. 옵저버(Observers) 생성
    auto healthBar = make_shared<HealthBar>();
    auto soundEffect = make_shared<SoundEffect>();
    auto gameOverHandler = make_shared<GameOverHandler>();

    // 3. 옵저버 등록
    player->Attach(healthBar);
    player->Attach(soundEffect);
    player->Attach(gameOverHandler);

    // 4. 플레이어 체력 변화
    player->TakeDamage(30); // 체력 100 -> 70
    player->TakeDamage(50); // 체력 70 -> 20
    player->TakeDamage(25); // 체력 20 -> 0, 게임 오버

    // 5. 플레이어 체력 회복
    player->Heal(50); // 체력 회복 (옵저버가 상태를 업데이트)

    return 0;
}
```