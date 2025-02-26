---
layout: single

title: "[기타 문제 C++] 정렬 문제"

categories:
    - AC_ETC
tag: [AC_ETC]

date: 2025-02-20
last_modified_at: 2025-02-20

order : 10000
---

# 정렬 문제

## 문제 1

이미 정렬된 두 정수형 배열 `arr1`과 `arr2`가 주어졌을 때, 두 배열에 공통으로 존재하는 원소만을 포함하는 배열을 반환하는 `solution()` 함수를 구현하시오.

제약조건  
- `arr1`과 `arr2`는 각각 길이가 1 이상 100,000 이하이다.
- `arr1`과 `arr2`는 오름차순으로 정렬되어 있다.
- 결과 배열도 오름차순으로 정렬되어 있어야 한다.

입출력 예시  
```
arr1 = [1, 2, 4, 5, 7]
arr2 = [2, 3, 5, 6]
결과: [2, 5]
```

풀이 #1
```cpp
#include <vector>
#include <unordered_set>

using namespace std;

vector<int> solution(vector<int> arr1, vector<int> arr2)
{
    vector<int> answer;

    unordered_set<int> set(arr1.begin(), arr1.end());

    for (int i = 0; i < arr2.size(); ++i)
    {
        if (set.find(arr2[i]) != set.end())
        {
            answer.push_back(arr2[i]);
        }
    }

    return answer;
}
```

풀이 #2  
투 포인터(two-pointer) 기법을 사용한 방법

```cpp
#include <vector>

using namespace std;

vector<int> solution(vector<int> arr1, vector<int> arr2)
{
    vector<int> intersection;
    int i = 0, j = 0;

    // 두 배열을 순회하며 공통 원소 찾기
    while (i < arr1.size() && j < arr2.size())
	{
        if (arr1[i] == arr2[j])
		{ // 같은 값이면 결과에 추가
            intersection.push_back(arr1[i]);
            i++; j++; // 두 포인터 모두 증가
        }
		else if (arr1[i] < arr2[j])
		{
            i++; // arr1[i]가 작다면 증가
        }
		else
		{
            j++; // arr2[j]가 작다면 증가
        }
    }
    return intersection;
}
```

## 문제 2

정수 `n`이 주어졌을 때, 각 자릿수를 오름차순으로 정렬하여 새로운 정수를 반환하는 `solution()` 함수를 구현하시오.

제약조건
- `n`은 1 이상 8,000,000,000 이하인 자연수이다.

입출력 예시
```
n = 873211
결과: 112378
```

풀이 #1
```cpp
#include <vector>
#include <algorithm>

using namespace std;

long long solution(long long n)
{
    long long answer = 0;

    vector<int> numList;

    while (n > 0)
    {
        numList.push_back(n % 10);
        n /= 10;
    }

    sort(numList.begin(), numList.end());

    for (int e : numList)
    {
        answer *= 10;
        answer += e;
    }

    return answer;
}
```

풀이 #2  
문자열을 사용한 방법

```cpp
#include <algorithm>
#include <string>

using namespace std;

long long solution(long long n)
{
    // 정수를 문자열로 변환
    string str = to_string(n);

    // 문자열을 오름차순 정렬 (작은 숫자부터 배치)
    sort(str.begin(), str.end());

    // 정렬된 문자열을 다시 정수로 변환하여 반환
    return stoll(str);
}
```

## 문제 3

문자열로 구성된 배열 `strings`와 문자 `c`가 주어졌을 때, `c`를 포함하는 문자열만 추려서 오름차순으로 정렬한 결과를 반환하는 `solution()` 함수를 구현하시오.

제약조건
- `strings`는 길이가 1 이상 50 이하인 벡터이다.
- 각 문자열은 소문자 알파벳으로 이루어져 있다.
- 각 문자열의 길이는 1 이상 100 이하이다.

입출력 예시
```
strings = ["apple", "banana", "cherry", "date"]
c = 'a'
결과: ["apple", "banana", "date"]
```

풀이 #1
```cpp
#include <vector>
#include <string>
#include <algorithm>

using namespace std;

vector<string> solution(vector<string> strings, char c)
{
    vector<string> answer;

    for (const string& e : strings)
    {
        if (e.find(c) != e.npos)
        {
            answer.push_back(e);
        }
    }

    sort(answer.begin(), answer.end());

    return answer;
}
```

## 문제 4

정수 배열 `arr`이 주어졌을 때, 연속된 숫자들이 가장 길게 이어지는 구간의 길이를 구하는 `solution()` 함수를 구현하시오.

제약조건
- `arr`의 길이는 1 이상 100,000 이하이다.
- `arr`의 각 원소는 1 이상 1,000,000 이하의 정수이다.

입출력 예시
```
arr = [100, 4, 200, 1, 3, 2]
결과: 4  (1, 2, 3, 4가 연속됨)
```

풀이 #1
```cpp
#include <vector>
#include <set>

using namespace std;

int solution(vector<int> arr)
{
    set<int> numSet(arr.begin(), arr.end()); // 중복 제거와 오름차순 정렬
    int answer = 0;

    // 연속된 숫자들의 길이를 확인
    for (int num : numSet)
    {
        // 연속된 수열의 시작점을 찾기 (num-1이 존재하지 않으면 시작점)
        if (numSet.find(num - 1) == numSet.end())
        {
            int currentNum = num;
            int currentStreak = 1;

            // 연속된 숫자가 있는 동안 계속 증가
            while (numSet.find(currentNum + 1) != numSet.end())
            {
                currentNum++;
                currentStreak++;
            }

            // 연속된 길이 갱신
            answer = max(answer, currentStreak);
        }
    }
    return answer;
}
```