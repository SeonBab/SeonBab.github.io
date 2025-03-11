---
layout: single

title: "[Design Pattern] 반복자 패턴"

categories:
    - DesignPattern
tag: [디자인 패턴]

date: 2025-03-11
last_modified_at: 2025-03-11

order : 1040
---

# 반복자 패턴

반복자(Iterator) 패턴은 객체의 내부 구조를 노출하지 않고 컬렉션(Collection) 혹은 컨테이너(Container)의 요소들을 하나씩 순회(Traverse)할 수 있도록 하는 행동(Behavioral) 디자인 패턴입니다.  
리스트, 스택, 트리, 그래프 등 어떠한 형태로 컬렉션이 구성되었든, 내부 구조를 모르는 상태로도 요소를 순회하고 탐색할 수 있는 방법(반복자 객체)을 제공합니다.

반복자 패턴의 핵심 아이디어는 다음과 같습니다.

1. 순회 로직을 컬렉션과 분리하여 별도의 객체(반복자)로 구현합니다.
2. 컬렉션 클래스는 데이터 구조 관리에만 집중하고, 순회 로직은 반복자가 담당합니다.
3. 반복자는 컬렉션 요소를 하나씩 꺼내는 방법, 순회 순서(알고리즘), 현재 인덱스/포인터 등을 내부에 캡슐화합니다.

반복자 패턴의 구조는 다음과 같습니다.

1. 반복자(Iterator)로 컬렉션 내부의 요소들을 순차적으로 접근할 수 있도록 도와주는 객체입니다.
    + `next`, `hasNext`등의 메서드를 제공하여 요소를 순회할 수 있도록 합니다.
2. 집합체(Aggregate, Collection)로 리스트, 배열, 맵 등의 컬렉션들이 있는 객체입니다.
    + 요소의 집합을 나타내는 인터페이스를 정의하며, 반복자를 생성하는 메서드를 가집니다.

반복자들은 일반적으로 다음 요소를 가져오는 메서드와 순회가 끝났는지 확인하는 메서드를 포함하는 공통 인터페이스를 구현합니다.  
클라이언트 코드는 해당 인터페이스만 보고 순회할 수 있으므로, 구체적인 자료구조와 알고리즘 세부사항은 몰라도 됩니다.  
예를 들어, `bool HasNext`함수로 다음 요소가 존재하는지 확인하고, `T Next`함수로 다음 요소를 가져오고, 내부적으로 포인터(현재 위치)를 한 칸 전진합니다.

각 반복자는 내부 상태(현재 인덱스, 이미 방문한 노드 등)를 독립적으로 유지하기 때문에, 여러 개의 반복자가 동시에 같은 컬렉션을 순회하더라도 서로 간섭하지 않습니다.  
트리 구조에서는 깊이 우선 탐색(DFS) 반복자, 너비 우선 탐색(BFS) 반복자 등 다양한 순회 방식을 지원할 수 있으며, 필요에 따라 적절한 반복자를 선택할 수 있습니다.

아래 문제를 해결하기 위해 반복자 패턴을 사용합니다.  

특정 컬렉션을 순회해야 할 때, 클라이언트 쪽에서 직접 해당 컬렉션의 내부 구조(예: 노드 링크, 인접 리스트, 배열 인덱스 등)를 알아야 한다면, 컬렉션 내부 구현이 클라이언트 코드에 노출됩니다.  
이렇게 되면, 컬렉션 변경(내부 구조 수정)이 일어날 때마다, 클라이언트 코드도 함께 수정해야 하므로 결합도가 높아집니다.

단순 배열이라면 `for` 또는 `foreach` 루프로 간단히 끝날 수 있지만, 트리, 그래프 등 복잡한 구조에서는 여러 순회 방법이 가능합니다.  
만약 컬렉션 클래스가 직접 순회 알고리즘 까지 구현하면, 본래의 책임(데이터 구조 관리) 외에 여러 로직까지 담당하게 되어, 단일 책임 원칙(SRP)을 해치게 됩니다.  
또한 동일 컬렉션을 동시에 다른 방식으로 순회하고 싶을 수도 있는데, 하나의 컬렉션에 내부 상태를 전부 기록해버리면 병렬 순회가 어려워집니다.

+ 장점
    + 클라이언트는 반복자 인터페이스만 사용하므로, 컬렉션의 내부 구조를 노출하지 않고도 동일한 방식으로 요소들을 순회할 수 있습니다.
    + 컬렉션은 데이터 저장/관리 책임에 집중하며, 반복자는 순회 로직에 집중하며 단일 책임 원칙(SRP)을 지킵니다.
    + 복잡한 순회 알고리즘을 컬렉션에서 분리함으로써 코드 구조가 깔끔해집니다.
    + 동시에 여러 반복자를 사용할 수 있습니다.
    + 새로운 순회 방식이 필요하다면 새 반복자 클래스를 만들고, 컬렉션에서 반환만 해주면 됩니다.

+ 단점
    + 간단한 자료구조에는 오히려 과할 수 있습니다.
    + 특정 자료구조의 직접적 접근보다 성능이 떨어질 수 있습니다.

## 예시

### 배열

플레이어의 인벤토리를 예시로 들어보겠습니다.  
내부적으로 배열일 수도 있고, 해시맵 또는 링크드 리스트, 트리형 구조로 인벤토리를 구성할 수 있습니다.  
하지만, 인벤토리 내 모든 아이템을 순회하며, 아이템 타입이나 속성에 따라 처리하려는 요구가 있을 수 있습니다.  
이때 반복자 패턴을 적용하여, 인벤토리가 어떻게 구현되었든, 공통 인터페이스를 사용해 아이템을 하나씩 꺼내 확인할 수 있습니다.

```cpp
// 반복자 인터페이스
template <typename T>
class IIterator
{
public:
	virtual bool HasNext() = 0;
	virtual T* Next() = 0;
};
```

제네릭(T)으로 설계하면 다양한 타입의 요소를 순회할 수 있습니다.

```cpp
// 컬렉션 인터페이스
template <typename T>
class IAggregate
{
	virtual IIterator<T>* CreateIterator() = 0;
};
```

`CreateIterator` 메서드를 통해 컬렉션에 맞는 반복자를 생성(또는 반환)합니다.

주의할 점은, 반복자 반환 타입이 `IIterator<T>`(인터페이스)라는 점입니다.  
이로써, 컬렉션 구현체가 무엇이든, 클라이언트는 반복자 인터페이스만 보고 순회할 수 있게 됩니다.

```cpp
// 구체 컬렉션 및 반복자의 응용을 위한 간단한 Item 클래스 예시
class Item
{
public:
    Item(std::string name, int weight)
    {
        _name = name;
        _weight = weight;
    }

    std::string GetName() { return _name; }
    int GetWeight() { return _weight; }

private:
    std::string _name;
    int _weight;
};
```

```cpp
// 구체 컬렉션 구현

template <typename T>
class InventoryIterator; // 반복자 선언

template <typename T>
class Inventory : public IAggregate<T>
{
public:
    Inventory(int size)
    {
        _items = new T*[size];
        _size = size;
        _lastIndex = 0;
    }

    ~Inventory()
    {
        delete[] _items;
    }

    void AddItem(T* item)
    {
        if (_size > _lastIndex)
        {
            _items[_lastIndex] = item;
            ++_lastIndex;
        }
        else
        {
            std::cout << "인벤토리가 가득 찼습니다." << std::endl;
        }
    }

    T* GetItemAt(int index)
    {
        if (index < 0 || index >= _lastIndex)
        {
            return nullptr;
        }

        return _items[index];
    }

    int Count()
    {
        return _lastIndex;
    }

    IIterator<T>* CreateIterator() override
    {
        return new InventoryIterator<T>(this);
    }

private:
    T** _items;
    int _size;
    int _lastIndex;
};
```

```cpp
// 구체 반복자 구현
template <typename T>
class InventoryIterator : public IIterator<T>
{
public:
    InventoryIterator(Inventory<T>* inventory)
    {
        _inventory = inventory;
        _currentIndex = 0;
    }

    bool HasNext() override
    {
        return _currentIndex < _inventory->Count();
    }

    T* Next() override
    {
        T* item = _inventory->GetItemAt(_currentIndex);
        _currentIndex++;
        return item;
    }

private:
    Inventory<T>* _inventory;
    int _currentIndex;
};
```

```cpp
// 사용 예시
int main()
{
    // 인벤토리 생성
    Inventory<Item>* inventory = new Inventory<Item>(5);

    // 아이템 추가
    inventory->AddItem(new Item("Sword", 10));
    inventory->AddItem(new Item("Shield", 5));
    inventory->AddItem(new Item("Potion", 1));

    // 반복자 받아오기
    IIterator<Item>* it = inventory->CreateIterator();

    while (it->HasNext())
    {
        Item* Current = it->Next();
        std::cout << "아이템: " << Current->GetName() << ", " << "무게: " << Current->GetWeight() << std::endl;
    }
}
```

출력은 다음과 같습니다.

```
아이템: Sword, 무게: 10
아이템: Shield, 무게: 5
아이템: Potion, 무게: 1
```

### 연결 리스트

```cpp
// 반복자 인터페이스
template <typename T>
class IIterator
{
public:
    virtual bool HasNext() = 0;
    virtual T Next() = 0;
};
```

```cpp
// 컬렉션 인터페이스
template <typename T>
class IAggregate
{
    virtual IIterator<T>* CreateIterator() = 0;
};
```

```cpp
// 구체 컬렉션 구현
template <typename T>
class ListNode
{
public:
    ListNode(T data)
    {
        _data = data;
        _next = nullptr;
    }

    T GetData() { return _data; }
    ListNode<T>* GetNext() { return _next; }

    // _next 값을 변경할 수 있도록 setter 추가
    void SetNext(ListNode<T>* next) { _next = next; }

private:
    T _data;
    ListNode<T>* _next;
};

template <typename T>
class LinkedListIterator;

template <typename T>
class LinkedListCollection : public IAggregate<T>
{
public:
    LinkedListCollection()
    {
        _head = nullptr;
        _count = 0;
    }

    // 소멸자: 동적 할당된 노드 삭제
    ~LinkedListCollection()
    {
        ListNode<T>* current = _head;

        while (current) {
            ListNode<T>* nextNode = current->GetNext();
            delete current;
            current = nextNode;
        }
    }

    // 연결 리스트 맨 뒤에 요소 추가
    void AddLast(T data)
    {
        ListNode<T>* newNode = new ListNode<T>(data);
        ++_count;

        if (!_head)
        {
            _head = newNode;
            return;
        }

        ListNode<T>* current = _head;

        while (current->GetNext())
        {
            current = current->GetNext();
        }

        current->SetNext(newNode);
    }
    
    // 편의 메서드: 특정 인덱스 요소 가져오기
    T GetElementAt(int index)
    {
        if (index < 0 || index >= _count)
        {
            return T();
        }

        ListNode<T>* current = _head;
        int currentIndex = 0;
        while (current)
        {
            if (currentIndex == index)
            {
                return current->GetData();
            }

            ++currentIndex;
            current = current->GetNext();
        }

        return T();
    }

    int Count()
    {
        return _count;
    }

    // IAggregate<T> 구현
    IIterator<T>* CreateIterator()
    {
        return new LinkedListIterator<T>(this);
    }

    // 반복자에서 Head에 접근하기 위한 Getter
    ListNode<T>* GetHead() { return _head; }
private:
    ListNode<T>* _head;
    int _count;
};
```

```cpp
// 구체 반복자 구현
template <typename T>
class LinkedListIterator : public IIterator<T>
{
public:
    LinkedListIterator(LinkedListCollection<T>* list)
    {
        _list = list;
        _current = list->GetHead();
    }

    bool HasNext() override
    {
        return _current != nullptr;
    }

    T Next() override
    {
        if (!HasNext()) return T();

        T data = _current->GetData();
        _current = _current->GetNext();
        return data;
    }

private:
    LinkedListCollection<T>* _list;
    ListNode<T>* _current;
};
```

```cpp
// 사용 예시
int main()
{
    LinkedListCollection<std::string>* linkedList = new LinkedListCollection<std::string>();
    linkedList->AddLast("Alice");
    linkedList->AddLast("Bob");
    linkedList->AddLast("Charlie");

    IIterator<std::string>* iterator = linkedList->CreateIterator();
    while (iterator->HasNext())
    {
        std::cout << iterator->Next() << std::endl;
    }
}
```

출력은 다음과 같습니다.

```
Alice
Bob
Charlie
```

### 트리

트리는 다양한 순회 방법(깊이 우선, 너비 우선, 전위/중위/후위 등)이 있습니다.

반복자 패턴을 통해 같은 트리에 대해서도 여러 순회 알고리즘을 제공할 수 있습니다.

```cpp
// 반복자 인터페이스
template <typename T>
class IIterator
{
public:
    virtual bool HasNext() = 0;
    virtual T Next() = 0;
};
```

```cpp
// 컬렉션 인터페이스
template <typename T>
class IAggregate
{
    virtual IIterator<T>* CreateIterator() = 0;
};
```

```cpp
// 트리 노드 클래스
template <typename T>
class TreeNode
{
public:
    T data;
    std::vector<std::shared_ptr<TreeNode<T>>> children;

    TreeNode(T value) : data(value) {}

    void AddChild(std::shared_ptr<TreeNode<T>> child)
    {
        children.push_back(child);
    }
};
```

```cpp
// 깊이 우선 탐색(DFS) 반복자
template <typename T>
class DepthFirstTreeIterator : public IIterator<T>
{
private:
    std::stack<std::shared_ptr<TreeNode<T>>> stack;

public:
    DepthFirstTreeIterator(std::shared_ptr<TreeNode<T>> root)
    {
        if (root) stack.push(root);
    }

    bool HasNext() override
    {
        return !stack.empty();
    }

    T Next() override
    {
        if (!HasNext()) return T();  // 기본값 반환

        auto node = stack.top();
        stack.pop();

        // 자식들을 역순으로 스택에 삽입 (왼쪽부터 방문)
        for (int i = node->children.size() - 1; i >= 0; --i)
        {
            stack.push(node->children[i]);
        }

        return node->data;
    }
};
```

```cpp
// 너비 우선 탐색(BFS) 반복자
template <typename T>
class BreadthFirstTreeIterator : public IIterator<T>
{
private:
    std::queue<std::shared_ptr<TreeNode<T>>> queue;

public:
    BreadthFirstTreeIterator(std::shared_ptr<TreeNode<T>> root)
    {
        if (root) queue.push(root);
    }

    bool HasNext() override
    {
        return !queue.empty();
    }

    T Next() override
    {
        if (!HasNext()) return T();  // 기본값 반환

        auto node = queue.front();
        queue.pop();

        for (const auto& child : node->children)
        {
            queue.push(child);
        }

        return node->data;
    }
};
```

```cpp
// 트리 컬렉션 클래스
template <typename T>
class TreeCollection : public IAggregate<T>
{
private:
    std::shared_ptr<TreeNode<T>> root;

public:
    TreeCollection(std::shared_ptr<TreeNode<T>> rootNode) : root(rootNode) {}

    std::shared_ptr<TreeNode<T>> GetRoot()
    {
        return root;
    }

    DepthFirstTreeIterator<T> CreateDFSIterator()
    {
        return DepthFirstTreeIterator<T>(root);
    }

    BreadthFirstTreeIterator<T> CreateBFSIterator()
    {
        return BreadthFirstTreeIterator<T>(root);
    }

    IIterator<T>* CreateIterator() override
    {
        return new DepthFirstTreeIterator<T>(root);
    }
};
```

```cpp
// 사용 예시
int main()
{
    // 트리 구성
    auto root = std::make_shared<TreeNode<std::string>>("Root");
    auto childA = std::make_shared<TreeNode<std::string>>("ChildA");
    auto childB = std::make_shared<TreeNode<std::string>>("ChildB");
    auto childA1 = std::make_shared<TreeNode<std::string>>("ChildA1");
    auto childB1 = std::make_shared<TreeNode<std::string>>("ChildB1");

    root->AddChild(childA);
    root->AddChild(childB);
    childA->AddChild(childA1);
    childB->AddChild(childB1);

    // 트리 컬렉션 생성
    TreeCollection<std::string> tree(root);

    // DFS 순회
    std::cout << "=== DFS ===\n";
    auto dfs = tree.CreateDFSIterator();
    while (dfs.HasNext())
    {
        std::cout << dfs.Next() << "\n";
    }

    // BFS 순회
    std::cout << "\n=== BFS ===\n";
    auto bfs = tree.CreateBFSIterator();
    while (bfs.HasNext())
    {
        std::cout << bfs.Next() << "\n";
    }
}
```

출력은 다음과 같습니다.

```
=== DFS ===
Root
ChildA
ChildA1
ChildB
ChildB1

=== BFS ===
Root
ChildA
ChildB
ChildA1
ChildB1
```