---
layout: single

title: "[Design Pattern] 빌더 패턴"

categories:
    - DesignPattern
tag: [디자인 패턴]

date: 2025-03-12
last_modified_at: 2025-03-12

order : 1050
---

# 빌더 패턴

빌더(Builder) 패턴은 복잡한 객체를 단계별(점진적으로) 생성할 때 사용되는 생성 패턴 중 하나입니다.

동일한 생성 코드로도 서로 다른 표현(또는 변형)의 객체를 만들 수 있도록 하는 생성(Creational) 디자인 패턴입니다.

빌더 패턴의 핵심 아이디어는 다음과 같습니다.

1. 단계별(점진적) 생성
    + 객체를 만들 때 필요한 여러 단계를 순차적으로 호출하여 완성해 나갈 수 있다.
    + 필요한 단계만 골라서 호출할 수 있다.
2. 동일한 생성 과정을 통해 여러 표현(Variations) 지원
    + 같은 빌더(생성 과정)를 사용해도, 조합하는 방식에 따라 서로 다른 객체를 만들 수 있다.
3. 복잡한 초기화 코드를 분리
    + 객체 자체는 비즈니스 로직에 집중하고, 생성 관련 복잡한 코드는 빌더에서 처리한다.
4. 디렉터(Director)
    + 객체를 어떻게 조합할지(생성 단계를 어느 순서로 호출)를 별도 클래스(디렉터)에 맡길 수 있다.
    + 같은 빌더라도, 디렉터가 호출하는 단계 순서만 바꿔도 결과물이 달라질 수 있다.
    + 클라이언트는 디렉터에게 생성 요청을 하면, 세부 생성 로직은 디렉터와 빌더가 알아서 처리하게된다.

+ 장점
    + 객체의 생성 과정과 표현 방법을 분리할 수 있어, 단일 책임 원칙(SRP)을 지킬 수 있습니다.
    + 새로운 속성을 추가하거나 수정할 때 영향이 적어 유지보수 용이하다.
    + 다양한 빌더를 만들어 여러 종류의 객체를 쉽게 생성 가능해 유연성이 증가한다.
    + 사용자는 복잡한 객체를 필요한 단계만 골라서 호출할 수 있고, 단계 순서를 유연하게 조절할 수 있습니다.
    + 디렉터에 생성 순서를 정의해 두면, 같은 빌더로도 다양한 객체를 만들 수 있고, 다른 빌더로 교체해서 또 다른 객체를 만들 수 있어 코드의 재사용성이 높습니다.

+ 단점
    + 여러 객체 표현이 있고, 각 객체에 맞는 구상 빌더가 많아지면 코드가 늘어날 수 있습니다.
    + 디렉터를 쓰지 않고 클라이언트가 직접 빌더를 호출해도 되지만, 이 경우 복잡한 생성 순서를 재사용하기 어렵습니다.
    + 단계가 늘어날수록, 빌더 인터페이스가 커질 수 있습니다.

## 예시

### 차량과 차량 설명서

차량(Car)과 차량 설명서(Manual)를 생성하는 예시입니다.

```cpp
// 빌더가 생성할 객체
class Car {
public:
    void SetEngine(const std::string& engineType) { engine = engineType; }
    void SetSeats(int count) { seats = count; }
    void SetTripComputer(bool hasTripComputer) { tripComputer = hasTripComputer; }
    void SetGPS(bool hasGPS) { gps = hasGPS; }

    void Show() const
    {
        std::cout 
            << "Car - Engine: " << engine 
            << ", Seats: " << seats
            << ", TripComputer: " << (tripComputer ? "Yes" : "No")
            << ", GPS: " << (gps ? "Yes" : "No") 
            << std::endl;
    }

private:
    std::string engine;
    int seats;
    bool tripComputer;
    bool gps;
};

// 빌더가 생성할 객체
class CarManual
{
public:
    void AddInstruction(const std::string& instruction)
    {
        instructions.push_back(instruction);
    }

    void Show() const
    {
        std::cout << "Car - Manual" << std::endl;

        for (const auto& instr : instructions)
        {
            std::cout << instr << std::endl;
        }
    }

private:
    std::vector<std::string> instructions;
};
```

```cpp
// 빌더 인터페이스
class IBuilder
{
public:
    virtual void Reset() = 0;
    virtual void SetSeats(int count) = 0;
    virtual void SetEngine(const std::string& engineType) = 0;
    virtual void SetTripComputer(bool hasTripComputer) = 0;
    virtual void SetGPS(bool hasGPS) = 0;
};
```

```cpp
// Car 객체를 만드는 빌더
class CarBuilder : public IBuilder
{
public:
    void Reset() override
    {
        car = std::make_unique<Car>();
    }

    void SetSeats(int count) override
    {
        car->SetSeats(count);
    }

    void SetEngine(const std::string& engineType) override
    {
        car->SetEngine(engineType);
    }

    void SetTripComputer(bool hasTripComputer) override
    {
        car->SetTripComputer(hasTripComputer);
    }

    void SetGPS(bool hasGPS) override
    {
        car->SetGPS(hasGPS);
    }

    std::unique_ptr<Car> GetResult()
    {
        return std::move(car);
    }

private:
    std::unique_ptr<Car> car;
};
```

```cpp
// CarManual 객체를 만드는 빌더
class CarManualBuilder : public IBuilder
{
public:
    void Reset() override
    {
        manual = std::make_unique<CarManual>();
    }

    void SetSeats(int count) override
    {
        manual->AddInstruction("Seats: " + std::to_string(count) + " 좌석을 사용해 주세요.");
    }

    void SetEngine(const std::string& engineType) override
    {
        manual->AddInstruction("Engine: '" + engineType + "' 시동 방법과 유지보수 사항을 확인하세요.");
    }

    void SetTripComputer(bool hasTripComputer) override
    {
        manual->AddInstruction(hasTripComputer ? "TripComputer: 주행 데이터 확인 방법은 5페이지를 참조하세요." : "TripComputer: (없음)");
    }

    void SetGPS(bool hasGPS) override
    {
        manual->AddInstruction(hasGPS ? "GPS: 지도 업데이트 방법은 10페이지를 참조하세요." : "GPS: (없음)");
    }

    std::unique_ptr<CarManual> GetResult()
    {
        return std::move(manual);
    }

private:
    std::unique_ptr<CarManual> manual;
};
```

```cpp
// 디렉터
class Director
{
public:
    void ConstructSportsCar(IBuilder& builder)
    {
        builder.Reset();
        builder.SetSeats(2);
        builder.SetEngine("SportEngine V8");
        builder.SetTripComputer(true);
        builder.SetGPS(true);
    }

    void ConstructSUV(IBuilder& builder)
    {
        builder.Reset();
        builder.SetSeats(5);
        builder.SetEngine("StandardEngine V6");
        builder.SetTripComputer(true);
        builder.SetGPS(false);
    }
};
```

```cpp
// 사용 예시
int main()
{
    Director director;

    // 자동차 생성
    CarBuilder carBuilder;
    director.ConstructSportsCar(carBuilder);
    std::unique_ptr<Car> sportsCar = carBuilder.GetResult();
    sportsCar->Show();

    director.ConstructSUV(carBuilder);
    std::unique_ptr<Car> suvCar = carBuilder.GetResult();
    suvCar->Show();

    std::cout << std::endl;

    // 자동차 매뉴얼 생성
    CarManualBuilder manualBuilder;
    director.ConstructSportsCar(manualBuilder);
    std::unique_ptr<CarManual> sportsCarManual = manualBuilder.GetResult();
    sportsCarManual->Show();

    std::cout << std::endl;

    director.ConstructSUV(manualBuilder);
    std::unique_ptr<CarManual> suvCarManual = manualBuilder.GetResult();
    suvCarManual->Show();
}
```

출력은 다음과 같습니다.

```
Car - Engine: SportEngine V8, Seats: 2, TripComputer: Yes, GPS: Yes
Car - Engine: StandardEngine V6, Seats: 5, TripComputer: Yes, GPS: No

Car - Manual
Seats: 2 좌석을 사용해 주세요.
Engine: 'SportEngine V8' 시동 방법과 유지보수 사항을 확인하세요.
TripComputer: 주행 데이터 확인 방법은 5페이지를 참조하세요.
GPS: 지도 업데이트 방법은 10페이지를 참조하세요.

Car - Manual
Seats: 5 좌석을 사용해 주세요.
Engine: 'StandardEngine V6' 시동 방법과 유지보수 사항을 확인하세요.
TripComputer: 주행 데이터 확인 방법은 5페이지를 참조하세요.
GPS: (없음)
```

### 게임 캐릭터

예를 들어, RPG 게임에서 플레이어 캐릭터(또는 NPC)를 생성할 때, 종족, 직업, 능력치, 장비 등을 단계별로 설정해야 할 수 있습니다.

1. 종족(엘프, 오크 등)
2. 직업(전사, 마법사, 도적 등)
3. 초기 능력치(힘, 지능, 민첩, 체력 등)
4. 초기 장비(무기, 방어구, 아이템 등)
5. 선택적 옵션(펫, 스킨, 스페셜 스킬 등)

어떤 캐릭터는 펫이 없을 수도 있고, 어떤 캐릭터는 특정 스킬을 가질 수 있다.

```cpp
// 빌더가 생성할 캐릭터 객체
class GameCharacter
{
public:
    std::string race;
    std::string job;
    int strength;
    int intelligence;
    int agility;
    int hp;
    std::string weapon;
    std::vector<std::string> items;
    bool hasPet;

    void Show() const
    {
        std::cout 
            << "[Character] Race=" << race 
            << ", Job=" << job
            << ", STR=" << strength 
            << ", INT=" << intelligence
            << ", AGI=" << agility 
            << ", HP=" << hp
            << ", Weapon=" << weapon 
            << ", Items=";
        for (const auto& item : items) std::cout << item << ", ";
        std::cout << "Pet=" << (hasPet ? "Yes" : "No") << std::endl;
    }
};
```

```cpp
// 빌더 인터페이스
class ICharacterBuilder
{
public:
    virtual void Reset() = 0;
    virtual void SetRace(const std::string& race) = 0;
    virtual void SetJob(const std::string& job) = 0;
    virtual void SetStats(int str, int intl, int agi, int hp) = 0;
    virtual void SetWeapon(const std::string& weapon) = 0;
    virtual void AddItem(const std::string& item) = 0;
    virtual void SetPet(bool hasPet) = 0;
    virtual std::unique_ptr<GameCharacter> GetResult() = 0;
};
```

```cpp
// 캐릭터 객체를 만드는 빌더
class CharacterBuilder : public ICharacterBuilder
{
public:
    CharacterBuilder() { Reset(); }

    void Reset() override
    {
        character = std::make_unique<GameCharacter>();
    }

    void SetRace(const std::string& race) override
    {
        character->race = race;
    }

    void SetJob(const std::string& job) override
    {
        character->job = job;
    }

    void SetStats(int str, int intl, int agi, int hp) override
    {
        character->strength = str;
        character->intelligence = intl;
        character->agility = agi;
        character->hp = hp;
    }

    void SetWeapon(const std::string& weapon) override
    {
        character->weapon = weapon;
    }

    void AddItem(const std::string& item) override
    {
        character->items.push_back(item);
    }

    void SetPet(bool hasPet) override
    {
        character->hasPet = hasPet;
    }

    std::unique_ptr<GameCharacter> GetResult() override
    {
        return std::move(character);
    }

private:
    std::unique_ptr<GameCharacter> character;
};
```

```cpp
// 디렉터
class CharacterDirector
{
public:
    void CreateBasicWarrior(ICharacterBuilder& builder)
    {
        builder.Reset();
        builder.SetRace("Human");
        builder.SetJob("Warrior");
        builder.SetStats(10, 3, 5, 100);
        builder.SetWeapon("Sword");
    }

    void CreateElfArcher(ICharacterBuilder& builder)
    {
        builder.Reset();
        builder.SetRace("Elf");
        builder.SetJob("Archer");
        builder.SetStats(5, 5, 10, 80);
        builder.SetWeapon("Bow");
        builder.AddItem("Arrow x100");
    }

    void CreateMagicianWithPet(ICharacterBuilder& builder)
    {
        builder.Reset();
        builder.SetRace("Human");
        builder.SetJob("Magician");
        builder.SetStats(3, 10, 4, 80);
        builder.SetWeapon("Staff");
        builder.SetPet(true);
    }
};
```

```cpp
// 사용 예시
int main()
{
    CharacterDirector director;
    CharacterBuilder builder;

    // 1) 전사 캐릭터 생성
    director.CreateBasicWarrior(builder);
    auto warrior = builder.GetResult();
    warrior->Show();

    // 2) 엘프 아처 생성
    director.CreateElfArcher(builder);
    auto elfArcher = builder.GetResult();
    elfArcher->Show();

    // 3) 마법사 + 펫 생성
    director.CreateMagicianWithPet(builder);
    auto mage = builder.GetResult();
    mage->Show();

    // 직접 빌더 사용
    builder.Reset();
    builder.SetRace("Orc");
    builder.SetJob("Shaman");
    builder.SetStats(6, 9, 5, 90);
    builder.SetWeapon("Sword");
    builder.SetPet(true);
    builder.AddItem("Healing Potion");
    auto orcShaman = builder.GetResult();
    orcShaman->Show();
}
```

출력은 다음과 같습니다.

```
[Character] Race=Human, Job=Warrior, STR=10, INT=3, AGI=5, HP=100, Weapon=Sword, Items=Pet=No
[Character] Race=Elf, Job=Archer, STR=5, INT=5, AGI=10, HP=80, Weapon=Bow, Items=Arrow x100, Pet=No
[Character] Race=Human, Job=Magician, STR=3, INT=10, AGI=4, HP=80, Weapon=Staff, Items=Pet=Yes
[Character] Race=Orc, Job=Shaman, STR=6, INT=9, AGI=5, HP=90, Weapon=Sword, Items=Healing Potion, Pet=Yes
```