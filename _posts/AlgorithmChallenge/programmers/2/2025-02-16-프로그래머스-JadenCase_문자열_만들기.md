---
layout: single

title: "[프로그래머스][C++] JadenCase 문자열 만들기"

categories:
    - Programmers
tag: [프로그래머스]

date: 2025-02-16
last_modified_at: 2025-02-16

order : 12951
---

# JadenCase 문자열 만들기

## 문제 링크

[JadenCase 문자열 만들기](https://school.programmers.co.kr/learn/courses/30/lessons/12951){: target="_blank"}

## 분석

모든 단어의 첫 문자는 대문자, 나머지는 소문자로 변환합니다.  
다만 숫자로 시작하는 단어는 이후의 글자를 대문자로 변환하지 않습니다.

단어는 공백을 기준으로 분리되며, 공백을 유지해야합니다.

## 풀이

직접 대문자와 소문자를 변환하는 방법입니다.

```cpp
#include <string>

using namespace std;

string solution(string s) {
    // 단어의 시작 여부를 저장
    bool isCapital = true;
    
    // s순회
    for (int i = 0; i < s.length(); ++i)
    {   
        // 공백인경우
        if (s[i] == ' ')
        {
            // 다음 문자는 단어의 첫 글자
            isCapital = true;
        }
        // 공백이 아닌 경우
        else
        {
            // 대문자로 수정해야하는 경우
            if (isCapital == true)
            {
                // 소문자인 경우
                if ('a' <= s[i] && s[i] <= 'z')
                {
                    s[i] -= 'a' - 'A';
                }

                // 더이상 문자가 단어의 첫 글자가 아니므로 값 수정
                isCapital = false;
            }
            else
            {
                // 대문자인 경우
                if ('A' <= s[i] && s[i] <= 'Z')
                {
                    s[i] += 'a' - 'A';
                }
            }
        }
    }
    
    return s;
}
```

---

표준 라이브러리 함수를 사용한 방법입니다.

```cpp
#include <string>

using namespace std;

string solution(string s) {
    // 단어의 시작 여부를 저장
    bool isCapital = true;
    
    // s순회
    for (int i = 0; i < s.length(); ++i)
    {
        // 공백인경우
        if (s[i] == ' ')
        {
            // 다음 문자는 단어의 첫 글자
            isCapital = true;
        }
        // 공백이 아닌 경우
        else
        {
            // 대문자로 수정해야하는 경우
            if (isCapital == true)
            {
                // 글자를 대문자로
                s[i] = toupper(s[i]);

                // 더이상 문자가 단어의 첫 글자가 아니므로 값 수정
                isCapital = false;
            }
            else
            {
                // 글자를 소문자로
                s[i] = tolower(s[i]);
            }
        }
    }
    
    return s;
}
```

## 성능 요약

시간 복잡도는 $O(n)$입니다.

- `s`를 순회하는 반복문 $O(n)$
- 이외의 모든 연산은 $O(1)$
- $O(n) + O(1)$

공간 복잡도는 고정된 크기의 상수 공간을 사용하기 때문에 $O(1)$입니다.

- 매개변수 `s`를 직접 수정하고, 반환하므로 별도의 공간을 사용하지 않음

표준 라이브러리 함수를 사용한 방법과 그렇지 않은 방법 모두 시간 복잡도와 공간 복잡도는 같습니다.

테스트 1 〉 통과 (0.01ms, 4.22MB)  
테스트 2 〉 통과 (0.01ms, 4.17MB)  
테스트 3 〉 통과 (0.01ms, 4.25MB)  
테스트 4 〉 통과 (0.01ms, 4.14MB)  
테스트 5 〉 통과 (0.01ms, 4.14MB)  
테스트 6 〉 통과 (0.01ms, 4.21MB)  
테스트 7 〉 통과 (0.01ms, 4.28MB)  
테스트 8 〉 통과 (0.01ms, 4.14MB)  
테스트 9 〉 통과 (0.01ms, 4.15MB)  
테스트 10 〉 통과 (0.01ms, 4.21MB)  
테스트 11 〉 통과 (0.01ms, 3.67MB)  
테스트 12 〉 통과 (0.01ms, 4.18MB)  
테스트 13 〉 통과 (0.01ms, 4.2MB)  
테스트 14 〉 통과 (0.01ms, 4.07MB)  
테스트 15 〉 통과 (0.01ms, 4.21MB)  
테스트 16 〉 통과 (0.01ms, 4.14MB)  
테스트 17 〉 통과 (0.01ms, 4.45MB)  
테스트 18 〉 통과 (0.01ms, 4.22MB)  