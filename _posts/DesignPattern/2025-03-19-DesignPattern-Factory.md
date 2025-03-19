---
layout: single

title: "[Design Pattern] Factory Pattern(팩토리 패턴)"

categories:
    - DesignPattern
tag: [디자인 패턴]

date: 2025-03-19
last_modified_at: 2025-03-19

order : 1060
---

# Factory Pattern

Factory Pattern(팩토리 패턴)은 객체 생성 로직을 별도의 팩토리(메서드, 클래스, 인터페이스 등)로 캡슐화하여, 클라이언트 코드가 특정 구체 클래스에 직접 의존하지 않도록 하는 생성(Creational) 디자인 패턴 중 하나입니다.

객체를 직접 생성하는 것이 아니라, 별도의 팩토리 클래스를 통해 객체를 생성합니다.  
즉, 구체 클래스를 직접 `new`키워드로 생성하지 않고, 하위 클래스나 별도 팩토리 객체에서 인스턴스 생성을 책임지게 합니다.

새로운 객체 타입을 추가하거나 객체 생성 로직이 바뀌어도 클라이언트의 기존 코드를 수정하지 않아도 어떤 객체를 생성할지 선택하는 로직을 분산/확장할 수 있게 해줍니다.

객체 생성 로직을 한곳에서 관리하여, 코드 중복을 방지합니다.

하위 클래스에서 이 메서드를 오버라이드해 다른 종류의 객체를 생성하게 함으로써, 생성 로직을 분기할 수 있습니다.

+ 장점
    - OCP(개방-폐쇄 원칙), DIP(의존성 역전 원칙) 등 SOLID 원칙 준수에 도움이 됩니다.
    - 객체 생성 로직을 캡슐화하여, 클라이언트 코드의 수정 범위를 최소화할 수 있습니다.
    - 팩토리만 교체(또는 확장)하면, 생성되는 객체를 손쉽게 변경할 수 있는 유연성을 가집니다.

+ 단점
    - 클래스와 구조가 다소 복잡해질 수 있습니다.
    - 분기 로직이 팩토리 쪽에 몰릴 수 있습니다.
    - 간단한 경우에는 `new`로 생성하는 게 더 나은 방법일 수 있습니다.

## 전략 패턴과의 차이

팩토리 패턴과 전략 패턴은 둘 다 유연하고 확장 가능한 코드를 작성한다는 점은 같습니다.  
하지만, 팩토리 패턴은 객체를 생성하는 과정 자체를 캡슐화 하며, 전략 패턴은 알고리즘(행동)을 캡슐화합니다.

팩토리 패턴은 객채의 생성 과정을 캡슐화하여, 객체를 직접 생성하는 것이 아닌 팩토리에서 생성하도록 합니다.  
즉, 객체를 생성할 때 사용합니다.

전략 패턴은 객체의 행위를 캡슐화하여 동적으로 변경할 수 있도록 합니다.  
동일한 작업을 수행하는 여러 알고리즘(전략) 중 하나를 선택해서 사용할 수 있도록 하는 것이 핵심입니다.  
즉, 객체의 동작(알고리즘)을 변경할 때 사용합니다.

## 팩토리 패턴의 종류

대표적인 종류는 다음과 같습니다.

+ Factory Method Pattern(팩토리 메서드 패턴): 제품이 단일하고 상속구조를 써야 하는 경우
+ Abstract Factory Pattern(추상 팩토리 패턴): 제품군이 여러 개고, 서로 관련성이 있는 경우
+ (확장) Simple Factory(간단 팩토리)라고 불리는 간단한 변형: 가볍게 팩토리를 도입하고 싶은 경우

### Factory Method Pattern

Factory Method(팩토리 메서드)는 추상 클래스(또는 인터페이스)에서 객체 생성을 위한 추상 메서드(또는 오버라이드 가능한 메서드)를 정의합니다.  
구상 클래스(서브클레스)에서 해당 메서드를 구현(오버라이딩)하여 구체 객체 생성 로직을 캡슐화하는 구조입니다.

다음과 같은 상황에서 사용합니다.

1. 서브클래스에 따라 다른 종류의 객체를 생성해야할 때 사용합니다.
    - 문서 애플리케이션(TextApplication, DrawingApplication), 게임 지역별 몬스터(ForestSpawner, CaveSpawner) 등
2. 객체 생성 과정을재정의(오버라이드)하고 싶을 때 사용합니다.
    - 프레임워크가 골격(Creator)만 두고, 사용자가 커스텀 서브클래스에서 생성 방식을 확장/재정의
3. `if-else`/`switch` 분기를 줄이고, 새로운 구체 클래스 추가 시 기존 코드 수정을 최소화하고 싶을 때 사용합니다.

구조는 다음과 같습니다.

1. Creator(추상 클래스/인터페이스)
    - 팩토리 메서드를 정의합니다.
    - 필요 시 객체 생성 전후 공통 작업(훅 메서드)을 구현
2. Concrete Creator(구상 서브클래스)
    - 추상 Creator를 상속받아 팩토리 메서드를 구체 구현
    - 어떤 구체 클래스 객체를 반환할지 결정

+ 장점
    - 클라이언트는 추상 Creator/인터페이스만 알고 있어도 되므로 구체 클래스의 의존을 최소화합니다.
    - 새로운 객체를 추가할 때 기존 클래스를 수정하지 않고, 서브클래스를 추가하면 되므로 OCP(개방-폐쇠)를 준수합니다.
    - 생성될 객체 종류를 서브클래스로 교체하여 쉽게 변경 및 확장이 가능하므로 유연한 확장이 가능합니다.
    
+ 단점
    - Creator 추상 클래스 + 여러 구상 Creator로 인해 클래스가 많아질 수 있습니다.
    - 단순한 경우, 그냥 `new`를 쓰는 편이 더 직관적일 수 있는 과설계 위험이 있습니다.
    - 분기 로직이(단일 Creator 내 여러 분기가 필요할 때) 일부 존재할 수 있습니다.

#### 예시

<details>
<summary><h5 style="display: inline;">1. 문서 애플리케이션</h5></summary>
<div markdown="1">

```cpp
#include <iostream>
#include <memory>

// (1) 구상 Creator 클래스 
// 사용될 인터페이스 & 구현 클래스
class IDocument
{
public:
	virtual void Open() = 0;
    virtual void Close() = 0;
    virtual void Save() = 0;
    virtual ~IDocument() = default;
};

class WordDocument : public IDocument
{
public:
	void Open() override { std::cout << "Word document opened." << std::endl; }
	void Close() override { std::cout << "Word document closed." << std::endl; }
	void Save() override { std::cout << "Word document saved." << std::endl; }
};

class PdfDocument : public IDocument {
public:
	void Open() override { std::cout << "PDF document opened." << std::endl; }
	void Close() override { std::cout << "PDF document closed." << std::endl; }
	void Save() override { std::cout << "PDF document saved." << std::endl; }
};

// (2) 추상 Creator 클래스
class Application
{
public:
	virtual std::unique_ptr<IDocument> CreateDocument() = 0; // 팩토리 메서드
	virtual ~Application() = default;

	std::unique_ptr<IDocument> NewDocument()
	{
		std::unique_ptr<IDocument> doc = CreateDocument();
		doc->Open();
		return doc;
	}

	void SaveDocument(std::unique_ptr<IDocument>& doc)
	{
		if (doc) doc->Save();
	}
};

// (3) 구상 Creator 클래스
class WordApplication : public Application
{
public:
	std::unique_ptr<IDocument> CreateDocument() override
	{
		return std::make_unique<WordDocument>();
	}
};

class PdfApplication : public Application
{
public:
	std::unique_ptr<IDocument> CreateDocument() override
	{
		return std::make_unique<PdfDocument>();
	}
};

// (4) 사용
int main()
{
	WordApplication wordApp;
	std::unique_ptr<IDocument> wordDoc = wordApp.NewDocument();
	wordApp.SaveDocument(wordDoc);

	PdfApplication pdfApp;
	std::unique_ptr<IDocument> pdfDoc = pdfApp.NewDocument();
	pdfApp.SaveDocument(pdfDoc);
}
```

`WordApplication`이 내부적으로 `WordDocument`를 생성하게 되므로, 클라이언트는 직접 `WordDocument`를 생성 할 필요가 없습니다.  
`PdfApplication`또한 같습니다.

</div>
</details>

<details>
<summary><h5 style="display: inline;">2. 게임 몬스터 스포너</h5></summary>
<div markdown="1">

```cpp
#include <iostream>
#include <memory>

// (1) 구상 Creator 클래스 
// 사용될 인터페이스 & 구현 클래스
class IMonster
{
public:
    virtual void Spawn() = 0;
    virtual void Attack() = 0;
    virtual ~IMonster() = default;
};

class Slime : public IMonster
{
public:
    void Spawn() override { std::cout << "Slime has spawned!" << std::endl; }
    void Attack() override { std::cout << "Slime attacks!" << std::endl; }
};

class Goblin : public IMonster
{
public:
    void Spawn() override { std::cout << "Goblin has spawned!" << std::endl; }
    void Attack() override { std::cout << "Goblin attacks!" << std::endl; }
};

// (2) 추상 Creator 클래스
class MonsterSpawner
{
public:
    virtual std::unique_ptr<IMonster> CreateMonster() = 0; // 팩토리 메서드
    virtual ~MonsterSpawner() = default;

    void SpawnMonster()
    {
        auto monster = CreateMonster();
        monster->Spawn();
        monster->Attack();
    }
};

// (3) 구상 Creator 클래스
class ForestSpawner : public MonsterSpawner
{
public:
    std::unique_ptr<IMonster> CreateMonster() override
    {
        return std::make_unique<Slime>();
    }
};

class CaveSpawner : public MonsterSpawner
{
public:
    std::unique_ptr<IMonster> CreateMonster() override
    {
        return std::make_unique<Goblin>();
    }
};

// (4) 사용
int main()
{
    ForestSpawner forest;
    CaveSpawner cave;

    forest.SpawnMonster(); // 슬라임이 등장
    cave.SpawnMonster();   // 고블린이 등장
}
```

</div>
</details>

### Abstract Factory Pattern

추상 팩토리 패턴은 서로 연관된(또는 의존 관계가 있는) 복수의 객체를 한꺼번에 생성해야 할 때 사용하는 패턴입니다.

서로 호환되는 객체들을 일관되게 만들고 싶을 때 사용합니다.  
Windows UI 세트(WindowsButton + WindowsCheckbox), Mac UI 세트(MacButton + MacCheckbox) 등

구조는 다음과 같습니다.

1. 추상 팩토리(인터페이스/추상 클래스)
    - 일련의 팩토리 메서드를 정의합니다.
2. 구체 팩토리(Concrete Factory)
    - 추상 팩토리를 구현하여, 특정 제품군에 맞는 객체들을 생성합니다.
3. 추상 제품(인터페이스/추상 클래스)
    - 각 제품군이 구현해야 할 공통 인터페이스입니다.
    - 예: `IButton`, `ICheckbox`
4. 구체 제품(Concrete Product)
    - 추상 제품 인터페이스를 구현한 실제 클래스입니다.
    - 예: WindowsButton, MacButton 등

+ 장점
    - 한 팩토리를 통해 생성되는 객체들은 서로 호환성을 가지며, 결과적으로 제품군 일관성을 가지게 됩니다.
    - 클라이언트는 추상 팩토리 인터페이스만 의존하므로, 구체 제품들을 몰라도 되므로 의존성 감소합니다.
    - 새로운 제품군(예: LinuxFactory)을 추가해도, 기존 코드를 크게 수정할 필요 없이 팩토리만 구현하면 되기 때문에 확장이 용이합니다.
    
+ 단점
    - 제품 종류가 많아질수록, 팩토리와 제품군에 대한 클래스/구현체 구현도 늘어날 수 있습니다. 
    - 생성해야 할 객체가 적어 단순한 경우 개념이 희미하다면, 오히려 복잡해 보일 수 있습니다.

#### 예시

<details>
<summary><h5 style="display: inline;">UI 컴포넌트</h5></summary>
<div markdown="1">

```cpp
#include <iostream>
#include <memory>

// (1) 인터페이스
class IButton
{
public:
    virtual void Render() = 0;
    virtual ~IButton() = default;
};

class ICheckbox
{
public:
    virtual void Render() = 0;
    virtual ~ICheckbox() = default;
};

// (2) 구체 제품
class WindowsButton : public IButton
{
public:
    void Render() override { std::cout << "Windows 버튼" << std::endl; }
};

class MacButton : public IButton
{
public:
    void Render() override { std::cout << "Mac 버튼" << std::endl; }
};

class WindowsCheckbox : public ICheckbox
{
public:
    void Render() override { std::cout << "Windows 체크박스" << std::endl; }
};

class MacCheckbox : public ICheckbox
{
public:
    void Render() override { std::cout << "Mac 체크박스" << std::endl; }
};

// (3) 추상 팩토리
class IGUIFactory
{
public:
    virtual std::unique_ptr<IButton> CreateButton() = 0;
    virtual std::unique_ptr<ICheckbox> CreateCheckbox() = 0;
    virtual ~IGUIFactory() = default;
};

// (4) 구체 팩토리
class WindowsFactory : public IGUIFactory
{
public:
    std::unique_ptr<IButton> CreateButton() override { return std::make_unique<WindowsButton>(); }
    std::unique_ptr<ICheckbox> CreateCheckbox() override { return std::make_unique<WindowsCheckbox>(); }
};

class MacFactory : public IGUIFactory
{
public:
    std::unique_ptr<IButton> CreateButton() override { return std::make_unique<MacButton>(); }
    std::unique_ptr<ICheckbox> CreateCheckbox() override { return std::make_unique<MacCheckbox>(); }
};

// (5) 사용
class AbstractFactoryExample
{
private:
    std::unique_ptr<IButton> _button;
    std::unique_ptr<ICheckbox> _checkbox;

public:
    AbstractFactoryExample(std::unique_ptr<IGUIFactory> factory)
        : _button(factory->CreateButton()), _checkbox(factory->CreateCheckbox())
    {
    }

    void RenderUI()
    {
        _button->Render();
        _checkbox->Render();
    }
};

int main()
{
    std::unique_ptr<IGUIFactory> windowsFactory = std::make_unique<WindowsFactory>();
    AbstractFactoryExample winApp(std::move(windowsFactory));
    winApp.RenderUI(); // Windows 버튼, Windows 체크박스

    std::unique_ptr<IGUIFactory> macFactory = std::make_unique<MacFactory>();
    AbstractFactoryExample macApp(std::move(macFactory));
    macApp.RenderUI(); // Mac 버튼, Mac 체크박스
}
```

</div>
</details>

### Simple Factory

Simple Factory(간단 팩토리)는 별도의 팩토리 클래스를 만들어 모든 객체 생성을 담당하여, 객체를 생성하는 방식입니다.  
정적 메서드를 기반으로합니다.

`GoF 디자인 패턴`에서 정식으로 정의한 패턴은 아니지만, 자주 사용됩니다.

별도의 팩토리 클래스(또는 팩토리 메서드) 하나만 만들어놓고, 메서드 내부에서 `if-else`나 `switch`를 사용하여 구체 객체를 생성해 반환합니다.

+ 장점
    - 객체 생성 로직을 한곳에 모아둘 수 있어서, 클라이언트에서는 단순하게 사용할 수 있습니다.
    - 구체 클래스를 직접 `new` 하지 않으므로, 클라이언트가 구현 클래스에 의존하지 않습니다.
    - 새로운 타입이 추가될 때, 심플 팩토리만 수정하면 되므로 수정 범위가 공통 팩토리 하나에 제한됩니다.
    
+ 단점
    - `if-else`/`switch` 분기 로직이 많아지면, 코드가 비대해질 수 있습니다.
    - 새로운 타입 추가 시에도 결국 심플 팩토리 내부 로직에서 수정이 필요합니다.
    - 새로운 타입이 추가될 경우, 팩토리 클래스를 수정해야 하므로, OCP(Open-Closed Principle) 위반 가능성이 있습니다.

#### 예시

<details>
<summary><h5 style="display: inline;">문서 도큐먼트</h5></summary>
<div markdown="1">

```cpp
#include <iostream>
#include <memory>
#include <string>
#include <stdexcept>

// (1) 인터페이스
class IDocument
{
public:
    virtual void Display() const = 0;
    virtual ~IDocument() = default;
};

// (2) 구체 클래스
class WordDocument : public IDocument
{
public:
    void Display() const override { std::cout << "Word Document" << std::endl; }
};

class PdfDocument : public IDocument
{
public:
    void Display() const override { std::cout << "PDF Document" << std::endl; }
};

// (3) 팩토리 클래스
class DocumentFactory
{
public:
    static std::unique_ptr<IDocument> CreateDocument(const std::string& type)
    {
        if (type == "Word")
            return std::make_unique<WordDocument>();
        else if (type == "PDF")
            return std::make_unique<PdfDocument>();
        else
            throw std::invalid_argument("Unknown Document Type");
    }
};

// (4) 사용 예제
int main()
{
    std::unique_ptr<IDocument> doc1 = DocumentFactory::CreateDocument("Word");
    std::unique_ptr<IDocument> doc2 = DocumentFactory::CreateDocument("PDF");

    doc1->Display(); // Word Document
    doc2->Display(); // PDF Document
}
```

</div>
</details>