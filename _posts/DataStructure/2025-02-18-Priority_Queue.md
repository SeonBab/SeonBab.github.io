---
layout: single

title: "[C++ Data Structure] Priority Queue"

categories:
    - DataStructure
tag: [Cpp, 자료구조]

date: 2025-02-18
last_modified_at: 2025-02-18

order : 35
---

# Priority Queue

우선순위 큐(Priority Queue)는 기본적으로 가장 큰 원소가 먼저 나오는(최대 힙)구조입니다.  
즉, 우선순위가 높다는 것은 값이 크다는 것을 의미합니다.

내부적으로 힙(Heap) 자료구조를 사용합니다.  
기본적으로 `std::vector`를 기반으로 동작합니다.

기본적으로 내림차순(최대 힙)으로 정렬됩니다.

삽입, 삭제(pop)은 항상 `O(log N)`이고, top()(가장 우선순위 높은 원소 조회)은 `O(1)`입니다.

가장 큰/작은 원소를 최대한 빠르게 추출해야 하는 문제에 최적입니다.

인덱스를 이용한 직접적인 접근이 불가능합니다.  
반복자(iterator)또한 제공하지 않아 직접 순회할 수 없습니다.

다양한 데이터 타입을 지원하며, 사용자 정의 타입도 사용할 수 있습니다.

## 초기화

선언은 다음과 같습니다.

```cpp
template <class T, class Container = std::vector<T>, class Compare = std::less<T>>

std::priority_queue<[DataType]> [변수이름]
std::priority_queue<[DataType], [Container], [Compare]> [변수이름]
```

```cpp
// 최대 힙
std::priority_queue<int> pq;
// 최소 힙
std::priority_queue<int, vector<int>, greater<int>> minPq;
```

내부적으로 벡터를 사용해서 생략해도 되지만 세 번째 템플릿 인자를 주어야 하므로, 두 번째 인자도 명시적으로 작성해야합니다.

객체(예: 구조체)들이 특정 기준으로 우선순위를 가져야 한다면 사용자 정의 비교 함수를 넣어줄 수 있습니다.

```cpp
#include <queue>

using namespace std;

struct Compare
{
    bool operator()(const int& a, const int& b)
    {
        return a > b;
    }
};

int main()
{
	priority_queue<int, vector<int>, Compare> queue;
}
```

```cpp
#include <queue>
#include <string>

using namespace std;

// 예시: 이름(name)과 점수(score)를 담은 구조체
struct Student
{
    string name;
    int score;
};

// 점수가 높은 순으로 우선순위가 결정되게 하고 싶은 경우
struct Compare
{
    bool operator()(const Student &a, const Student &b)
    {
        return a.score < b.score;
        // a < b면 true → a 우선순위가 낮다고 판단
        // 즉, score 큰게 먼저 나오게 (내림차순)
    }
};

int main()
{
    priority_queue<Student, vector<Student>, Compare> pq;
}
```

## 주요 함수

### push

새로운 요소를 삽입하거나 기존 객체를 복사하거나 이동합니다.

삽입하는 값이 복사 또는 이동 할 때 복사 또는 이동 생성자가 필요합니다.

내부적으로 `std::push_heap`을 사용합니다.

```cpp
// 복사 삽입
// Lvalue 참조
push(const value_type& value)
// 복사 삽입
// Rvalue 참조
push(const T& value)
// 이동 삽입
push(T&& value)
```

```cpp
std::priority_queue<int> pq;

pq.push(5);
pq.push(2);
```

### emplace

```cpp
emplace(Args&&... args)
```

### pop

내부적으로 `std::pop_heap`을 사용합니다.

```cpp
std::priority_queue<int> pq;

pq.push(5);
pq.push(8);
pq.push(2);

pq.pop(); // 8 제거
```

### top

최상위(우선순위가 가장 높은) 원소 접근

```cpp
std::priority_queue<int> pq;

pq.push(5);
pq.push(2);
pq.push(3);

pq.top(); // 5 조회
```

### empty

`std::priority_queue`가 비어 있으면 `true`값을 반환하고, 그렇지 않으면 `false`를 반환합니다.

```cpp
std::priority_queue<int> pq;
std::cout << pq.empty() << std::endl;  // true
pq.push(10);
std::cout << pq.empty() << std::endl;  // false
```

### size

현재 저장된 요소의 개수를 반환합니다.

반환 자료형은 `size_t`입니다.

```cpp
std::priority_queue<int> pq;
std::cout << pq.size() << std::endl;  // 0
pq.push(5);
pq.push(15);
std::cout << pq.size() << std::endl;  // 2
```