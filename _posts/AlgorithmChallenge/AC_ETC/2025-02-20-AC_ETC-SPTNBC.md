---
layout: single

title: "[기타 문제 C++] 특정 범위의 원소 개수 출력"

categories:
    - AC_ETC
tag: [AC_ETC]

date: 2025-02-20
last_modified_at: 2025-02-20

order : 10000
---

# 문제

N개의 정수로 이루어진 정렬된 수열이 주어집니다.  
이어서 Q개의 쿼리가 주어지는데, 각 쿼리는 두 정수 A, B로 이루어져 있습니다.

각 쿼리마다 수열에서 A 이상 B 이하인 원소의 개수를 출력하세요.

## 제한사항

첫째 줄에 수열의 크기 N(1 ≤ N ≤ 100,000)이 주어집니다.  
둘째 줄에 N개의 정수가 오름차순으로 주어집니다.  
셋째 줄에 쿼리의 개수 Q(1 ≤ Q ≤ 10,000)가 주어집니다.  
다음 Q개의 줄에 각각 두 정수 A, B가 주어집니다.

## 예제 입력

예제 입력

```
10
1 2 3 3 3 6 7 8 9 9
3
3 5
2 3
6 9
```  

예제 출력

```
3 // 3 5 입력 시
4 // 2 3 입력 시
5 // 6 9 입력 시
```

## 분석

수열은 최소한 1개 이상이 주어집니다.

`N`개의 정수는 오름차순으로 주어집니다.

## 풀이

```cpp
#include <iostream>
#include <algorithm>
#include <vector>

using namespace std;

int main()
{
	int n;
	std::cin >> n;

	vector<int> vec;

	for (int i = 0; i < n; ++i)
	{
		int inputNum;
		std::cin >> inputNum;

		vec.push_back(inputNum);
	}

	int q;
	std::cin >> q;

	for (int i = 0; i < q; ++i)
	{
		int A, B;
		std::cin >> A >> B;

		auto first = lower_bound(vec.begin(), vec.end(), A);
		auto last = upper_bound(vec.begin(), vec.end(), B);

		cout << distance(first, last) << endl;
	}
}
```

## 성능 요약

시간 복잡도는 $O(q log n)$입니다.

- `n`번의 입력을 받아 저장하는 과정 $O(q)$
- 각 쿼리에서 범위를 찾는 과정 $O(log n)$
- $O(q log n)$

공간 복잡도는 입력을 제외하고, 고정된 크기의 상수 공간을 사용하기 때문에 $O(1)$입니다.