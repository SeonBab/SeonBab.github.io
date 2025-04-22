---
layout: single

title: "[Design Pattern] 상태 패턴"

categories:
    - DesignPattern
tag: [디자인 패턴]

date: 2025-03-10
last_modified_at: 2025-03-10

order : 1020
---

# 상태 패턴

상태(State) 패턴은 객체 내부 상태에 따라 행동(로직)이 달라지는 문제를 해결하기 위한 패턴입니다.  
즉, 객체가 어떤 상태에 있느냐에 따라 같은 메서드 호출도 서로 다른 로직을 수행해야 할 때, 상태를 별도의 클래스로 분리하여 유연하고 깔끔한 코드를 작성할 수 있도록 해줍니다.

예를 들어, 자판기의 경우 동전이 있는지, 상품이 매진되었는지에 따라 자판기의 동작이 달라집니다.  
즉, 현재 상태가 달라지면 같은 메서드 호출도 서로 다른 로직을 수행하게 되는 상황입니다.  
이를 보통 `if-else`나 `switch`문으로 처리하기 쉽지만, 코드가 복잡해지고 유지보수성이 떨어지는 문제가 생깁니다.

게임에서는 일시 정지, 플레이어 캐릭터의 걷기/뛰기 등 다양한 상태 변화를 관리하는 데 사용됩니다.

상태 패턴은 세 가지 주요 구성 요소로 이루어집니다.

1. 상태 인터페이스(State Interface): 상태별 동작을 정의하는 인터페이스로 각 상태에서 수행할 메서드들을 선언합니다.
2. 구체 상태 클래스(Concrete State): 상태 인터페이스를 구현하는 클래스이며, 필요에 따라 다른 상태로 전환하는 로직을 포함합니다.  
3. 컨텍스트 클래스(Context): 현재 상태를 참조해 상태 변경을 처리해주며, 상태에 따라 달라지는 동작을 위임합니다.

전력 패턴과 비슷하지만, 전략 패턴은 런타임에 교체할 수 있는 알고리즘에 집중하고, 상태 패턴은 객체 내부 상태에 따라 행동이 바뀌고, 상태가 서로 전환된다는 점에 집중한다는 점이 다릅니다.  
상태 패턴에서는 행동 수행 후 상태가 스스로 바뀌는 로직이 핵심이고, 전략 패턴에서는 외부에서 전략을 갈아끼우는 개념이 주가 됩니다.

상태 패턴은 상태를 별도의 클래스로 캡슐화하고, 상태 전환 로직도 그 클래스 안에서 처리하게 하여, 코드를 더 깔끔하고 확장성 있게 만들어줍니다.

+ 장점
    + 조건문을 사용하지 않고도 상태 전환을 처리할 수 있어, 상태별 클래스로 캡슐화가 가능하고 코드 가독성이 향상됩니다.
    + 새로운 상태를 추가할 때 기존 코드를 거의 수정하지 않고 새로운 상태 클래스를 추가하여 확장이 가능합니다.
    + 각 상태 클래스가 자신이 처리해야 할 로직만 담당하므로, 응집도가 높아지고 유지보수성이 향상됩니다.

+ 단점
    + 상태가 많아질수록 별도의 클래스 파일 수가 증가할 수 있습니다.
    + 상태간 전환 로직이 분산되어 있으므로, 전체 흐름을 파악하고 체계적으로 관리해야 합니다.

## 예시

```cpp
// 상태 인터페이스
class IState
{
public:
    virtual void InsertCoin() = 0;
    virtual void EjectCoin() = 0;
    virtual void SelectItem() = 0;
    virtual void Dispense() = 0;
};
```

각 상태에서 수행해야 할 주요 메서드를 정의했습니다.

자판기는 이 `IState`만 알고, 실제 로직은 구체 상태 클래스가 담당합니다.

```cpp
// 구체 상태 클래스

class VendingMachine; // 전방 선언

// 동전이 없는 상태
class NoCoinState : public IState
{
public:
    NoCoinState(VendingMachine* vendingMachine)
    {
        this->vendingMachine = vendingMachine;
    }

    void InsertCoin() override
    {
        std::cout << "동전을 넣으셨습니다." << std::endl;
        vendingMachine->SetState(vendingMachine->GetHasCoinState());
    }
    void EjectCoin() override
    {
        std::cout << "반환할 동전이 없습니다." << std::endl;
    }
    void SelectItem() override
    {
        std::cout << "동전을 넣어야 상품을 선택할 수 있습니다." << std::endl;
    }
    void Dispense() override
    {
        std::cout << "동전이 없으므로 상품을 배출할 수 없습니다." << std::endl;
    }

private:
    VendingMachine* vendingMachine;
};

// 동전이 있는 상태
class HasCoinState : public IState
{
public:
    HasCoinState(VendingMachine* vendingMachine)
    {
        this->vendingMachine = vendingMachine;
    }

    void InsertCoin() override
    {
        std::cout << "이미 동전이 있습니다." << std::endl;
    }
    void EjectCoin() override
    {
        std::cout << "동전을 반환합니다." << std::endl;
        vendingMachine->SetState(vendingMachine->GetNoCoinState());
    }
    void SelectItem() override
    {
        std::cout << "동전을 넣어야 상품을 선택할 수 있습니다." << std::endl;

        // 재고가 있는지 확인
        if (vendingMachine->GetItemCount() > 0)
        {
            vendingMachine->SetState(vendingMachine->GetDispensingState());
        }
        else
        {
            vendingMachine->SetState(vendingMachine->GetSoldOutState());
        }
    }
    void Dispense() override
    {
        std::cout << "아직 상품을 선택하지 않았습니다." << std::endl;
    }

private:
    VendingMachine* vendingMachine;
};

// 상품 배출 중인 상태
class DispensingState : public IState
{
public:
    DispensingState(VendingMachine* vendingMachine)
    {
        this->vendingMachine = vendingMachine;
    }

    void InsertCoin() override
    {
        std::cout << "상품을 배출 중입니다. 잠시만 기다려주세요." << std::endl;
    }
    void EjectCoin() override
    {
        std::cout << "이미 상품을 배출 중이라 동전을 반환할 수 없습니다." << std::endl;
    }
    void SelectItem() override
    {
        std::cout << "이미 상품이 선택되었습니다." << std::endl;
    }
    void Dispense() override
    {
        std::cout << "상품을 배출합니다!" << std::endl;
        vendingMachine->ReleaseItem();

        if (vendingMachine->GetItemCount() > 0)
        {
            vendingMachine->SetState(vendingMachine->GetNoCoinState());
        }
        else
        {
            std::cout << "상품이 모두 소진되었습니다." << std::endl;
            vendingMachine->SetState(vendingMachine->GetSoldOutState());
        }
    }

private:
    VendingMachine* vendingMachine;
};

// 상품 매진 상태
class SoldOutState : public IState
{
public:
    SoldOutState(VendingMachine* vendingMachine)
    {
        this->vendingMachine = vendingMachine;
    }

    void InsertCoin() override
    {
        std::cout << "상품이 소진되어 동전을 넣을 수 없습니다." << std::endl;
    }
    void EjectCoin() override
    {
        std::cout << "반환할 동전이 없습니다." << std::endl;
    }
    void SelectItem() override
    {
        std::cout << "상품이 소진되었습니다." << std::endl;
    }
    void Dispense() override
    {
        std::cout << "배출할 상품이 없습니다." << std::endl;
    }

private:
    VendingMachine* vendingMachine;
};
```

각 상태 클래스는 `IState` 인터페이스를 구현합니다.

메서드 호출 시, 어떤 로직을 수행하고 상태를 어떻게 전환할지를 정의합니다.

```cpp
// 컨텍스트 클래스
class VendingMachine
{
public:
    VendingMachine(int newitemCount)
    {
        noCoinState = new NoCoinState(this);
        hasCoinState = new HasCoinState(this);
        dispensingState = new DispensingState(this);
        soldOutState = new SoldOutState(this);

        itemCount = newitemCount;

        // 현재 상태 초기화
        currentState = (itemCount > 0) ? noCoinState : soldOutState;
    }

    ~VendingMachine()
    {
        delete noCoinState;
        delete hasCoinState;
        delete dispensingState;
        delete soldOutState;
    }

    IState* GetNoCoinState() { return noCoinState; }
    IState* GetHasCoinState() { return hasCoinState; }
    IState* GetDispensingState() { return dispensingState; }
    IState* GetSoldOutState() { return soldOutState; }

    void SetState(IState* newState)
    {
        currentState = newState;
    }

    int GetItemCount() { return itemCount; }

    void ReleaseItem()
    {
        if (itemCount > 0)
        {
            std::cout << "아이템이 굴러 나옵니다!" << std::endl;
            --itemCount;
        }
    }

    // 자판기에서 제공하는 동작 메서드들
    void InsertCoin() { currentState->InsertCoin();  }
    void EjectCoin() {  currentState->EjectCoin(); }
    void SelectItem() { currentState->SelectItem();  }
    void Dispense() { currentState->Dispense(); }

private:
    // 상태 객체 미리 생성
    IState* noCoinState;
    IState* hasCoinState;
    IState* dispensingState;
    IState* soldOutState;

    // 현재 상태
    IState* currentState;

    // 재고 (알맹이 수)
    int itemCount;
};
```

자판기 클래스는 현재 상태(`currentState`)를 가지고 있습니다.

요청이 들어오면(`InsertCoin`, `EjectCoin` 등) 자판기는 현재 상태 객체의 해당 메서드를 호출합니다.

상태 전환은 `SetState`함수를 통해 일어나며, 상태 객체에서 상황에 따라 적절한 상태로 전환을 지시합니다.

```cpp
// 실제 사용
int main()
{
    VendingMachine* machine = new VendingMachine(2); // 상품 재고 2개, NoCoinState

    // 1) 동전 투입 -> 상품 선택 -> 상품 배출
    machine->InsertCoin(); // 상품 재고 2개, HasCoinState
    machine->SelectItem(); // 상품 재고 2개, DispensingState
    machine->Dispense();   // 상품 재고 1개, NoCoinState

    std::cout << std::endl;

    // 2) 동전 투입 -> 동전 반환
    machine->InsertCoin(); // 상품 재고 1개, HasCoinState
    machine->EjectCoin();  // 상품 재고 1개, NoCoinState

    std::cout << std::endl;

    // 3) 동전 투입 -> 상품 선택 -> 상품 배출 (마지막 재고)
    machine->InsertCoin(); // 상품 재고 1개, HasCoinState
    machine->SelectItem(); // 상품 재고 1개, DispensingState
    machine->Dispense();   // 상품 재고 0개, SoldOutState

    std::cout << std::endl;

    // 4) 재고가 없으므로 매진 상태
    machine->InsertCoin(); // 상품 재고 0개, SoldOutState
    machine->SelectItem(); // 상품 재고 0개, SoldOutState
    machine->Dispense();   // 상품 재고 0개, SoldOutState

    delete machine;
}
```

실행하면 각 상태에서 정의된 로직대로 메시지가 출력되고, 재고가 0이 되면 `SoldOutState`로 전환됩니다.

실제 상태 변화 시퀀스:

- 자판기 생성 시, 재고가 2개이면 `NoCoinState`에서 시작합니다.
- 첫 번째 사용:
    - `InsertCoin()` → `NoCoinState`에서 `HasCoinState`로 전환
    - `SelectItem()` → `HasCoinState`에서 `DispensingState`로 전환
    - `Dispense()` → `DispensingState` 처리 후, 재고가 남으므로 `NoCoinState`로 전환
- 두 번째 사용 (동전 투입 후 동전 반환):
    - `InsertCoin()` → `NoCoinState`에서 `HasCoinState`로 전환
    - `EjectCoin()` → `HasCoinState`에서 동전 반환 후 `NoCoinState`로 전환
- 세 번째 사용:
    - 동전 투입 후 상품 선택, 배출 → 재고가 0이 되어 `SoldOutState`로 전환
- 네 번째 사용:
    - 재고가 없으므로 동전 투입 시 “상품이 소진되어…” 등의 메시지가 출력됩니다.