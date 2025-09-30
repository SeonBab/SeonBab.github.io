---
layout: single

title: "[수학] 스칼라"

categories:
    - Math
tag: [수학]

date: 2025-02-05
last_modified_at: 2025-09-30

order : 60
---

# 스칼라

스칼라(Scalar)는 방향을 가지지 않고, 크기만 있는 수를 의미합니다.  
주로 실수로 표현됩니다.

일반적으로 속도 크기나 질량을 나타냅니다.

## 스칼라 연산

+ 덧셈(+): $3 + 5 = 8$
+ 뺄셈(-): $8 - 5 = 3$
+ 곱셈($\times$): $2 \times 4 = 8$
+ 나눗셈($\div$): $10 \div 2 = 5$

만약 스칼라와 벡터를 곱하면 벡터의 크기만 변하고 방향은 그대로 유지됩니다.  
예를 들어, 벡터 $v = (2,3)$에 스칼라 2를 곱하면 $2 \times (2,3) = (4, 6)$입니다.

## 스칼라의 활용

2D 게임에서 스칼라는 속력이나 시간, 스케일링과 같은 물리적 개념에 자주 사용됩니다.  
예를 들어, 게임에서 캐릭터가 3.0m/s 속도로 2초 동안 움직였다면, 이동 거리 $d$는 다음과 같이 표현할 수 있습니다.

$d = speed \times time = 3.0 \times 2.0 = 6.0$

```cpp
double speed = 3.0;    // m/s
double time = 2.0;     // seconds
double distance = speed * time; // 6.0 meters
std::cout << "Distance: " << distance << " meters\n";
```