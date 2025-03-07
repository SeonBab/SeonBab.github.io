---
layout: single

title: "[Algorithm] 선택 정렬"

categories:
    - Algorithm
tag: [알고리즘]

date: 2025-03-07
last_modified_at: 2025-03-07

order : 20
---

# 선택 정렬

선택 정렬(Selection Sort)은 가장 작은 원소를 찾아서 앞쪽부터 차례로 정렬하는 정렬 알고리즘입니다.

오름차순 정렬을 기준으로 다음과 같습니다.  
1. 주어진 배열을 끝까지 탐색해 가장 작은 값을 찾아 첫 번째 요소와 교환합니다.
2. 주어진 배열을 끝까지 탐색해 두 번째로 가장 작은 값을 찾아 두 번째 요소와 교환합니다.
3. 즉, 가장 작은 값을 찾아 특정 위치와 값을 교환합니다.

![Selection_Sort-Animation]({{site.url}}/images/Algorithm/2025-03-07-Selection_Sort/Selection_Sort-Animation.gif){: width="80" height="80"}

추가적인 메모리 공간을 사용하지 않는 제자리 정렬(In-place sorting)입니다.  
따라서 공간 복잡도는 $O(1)$입니다.

가장 작은 원소를 탐색하는데 $(n - 1) + (n - 2) + ... + 1 = \frac{n(n-1)}{2}$번의 비교가 필요하므로, 시간 복잡도는 항상 $O(n^2)$입니다.  
배열의 크기가 커질수록 시간 복잡도가 $O(n^2)$으로 증가하므로, 비효율적입니다.

동일한 값이 있을 경우 정렬 후 그 값들끼리 순서가 유지되지 않는 불안정 정렬(Unstable Sort)입니다.  
하지만, 구현 방법을 다르게 하여 안정 정렬(stable sort)이 되게 할 수도 있습니다.

버블 정렬과 효율성은 같지만 교환을 수행하는 횟수가 적기 때문에 교환 연산에 비용이 많이 드는 경우 선택 정렬이 더 빠를 수 있습니다.

예시 #1  
```cpp
#include <iostream>
#include <vector>
#include <climits>
#include <algorithm>

using namespace std;

int main()
{
    vector<int> vec{ 1, 5, 2, 7, 3, 4, 7, 3, 9, 8, 1, 2 };

    // 배열을 순회
    for (int i = 0; i < vec.size() - 1; ++i)
    {
        int minValue = INT_MAX; // 가장 작은 값
        int minValueIndex = INT_MAX;    // 가장 작은 값의 인덱스

        // 배열에 정리된 인덱스는 제외하고 순회
        for (int j = i + 1; j < vec.size(); ++j)
        {
            // 가장 작은 값인 경우
            if (minValue > vec[j])
            {
                minValue = vec[j];
                minValueIndex = j;
            }
        }

        // 값을 옮기는 연산
        swap(vec[i], vec[minValueIndex]);
    }

    // 출력
    for (int e : vec)
    {
        cout << e << " ";   // 1 1 2 2 3 3 4 5 7 7 8 9
    }
}
```