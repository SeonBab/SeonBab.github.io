---
layout: single

title: "[Algorithm] 비트마스크(Bitmask)"

categories:
    - Algorithm
tag: [알고리즘]

date: 2025-03-18
last_modified_at: 2025-03-18

order : 1010
---

# 비트마스크

비트마스크(Bitmask)는 정수의 이진수 표현을 활용하여 데이터를 효율적으로 저장하고 연산하는 기법입니다.

비트 연산을 사용합니다.

부분 집합을 표시하거나, 여러 개의 참/거짓 상태를 하나의 정수로 다룰 때 사용합니다.

비트마스크에서는 각 비트(0 또는 1)가 특정한 상태를 나타냅니다.

예를 들어, 4비트(0000)짜리 정수를 사용한다고 가정하면 다음과 같습니다.

+ 0001 → 첫 번째 상태 ON
+ 0010 → 두 번째 상태 ON
+ 0100 → 세 번째 상태 ON
+ 1000 → 네 번째 상태 ON

이렇게 하나의 정수로 여러 개의 상태를 나타낼 수 있습니다.

예를 들어, 0110인 경우 다음과 같습니다.

두 번째와 세 번째 상태가 ON(1)이고, 첫 번째와 네 번째 상태는 OFF(0)라는 뜻입니다.

## 예시

부분집합 생성

```cpp
#include <iostream>
#include <vector>

using namespace std;

int main()
{
    vector<int> arr = { 1, 2, 3 };
    int n = arr.size();

    // 1 << n은 2^n를 의미
    for (int i = 0; i < (1 << n); i++)
    {
        cout << "{ ";

        for (int j = 0; j < n; j++)
        {
            // 1 << j는 1을 왼쪽으로 j칸 이동한 값입니다.
            // 두 수의 비트를 비교해, 비트 자리수가 둘다 1인 경우
            if (i & (1 << j))
            {
                cout << arr[j] << " ";
            }
        }

        cout << endl;
    }
}
```