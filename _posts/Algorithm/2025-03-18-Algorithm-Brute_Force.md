---
layout: single

title: "[Algorithm] 브루트 포스(Brute Force)"

categories:
    - Algorithm
tag: [알고리즘]

date: 2025-03-18
last_modified_at: 2025-03-18

order : 1000
---

# 브루트 포스

브루트 포스(Brute Force)는 모든 경우의 수를 직접 탐색하여 답을 찾은 방법입니다.  
모든 경우를 고려하기 때문에 항상 정답을 찾을 수 있습니다.

완전 탐색(Exhaustive Search) 방법이라고도 불립니다.

알고리즘이 복잡하지 않으며, 구현하기 쉽습니다.  
하지만, 경우의 수가 많아지면 시간 복잡도가 커져서 비효율적일 수 있습니다.

다음과 같은 상황에서 사용합니다.

1. 데이터의 크기가 작을 때
2. 확실한 정답을 찾을 때
3. 더 효율적인 알고리즘을 찾기 어렵거나 구현하기 어려울 때

일반적으로 $O(N!)$, $O(2^N)$, $O(N^2)$, $O(N^3)$등의 시간 복잡도를 가집니다.

브루트포스를 개선하는 기법으로는 다음과 같은 기법 등이 있습니다.

1. 백트래킹, 가지치기
2. 메모이제이션
3. 그리디 알고리즘 및 분할 정복
4. 비트마스크

## 예시

중첩 반복문을 통해 모든 숫자를 나열하는 경우

```cpp
#include <iostream>

using namespace std;

int main()
{
    for (int i = 1; i <= 3; i++)
    {
        for (int j = 1; j <= 3; j++)
        {
            for (int k = 1; k <= 3; k++)
            {
                cout << i << " " << j << " " << k << endl;
            }
        }
    }
}
```

---

순열 탐색

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

int main()
{
    vector<int> nums = {1, 2, 3};

    do {
        for (int num : nums)
        {
            cout << num << " ";
        }

        cout << endl;

    } while (next_permutation(nums.begin(), nums.end()));
}
```