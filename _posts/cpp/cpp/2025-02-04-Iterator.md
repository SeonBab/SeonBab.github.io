---
layout: single

title: "[C++] 반복자"

categories:
    - Cpp
tag: [Cpp]

date: 2025-02-04
last_modified_at: 2025-02-04

order : 100010
---

# 반복자

반복자(Iterator)는 컨테이너의 요소를 순회하는 데 사용되는 객체입니다.

포인터와 유사하지만, 컨테이너의 내부 구현과 무관하게 요소를 접근할 수 있도록 설계되어있습니다.

컨테이너마다 반복자 타입이 다릅니다.  
`std::vector<int>::iterator`, `std::list<int>::iterator`, `std::map<std::string, int>::iterator`

컨테이너가 변경되면 기존 반복자는 무효화될 수 있습니다.  
예시로 `std::vector`에서 `push_back`함수를 호출하면 기존 반복자가 무효화됩니다.

`it != container.end()` 조건을 확인하지 않고 사용하면 `Segmentation Fault`가 발생할 수 있습니다.

## 입력 반복자

한 방향(`++`)으로만 이동 가능합니다.  
`--`가 불가능합니다.

한 번 읽으면 다시 읽을 수 없습니다.(소모형)

읽기 전용이며, 수정은 불가능합니다.

대표적인 예: `std::istream_iterator`

```cpp
#include <iostream>
#include <iterator>

int main()
{
    std::istream_iterator<int> inIter(std::cin);
    int num = *inIter;  // 입력된 첫 번째 정수를 가져옴
    std::cout << "입력된 값: " << num << std::endl;
}
```

## 출력 반복자

한 방향(`++`)으로만 이동 가능합니다.  
`--`가 불가능합니다.

값을 읽을 수 없으며, 한 번만 쓸 수 있습니다.(소모형)

대표적인 예: `std::ostream_iterator`

```cpp
#include <iostream>
#include <iterator>

int main()
{
    std::ostream_iterator<int> outIter(std::cout, " ");
    *outIter = 42;  // 콘솔에 42 출력
}
```

## 순방향 반복자

한 방향(`++`)으로만 이동 가능합니다.  
`--`가 불가능합니다.

읽기와 수정이 가능하며, 같은 위치를 여러 번 읽을 수 있습니다.

대표적인 컨테이너: `std::forward_list`

```cpp
#include <iostream>
#include <forward_list>

int main()
{
    std::forward_list<int> flist = {1, 2, 3, 4};
    std::forward_list<int>::iterator it = flist.begin();

    while (it != flist.end())
    {
        std::cout << *it << " ";
        ++it;
    }
}
```

## 역방향 반복자

양 방향(`++`, `--`)으로 모두 이동 가능합니다.  
`++`를 사용하면 이전 원소로 이동하고, `--`를 사용하면 다음 원소로 이동합니다.

읽기와 수정이 가능하며, 같은 위치를 여러 번 읽을 수 있습니다.

대표적인 예: `reverse_iterator`

`reverse_iterator`는 내부적으로 정방향 반복자를 기반으로 동작합니다.  
`base()` 함수를 통해 정방향 반복자로 변환할 수 있고, 역방향 반복자가 가리키는 원소의 다음를 가리키게 됩니다.

```cpp
#include <iostream>
#include <vector>

int main()
{
    std::vector<int> vec = { 10, 20, 30, 40, 50 };

    std::vector<int>::reverse_iterator rit = vec.rbegin();
    std::vector<int>::iterator it = rit.base();  // base()를 사용하여 정방향 반복자로 변환

    --it;
    --it;

    std::cout << "rit: " << *rit << std::endl;  // 50 (rbegin)
    std::cout << "it: " << *it << std::endl;    // 40 (base()가 가리키는 위치)
}
```

일반적으로 컨테이너를 역순으로 순회할 때 `rbegin`함수와 `rend`함수를 사용합니다.

```cpp
#include <iostream>
#include <vector>

int main()
{
    std::vector<int> vec = {10, 20, 30, 40, 50};

    for (std::vector<int>::reverse_iterator rit = vec.rbegin(); rit != vec.rend(); ++rit)
    {
        std::cout << *rit << " ";
    }
}
```

## 양방향 반복자

양 방향(`++`, `--`)으로 모두 이동 가능합니다.

대표적인 컨테이너: `std::list`, `std::map`, `std::set`

```cpp
#include <iostream>
#include <list>

int main()
{
    std::list<int> list = {10, 20, 30};
    std::list<int>::iterator it = list.begin();

    while (it != list.end())
    {
        std::cout << *it << " ";
        ++it;
    }
    std::cout << std::endl;

    // 역방향 탐색
    it = list.end();
    while (it != list.begin()) {
        --it;
        std::cout << *it << " ";
    }
}
```

## 임의 접근 반복자

`++`, `--`, `+`, `-`, `[]` 연산을 지원합니다.

가장 강력한 반복자로, 배열과 유사한 동작을 수행합니다.

대표적인 컨테이너: `std::vector`, `std::deque`, `std::array`

```cpp
#include <iostream>
#include <vector>

int main()
{
    std::vector<int> vec = {100, 200, 300};
    std::vector<int>::iterator it = vec.begin();

    std::cout << *(it + 1) << std::endl;  // 200 출력
    it += 2;
    std::cout << *it << std::endl;  // 300 출력
}
```

## 상수 반복자

컨테이너의 데이터를 수정할 수 없는 반복자입니다.

양 방향(`++`, `--`)으로 모두 이동 가능합니다.

`read-only` 용도로 사용하게 됩니다.

`cbegin`함수와 `cend`함수를 사용합니다.

```cpp
#include <iostream>
#include <vector>

int main()
{
    std::vector<int> vec = {10, 20, 30, 40, 50};

    for (std::vector<int>::const_iterator it = vec.cbegin(); it != vec.cend(); ++it)
    {
        std::cout << *it << " ";
    }
}
```

## 삽입 반복자

컨테이너에 새로운 원소를 삽입할 때 사용합니다.

대표적인 예: `std::back_inserter`, `std::insert_iterator`, `std::front_inserter`

`std::insert_iterator`는 `std::set`이나 `std::map`같은 정렬된 컨테이너에서도 사용 할 수 있습니다.

`std::front_inserter`는 `std::list`나 `std::deque`에서만 사용할 수 있습니다.

```cpp
#include <vector>
#include <iterator>

int main()
{
    std::vector<int> vec = {1, 2, 3};
    std::vector<int> vec2 = {4, 5, 6};
    // vec 뒤에 vec2를 삽입 => vec {1,2,3,4,5,6}
    std::copy(vec2.begin(), vec2.end(), std::back_inserter(vec));
}
```

## 반복자 연산

포인터와 유사한 연산을 지원합니다.

|연산자|설명|
|---|---|
|`*it`|현재 요소 참조|
|`it->member`|구조체 또는 클래스 멤버 접근 ((*it).member와 동일)|
|`++it`|다음 요소로 이동|
|`--it`|이전 요소로 이동 (양방향 반복자, 임의 접근 반복자 등)|
|`it + n`|`n`번째 요소로 이동 (임의 접근 반복자)|
|`it - n`|`n`번째 이전 요소로 이동 (임의 접근 반복자)|
|`it1 - it2`|두 반복자 간 거리 계산 (임의 접근 반복자), (양방향 반복자에서는 X)|
|`>`, `<`, `>=`, `<=`|두 반복자 비교 (임의 접근 반복자)|
|`it1 == it2`|두 반복자 비교|
|`it1 != it2`|두 반복자 비교|