---
layout: single

title: "[C++ STL] numeric"

categories:
    - Cpp
tag: [Cpp]

date: 2025-02-18
last_modified_at: 2025-02-18

order : 300000
---

# numeric

`#include <numeric>`로 사용할 수 있습니다.

수치 연산과 관련된 여러 유용한 함수를 제공합니다.  
누적 합산, 곱셈, 부분 합, 내부 곱, 최대공약수/최소공배수 등과 같은 다양한 수치 연산을 지원합니다.

반복적인 수치 연산을 단순화할 수 있습니다.

## partial_sum

`partial_sum`함수는 주어진 범위에서 부분합(누적 합)을 계산해줍니다.  
입력된 범위의 요소들을 순차적으로 더해가면서 각 단계의 결과를 출력 범위에 저장합니다.

```cpp
template <class InputIt, class OutputIt>
OutputIt partial_sum(InputIt first, InputIt last, OutputIt d_first);
template <class InputIt, class OutputIt, class BinaryOp>
OutputIt partial_sum(InputIt first, InputIt last, OutputIt d_first, BinaryOp op);
```

`first`와 `last`는 연산을 수행할 범위의 시작과 끝을 나타내는 반복자입니다.  
`d_first`는 결과를 저장할 출력 반복자입니다.  
`op`는 이항 연산자로 두 개의 인자를 받는 함수 또는 람다식입니다.

```cpp
#include <iostream>
#include <numeric>
#include <vector>

int main()
{
    std::vector<int> nums = {1, 2, 3, 4};
    std::vector<int> result(nums.size());

    std::partial_sum(nums.begin(), nums.end(), result.begin());

    std::cout << result[0] << std::endl; // 1
    std::cout << result[1] << std::endl; // 3 (1 + 2)
    std::cout << result[2] << std::endl; // 6 (1 + 2 + 3)
    std::cout << result[3] << std::endl; // 10 (1 + 2 + 3 + 4)

    std::cout << result[3] - result[1] << std::endl; // 7 (1 + 2 + 3 + 4 - 1 - 2)
}
```

## accumulate

`accumulate` 함수는 주어진 범위의 요소를 누적하여 합산하거나, 사용자가 지정한 연산을 사용해 누적 계산을 수행합니다.  
즉, 총합을 구하는데 사용됩니다.

```cpp
template <class InputIt, class T>
T accumulate(InputIt first, InputIt last, T init);
template <class InputIt, class T, class BinaryOp>
T accumulate(InputIt first, InputIt last, T init, BinaryOp op);
```

`first`와 `last`는 연산을 수행할 범위의 시작과 끝을 나타내는 반복자입니다.  
`d_first`는 초기값으로 연산의 시작점이며, 결과 값의 자료형을 결정하기도 합니다.  
`OutputIt`는 모든 요소를 누적한 결과로 반환합니다.  
`op`는 이항 연산자로 두 개의 인자를 받는 함수 또는 람다식입니다.

예시 #1

```cpp
#include <iostream>
#include <numeric>
#include <vector>

using namespace std;

int main()
{
    vector<int> v = { 1, 2, 3, 4 };

    int sum = accumulate(v.begin(), v.end(), 0);
    cout << sum << endl; // 10
}
```

예시 #2

```cpp
#include <iostream>
#include <numeric>
#include <vector>

using namespace std;

int main()
{
    vector<int> v = { 1, 2, 3, 4 };

    int product = accumulate(v.begin(), v.end(), 1, multiplies<int>());
    cout << product << endl; // 24
}
```

예시 #3

```cpp
#include <iostream>
#include <numeric>
#include <vector>

using namespace std;

int main()
{
    vector<int> v = { 1, 2, 3, 4, 5 };
    int sum = accumulate(v.begin(), v.end(), 0, [](int a, int b)
        {
            return a + b + 2;
        });

    cout << sum << endl; // 25
}
```