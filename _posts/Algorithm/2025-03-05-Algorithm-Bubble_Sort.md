---
layout: single

title: "[Algorithm] 버블 정렬"

categories:
    - Algorithm
tag: [알고리즘]

date: 2025-03-05
last_modified_at: 2025-03-05

order : 10
---

# 버블 정렬

버블 정렬(Bubble Sort)은 인접한 두 개의 요소를 비교하여 정렬하는 정렬 알고리즘입니다.  
각 반복에서 가장 큰(혹은 작은)값이 끝으로 이동하는 방식으로 동작합니다.

오름차순 정렬을 예시로 다음과 같습니다.  
1. 배열의 첫 번째 원소부터 시작하여 인접한 두 개의 원소를 비교합니다.
2. 만약 두 원소의 순서가 맞지 않다면 교환합니다.
3. 이 과정을 배열 끝까지 반복하면, 가장 큰 원소가 배열의 마지막 위치로 이동합니다.
4. 다시 처음부터 비교를 시작하지만, 정렬된 마지막 요소는 제외하고 비교합니다.
5. 이 과정을 배열이 정렬될 때까지 반복합니다.

![Bubble_Sort-example]({{site.url}}/images/Algorithm/2025-03-05-Bubble_Sort/Bubble_Sort-example.gif)

처음에는 $n - 1$번 비교하고, 다음은 $n - 2$번 비교하며, 마지막에는 1번 비교합니다.  
따라서 비교 횟수는 $(n - 1) + (n - 2) + ... + 1 = n(n-1)/2$ 이므로, 시간 복잡도는 최선 $\Omega(n)$, 평균 $\theta(n^2)$ 최악 $O(n^2)$입니다.

구현이 간단하며, 추가적인 메모리 공간을 사용하지 않는 제자리 정렬(In-place sorting)입니다.

배열의 크기가 커질수록 시간 복잡도가 $O(n^2)$으로 증가하므로, 비효율적입니다.

예시 #1

```cpp
#include <iostream>
#include <vector>

using namespace std;

int main()
{
    vector<int> vec{ 1, 5, 2, 7, 3, 4, 7, 3, 9, 8, 1, 2 };

    for (int i = 0; i < vec.size(); ++i)
    {
        for (int j = 0; j < vec.size() - 1 - i; ++j)
        {
            if (vec[j] > vec[j + 1])
            {
                int temp = vec[j];
                vec[j] = vec[j + 1];
                vec[j + 1] = temp;
            }
        }
    }

    for (int e : vec)
    {
        cout << e << " ";   // 1 1 2 2 3 3 4 5 7 7 8 9
    }
}
```