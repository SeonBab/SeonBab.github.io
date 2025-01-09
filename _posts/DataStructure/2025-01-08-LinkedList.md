---
layout: single

title: "[Data Structure] 링크드 리스트"

categories:
    - DataStructure
tag: [Cpp, 자료구조]

date: 2025-01-08
last_modified_at: 2025-01-09

order : 10
---

# 링크드 리스트

링크드 리스트(Linked List)는 데이터를 저장하는 노드들이 연결된 형태로 구성된 선형 자료구조입니다.  
각 노드는 데이터를 저장하는 부분과 다음 노드를 가리키는 포인터를 포함합니다.

배열과 다르게 링크드 리스트는 동적 메모리를 사용해 크기를 자유롭게 조정할 수 있습니다.  
또한 메모리 상에서 연속적이지 않아도 됩니다.

링크드 리스트는 원소의 삽입/삭제에 데이터를 이동하지 않아도 되기 때문에 $O(1)$의 시간 복잡도를 가지는 자료구조입니다.

조회는 시간 복잡도가 $O(N)$으로 효율이 좋지 않습니다.

포인터를 저장하는 추가적인 메모리가 필요하다는 단점이 있습니다.

## 예시

예시로 총 5칸을 실은 화물 열차를 만들었습니다.  
각 화물칸은 다음 칸을 연결짓는 연결고리로 이어져 있습니다.

```cpp
train_compartments = ["기관실"] -> ["시멘트"] -> ["자갈"] -> ["밀가루"] -> ["우편"]
```

우편 칸에 잠시 일이 생겼습니다! 시멘트 칸을 지나, 자갈칸을 지나, 밀가루 칸을 지나 겨우 우편칸에 도착했습니다.

```cpp
처음 상태              내 위치
train_compartments = ["기관실"] -> ["시멘트"] -> ["자갈"] -> ["밀가루"] -> ["우편"]

1번 이동                            내 위치
train_compartments = ["기관실"] -> ["시멘트"] -> ["자갈"] -> ["밀가루"] -> ["우편"]

2번 이동                                         내 위치
train_compartments = ["기관실"] -> ["시멘트"] -> ["자갈"] -> ["밀가루"] -> ["우편"]

3번 이동                                                     내 위치
train_compartments = ["기관실"] -> ["시멘트"] -> ["자갈"] -> ["밀가루"] -> ["우편"]

4번 이동                                                                  내 위치
train_compartments = ["기관실"] -> ["시멘트"] -> ["자갈"] -> ["밀가루"] -> ["우편"]
```

앗 그런데, 자갈 칸과 밀가루 칸 사이에 흑연이라는 칸을 넣기로 했습니다.  
그래서, 화물칸의 연결고리를 이어 붙였습니다.  
간단하게 자갈 칸의 연결고리를 흑연 칸에 연결하고, 흑연 칸의 연결고리를 밀가루 칸으로 연결했습니다.

```
처음 상태
["기관실"] -> ["시멘트"] -> ["자갈"] -> ["밀가루"] -> ["우편"]
                              ["흑연"] 을 중간에 넣어야 합니다

1. 자갈 칸의 연결고리를 흑연 칸으로 연결하고,
["자갈"] -> ["흑연"]   ["밀가루"] -> ["우편"]

2. 흑연 칸으로 연결고리를 밀가루 칸으로 연결합니다. 
["자갈"] -> ["흑연"] -> ["밀가루"] -> ["우편"]
```

이렇게 유동적으로 연결고리를 떼었다가 붙였다가 할 수 있는 자료구조를 링크드 리스트라고 합니다.

위의 예제에서 각 화물 칸들을 링크드 리스트에서는 노드라고 합니다.  
맨 앞의 노드를 Head, 맨 뒤의 노드(포인터가 NULL)를 Tail이라고 합니다!

현재 노드가 가리키는 다음 화물 칸을 포인터라고 합니다.

```
["자갈"] -> ["흑연"] -> ["밀가루"] -> ["우편"]
```

여기서 자갈의 포인터는 흑연, 흑연의 포인터는 밀가루, 밀가루의 포인터는 우편, 우편의 포인터는 NULL입니다.

## 종류

다음과 같은 링크드 리스트가 있습니다.

단일 링크드 리스트(Singly Linked List)는 각 노드가 다음 노드를 가리키는 포인터만 포함합니다.

이중 링크드 리스트(Doubly Linked List)는 각 노드가 이전 노드와 다음 노드를 가리키는 포인터를 포함합니다.

원형 링크드 리스트(Circular Linked List)는 마지막 노드가 첫 번째 노드를 가리켜 순환 구조를 만듭니다.

### 단일 링크드 리스트 구현해보기

```cpp
// 노드 클래스 정의
class Node {
private:
    std::string data;    // 데이터를 저장할 변수
    Node* next;     // 다음 노드를 가리키는 포인터

public:
    // 생성자
    // 초기에는 다음 노드가 없으므로 nullptr로 초기화
    Node(std::string data) : data(data), next(nullptr) {}

    // LinkedList 클래스에서 Node의 private 멤버에 접근 가능하도록 친구로 설정!
    friend class LinkedList;
};

// 단일 링크드 리스트 클래스 정의
class LinkedList {
private:
    Node* head; // 리스트의 첫 번째 노드를 가리키는 포인터
    int nodeCount;  // 노드 개수를 추적하기 위한 변수

public:
    // 매개변수를 받는 생성자
    LinkedList(std::string value) {
        this->head = new Node(value);
        this->nodeCount = 1;
    }

    // 소멸자
    ~LinkedList() {
        Node* temp = head;

        // 모든 요소를 순회하고 메모리를 해제
        while (temp != nullptr)
        {
            Node* nextNode = temp->next;
            delete temp;
            temp = nextNode;
        }
    }

    // LinkedList 가장 끝에 있는 노드에 새로운 노드를 연결합니다.
    void append(std::string value) {

        Node* newNode = new Node(value);

        // 리스트가 비어있으면 head가 새 노드를 가리킴
        if (head == nullptr) {
            head = newNode; 
        }
        // 리스트가 비어있지 않다
        else {
            Node* temp = head;

            // 리스트 끝까지 이동
            while (temp->next != nullptr) {
                temp = temp->next; 
            }

            temp->next = newNode;
        }

        this->nodeCount++;
    }

    // n번째 인덱스에 있는 노드를 가져오고 싶을 때
    Node* getNode(int index) {
        // nodeCount를 근거로 index가 유효한지를 판단
        if (0 > index || index >= nodeCount) {
            throw std::out_of_range("유효하지 않은 인덱스!");
        }

        // 원하는 위치에 도착할 때까지 다음 노드로 이동
        Node* node = this->head;
        for (int i = 0; i < index; i++) {
            node = node->next;  
        }

        return node;
    }

    void addNode(int index, std::string value) {
        // ["자갈"] -> ["흑연"] -> ["밀가루"] -> ["우편"]
        // 이때까지는, this->head = ["자갈"]
        // ["짱짱"]이라는 친구를 0번째 index로 추가
        // this->head = ["짱짱"]
        // ["짱짱"] -> ["자갈"] -> ["흑연"] -> ["밀가루"] -> ["우편"]
        Node* newNode = new Node(value);  // 새 노드 생성

        // 0번째에 추가를 하고 싶다면 해드 변경
        if (index == 0) {  
            newNode->next = this->head;  // 원래 Head였던 노드를 새 노드의 next로 지정
            this->head = newNode;  // Head를 새 노드로 변경
            this->nodeCount++;
            return;
        }

        // [3] - [4] - [5]에서 [3] - [4] - [6] - [5]로 6을 중간에 삽입한다고 가정
        // 추가하고 싶은 index의 이전 노드 정보를 가져옴, 여기선 [4] 입니다.
        Node* node = this->getNode(index - 1);

        // 이전 노드([4])의 포인터([5])를 next_node로 임시 저장
        Node* nextNode = node->next;

        // 이전 노드([4])의 포인터를 [6]으로 지정합니다!
        // [4] -> [6]
        node->next = newNode;

        // 새로 삽입한 노드([6])의 포인터를 next_node인 [5]로 지정
        // [6] -> [5]
        newNode->next = nextNode;
        this->nodeCount++;
    }

    // 노드 삭제 (특정 값 삭제)
    bool remove(std::string value) {
        // 리스트가 비어있으면 아무것도 하지 않음
        if (head == nullptr) return false;

        // 해드의 값을 삭제해야 한다면
        if (head->data == value) {
            Node* temp = head; // 해드 임시 저장
            
            head = head->next; // head를 다음 노드로 이동

            delete temp; // 임시 저장한 해드 메모리 해제
            this->nodeCount--;  // 노드 삭제 후 감소

            return true; // 삭제 성공
        }

        // 특정 값을 가진 노드 찾기
        Node* temp = head;
        while (temp->next != nullptr && temp->next->data != value) {
            temp = temp->next;
        }

        // 특정 값을 가진 노드를 찾았다면
        if (temp->next != nullptr) {
            Node* nodeToDelete = temp->next; // 삭제 할 노드 임시 저장
            temp->next = temp->next->next; // 삭제 할 노드를 건너뜀
            delete nodeToDelete; // 노드 삭제
            this->nodeCount--;  // 노드 삭제 후 감소

            return true; // 삭제 성공
        }

        return false; // 삭제 실패(값을 찾지 못한 경우)
    }
};
```

### 이중 연결 리스트 구현해보기

이중 연결 리스트는 이전 노드를 가리키는 포인터, 다음 노드를 가리키는 포인터 두 개의 포인터를 가지는 자료구조입니다.

이런 구조로 인해, 양방향 순회가 가능하고 삽입/삭제가 특정 위치에서 효율적으로 이루어집니다.

양방향 순회가 가능합니다.

단일 연결 리스트보다 추가적인 포인터를 가지므로 메모리 소모가 큽니다.

```cpp
#include <iostream>
using namespace std;

// 노드 구조 정의
struct Node {
    int data;
    Node* prev; // 이전 노드 포인터
    Node* next; // 다음 노드 포인터

    Node(int val) : data(val), prev(nullptr), next(nullptr) {}
};

// 이중 연결 리스트 클래스
class DoublyLinkedList {
private:
    Node* head; // 첫 번째 노드
    Node* tail; // 마지막 노드

public:
    DoublyLinkedList() : head(nullptr), tail(nullptr) {}

    // 노드 추가 (리스트 끝에 추가)
    void append(int data) {
        Node* newNode = new Node(data);
        if (!head) { // 리스트가 비어있는 경우
            head = tail = newNode;
        } else {
            tail->next = newNode;
            newNode->prev = tail;
            tail = newNode;
        }
    }

    // 노드 삭제
    void remove(int data) {
        Node* current = head;
        while (current) {
            if (current->data == data) {
                if (current->prev) {
                    current->prev->next = current->next;
                } else {
                    head = current->next; // 첫 번째 노드 삭제
                }

                if (current->next) {
                    current->next->prev = current->prev;
                } else {
                    tail = current->prev; // 마지막 노드 삭제
                }

                delete current;
                return;
            }
            current = current->next;
        }
    }

    // 리스트 출력
    void print() const {
        Node* current = head;
        while (current) {
            cout << current->data << " ";
            current = current->next;
        }
        cout << endl;
    }

    // 역순 출력
    void printReverse() const {
        Node* current = tail;
        while (current) {
            cout << current->data << " ";
            current = current->prev;
        }
        cout << endl;
    }

    ~DoublyLinkedList() {
        Node* current = head;
        while (current) {
            Node* temp = current;
            current = current->next;
            delete temp;
        }
    }
};
```