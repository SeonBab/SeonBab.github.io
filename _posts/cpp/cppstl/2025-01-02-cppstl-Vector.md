---
layout: single

title: "[C++ STL] 벡터"

categories:
    - CppSTL
tag: [Cpp, CppSTL]

date: 2025-01-02
last_modified_at: 2025-01-02

order : 10
---

# 벡터

`#include <vector>`헤더 파일에 존재합니다.

벡터(vector)는 동적 배열을 구현한 컨테이너로, 크기를 동적으로 변경할 수 있는 배열처럼 작동합니다.  
배열과 비슷하지만 메모리 관리와 크기 조정같은 작업을 자동으로 처리해줍니다.

벡터의 크기가 할당된 메모리의 크기만큼 사용할 경우 기존 메모리를 새로운 크기로 재할당 합니다.  
이 과정에서 용량(메모리)의 크기가 2배가 되며, 복사가 발생합니다.

내부적으로 동적 메모리를 관리하므로, 사용자가 직접 `new`나 `delete`를 사용할 필요가 없습니다.

내부적으로 요소들이 연속적인 메모리에 저장되므로, 일반 배열처럼 빠른 임의 접근(랜덤 액세스)이 가능합니다.  
접근 시간은 $O(1)$입니다.

다양한 데이터 타입을 지원하며, 사용자 정의 타입도 저장할 수 있습니다.

## 벡터 초기화

```cpp
// 기본 생성 및 초기화 없이 정의
std::vector<int> numbers1;
// 오른쪽 변수 값으로 초기화
std::vector<int> numbers2 = { 1, 2, 4, 7, 2, 9, 231 };
// 다른 벡터를 기반으로 복사 초기화
std::vector<int> numbers3 = numbers2;
std::vector<int> numbers4(numbers2);
std::vector<int> numbers5{ numbers2 };
// 특정 크기로 생성 및 초기화 없이 정의
std::vector<double> numbers3(3);
// 특정 크기와 초기값으로 정의
std::vector<std::string> words(2, "string");
```

## 주요 연산자

### [ ]

인덱스를 사용해 요소에 접근합니다.

범위를 확인하지 않습니다.

```cpp
numbers[1];
numbers[5];
```

## 주요 함수

### push_back

벡터 끝에 요소를 추가합니다.

이미 생성된 객체를 추가하므로 복사 또는 이동 생성자가 필요합니다.

```cpp
numbers1.push_back(10);
numbers1.push_back(20);
words.push_back("abc");
words.push_back("apple");
```

### emplace_back

`push_back`함수와 비슷하지만 컨테이너의 끝에 요소를 "직접 생성"하여 추가합니다.  
즉, 필요한 인자가 전달되면 벡터 내부에서 객체가 직접 생성되어 추가됩니다.

객체를 생성하는 데 여러 인자를 전달해야 하는 경우 사용합니다.  
복사나 이동 연산을 피하고 싶을 때 사용합니다.  
객체 생성 과정에서 성능 최적화가 중요한 경우 사용합니다.

`emplace_back`은 객체를 생성하는 데 필요한 정확한 인자를 전달해야 합니다.  
생성자가 없는 객체나 초기화 인자가 잘못된 경우 컴파일 에러가 발생할 수 있습니다.

```cpp
numbers2.emplace_back(13);
numbers2.emplace_back(17);
words.push_back("abc");
words.push_back("apple");
```

### insert

```cpp
//3번째 인덱스에 8삽입하고, 뒤에 인덱스는 1씩 밀립니다.
numbers1.insert(numbers1.begin() + 3, 8);
// 끝에 5가 3번 삽입됩니다.
numbers1.insert(numbers1.end() + 3, 3, 5);
```

### swap

두 벡터의 요소와 용량을 교환합니다.

```cpp
numbers1.swap(numbers2);
```

### at

`at`함수는 인덱스 범위를 확인하고, 접근합니다.

```cpp
numbers2.at(1);
numbers2.at(5);
```

### front

첫번째 요소의 참조를 반환합니다.

```cpp
numbers1.front();
```

### back

마지막 요소의 참조를 반환합니다.

```cpp
numbers.back();
```

### size

현재 저장된 요소의 개수를 반환합니다.

```cpp
numbers.size();
```

### capacity

메모리가 할당된 총 크기를 반환합니다.

```cpp
numbers.capacity();
```

### empty

벡터가 비었으면 `true`값이 반환됩니다.  
만약 비어있지 않으면 `flase`가 반환됩니다.

```cpp
numbers1.empty();
```

### resize

벡터의 크기인 size를 변경합니다.

새로 추가된 요소는 기본값이나 지정된 값으로 초기화 됩니다.

필요한 경우 메모리가 재할당됩니다.

```cpp
// 크기를 3으로 변경하고 더 커졌을 경우 기본값으로 초기화합니다.
numbers1.resize(3);
// 크기를 7로 변경하고 더 커졌을 경우 5로 초기화합니다.
numbers1.resize(7, 5);
```

### reserve

벡터에 미리 동적 할당을 받아 최소 용량을 설정해둡니다.  
이미 확보된 용량 크기인 경우 아무 작업도 수행하지 않습니다.

동적 메모리 재할당으로 인한 비용을 줄이는 데 사용됩니다.  
이 과정에서 실제 크기에는 영향을 주지 않습니다.

빈 슬롯은 초기화되지 않습니다.

```cpp
// 최소 용량을 10으로 설정합니다.
numbers1.reserve(10);
```

### shrink_to_fit

남은 여유 용량(메모리)를 반환하여 capacity를 size와 같게 맞춥니다.

```cpp
numbers1.shrink_to_fit();
```

### pop_back

마지막 요소를 제거합니다.

```cpp
numbers2.pop_back();
```

### erase

특정 위치나 범위의 요소를 제거합니다.

```cpp
// 두 번째 요소 삭제
numbers1.erase(numbers.begin() + 1);
```

### clear

모든 요소를 제거합니다.

요소만 제거하며, 메모리는 남아있습니다.  
즉, size만 줄어들고 capacity는 변하지 않습니다.

```cpp
numbers1.clear();
```

### begin

벡터의 첫 번째 요소를 가리키는 반복자를 반환합니다.  
반복자는 포인터처럼 작동합니다.

벡터의 요소를 순회하거나 반복자(Iterator)를 사용할 때 사용됩니다.

```cpp
numbers1.begin();

// 반복자를 이용한 요소 출력
for (auto it = numbers1.begin(); it != numbers1.end(); ++it) {
    std::cout << *it << " ";
}
```

### end

벡터의 마지막 요소의 바로 다음 위치를 가리키는 반복자를 반환합니다.

실제 요소를 가리키지 않고, 종료 조건으로 사용됩니다.

```cpp
numbers1.end();

// 반복자를 이용한 요소 출력
for (auto it = numbers1.begin(); it != numbers1.end(); ++it) {
    std::cout << *it << " ";
}
```

### rbegin

마지막 요소를 기리키는 반복자를 반환합니다.

C++11에서 추가됐습니다.

```cpp
numbers1.rbegin();
```

### rend

첫 번째 요소의 앞을 가리키는 반복자를 반환합니다.

실제 요소를 가리키지 않고, 종료 조건으로 사용됩니다.

C++11에서 추가됐습니다.

```cpp
numbers1.rend();
```

### cbegin 

벡터의 첫 번째 요소를 가리키는 읽기 전용(const) 시작 반복자를 반환합니다.

C++11에서 추가됐습니다.

```cpp
numbers1.cbegin();
```

### cend

벡터의 마지막 요소의 바로 다음 위치를 가리키는 읽기 전용(const) 반복자를 반환합니다.

C++11에서 추가됐습니다.

```cpp
numbers1.cend();
```