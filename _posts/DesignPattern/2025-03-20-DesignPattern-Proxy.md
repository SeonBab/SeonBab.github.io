---
layout: single

title: "[Design Pattern] Proxy Pattern(프록시 패턴)"

categories:
    - DesignPattern
tag: [디자인 패턴]

date: 2025-03-20
last_modified_at: 2025-03-20

order : 1080
---

# Proxy Pattern

Proxy Pattern(프록시 패턴)은 어떤 객체(RealSubject)에 대한 접근을 제어하거나, 실제 객체의 대리(Proxy) 역할을 수행하는 구조(Structural) 디자인 패턴입니다.  
대리인(Proxy)이라는 뜻처럼, 원본(RealSubject)을 대신하여 동작, 접근 제어, 로깅, 캐싱 등의 역할을 수행할 수 있습니다.

클라이언트는 프록시를 원본 객체처럼 사용하지만, 직접 접근하지 않고 프록시를 통해 접근합니다.  
실제 호출은 내부적으로 원본 객체에게 위임하거나 프록시 자체가 추가 로직을 진행합니다.

프록시(Proxy)는 원본 객체를 감싸고 요청을 가로채거나(Intercept) 위임(Delegate)하거나, 추가 로직을 실행할 수 있습니다.  
프록시는 동일한 인터페이스(Interface)를 구현하여, 클라이언트가 원본 객체와 동일한 방식으로 사용할 수 있도록 보장합니다.

+ 구성 요소
    - Subject (추상 인터페이스)
    - RealSubject와 Proxy가 구현해야 하는 공통 인터페이스
    - RealSubject (실제 객체)
    - 실제 동작을 수행하는 객체
    - Proxy (대리 객체)
    - RealSubject에 대한 접근을 제어하는 객체

+ 프록시 패턴의 주요 목적
    - 객체의 생성 및 접근을 제어 (Lazy Initialization)
    - 비용이 큰 객체를 미리 로드하지 않고 필요할 때만 생성 (Virtual Proxy)
    - 원격 객체를 로컬에서 제어 (Remote Proxy)
    - 접근 권한을 제한 (Protection Proxy)
    - 로깅 및 캐싱 기능을 추가 (Smart Proxy)

+ 장점
    - 객체 생성을 지연시켜 불필요한 리소스 사용을 줄여 성능 최적화 가능 (Lazy Initialization)
    - 접근 권한을 제어하여 보안 강화 (Protection Proxy)
    - 객체의 추가 기능을 손쉽게 확장 가능 (예: 로깅, 캐싱, 로드 밸런싱)
    - 네트워크 호출을 추상화하여 원격 객체와의 인터페이스 제공 (Remote Proxy)

+ 단점
    - 프록시 호출이 늘어날수록, 실제 객체 호출보다 오버헤드가 추가되어 객체 접근이 느려질 수 있음
    - 클래스 수가 늘어나고, 구현이 복잡해질 수 있음
    - 객체 간의 의존성이 증가할 가능성 있음
    - 중간 단계(프록시)가 많아지면, 문제 발생 시 콜스택 추적이 복잡해짐

+ 프록시의 종류
    - 원격(Remote) 프록시: 원격 서버에 있는 객체에 접근하며, 로컬에서 접근하는 것처럼 보이게 합니다.
    - 가상(Virtual) 프록시: 무거운 객체의 지연 로딩 또는 캐싱을 담당하며, 실제 객체가 필요할 때만 생성합니다.
    - 보호(Protection) 프록시: 권한 제어, 역할(Role)에 따른 접근 제한 등을 처리합니다.
    - 캐싱(Caching) 프록시: 결과를 캐싱하여 반복적인 요청에 대해 성능 최적화 수행합니다.
    - 스마트(Smart) 프록시: 참조가 일어날 때마다 추가 동작 로깅, 캐싱, 레퍼런스 카운팅, 메모리 관리 등의 추가 기능을 제공합니다.

어댑터 패턴은 다른 인터페이스를, 프록시 패턴은 같은 인터페이스를 랩핑하여 객체를 사용하는 방식이라고 할 수 있습니다.

## 예시

<details>
<summary><h5 style="display: inline;">1. 접근 제어/권한 관리</h5></summary>
<div markdown="1">

특정 객체(예: DB 연결, 네트워크 자원 등)에 직접 접근이 위험하거나, 권한 관리가 필요할 수 있습니다.

예를 들어, 관리자 권한이 필요한 기능을, 모든 클라이언트가 직접 호출하기에 앞서, 프록시에서 인증이나 권한 확인 로직을 수행하도록 만들 수 있습니다.

```cpp
#include <iostream>
#include <memory>
#include <string>

// 인터페이스 (Abstract Class)
class IAdminService
{
public:
    virtual void PerformSensitiveOperation() = 0;
    virtual ~IAdminService() = default;
};

// 실제 서비스 (RealSubject)
class AdminService : public IAdminService
{
public:
    void PerformSensitiveOperation() override
    {
        std::cout << "중요한 작업을 수행합니다." << std::endl;
    }
};

// 프록시 (Proxy) - 권한 체크 추가
class AdminServiceProxy : public IAdminService
{
private:
    std::unique_ptr<IAdminService> realService;
    std::string userRole;

public:
    AdminServiceProxy(const std::string& role) : userRole(role)
    {
        realService = std::make_unique<AdminService>();
    }

    void PerformSensitiveOperation() override
    {
        if (userRole != "Admin")
        {
            std::cout << "권한이 없습니다!" << std::endl;
            return;
        }
        realService->PerformSensitiveOperation();
    }
};

// 사용 예제
int main()
{
    std::unique_ptr<IAdminService> proxy1 = std::make_unique<AdminServiceProxy>("Admin");
    proxy1->PerformSensitiveOperation(); // 성공

    std::unique_ptr<IAdminService> proxy2 = std::make_unique<AdminServiceProxy>("User");
    proxy2->PerformSensitiveOperation(); // 실패
}
```

</div>
</details>

<details>
<summary><h5 style="display: inline;">1.1 게임에서의 접근 제어/권한 관리</h5></summary>
<div markdown="1">

특정한 NPC(상점, 던전 입구) 등은 플레이어의 레벨, 아이템 보유 여부, 퀘스트 진행도에 따라 접근이 제한될 수 있습니다.

프록시 패턴을 사용하면 접근 가능한지 여부를 미리 체크하고, 허용된 경우에만 원본 객체(NPC)와 상호작용할 수 있습니다.

```cpp
#include <iostream>
#include <memory>

// NPC 상점(RealSubject)
class IShop
{
public:
    virtual void Enter() = 0;
    virtual ~IShop() = default;
};

// 실제 상점 구현
class RealShop : public IShop
{
public:
    void Enter() override
    {
        std::cout << "상점에 입장했습니다! 아이템을 구매할 수 있습니다." << std::endl;
    }
};

// 프록시 (플레이어 레벨 체크)
class ShopProxy : public IShop
{
private:
    std::unique_ptr<RealShop> realShop;
    int playerLevel;
    int requiredLevel;

public:
    ShopProxy(int playerLevel, int requiredLevel)
        : playerLevel(playerLevel), requiredLevel(requiredLevel)
    {
    }

    void Enter() override
    {
        if (playerLevel < requiredLevel)
        {
            std::cout << "레벨 " << requiredLevel << " 이상만 입장 가능! (현재 레벨: " << playerLevel << ")" << std::endl;
            return;
        }

        if (!realShop)
        {
            realShop = std::make_unique<RealShop>();
        }

        realShop->Enter();
    }
};

// 사용 예시
int main()
{
    std::cout << "[플레이어 레벨: 5]" << std::endl;
    ShopProxy shop(5, 10);
    shop.Enter(); // 입장 불가

    std::cout << std::endl;

    std::cout << "[플레이어 레벨: 15]" << std::endl;
    ShopProxy highLevelShop(15, 10);
    highLevelShop.Enter(); // 정상 입장
}
```

</div>
</details>

<details>
<summary><h5 style="display: inline;">2. 지연로딩/비용 절감</h5></summary>
<div markdown="1">

어떤 객체(예: 대용량 이미지, 네트워크 연결, DB 쿼리)의 생성 비용이 매우 클 때, 필요한 시점까지 실제 객체를 만들지 않고, 프록시로 대체해 둘 수 있습니다.

예를 들어, 가상 프록시(Virtual Proxy)를 사용하면, 클라이언트가 실제로 접근하기 전까지 원본 객체를 생성하지 않음으로써 초기 비용을 줄일 수 있습니다.

```cpp
#include <iostream>
#include <memory>
#include <string>

// 인터페이스 (Abstract Class)
class IImage
{
public:
    virtual void Display() = 0;
    virtual ~IImage() = default;
};

// 실제 이미지 객체 (RealSubject)
class HighResolutionImage : public IImage
{
private:
    std::string fileName;

    void LoadFromDisk()
    {
        std::cout << "이미지 " << fileName << "를 디스크에서 로딩 중..." << std::endl;
    }

public:
    HighResolutionImage(const std::string& file) : fileName(file)
    {
        LoadFromDisk();
    }

    void Display() override
    {
        std::cout << "이미지 " << fileName << " 표시 중" << std::endl;
    }
};

// 프록시 (Proxy) - 실제 객체를 필요할 때만 생성
class ImageProxy : public IImage
{
private:
    std::unique_ptr<HighResolutionImage> realImage;
    std::string fileName;

public:
    ImageProxy(const std::string& file) : fileName(file) {}

    void Display() override
    {
        if (!realImage)
        {
            realImage = std::make_unique<HighResolutionImage>(fileName);
        }
        realImage->Display();
    }
};

// 사용 예제
int main()
{
    std::unique_ptr<IImage> image = std::make_unique<ImageProxy>("photo.jpg");
    std::cout << "이미지를 아직 로드하지 않음" << std::endl;

    image->Display(); // 여기서야 비로소 이미지가 로드됨
    image->Display(); // 두 번째 호출에서는 로딩 없이 바로 표시
}
```

</div>
</details>

<details>
<summary><h5 style="display: inline;">2.1 게임에서의 지연로딩/비용 절감</h5></summary>
<div markdown="1">

오픈월드 같은 게임에서는 맵의 모든 오브젝트를 한 번에 로딩하면 성능에 문제가 생길 수 있습니다.

프록시를 사용하면 플레이어가 특정 위치에 도달할 때만 NPC, 아이템, 환경을 로딩할 수 있습니다.

```cpp
#include <iostream>
#include <memory>
#include <string>

// NPC 인터페이스
class INPC
{
public:
    virtual ~INPC() = default;
    virtual void Interact() = 0;
};

// 실제 NPC (무거운 로딩 필요)
class RealNPC : public INPC
{
private:
    std::string name;

    void LoadNPC()
    {
        std::cout << "NPC " << name << " 로딩 중... (대화 가능)" << std::endl;
    }

public:
    RealNPC(const std::string& npcName) : name(npcName)
    {
        LoadNPC();
    }

    void Interact() override
    {
        std::cout << "NPC " << name << ": '안녕하세요, 여행자!'" << std::endl;
    }
};

// NPC 프록시 (필요할 때만 로딩)
class NPCProxy : public INPC
{
private:
    std::unique_ptr<RealNPC> realNPC;
    std::string name;
    bool isPlayerNearby = false;

public:
    NPCProxy(const std::string& npcName) : name(npcName) {}

    void UpdatePlayerProximity(bool isNearby)
    {
        isPlayerNearby = isNearby;
    }

    void Interact() override
    {
        if (!isPlayerNearby)
        {
            std::cout << "NPC " << name << "은 너무 멀리 있습니다." << std::endl;
            return;
        }
        if (!realNPC)
        {
            realNPC = std::make_unique<RealNPC>(name);
        }
        realNPC->Interact();
    }
};

// 사용 예시
int main()
{
    NPCProxy npc("마을 장로");

    std::cout << "플레이어가 멀리 있음" << std::endl;
    npc.Interact(); // 접근 불가

    std::cout << std::endl;

    std::cout << "플레이어가 가까이 감" << std::endl;
    npc.UpdatePlayerProximity(true);
    npc.Interact(); // NPC 로딩 후 대화 가능
}
```

</div>
</details>

<details>
<summary><h5 style="display: inline;">3. 로깅/트래킹/모니터링</h5></summary>
<div markdown="1">

메서드 호출이 일어날 때마다 프록시가 중간에 개입하여, 로깅이나 사용 통계를 남길 수 있습니다.

예를 들어, 네트워크 호출 전/후에 로그를 찍거나, 요청 카운트를 세는 등 부수적인 기능을 쉽게 추가할 수 있습니다.

```cpp
#include <iostream>

class ISubject
{
public:
    virtual void Request() = 0;
    virtual ~ISubject() {}
};

class RealSubject : public ISubject
{
public:
    void Request() override
    {
        std::cout << "RealSubject::Request() 호출됨" << std::endl;
    }
};

class LoggingProxy : public ISubject
{
private:
    ISubject* realSubject;

public:
    LoggingProxy(ISubject* subject) : realSubject(subject) {}

    void Request() override
    {
        // 1) 접근 제어, 로깅, 권한 확인 등 부가 로직
        std::cout << "[LOG] 메서드 호출: Request()" << std::endl;
        // 2) 실제 객체에게 요청을 위임
        realSubject->Request();
        // 3) 접근 후 후처리 로직 (예: 캐싱, 통계 등)
        std::cout << "[LOG] 메서드 호출 완료." << std::endl;
    }
};

int main()
{
    RealSubject realSubject;
    LoggingProxy proxy(&realSubject);

    proxy.Request();
}
```

</div>
</details>

<details>
<summary><h5 style="display: inline;">3.1 게임에서의 로깅/트래킹/모니터링</h5></summary>
<div markdown="1">

게임의 중요한 액션(예: 보스전 시작, 아이템 구매, 퀘스트 완료)이 일어날 때, 로그를 자동으로 기록하거나 분석 시스템에 전송할 수 있습니다.

```cpp
#include <iostream>
#include <memory>
#include <string>

// 플레이어 액션 인터페이스
class IGameAction
{
public:
    virtual void Execute() = 0;
    virtual ~IGameAction() = default;
};

// 실제 퀘스트 완료 처리 (RealSubject)
class CompleteQuest : public IGameAction
{
private:
    std::string questName;

public:
    CompleteQuest(const std::string& name) : questName(name) {}

    void Execute() override
    {
        std::cout << "퀘스트 '" << questName << "' 완료!" << std::endl;
    }
};

// 실제 아이템 구매 처리 (RealSubject)
class PurchaseItem : public IGameAction
{
private:
    std::string itemName;
    int price;

public:
    PurchaseItem(const std::string& name, int price) : itemName(name), price(price) {}

    void Execute() override
    {
        std::cout << itemName << "'을(를) " << price << " 골드에 구매했습니다." << std::endl;
    }
};

// 로깅 프록시 (Logging Proxy)
class LoggingProxy : public IGameAction
{
private:
    std::unique_ptr<IGameAction> realAction;
    std::string actionType;

public:
    LoggingProxy(std::unique_ptr<IGameAction> action, const std::string& type)
        : realAction(std::move(action)), actionType(type)
    {
    }

    void Execute() override
    {
        std::cout << "[LOG] " << actionType << " 시작..." << std::endl;
        realAction->Execute();
        std::cout << "[LOG] " << actionType << " 완료." << std::endl;
    }
};

// 사용 예시
int main()
{
    std::cout << "게임 시작!" << std::endl;

    std::cout << std::endl;

    // 퀘스트 완료 (프록시를 사용하여 자동으로 로그 기록)
    auto quest = std::make_unique<LoggingProxy>(std::make_unique<CompleteQuest>("고대 유적 탐험"), "퀘스트 완료");
    quest->Execute();

    std::cout << std::endl;

    // 아이템 구매 (프록시를 사용하여 자동으로 로그 기록)
    auto purchase = std::make_unique<LoggingProxy>(std::make_unique<PurchaseItem>("드래곤 검", 5000), "아이템 구매");
    purchase->Execute();
}
```

</div>
</details>

<details>
<summary><h5 style="display: inline;">4. 네트워크/원격 호출</h5></summary>
<div markdown="1">

원격 객체(서버)에 실제로 연결하고 싶을 때, Proxy가 마치 로컬 객체인 것처럼 보여주고, 내부적으로는 원격 RPC를 수행할 수도 있습니다(Remote Proxy(원격 프록시))

내부적으로는 RPC, Socket 통신 등을 통해 원격 객체를 호출하지만, 클라이언트 입장에서는 지역(Local) 객체처럼 보이게 됩니다.

```cpp
#include <iostream>
#include <memory>
#include <string>

class IRemoteService
{
public:
    virtual ~IRemoteService() = default;
    virtual std::string GetData() = 0;
};

class RealRemoteService : public IRemoteService
{
public:
    std::string GetData() override
    {
        // 서버에서 복잡한 DB 조회나 연산을 수행한다고 가정
        return "Server Response: 데이터 처리 완료";
    }
};

class RemoteServiceProxy : public IRemoteService
{
private:
    std::unique_ptr<IRemoteService> realService;
    std::string serverAddress;

public:
    explicit RemoteServiceProxy(const std::string& address) : serverAddress(address) {}

    std::string GetData() override
    {
        std::cout << "[Proxy] '" << serverAddress << "' 서버에 연결 중..." << std::endl;

        // 실제 서버에 연결(여기서는 단순 시뮬레이션)
        // 네트워크 호출 전, 인증이나 권한 등 추가 로직 수행 가능
        if (!realService)
        {
            // 예: Proxy가 내부적으로 RealRemoteService를 생성
            realService = std::make_unique<RealRemoteService>();
        }

        return realService->GetData();
    }
};

int main()
{
    // 로컬에서는 서버 주소만 알고, 원격 프록시를 통해 서버 기능을 사용
    std::unique_ptr<IRemoteService> serviceProxy = std::make_unique<RemoteServiceProxy>("http://myserver.com/api");

    std::string result = serviceProxy->GetData();
    std::cout << result << std::endl;
}
```

이 예시는 실제 네트워크 소켓 연결이나 RPC 라이브러리를 사용하지 않고, 간단히 시뮬레이션한 코드입니다.

실제로는 원격 서버 주소를 가지고 통신하여 `RealRemoteService`의 기능을 대리호출하는 방식으로 동작할 수 있습니다.

</div>
</details>

<details>
<summary><h5 style="display: inline;">4.1 게임에서의 네트워크/원격 호출</h5></summary>
<div markdown="1">

온라인 멀티플레이 게임에서는 아이템 사용, 공격, 상점 거래 등이 서버에서 검증되어야 합니다.

프록시를 사용하면 로컬에서 실행되는 것처럼 보이지만, 실제로는 서버에서 처리하는 방식으로 구현할 수 있습니다.

```cpp
#include <iostream>
#include <string>
#include <memory>

// 아이템 인터페이스
class IItem
{
public:
    virtual void Use() = 0;
    virtual ~IItem() {}
};

// 실제 서버에서 아이템 사용 처리
class RealItem : public IItem
{
private:
    std::string itemName;

public:
    RealItem(const std::string& name) : itemName(name) {}

    void Use() override
    {
        std::cout << itemName << "' 사용 완료 (서버에서 검증됨)" << std::endl;
    }
};

// 원격 프록시 (서버와 통신)
class ItemProxy : public IItem
{
private:
    std::unique_ptr<RealItem> realItem;
    std::string itemName;
    bool isNetworkAvailable;

public:
    ItemProxy(const std::string& name, bool networkAvailable)
        : itemName(name), isNetworkAvailable(networkAvailable)
    {
    }

    void Use() override
    {
        if (!isNetworkAvailable)
        {
            std::cout << "네트워크 오류! '" << itemName << "' 사용 불가." << std::endl;
            return;
        }

        if (!realItem)
        {
            realItem = std::make_unique<RealItem>(itemName);
        }

        realItem->Use();
    }
};

// 사용 예시
int main()
{
    std::unique_ptr<IItem> potion = std::make_unique<ItemProxy>("체력 포션", false);
    potion->Use(); // 네트워크 오류

    std::cout << std::endl;

    std::cout << "네트워크 연결됨" << std::endl;
    std::unique_ptr<IItem> onlinePotion = std::make_unique<ItemProxy>("체력 포션", true);
    onlinePotion->Use(); // 서버에서 사용 완료
}
```

</div>
</details>