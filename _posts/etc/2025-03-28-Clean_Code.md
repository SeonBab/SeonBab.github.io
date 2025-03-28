---
layout: single

title: "클린 코드를 적용해야하는 경우"

categories:
    - etc
tag: [etc]

date: 2025-03-27
last_modified_at: 2025-03-27

order : 30

mermaid: true
---

# 클린 코드

클린 코드란 가독성이 좋고 유지보수가 쉬운 코드를 의미합니다.

이해하기 쉽고, 수정과 확장이 용이하며, 버그 발생 가능성이 적은 코드를 목표로 합니다.

일반적으로 다음과 같은 문제가 있습니다.  

+ 불필요한 복잡성이 증가하고, 가독성이 낮아지며 유지보수 비용이 증가합니다.  
+ 메모리에 할당될 수 있으며, 최적화에 좋지 않습니다.

클린 코드를 적용한다면 다음과 같습니다.

+ 코드가 복잡한 경우 코드 가독성을 향상시키고, 유지보수가 용이해지도록 해줍니다.




좋은 코드는 말이 필요 없고, 나쁜 코드는 아무리 설명해도 부족하다.

## 클린 코드를 적용해야하는 예시

<details>
<summary><h5 style="display: inline;">1. 난해한 변수, 함수, 클래스 이름(Mysterious Name)</h5></summary>
<div markdown="1">

변수명과 함수명은 이름만 보고도 무슨 일을 하는지 알아야합니다.

명확한 이름이 떠오르지 않는다면 설계가 잘못됐을 수 있습니다.

의미를 정확히 표현해야합니다.  
이 과정에서 축약어를 남발하거나 포괄적인 이름은 좋지 않습니다.

함수명은 동사, 변수명은 명사를 사용합니다.

클래스명은 객체의 역할을 명확하게 전달해야합니다.

나쁜 예시
```cpp
// 이름만 보고는 이게 뭔지 알 수가 없다
void DoIt(int x);
void Func();

// 의미가 전혀 안 드러나는 변수들
int a, b, c;
a = 10;
b = 20;
c = a + b;

// 축약어 사용으로 인해 너무 짧거나 모호한 네이밍
int cstmr;  // 고객(Customer)?
int trnsAmt; // 거래 금액(Transaction Amount)?

// 애매한 클래스 이름
// 어떤 데이터를 관리하는지 이해하기 어렵다
class DataManager
{
    void save();
    void load();
};
```

좋은 예시
```cpp
// 함수의 의도가 명확한 경우
void AttackEnemy(int DamageAmount);

// 변수의 역할이 명확하다
float CurrentHealth;
int EnemyCount;
int itemPrice = 10;
int tax = 20;
int totalPrice = itemPrice + tax;

// 명확한 이름 사용
int customer;
int transactionAmount;

// 명확한 클래스 이름
// 이름을 통해 사용자 데이터를 관리하는 클래스임을 알 수 있음
class UserDataStorage
{
    void saveUser();
    void loadUser();
};
```

</div>
</details>

<details>
<summary><h5 style="display: inline;">2. 중복 코드(Duplicated Code)</h5></summary>
<div markdown="1">

중복 코드는 동일하거나 유사한 코드가 여러 곳에 반복적으로 나타나는 현산을 의미합니다.

확장성을 저하시킵니다.  
예를 들어, 특정 로직을 수정할 때 여러 곳을 수정해야 하므로 실수할 가능성이 높아집니다.

`DRY - Don't Repeat Yourself`라는 정보의 반복을 줄이는 것을 목표로 하는 소프트웨어 개발의 기본 원칙이 있습니다.

중복되는 코드는 하나만 수정해도 되도록 모아놓아야 합니다.  
함수나 템플릿을 사용하여 중복을 제거할 수 있습니다.  
변수의 경우는 구조체나 클래스를 사용해서 중복을 제거할 수 있습니다.

나쁜 예시
```cpp
// 같은 로직으로 중복되는 코드를 가진 함수
// 플레이어 데미지 처리
void PlayerTakeDamage(float Amount)
{
    Health -= Amount;
    if (Health <= 0)
    {
        Die();
    }
}

// 보스 데미지 처리
void BossTakeDamage(float Amount)
{
    Health -= Amount;
    if (Health <= 0)
    {
        SummonMinions(); // 보스라서 특별히 미니언을 소환
        Die();
    }
}
```

좋은 예시
```cpp
// 공통 부모 클래스에서 데미지 로직을 통일
class ACharacterBase
{
protected:
    virtual void OnDeath() { /* 사망에 대한 처리 */ }
    
public:
    void TakeDamage(float Amount)
    {
        Health -= Amount;
        if (Health <= 0) 
        {
            OnDeath();
        }
    }
};

// 플레이어
class APlayerCharacter : public ACharacterBase
{
protected:
    virtual void OnDeath() override 
    {
        // 플레이어 전용 사망 처리
    }
};

// 보스
class ABoss : public ACharacterBase
{
protected:
    virtual void OnDeath() override
    {
        SummonMinions();
        // 보스 전용 사망 처리
    }
};
```

나쁜 예시
```cpp
// 구조체 없이 비슷한 데이터 그룹이 반복되는 경우
string user1Name = "Alice";
int user1Age = 25;

string user2Name = "Bob";
int user2Age = 30;
```

좋은 예시
```cpp
// 구조체 혹은 클래스를 사용
struct User
{
    string name;
    int age;
};

User users[] = {% raw %}{{"Alice", 25}, {"Bob", 30}}{% endraw %};
```

</div>
</details>

<details>
<summary><h5 style="display: inline;">3. 코드가 긴 함수</h5></summary>
<div markdown="1">

한 파일이나 함수에 너무 많은 코드가 있으면 이해하기 어려워 가독성이 저하됩니다.  
특정 기능을 수정하려면 긴 코드에서 찾아야 하므로, 유지보수가 어렵습니다.  
같은 기능을 다시 사용하려면 복사&붙여넣기를 반복하므로 재사용성이 떨어지며, 중복되는 코드를 가지게 됩니다.  
단일 책임 원칙에 위배됩니다.

함수의 코드를 짧게 만들어야 공유하기 좋습니다.  
주석이 필요하다고 느껴지는 부분은 별도의 함수, 멤버함수로 빼도 좋습니다.  
함수를 분리하고, 모듈화해주어 해결할 수 있습니다.

나쁜 예시
```cpp
void AMyCharacter::Tick(float DeltaTime)
{
    Super::Tick(DeltaTime);

    // 1. 이동 처리
    // 2. 점프 처리
    // 3. 공격 처리
    // 4. 버프/디버프 처리
    // 5. 체력 체크
    // 6. 애니메이션 업데이트
    // ...
    // ...
}
```

좋은 예시
```cpp
void AMyCharacter::Tick(float DeltaTime)
{
    Super::Tick(DeltaTime);
    
    HandleMovement(DeltaTime);
    HandleJump();
    HandleAttack();
    UpdateAnimation();
}

void AMyCharacter::HandleMovement(float DeltaTime)
{
    // 이동 관련 로직만 심플하게!
}

void AMyCharacter::HandleJump()
{
    // 점프 관련 로직만 모아둠
}

void AMyCharacter::HandleAttack()
{
    // 공격 로직
}
```

</div>
</details>

<details>
<summary><h5 style="display: inline;">4. 매개변수 과부하(Parameter Bloat)</h5></summary>
<div markdown="1">

매개변수 과부하는 함수에 매개변수가 너무 많은 경우를 의미합니다.

인자가 많으면 코드가 복잡하며, 매개변수 순서를 실수할 가능성이 증가합니다.

구조체, 클래스로 묶어서 그룹화를 하고, 매개변수로 사용하는 방법이 있습니다.  
불필요한 인수가 있다면 제거합니다.

나쁜 예시
```cpp
void InitWeapon(FString Name, float Damage, float FireRate, int32 AmmoCount, float ReloadTime, USkeletalMesh* Mesh, USoundBase* Sound)
{
    
}
```

좋은 예시
```cpp
struct FWeaponData
{
    FString Name;
    float Damage;
    float FireRate;
    int32 AmmoCount;
};

struct FWeaponAssets
{
    USkeletalMesh* Mesh;
    USoundBase* Sound;
};

void InitWeapon(const FWeaponData& InData, const FWeaponAssets& InAssets)
{
    
}
```

</div>
</details>

<details>
<summary><h5 style="display: inline;">5. 전역 변수(Global Variable)</h5></summary>
<div markdown="1">

전역 변수는 프로그램 어디서든 접근할 수 있는 변수입니다.

무분별하게 사용한다면 유지보수와 디버깅이 어려워진다는 문제가 발생합니다.

전역 변수는 최대한 사용하지 않아야 합니다.  
지역 변수로 사용하거나 클래스를 통해 캡슐화하는 방법이 있습니다.  
언리얼 엔진의 경우 `Subsystem`이 있습니다.  
[Subsytem 공식 문서](https://dev.epicgames.com/documentation/ko-kr/unreal-engine/programming-subsystems-in-unreal-engine){: target="_blank"}

나쁜 예시
```cpp
#include <iostream>

using namespace std;

int score = 0; // 전역 변수

void addScore(int value)
{
    score += value;
}

void showScore()
{
    cout << "Current Score: " << score << endl;
}

int main()
{
    addScore(10);
    showScore();
}
```

좋은 예시
```cpp
#include <iostream>

using namespace std;

void showScore(int score)
{
    cout << "Current Score: " << score << endl;
}

int main()
{
    int score = 0; // 지역 변수 사용
    score += 10;
    showScore(score);
}
```

```cpp
#include <iostream>

using namespace std;

// 클래스 사용
class Game
{
private:
    int score;

public:
    Game() : score(0) {} // 초기화

    void addScore(int value) { score += value; }
    void showScore() { cout << "Current Score: " << score << endl; }
};
```

```cpp
// 언리얼 Subsystem을 사용
UCLASS()
class UScoreSystem : public UGameInstanceSubsystem
{
    GENERATED_BODY()

private:
    int32 Score;

public:
    void AddScore(int32 Amount)
    {
        Score += Amount;
        // 점수가 변경됐음을 알리는 로직
    }

    int32 GetScore() const { return Score; }
};

// 사용 예시
void AEnemy::OnDefeated()
{
    if (UGameInstance* GI = GetGameInstance())
    {
        if (UScoreSystem* ScoreSys = GI->GetSubsystem<UScoreSystem>())
        {
            ScoreSys->AddScore(50);
        }
    }
}
```

</div>
</details>

<details>
<summary><h5 style="display: inline;">6. 가변 데이터(Mutable Data)</h5></summary>
<div markdown="1">

가변 데이터는 생성된 후에 값이나 상태를 변경할 수 있는 데이터를 의미합니다.

접근 제어자가 `public`인 경우 클래스 멤버가 외부에 공개되며, 누구나 접근하고 수정할 수 있다는 문제가 있습니다.  
이로인해 의도치 않게 값이 변경될 수 있으며, 어디서 값이 변경되었는지 추적하기 어려울 수 있습니다.

캡슐화하고, Get/Set 함수 등을 통해 변경하도록 변경 가능한 범위를 최소화해야합니다.  
수정할 필요가 없다면 불변 데이터(Immutable Data)로 설정하는 것이 좋습니다.

나쁜 예시
```cpp
class APlayerCharacter
{
public:
    float Health;
    int32 Level;
};

void SomeRandomFunc(APlayerCharacter* Player)
{
    Player->Health = 99999.f;
    Player->Level = 999;
}
```

좋은 예시
```cpp
// 캡슐화
class APlayerCharacter
{
private:
    float Health;
    int32 Level;

public:
    float GetHealth() const { return Health; }
    int32 GetLevel() const { return Level; }

    void TakeDamage(float Amount)
    {
        Health = FMath::Max(0.0f, Health - Amount);
        // 데미지 받은 로직은 여기에만!
    }

    void LevelUp()
    {
        Level++;
        Health = 100.f * Level;
    }
};
```

</div>
</details>

<details>
<summary><h5 style="display: inline;">7. 뒤엉킨 변경(Divergent Change)</h5></summary>
<div markdown="1">

뒤엉킨 변경이란 단일 책임 원칙(SRP, Single Responsibility Principle) 위반으로 인해 하나의 클래스가 너무 많은 역할을 수행하면서, 여러 이유로 자주 변경되는 문제를 의미합니다.

다른 맥락의 동작은 각각 다른 모듈로 분리해 단일 책임을 지켜야합니다.

역할별로 클래스를 분리합니다.

나쁜 예시
```cpp
// 한 클래스가 여러가지의 역할 수행
class AGameManager
{
public:
    // (1) 데이터 관련
    void LoadPlayerData();
    void SavePlayerData();

    // (2) 게임플레이 관련
    void StartNewGame();
    void SpawnEnemies();

private:
    // (1) 데이터 관련 필드
    FString SaveFilePath;

    // (2) 게임플레이 관련 필드
    TArray<AEnemy*> ActiveEnemies;
};
```

좋은 예시
```cpp
// 역할별로 클래스를 분리
// (1) 데이터 전용 클래스
class UPlayerDataManager : public UGameInstanceSubsystem
{
public:
    void LoadPlayerData();
    void SavePlayerData();
    // ...
};

// (2) 게임플레이 전용 클래스
class UGameplayManager : public UGameInstanceSubsystem
{
public:
    void StartNewGame();
    void SpawnEnemies();
};
```

</div>
</details>

<details>
<summary><h5 style="display: inline;">8. 샷건 수술/산탄총 수술(Shotgun Surgery)</h5></summary>
<div markdown="1">

샷건 수술은 작은 변경을 위해 여러 클래스나 여러 곳을 수정해야 하는 상황을 설명하는 디자인 문제입니다.

하나의 변경 요구에 대해 여러 파일이나 클래스를 수정해야 하므로, 코드 유지보수가 복잡하고, 변경이 번거롭다는 문제가 있습니다.

책임을 명확하게 분리하여, 관련된 기능을 하나의 클래스로 모아주어야 합니다.

나쁜 예시
```cpp
class APlayerCharacter : public ACharacter
{
public:
    void TakeDamage(float Amount)
    {
        // 데미지 로직 1
    }
};

class AWeapon : public AActor
{
public:
    float CalculateDamage()
    {
        // 데미지 로직 2
        return 0.0f;
    }
};

class AMyGameMode : public AGameModeBase
{
public:
    void UpdateDamageLeaderboard()
    {
        // 데미지 로직 3
    }
};
```

좋은 예시
```cpp
// 관련된 기능을 하나의 클래스로 모아주는 방법
class UDamageSystem
{
public:
    float CalculateDamage(AWeapon* Weapon, ACharacter* Target);
    void ApplyDamage(AWeapon* Weapon, ACharacter* Target);
    void UpdateDamageLeaderboard(ACharacter* Damager, ACharacter* Target, float Amount);
};
```

</div>
</details>

<details>
<summary><h5 style="display: inline;">9. 기능 편애(Feature Envy)</h5></summary>
<div markdown="1">

기능 편애는 한 클래스가 다른 클래스의 메서드나 데이터를 과도하게 호출하여, 해당 클래스의 책임을 넘어서서 다른 클래스에 대한 지나친 의존성을 가지는 경우를 의미합니다.  
즉, 한 클래스가 다른 클래스의 메서드를 지나치게 호출하고, 해당 클래스의 내부 구조에 대해 너무 많이 알게되는 문제입니다.

클래스의 책임이 명확하지 않게 되어, 응집력이 떨어지고 클래스가 비대해집니다.  
한 클래스가 다른 클래스를 과도하게 참조하게 되므로 결합도가 증가합니다.  
다른 클래스의 세부 구현을 의존하므로, 호출한 메서드나 데이터가 수정된다면 사용하는 클래스도 함께 수정해야할 수 있습니다.

함수를 데이터가 있는 곳으로 옮기는 기능 이동(Move Feature) 방법이 있습니다.

나쁜 예시
```cpp
class UDamageCalculator
{
public:
    float CalculateDamageReduction(AMyCharacter* Character, float Damage)
    {
        // Character의 정보를 사용
        float HealthPercent = Character->GetHealth() / Character->GetMaxHealth();
        float ArmorFactor   = Character->GetArmor() * 0.1f;
        // ...
        return Damage * (1.0f - ArmorFactor * HealthPercent);
    }
};
```

좋은 예시
```cpp
class AMyCharacter : public ACharacter
{
public:
    float CalculateDamageReduction(float Damage) const
    {
        float HealthPercent = Health / MaxHealth;
        float ArmorFactor   = Armor * 0.1f;
        // ...
        return Damage * (1.0f - ArmorFactor * HealthPercent);
    }
};

class UDamageCalculator
{
public:
    float CalculateDamageReduction(AMyCharacter* Character, float Damage)
    {
        // Character가 스스로 계산
        return Character->CalculateDamageReduction(Damage);
    }
};
```

</div>
</details>

<details>
<summary><h5 style="display: inline;">10. 데이터 뭉치(Data Clumps)</h5></summary>
<div markdown="1">

데이터 뭉치는 여러 클래스에서 자주 함께 사용되는 여러 변수들이 한 곳에서, 한 번에 처리되고 있는 상황을 의미합니다.

자주 함께 쓰이는 데이터는 하나로 묶으면 의미가 명확해집니다.

중복되는 필드나 매개변수 그룹을 구조체나 클래스로 분리하는 방법이 있습니다.

나쁜 예시
```cpp
void FireWeapon(float Damage, float Range, float Accuracy);
void ShowWeaponStats(float Damage, float Range, float Accuracy);
void UpgradeWeapon(float& Damage, float& Range, float& Accuracy);
```

좋은 예시
```cpp
// 무기 스탯 구조체
struct FWeaponStats
{
    GENERATED_BODY()

    UPROPERTY(EditAnywhere, BlueprintReadWrite)
    float Damage;

    UPROPERTY(EditAnywhere, BlueprintReadWrite)
    float Range;

    UPROPERTY(EditAnywhere, BlueprintReadWrite)
    float Accuracy;
};

void FireWeapon(const FWeaponStats& Stats);
void ShowWeaponStats(const FWeaponStats& Stats);
void UpgradeWeapon(FWeaponStats& Stats);
```

</div>
</details>

<details>
<summary><h5 style="display: inline;">11. 기본형 집착(Primitive Obsession)</h5></summary>
<div markdown="1">

기본형 집착은 의미나 책임을 나타내는 클래스를 사용하지 않고, 기본 데이터타입을 지나치게 사용하는 문제를 의미합니다.  
즉, 복잡한 데이터를 단순한 기본형(int, string)에 과도하게 의존하는 경향입니다.

같은 로직을 반복하게 되므로, 불필요한 중복을 발생시킵니다.  
의미가 표현되지 않아 파악하기 어려워져 가독성이 떨어집니다.

의미를 가지는 클래스나 구조체를 사용하는 방법이 있습니다.  
추가로 형식 검증 등의 검증 로직을 추가로 구현할 수 있습니다.

나쁜 예시
```cpp
float Health;
float MaxHealth;

FString PhoneNumber;
```

좋은 예시
```cpp
// 체력을 표현하는 클래스
class FHealth
{
public:
    FHealth(float InCurrent, float InMax)
        : Current(InCurrent), Max(InMax) {}

    void ApplyDamage(float Amount)
    {
        Current = std::max(0.f, Current - Amount);
    }
    
private:
    float Current;
    float Max;
};
```

</div>
</details>

<details>
<summary><h5 style="display: inline;">12. 반복되는 스위치문(Repeated Switches)</h5></summary>
<div markdown="1">

`Switch`문을 사용해서 분기를 처리할 경우 새로운 분기가 생길 때마다 여러 `Switch`문을 전부 수정해야하는 경우 비효율적입니다.

다형성 구조를 활용하여 인터페이스나 부모 클래스에서 처리하도록 구현하는 방법이 있습니다.  
전략 패턴을 사용하는 방법이 있습니다.

나쁜 예시
```cpp
switch (WeaponType)
{
    case EWeaponType::Sword:
        return DoSwordAttack();
    case EWeaponType::Bow:
        return DoBowAttack();
    // ...
}
```

좋은 예시
```cpp
// 다형성 활용
// 무기 베이스
class AWeapon
{
public:
    virtual void Attack();
};

// 무기별 클래스
class ASword : public AWeapon
{
public:
    virtual void Attack() override { /* 칼 공격 로직 */ }
};

class ABow : public AWeapon
{
public:
    virtual void Attack() override { /* 활 공격 로직 */ }
};
```

</div>
</details>

<details>
<summary><h5 style="display: inline;">13. 반복문(Loops)</h5></summary>
<div markdown="1">

반복문은 성능 저하의 원인으로 비즈니스 로직을 다 넣게되면 비효율적입니다.

중첩 반복문은 사용하지 않는 것이 좋습니다.

나쁜 예시
```cpp
// 인벤토리에서 무거운 아이템을 찾아서 무게를 계산하는 과정
void ProcessHeavyItems()
{
    TArray<UItem*> Items = GetAllItems();
    TArray<UItem*> HeavyItems;

    // (1) 무거운 아이템 골라내기
    for (int32 i = 0; i < Items.Num(); i++)
    {
        if (Items[i]->Weight > 10.f)
        {
            HeavyItems.Add(Items[i]);
        }
    }

    // (2) 무게 총합 계산
    float TotalWeight = 0.f;
    for (int32 j = 0; j < HeavyItems.Num(); j++)
    {
        TotalWeight += HeavyItems[j]->Weight;
    }

    // (3) 너무 무거우면 효과 적용
    if (TotalWeight > 50.f)
    {
        ApplySlowEffect();
    }
}
```

좋은 예시
```cpp
// 언리얼 엔진 기준
void ProcessHeavyItems()
{
    TArray<UItem*> Items = GetAllItems();

    // 필터링 함수(FilterByPredicate) 사용
    auto HeavyItems = Items.FilterByPredicate([](UItem* Item)
    {
        return Item->Weight > 10.f;
    });

    // 무게 계산 + 효과 적용
    float TotalWeight = 0.f;
    for (UItem* Item : HeavyItems)
    {
        TotalWeight += Item->Weight;
    }

    if (TotalWeight > 50.f)
    {
        ApplySlowEffect();
    }
}
```

</div>
</details>

<details>
<summary><h5 style="display: inline;">14. 게으른 요소/성의 없는 요소(Lazy Element)</h5></summary>
<div markdown="1">

게으른 요소란 필요하지 않은 클래스, 메서드, 변수 등이 코드에 남아있지만 실제로 거의 사용되지 않는 경우입니다.

다음과 같은 예시가 있습니다.

+ 언젠가 필요할 수도 있겠지로 남겨둔 코드  
+ 과거에는 사용했지만, 리팩토링 후 불필요해진 코드  
+ 기능이 너무 작거나, 다른 클래스에 흡수될 수 있는 불필요한 클래스/함수

함수, 변수, 작거나 불필요한 클래스는 다른 클래스로 합치거나 제거하는 방법 등이 있습니다.

나쁜 예시
```cpp
// 과도하게 중간 함수만 존재
class AProjectile
{
public:
    void Launch(const FVector& Dir, float Speed)
    {
        // 다른 함수를 호출만 하며, 다른 작업을 하지 않음
        LaunchProjectile(Dir, Speed);
    }

private:
    void LaunchProjectile(const FVector& Dir, float Speed)
    {
        // 실제 로직
        ProjectileMovement->Velocity = Dir * Speed;
    }
};

// 더이상 사용되지 않는 함수가 존재
class AEnemy
{
public:
    void Move() { /* 이동 코드 */ }
    void Attack() { /* 공격 코드 */ }

    // 이 함수는 현재 사용되지 않음
    void Hide() { /* 숨는 동작 */ }
};
```

좋은 예시
```cpp
// 불필요한 함수 제거
class AProjectile
{
public:
    void Launch(const FVector& Dir, float Speed)
    {
        ProjectileMovement->Velocity = Dir * Speed;
    }

private:
    UProjectileMovementComponent* ProjectileMovement;
};

class AEnemy
{
public:
    void Move() { /* 이동 코드 */ }
    void Attack() { /* 공격 코드 */ }
}
```

</div>
</details>

<details>
<summary><h5 style="display: inline;">15. 추측성 일반화(Speculative Generality)</h5></summary>
<div markdown="1">

추측성 일반화는 현재 필요하지 않은 기능이나 확장을 위해 미리 복잡한 구조를 만들어 놓는 것을 의미합니다.

미래 대비보다 현재 필요한 기능이나 문제 해결을 우선적으로 수행하여 불필요한 추상화를 걷어내야합니다.

나쁜 예시
```cpp
// 불필요한 인터페이스
class IWeapon
{
public:
    virtual void Attack() = 0;
};

// 단 하나의 클래스만 존재
class Sword : public IWeapon
{
public:
    void Attack() override { }
};
```

좋은 예시
```cpp
// 필요할 경우 인터페이스 사용
class Sword
{
public:
    void Attack() { }
};
```

</div>
</details>

<details>
<summary><h5 style="display: inline;">16. 특화된 필드(Specialized Fields)</h5></summary>
<div markdown="1">

특화된 필드는 특정 기능에만 필요한 필드를 의미합니다.

클래스를 분리하거나 공통된 인터페이스로 관리하는 방법이 있습니다.

나쁜 예시
```cpp
class AEnemy : public ACharacter
{
public:
    // 일반 공격
    float Health;

    // 원거리 공격 전용 (근접 적은 안 씀)
    float ProjectileSpeed;
    UParticleSystem* ProjectileEffect;

    // 텔레포트 전용 (다른 적은 안 씀)
    float TeleportCooldown;
    float LastTeleportTime;
};
```

좋은 예시
```cpp
// 언리얼 엔진의 경우 컴포넌트로 분리
class URangedAttackComponent : public UActorComponent
{
    float ProjectileSpeed;
    void ExecuteAttack();
};

class UTeleportComponent : public UActorComponent
{
    float TeleportCooldown;
    void ExecuteTeleport();
};

// 적 캐릭터
class AEnemy : public ACharacter
{
    float Health;
    URangedAttackComponent* RangedComp;   // 원거리 적만 붙임
    UTeleportComponent* TeleportComp;     // 텔레포트 적만 붙임
};
```

</div>
</details>

<details>
<summary><h5 style="display: inline;">17. 임시 필드(Temporary Field)</h5></summary>
<div markdown="1">

임시 필드는 일시적으로만 필요한 데이터를 저장하는 변수나 필드를 의미합니다.

목적이 분명치 않은 필드는 코드 복잡도를 높이는 원인입니다.

사용되지 않는 시점이 더 많다면 다른 구조 또는 클래스로 분리하거나 제거합니다.

나쁜 예시
```cpp
class Order
{
private:
    double price;
    int quantity;

    // 임시 필드
    double tempTax;

public:
    double CalculateTotal()
    {
        tempTax = price * 0.1;  // 세금 계산
        return price * quantity + tempTax;
    }
};

// 불필요한 임시 변수
void UpdateInfo(std::string newName, std::string newAddress)
    {
        std::string tempName = newName;  // 임시 변수
        std::string tempAddress = newAddress;  // 임시 변수

        name = tempName;
        address = tempAddress;
    }
```

좋은 예시
```cpp
// 불필요한 임시 필드 제거
class Order
{
private:
    double price;
    int quantity;

public:
    double CalculateTotal()
    {
        return price * quantity + price * 0.1;
    }
};

// 불필요한 임시 변수 제거
void UpdateInfo(std::string newName, std::string newAddress)
    {
        name = newName;
        address = newAddress;
    }
```

</div>
</details>

<details>
<summary><h5 style="display: inline;">18. 메시지 체인(Message Chains)</h5></summary>
<div markdown="1">

메시지 체인은 여러 객체에 연속적으로 메시지를 전달하는 구조를 의미합니다.  
즉, 하나의 객체가 다른 객체에게 메시지를 보내고, 그 객체가 또 다른 객체에 메시지를 보내는 구조입니다.

객체를 줄줄이 호출하면 내부 구조가 노출돼 결합도가 커지며, 불필요한 책임을 가지게 됩니다.

디미터의 법칙(Demeter's Law) 또는 최소 지식의 법칙에 위배됩니다.

책임을 분리하고, 자신의 책임만 수행하도록 하는 방법이 있습니다.

나쁜 예시
```cpp
// 연속적으로 이어진 참조
void APlayer::PlayWeaponSound()
{
    if (Inventory
        && Inventory->EquippedWeapon
        && Inventory->EquippedWeapon->SoundData
        && Inventory->EquippedWeapon->SoundData->AttackSound)
    {
        UGameplayStatics::PlaySound2D(this, Inventory->EquippedWeapon->SoundData->AttackSound);
    }
}
```

좋은 예시
```cpp
// 책임을 분리
void APlayer::PlayWeaponSound()
{
    USoundBase* AttackSound = GetEquippedWeaponSound();
    if (AttackSound)
    {
        UGameplayStatics::PlaySound2D(this, AttackSound);
    }
}

USoundBase* APlayer::GetEquippedWeaponSound()
{
    // 아래 호출부에서 직접 소리를 반환
    return Inventory ? Inventory->GetAttackSound() : nullptr;
}

USoundBase* UInventoryComponent::GetAttackSound()
{
    if (!EquippedWeapon) return nullptr;
    return EquippedWeapon->GetAttackSound();
}

USoundBase* AWeapon::GetAttackSound()
{
    return SoundData ? SoundData->AttackSound : nullptr;
}
```

</div>
</details>

<details>
<summary><h5 style="display: inline;">19. 중재자(Middle Man)</h5></summary>
<div markdown="1">

중재자는 객체 간의 직접적인 상호작용을 피하려할 때 사용하는 구조입니다.  
이 구조를 과도하게 사용하면, 불필요한 중간 객체가 증가할 수 있습니다.

실질적 로직 없이 위임만 하는 클래스는 직접 연결해도 문제가 없다면 중간 단계를 제거하는 방법이 있습니다.  
즉, 직관적인 구조로 수정하는 방법입니다.

나쁜 예시
```cpp
// 불필요하게 중재자가 된 경우
// 별다른 로직 없이 함수를 호출하기만 함
// 언리얼 엔진 예시
class AMyPlayerController : public APlayerController
{
public:
    void MoveForward(float Value)  { Character->MoveForward(Value); }
    void MoveRight(float Value)    { Character->MoveRight(Value); }
    void Jump()                    { Character->Jump(); }
    void StartFire()               { Character->StartFire(); }
    void StopFire()                { Character->StopFire(); }
    // ...

private:
    AMyCharacter* Character;
};
```

좋은 예시
```cpp
// 직접 캐릭터에 입력 바인딩으로 중재자를 제거
// 언리얼 엔진 예시
void AMyPlayerController::SetupInputComponent()
{
    Super::SetupInputComponent();

    // 현재 캐릭터 가져오기
    AMyCharacter* MyChar = Cast<AMyCharacter>(GetCharacter());
    if (MyChar && InputComponent)
    {
        // 캐릭터가 필요한 입력을 직접 바인딩
        MyChar->SetupPlayerInput(InputComponent);
    }
}

void AMyCharacter::SetupPlayerInput(UInputComponent* PlayerInputComponent)
{
    PlayerInputComponent->BindAxis("MoveForward", this, &AMyCharacter::MoveForward);
    PlayerInputComponent->BindAxis("MoveRight", this, &AMyCharacter::MoveRight);
    // ...
}
```

</div>
</details>

<details>
<summary><h5 style="display: inline;">20. 내부자 거래(Insider Trading)</h5></summary>
<div markdown="1">

내부자 거래는 모듈 간에 비공개 데이터의 거래가 많은 경우를 의미합니다.

비공개 데이터가 과하게 오가면 결합도가 높아집니다.

필요한 정보만 교환할 수 있게 인터페이스 범위를 명확하게 정의해야합니다.

나쁜 예시
```cpp
// AEnemy가 APlayerCharacter의 내부 변수까지 참조
void AEnemy::Attack(APlayerCharacter* Player)
{
    if (!Player->bIsInvulnerable)
    {
        float Damage = AttackDamage - Player->EquippedArmor->DamageReduction;
        Player->CurrentHealth -= Damage;

        // UI도 직접 갱신
        Player->PlayerHUD->UpdateHealthBar(Player->CurrentHealth, Player->MaxHealth);
    }
}
```

좋은 예시
```cpp
// AEnemy는 공개된 함수를 호출
void AEnemy::Attack(APlayerCharacter* Player)
{
    if (Player && Player->CanBeAttacked())
    {
        Player->ReceiveDamage(AttackDamage);
    }
}

// Player
bool APlayerCharacter::CanBeAttacked() const
{
    return !bIsInvulnerable;
}

void APlayerCharacter::ReceiveDamage(float Damage)
{
    // 갑옷 계산, HUD 업데이트 등 내부적으로 처리
    float ActualDamage = EquippedArmor ? EquippedArmor->ApplyReduction(Damage) : Damage;

    CurrentHealth = FMath::Max(0.f, CurrentHealth - ActualDamage);
    PlayerHUD->UpdateHealth(CurrentHealth, MaxHealth);
}
```

</div>
</details>

<details>
<summary><h5 style="display: inline;">21. 거대한 클래스(Large Class)</h5></summary>
<div markdown="1">

거대 클래스는 너무 많은 책임을 지고, 필드와 메서드가 폭발적으로 늘어난 클래스를 의미합니다.

중복이 생기고 관리가 어려워지므로 역할이나 기능별로 클래스를 분리해야합니다.  
사용 패턴을 분석해서 클래스를 쪼개면 유지보수가 수월해집니다.

나쁜 예시
```cpp
class AGameCharacter : public ACharacter
{
public:
    // 이동 처리
    void MoveForward(float Value);
    void MoveRight(float Value);
    // 전투 처리
    void Attack();
    void Reload();
    // 인벤토리 처리
    void AddItem(UItem* Item);
    void RemoveItem(UItem* Item);
    // 퀘스트 처리
    void AcceptQuest(UQuest* Quest);
    void CompleteQuest(UQuest* Quest);
    // 대화 처리
    void StartDialogue();
    void EndDialogue();
    // ... 계속 ...
};
```

좋은 예시
```cpp
// 언리얼 엔진의 컴포넌트를 사용해서 분리하는 방법
class AGameCharacter : public ACharacter
{
public:
    AGameCharacter();
    // 핵심 동작만 유지, 나머지는 분리
private:
    UPROPERTY()
    UMovementComponent* MovementComp;

    UPROPERTY()
    UCombatComponent* CombatComp;

    UPROPERTY()
    UInventoryComponent* InventoryComp;

    UPROPERTY()
    UQuestComponent* QuestComp;
    // ...
};
```

</div>
</details>

<details>
<summary><h5 style="display: inline;">22. 서로 다른 인터페이스의 대안 클래스들(Alternative Classes with Different Interfaces)</h5></summary>
<div markdown="1">

서로 다른 인터페이스의 대안 클래스들은 서로 비슷한 기능을 제공하는 클래스들이지만 서로 다른 방식으로 구현된 경우를 의미합니다.  
즉, 각 클래스가 제공하는 함수가 달라 클래스 타입에 따라 다른 함수를 호출해야하는 경우입니다.

클래스를 교체하려면 인터페이스가 호환되어야합니다.  
유사 기능 클래스끼리 일관된 형식을 갖추는 것이 좋습니다.

나쁜 예시
```cpp
// 같은 로직을 수행하지만 제공되는 인터페이스가 다른 경우
class ARangedWeapon
{
public:
    void FireProjectile();
    void Reload();
};

class AMeleeWeapon
{
public:
    void PerformAttack();
    void SharpenBlade();
};

// 플레이어 캐릭터
void APlayerCharacter::Attack()
{
    if (CurrentRangedWeapon)
        CurrentRangedWeapon->FireProjectile();
    else if (CurrentMeleeWeapon)
        CurrentMeleeWeapon->PerformAttack();
}
```

좋은 예시
```cpp
class AWeapon : public AActor
{
public:
    virtual void Attack() = 0;  // 추상 메서드
    virtual void Reload() {}    // 기본 구현(근접 무기는 비워둘 수도 있다.)
};

class ARangedWeapon : public AWeapon
{
public:
    virtual void Attack() override { /* 원거리 공격 */ }
    virtual void Reload() override { /* 탄약 보충 */ }
};

class AMeleeWeapon : public AWeapon
{
public:
    virtual void Attack() override { /* 근접 공격 */ }
    // Reload()는 기획에 따라 오버라이드
};

// 플레이어 캐릭터
void APlayerCharacter::Attack()
{
    if (CurrentWeapon)
    {
        CurrentWeapon->Attack(); // 무기 종류 관계없이 한 번에 호출
    }
}
```

</div>
</details>

<details>
<summary><h5 style="display: inline;">23. 데이터 클래스(Data Class)</h5></summary>
<div markdown="1">

데이터 클래스는 데이터 필드와 Get/Set함수로 이루어진 클래스를 의미합니다.  
데이터 필드와 Get/Set만 있는 클래스는 다른 곳에서 함부로 조작되기 쉽습니다.

데이터 클래스를 사용하는 클래스로 옮기는 방법이 있습니다.  
만약 다양한 곳에서 데이터 클래스를 사용하기 때문에 옮길 수 없다면, 변경될 필요가 없는 필드는 세터를 제거해 안정성을 높혀야합니다.

나쁜 예시
```cpp
class FPlayerStats
{
public:
    float GetHealth() const { return Health; }
    void SetHealth(float H) { Health = H; }
    // ...
private:
    float Health;
    float MaxHealth;
    // ...
};

// 플레이어가 Stats를 조작
void APlayerCharacter::TakeDamage(float Damage)
{
    float NewHealth = PlayerStats.GetHealth() - Damage;
    PlayerStats.SetHealth(FMath::Max(0.f, NewHealth));
    // 기타 작업 포함...
}
```

좋은 예시
```cpp
class FPlayerStats
{
public:
    // 함수 안에서 로직 처리
    void ApplyDamage(float Damage)
    {
        float ActualDamage = Damage * (1.0f - Defense / 100.f);
        Health = FMath::Max(0.f, Health - ActualDamage);
    }

    bool IsDead() const { return Health <= 0.f; }

    // ...

private:
    float Health;
    float Defense;
    // ...
};

// 플레이어
void APlayerCharacter::TakeDamage(float Damage)
{
    PlayerStats.ApplyDamage(Damage);
    if (PlayerStats.IsDead())
    {
        Die();
    }
}
```

</div>
</details>

<details>
<summary><h5 style="display: inline;">24. 상속 포기(Refused Bequest)</h5></summary>
<div markdown="1">

상속 포기는 서브클래스가 부모의 기능 중 일부만 필요하거나 인터페이스가 맞지 않은 경우를 의미합니다.  
이 경우 리스코프치환에 어긋나게 됩니다.

비어있는 구현이나 예외 처리가 늘어나게 됩니다.

컨포지션을 사용하는 방법이 있습니다.  
상속시 더 작은 인터페이스를 설정하는 방법 등으로 클래스를 분리하는 방법이 있습니다.

나쁜 예시
```cpp
class AWeapon
{
public:
    virtual void Attack();
    virtual void Reload(); // 근접 무기는 재장전 필요 X
};

class AMeleeWeapon : public AWeapon
{
public:
    virtual void Reload() override
    {
        // 근접 무기에선 의미가 없으니 비워두게됨
    }
};
```

좋은 예시
```cpp
class ABaseWeapon : public AActor
{
public:
    virtual void Attack() = 0; // 모든 무기는 공격 기능
};

class ARangedWeapon : public ABaseWeapon
{
public:
    virtual void Attack() override { /* 발사 로직 */ }
    void Reload() { /* 탄약 보충 */ }
};

class AMeleeWeapon : public ABaseWeapon
{
public:
    virtual void Attack() override { /* 근접 공격 로직 */ }
    // Reload()는 없음!
};
```

</div>
</details>

<details>
<summary><h5 style="display: inline;">25. 주석의 남용(Comments)</h5></summary>
<div markdown="1">

주석은 필요한 정보만 담고, 근본적인 해석을 위해 사용하는 것이 좋다.

코드만으로 명확하게 이해되는게 가장 좋습니다.

주석이 필요한 상황일 경우, 주석이 필요없는 코드로 바꾸는 것이 좋습니다.

나쁜 예시
```cpp
// 함수에 각 단계별 설명이 가득한 경우
void AEnemy::UpdateBehavior()
{
    // 1. 플레이어 위치 가져오기
    // 2. 시야 범위 확인
    // 3. 시야 각도 계산
    // 4. 라인 트레이스 해서 장애물 있는지
    // 5. 없으면 공격, 있으면 패트롤
    // ...
}
```

좋은 예시
```cpp
void AEnemy::UpdateBehavior()
{
    if (CanSeePlayer())
    {
        EngagePlayer();
    }
    else
    {
        PatrolArea();
    }
}

bool AEnemy::CanSeePlayer()
{
    return IsWithinSightRange() && IsInFieldOfView() && HasLineOfSight();
}
```

</div>
</details>