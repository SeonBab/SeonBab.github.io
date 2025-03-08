---
layout: single

title: "[Algorithm] 삽입 정렬"

categories:
    - Algorithm
tag: [알고리즘]

date: 2025-03-08
last_modified_at: 2025-03-08

order : 30
---

# 삽입 정렬

삽입 정렬(Insertion Sort)은 정렬된 부분과 비교하면서 적절한 위치에 삽입하는 정렬 알고리즘입니다.

오름차순 정렬을 기준으로 다음과 같습니다.  
1. 배열의 두 번째 원소부터 시작해 현재 원소의 앞(정렬된 부분)과 비교합니다.
2. 현재 원소보다 큰 원소들은 한 칸씩 뒤로 이동해 현재 원소가 들어갈 자리를 만들어줍니다.
3. 만들어진 자리에 현재 원소가 삽입됩니다.
4. 이 과정을 마지막 원소까지 반복합니다.

시간 복잡도는 최선 $\Omega(n)$, 평균 $\theta(n^2)$ 최악 $O(n^2)$입니다.

추가적인 메모리 공간을 사용하지 않는 제자리 정렬(In-place sorting)입니다.  
따라서 공간 복잡도는 $O(1)$입니다.

동일한 값이 있을 경우 정렬 후 그 값들끼리 순서가 유지되는 안정 정렬(stable sort)입니다.

## 예시

예시 #1
```cpp
#include <iostream>
#include <vector>

using namespace std;

int main()
{
    vector<int> vec{ 1, 5, 2, 7, 3, 4, 7, 3, 9, 8, 1, 2 };

    for (int i = 1; i < vec.size(); ++i)
    {
        int key = vec[i];

        int j = i - 1;
        while (j >= 0 && vec[j] > key)
        {
            vec[j + 1] = vec[j];
            --j;
        }

        vec[j + 1] = key;
    }

    for (int e : vec)
    {
        cout << e << " ";
    }
}
```