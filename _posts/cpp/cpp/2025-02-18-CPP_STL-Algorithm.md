---
layout: single

title: "[C++ STL] algorithm"

categories:
    - Cpp
tag: [Cpp]

date: 2025-02-18
last_modified_at: 2025-02-18

order : 200000
---

# 알고리즘

`#include <algorithm>`로 사용할 수 있습니다.

표준 라이브러리에서 제공하는 다양한 알고리즘 함수를 포함하고 있습니다.  
정렬, 탐색, 변환, 수치 연산 등의 기능을 제공합니다.

반복적인 알고리즘을 직접 구현할 필요 없이 표준화된 함수를 사용해 코드의 가독성과 효율성을 높일 수 있습니다.  
코딩 테스트나 알고리즘 문제를 풀다 보면 모든 코드를 구현하기에 시간이 부족할 수 있습니다.  
해당 헤더에서 제공하는 기능들을 사용해서 시간을 효율적으로 사용하는 것이 좋습니다.

## next_permutation & prev_permutation

`next_permutation`함수와 `prev_permutation`함수는 순열 생성 함수입니다.  
해당 함수를 사용하기 위해서는 원본 데이터가 정렬된 상태여야 합니다.

한 번 호출될 때 $O(n)$, 모든 순열을 생성하려면 $O(n!)$의 시간 복잡도를 가집니다.

`next_permutation`함수는 주어진 범위를 사전순(lexicographical)으로 다음 순열로 바꿔줍니다.  
`prev_permutation`함수는 주어진 범위를 사전순으로 이전 순열로 바꿔줍니다.

```cpp
bool std::next_permutation(BidirectionalIterator first, BidirectionalIterator last);
bool std::prev_permutation(BidirectionalIterator first, BidirectionalIterator last);
```

`first`는 순열의 시작 반복자, `last`는 순열의 끝 반복자입니다.  
`next_permutation`함수는 다음 순열이 존재한다면 `true`, 마지막 순열이었다면 `false`를 반환하고, 배열을 처음 순열(오름차순)로 변경합니다.  
`prev_permutation`함수는 이전 순열이 존재한다면 `true`, 첫 번째 순열이었다면 `false`를 반환하고, 배열의 마지막 순열(내림차순)로 변경합니다.

예시 #1

```cpp
#include <iostream>
#include <algorithm>
#include <vector>

using namespace std;

int main()
{
    vector<int> vec = {1, 2, 3};

    do
    {
        for (int x : vec)
        {
            cout << x << " ";
        }
        cout << endl;
    } while (next_permutation(vec.begin(), vec.end())); // 다음 순열이 존재하는 한 계속 탐색
}
```

```
1 2 3
1 3 2
2 1 3
2 3 1
3 1 2
3 2 1
```

예시 #2  
위의 순열을 거꾸로 출력한다면 다음과 같습니다.

```cpp
#include <iostream>
#include <algorithm>
#include <vector>

using namespace std;

int main()
{
	vector<int> vec = { 1, 2, 3 };

	sort(vec.begin(), vec.end(), greater<int>());	// {3, 2, 1}로 시작

	do
	{
		for (int e : vec)
		{
			cout << e << " ";
		}

		cout << endl;
	} while (prev_permutation(vec.begin(), vec.end()));
}
```

출력은 다음과 같습니다.

```
3 2 1
3 1 2
2 3 1
2 1 3
1 3 2
1 2 3
```

예시 #3  
배열 {1, 2, 3, 4}에서 길이가 3인 순열을 모두 출력합니다.  
단, 순열의 첫 번째 원소는 항상 1이어야 합니다.

```cpp
#include <iostream>
#include <algorithm>
#include <vector>

using namespace std;

int main()
{
	vector<int> vec = { 1, 2, 3, 4 };

	do
	{
		for (int e : vec)
		{
			cout << e << " ";
		}

		cout << endl;
	} while (next_permutation(vec.begin() + 1, vec.end()));
}
```

출력은 다음과 같습니다.

```
1 2 3 4
1 2 4 3
1 3 2 4
1 3 4 2
1 4 2 3
1 4 3 2
```

## nth_element

`nth_element`함수는 주어진 범위에서 `n`번째 원소를 찾고, 이를 기준으로 왼쪽은 `n`번째보다 작은 원소들, 오른쪽은 `n`번째보다 큰 원소들로 부분 정렬을 수행합니다.

평균 적으로 $O(n)$, 최악의 경우 $O(n^2)$의 시간 복잡도를 가집니다.

```cpp
void std::nth_element(RandomIt first, RandomIt nth, RandomIt last);
void std::nth_element(RandomIt first, RandomIt nth, RandomIt last, Compare comp);
```

`first`는 정렬 범위의 시작 반복자, `last`는 정렬 범위의 끝 반복자입니다.  
`nth`는 `nth`번째 위치로 0-based index입니다.  
`comp`는 비교 함수로 기본값은 오름차순(<)입니다.

```cpp
#include <iostream>
#include <algorithm>
#include <vector>

using namespace std;

int main()
{
	vector<int> vec = { 15, 3, 9, 8, 5, 2, 10, 7, 6 };
	vector<int> coutVec;

	for (int e : vec)
	{
        // 짝수만
		if (e % 2 == 0)
		{
			coutVec.push_back(e);
		}
	}

	nth_element(coutVec.begin(), coutVec.begin() + 2, coutVec.end());

	cout << coutVec[2] << endl; // 8
}
```

## unique

`unique`함수는 연속된 중복 요소를 제거한 끝 위치를 반환하는 함수입니다.  
중복되지 않은 요소는 컨테이너의 앞부분으로 이동시키고, 중복된 요소는 삭제하지 않고 뒤쪽에 위치하게 합니다.  
중복을 제거한 후 실제 컨테이너의 크기를 변경하려면 `erase`함수를 사용해주어야 합니다.

해당 함수를 사용하기 위해서는 원본 데이터가 정렬된 상태여야 합니다.

```cpp
template <class ForwardIt>
ForwardIt unique(ForwardIt first, ForwardIt last);
template <class ForwardIt, class BinaryPred>
ForwardIt unique(ForwardIt first, ForwardIt last, BinaryPred p);
```

`first`와 `last`는 연산을 수행할 범위의 시작과 끝을 나타내는 반복자입니다.  
`ForwardIt`는 중복 제거 후 유효한 범위의 끝(iterator)을 반환합니다.  
`BinaryPred`는 `true`를 반환하면 두 요소가 중복된 것으로 간주됩니다.

예시 #1

```cpp
#include <algorithm>
#include <vector>

int main()
{
    std::vector<int> v = { 1, 4, 4, 2, 2, 2, 5, 1 };
    std::sort(v.begin(), v.end()); // 정렬 {1, 1, 2, 2, 2, 4, 4, 5}

    auto newEnd = std::unique(v.begin(), v.end()); // {1, 2, 4, 5, ?, ?, ?, ?}
    v.erase(newEnd, v.end()); // 중복되는 구간 삭제
}
```

## lower_bound & upper_bound

`lower_bound`함수는 특정 값 이상의 값이 처음 나오는 위치를 찾습니다.  
`upper_bound`함수는 특정 값 초과의 값이 처음 나오는 위치를 찾습니다.

이진 탐색을 사용합니다.

시간 복잡도는 $O(log n)$을 가집니다.

```cpp
template <class ForwardIt, class T>
ForwardIt lower_bound(ForwardIt first, ForwardIt last, const T& value);
template <class ForwardIt, class T>
ForwardIt upper_bound(ForwardIt first, ForwardIt last, const T& value);
```

`first`와 `last`는 연산을 수행할 범위의 시작과 끝을 나타내는 반복자입니다.  
`value`는 비교하려는 값입니다.  
반환값인 `ForwardIt`는 특정 값 이상 혹은 초과의 값이 처음 나오는 위치를 찾지만, 만약 찾지 못하는 경우 `last`를 반환합니다.

예시 #1

```cpp
#include <iostream>
#include <algorithm>
#include <vector>

int main()
{
    std::vector<int> v = { 1, 2, 4, 4, 5, 7, 8 };

    auto lb = std::lower_bound(v.begin(), v.end(), 4);
    auto ub = std::upper_bound(v.begin(), v.end(), 4);

    std::cout << lb - v.begin() << std::endl; // 2
    std::cout << ub - v.begin() << std::endl; // 4
}
```

예시 #2  
특정 값을 가진 원소의 개수를 구하는 방법입니다.

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

int main()
{
    std::vector<int> vec = { 1, 3, 3, 3, 5, 7, 9 };

    int x = 3;
    int count = std::upper_bound(vec.begin(), vec.end(), x) - std::lower_bound(vec.begin(), vec.end(), x);

    std::cout << count << std::endl; // 3
}
```

## transform

`transform`함수는 특정 구간 내의 원소들을 원하는 대로 변환시켜줍니다.

```cpp
// 단항
template <class InputIt, class OutputIt, class UnaryOp>
OutputIt transform(InputIt first1, InputIt last1, OutputIt d_first, UnaryOp unary_op);
```

`first1`와 `last1`은 연산을 수행할 범위의 시작과 끝을 나타내는 반복자입니다.  
`d_first`은 연산 결과를 저장할 컨테이너의 시작 반복자입니다.  
`unary_op`는 단일 인자를 받는 함수 또는 람다식입니다.

```cpp
// 이항
template <class InputIt1, class InputIt2, class OutputIt, class BinaryOp>
OutputIt transform(InputIt1 first1, InputIt1 last1, InputIt2 first2, OutputIt d_first, BinaryOp binary_op);
```

`first1`와 `last1`은 연산을 수행할 범위의 시작과 끝을 나타내는 반복자입니다.  
`first2`는 `first1`과 함께 연산될 입력입니다.  
`d_first`은 연산 결과를 저장할 컨테이너의 시작 반복자입니다.  
`binary_op`는 두 개의 인자를 받는 함수 또는 람다식입니다.

예시 #1  
단항 연산을 하는 방법입니다.  

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
#include <cmath> // pow 함수

using namespace std;

int main()
{
    vector<int> v1 = { 1, 2, 3, 4, 5 };
    vector<int> v2(5); // v1과 같은 크기

    transform(v1.begin(), v1.end(), v2.begin(), [](int x)
    {
        return pow(x, 2); // 각 원소를 제곱
    });

    for(int x : v2) cout << x << " "; // 1 4 9 16 25
}
```

예시 #2  
이항 연산을 하는 방법입니다.

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
#include <numeric> // plus<int>

using namespace std;

int main() {
    vector<int> v1 = { 1, 2, 3, 4, 5 };
    vector<int> v2 = { 6, 7, 8, 9, 10 };
    vector<int> v3(5);

    transform(v1.begin(), v1.end(), v2.begin(), v3.begin(), plus<int>());

    for (int x : v3) cout << x << " "; // 7 9 11 13 15
}
```