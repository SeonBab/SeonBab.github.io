---
layout: single

title: "[C++ STL] 맵"

categories:
    - CppSTL
tag: [Cpp, CppSTL]

date: 2025-01-06
last_modified_at: 2025-01-06

order : 50
---

# 맵

`<map>`헤더에 존재합니다.

키-값(key-value) 쌍을 저장하고 관리하는 연관 컨테이너입니다.

키는 고유한 값으로 중복될 수 없습니다.  
중복 키를 허용하려면 `std::multimap`을 사용해야 합니다.

균형 이진 트리를 사용합니다.

맵은 기본적으로 키를 오름차순으로 정렬한 상태에서 데이터가 저장됩니다.

맵은 효율적으로 데이터 검색, 삽입, 삭제가 가능합니다.
검색, 삽입, 삭제 작업은 $O(log \ n)$의 시간 복잡도를 가집니다.

다양한 데이터 타입을 지원하며, 사용자 정의 타입도 사용할 수 있습니다.

## 맵 초기화

키와 값의 쌍으로 구성된 `std::pair<Key, Value>` 형태입니다.

초기화 하는 방법은 다음과 같습니다.

```cpp
// 유니폼 초기화
std::map<std::string, int> map1 =
{
    {"Dog", 4},
    {"Cat", 2},
    {"Bird", 8}
}

// 개별적으로 추가
std::map<std::string, int> map2;

map2["Apple"] = 3;
map2["Banana"] = 5;

// insert 함수 사용
map2.insert(std::make_pair("Cherry", 2));
```

## 주요 연산자

### [ ]

키를 이용해 값을 삽입하거나 변경합니다.  
키가 없는 경우 새로 삽입됩니다.

```cpp
map1["Six"] = 6;
map1["Two"] = 2;
```

## 주요 함수

### insert

기본 객체를 복사하거나 이동해서 새로운 키-값 쌍을 삽입합니다.

키와 값은 복사 또는 이동 생성자가 필요합니다.

```cpp
map1.insert(std::make_pair("Grape", 1));
map1.insert(std::make_pair("Cherry", 2));

// 기존 맵을 다른 맵에 복사
map<string, int> copiedMap;
copiedMap.insert(map1.begin(), map1.end());
```

### emplace

`insert`함수와 비슷하지만 요소를 "직접 생성"하여 추가합니다.

복사나 이동 연산을 피하고 싶을 때 사용합니다.  
객체 생성 과정에서 성능 최적화가 중요한 경우 사용합니다.

`emplace`은 객체를 생성하는 데 필요한 정확한 인자를 전달해야 합니다.  
생성자가 없는 객체나 초기화 인자가 잘못된 경우 컴파일 에러가 발생할 수 있습니다.

### find

특정 키를 검색하고, 해당 키를 찾았다면 그 반복자를 반환합니다.  
찾지 못했다면 `end()`를 반환합니다.

```cpp
auto it = map1.find(2);
```

### count

특정 키가 맵에 존재하는지 확인합니다.  
반환 값은 0또는 1입니다.

```cpp
map.count(3);
```

### at

특정 키의 값을 반환합니다.  
키가 존재하지 않으면 예외(std::out_of_range)를 던집니다.

```cpp
map.at(3);
```

### erase

특정 키나 반복자 범위를 통해 요소를 삭제합니다.

```cpp
// Key 2 삭제
map.erase(2);
// 첫 번째 요소 삭제
map.erase(map.begin());
```

### clear

맵의 모든 요소를 삭제합니다.

```cpp
map.clear();
```

### empty

맵이 비었으면 `true`값이 반환됩니다.  
만약 비어있지 않으면 `false`가 반환됩니다.

```cpp
map.empty();
```

### size

현재 맵에 저장된 요소의 개수를 반환합니다.

```cpp
map.size();
```

### begin

맵의 첫 번째 요소를 가리키는 반복자를 반환합니다.  
반복자는 포인터처럼 작동합니다.

맵의 요소를 순회할 때 사용됩니다.

### end

맵의 마지막 요소를 가리키는 반복자를 반환합니다.

실제 요소를 가리키지 않고, 종료 조건으로 사용됩니다.

### swap

두 맵의 모든 요소를 서로 바꿉니다.

복사나 이동하는 개념이 아닌 각 맵의 내부 데이터 구조를 참조하는 방식으로 서로 교환하는 방법입니다.  
때문에 $O(1)$의 시간 복잡도를 가집니다.

두 객체가 같은 타입이어야 합니다.

```cpp
map1.swap(map2);
```