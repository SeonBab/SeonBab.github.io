---
layout: single

title: "[수학] 함수"

categories:
    - Math
tag: [수학]

date: 2025-02-13
last_modified_at: 2025-02-13

order : 100
---

# 함수

함수(Function)는 입력(Input)을 주었을 때, 일정한 규칙에 따라 출력(Output)을 결정하는 대응 관계를 의미합니다.

보통 $f(x)$형태로 표현하며, 이는 $x$를 입력하면 $f(x)$의 결과를 출력한다라는 뜻입니다.  
괄호 앞에 $f$는 함수 이름을 의미하며, 다른 글자를 사용해도 무방합니다.

예시  
$f(x) = 2x$ 라고 하면, $x = 3$,  $f(3) = 6$

## 정의역, 공역, 치역

정의역(Domain)은 함수가 적용될 수 있는 입력 값들의 집합을 의미합니다.

공역(Codomain)은 함수의 출력 값들이 포함될 수 있는 집합을 의미합니다.  
이때, 함수가 실제로 출력하는 값들과는 다를 수 있습니다.

치역(Range)는 함수가 실제로 만들어내는 출력 값들의 집합을 의미합니다.  
치역은 공역의 부분집합이 됩니다.

예시 #1  
함수 $f(x) = x^2$
정의역: $..., -3, -2, -1, 0, 1, 2, 3, ...$  
공역: 모든 실수 $\mathbb R$ 혹은 0 이상의 정수 $\mathbb N_0$ 혹은 $0, 1, 4, 9, ...$  
치역: $0, 1, 4, 9, ...$

예시 #2  
함수 $f(x) = \sin x$  
정의역: 모든 실수 $\mathbb R$  
공역: 모든 실수 $\mathbb R$ 혹은 $[-1, 1]$
치역: $-1 \leq y \leq 1$

## 단사, 전사, 전단사 함수

함수는 입력과 출력의 연결 방식에 따라 크게 세 가지로 분류됩니다.

단사 함수는 서로 다른 입력 값이 항상 서로 다른 출력 값을 가지는 함수를 의미합니다.  
즉, 하나의 입력 값이 하나의 출력값으로 매핑되며, 같은 출력값을 가지는 서로 다른 입력값이 없습니다.  
그래프에서 가로선과 한 점에서만 교차합니다.

예시 #1  
$f(x) = 2x + 3$는 서로 다른 $x$에 대해 항상 서로 다른 $f(x)$가 나오므로 단사입니다.

예시 #2  
$f(x) = x^2$는 $f(2)=4, f(−2)=4$ 으로 서로 다른 입력 값이 같은 출력 값을 갖기 때문에 단사 함수가 아닙니다.

---

전사 함수는 함수의 공역 내 모든 값이 적어도 하나의 정의역 원소와 연결된 함수를 의미합니다.  
즉, 모든 공역의 원소가 적어도 하나의 정의역 원소와 연결되어야 합니다.

예시 #1  
$f(x) = x^3$는 입력이 실수 전체일 때 출력도 실수 전체가 되므로 전사입니다.

예시 #2  
$f(x) = x^2$는 어떤 값을 넣더라도 치역은 $y \geq 0$인 값들만 출력되므로 전사 함수가 아닙니다.

---

전단사(Bijective) 함수는 단사 함수이면서 전사 함수인 함수를 의미합니다.  
함수의 모든 원소가 1:1 대응을 가집니다.  
이러한 전단사 함수는 역함수가 항상 존재합니다.

예시 #1  
$f(x) = x$

예시 #2  
입력이 실수일 때 $f(x) = x + 2$  
$x_1 \neq x_2 \Rightarrow f(x_1​) \neq f(x_2​)$로 단사입니다.  
모든 실수 $y$$에 대해 $y = x + 2$를 만족하는 $x$가 존재하므로 전사입니다.  
그러므로 전단사입니다.

## 역함수

함수 $f(x)$의 역함수 $f^{-1}(x)$는 원래 함수의 출력을 입력으로 받아 원래의 입력을 반환하는 함수를 의미합니다.

$f^{-1}(f(x)) = x$

원래 함수가 전단사 함수일 때 존재합니다.  
단사 함수가 아니면, 하나의 출력이 여러 개의 입력을 가지므로 역함수를 정의할 수 없습니다.  
전사 함수가 아니면, 공역의 일부 값에서 역함수가 존재하지 않게 됩니다.

역함수를 구하는 방법은 아래와 같습니다.

1. $y=f(x)$로 놓기
2. $x$에 대해 정리하기
3. $x$를 $y$로 바꿔서 $f^{-1}(x)$를 표현

예시  
$f(x) = 2x + 3$의 역함수

1. $y = 2x + 3$
2. $x = \frac{y - 3}{2}$
3. $f^{-1}(x) = \frac{x - 3}{2}$

## 선형성

선형성(Linearity)이란 어떤 함수 $f(x)$가 가산성과 1차 동차성을 모두 만족한 경우 선형성을 갖는다고 합니다.

예시 #1  
수학적 선형함수  
$f(x) = a \times x$

예시 #2  
완전 선형 함수  
$f(x) = 2x$  
$f(x + y) = 2(x + y) = 2x + 2y = f(x) + f(y)$  
$f(a \cdot x) = 2(ax) = a \cdot 2x = a \cdot f(x)$

선형성이 게임에서 적용되는 부분은 다양하게 있을 수 있고, 대표적으로 선형 보간(Lerp: Linear Interpolation)이 있습니다.  
선형 보간은 두 점 사이의 값을 특정 비율(t)에 따라 계산하는 기법을 의미하며, 이를 통해 A에서 B로 이동하는 경로를 부드럽게 만들 수 있습니다.

예를 들어, 2D 공간에서 두 점 $(x_0, y_0)$과 $(x_1, y_1)$이 있다고 가정하면, 보간된 점 $(x, y)$는 다음과 같이 계산됩니다.

$x(t) = (1 - t) x_0 + t x_1$  
$y(t) = (1 - t) y_0 + t y_1$

여기서 $t$는 보간 비율이며, $0 \leq t \leq 1$의 범위를 가집니다.

실제 예시로 구해보자면, 두 점 $(x_0, y_0)$, $(x_1, y_1)$의 보간된 점은 $(5.0, 2.5)$입니다.

- $(x_0, y_0) = (0, 0) = (1 − 0.5) \cdot 0 + 0.5 \cdot 10 = 5$
- $(x_1, y_1) = (10, 5) = (1 − 0.5) \cdot 0 + 0.5 \cdot 5 = 2.5$
- $t = 0.5$

```cpp
#include <iostream>

struct Vector2 {
    float x, y;

    // 2D 선형 보간 함수
    Vector2 LerpTo(const Vector2& target, float t) const {
        return {
            (1 - t) * x + t * target.x,
            (1 - t) * y + t * target.y
        };
    }

    void Print() const {
        std::cout << "Position: (" << x << ", " << y << ")\n";
    }
};

int main() {
    Vector2 start = {0.0f, 0.0f};  // 시작점
    Vector2 end = {10.0f, 5.0f};   // 끝점

    for (float t = 0.0f; t <= 1.0f; t += 0.2f) {
        Vector2 interpolated = start.LerpTo(end, t);
        interpolated.Print();
    }

    return 0;
}
```

```cpp
Position: (0.0, 0.0)
Position: (2.0, 1.0)
Position: (4.0, 2.0)
Position: (6.0, 3.0)
Position: (8.0, 4.0)
Position: (10.0, 5.0)
```

보간 함수 $L(t, v_0, v_1)$는 다음과 같이 일반화 할 수 있습니다.

$L(t, v_0, v_1) = (1 - t) v_0 + t v_1$

가산성 증명  
$L(t,v_0 + v_0′, v_1 + v_1′) = (1 − t)(v_0 + v_0′) + t(v_1 + v_1′)$  
$L(t, v_0, v_1) + L(t, v_0', v_1')= (1 - t)v_0 + t v_1 + (1 - t)v_0' + t v_1'$

1차 동차성 증명  
$L(t, αv_0, αv_1) = (1 − t)(αv_0) + t(αv_1)$  
$= \alpha ((1 - t)y_0 + t y_1 )$  
$= \alpha L(t, y_0, y_1)$

### 가산성

가산성(Additivity)이란 두 입력 $x$, $y$를 더하여 입력한 것에 대한 출력이, $x$, $y$ 각각에 대한 출력을 합한 것과 동일한 경우를 의미합니다.

$f(x + y) = f(x) + f(y)$

### 1차 동차성

1차 동차성(Homogeneity) 어떤 함수의 입력을 $a$배 했을 때의 출력이 원래 입력의 출력을 $a$배한 것과 동일한 경우를 의미합니다.  
즉, 임의의 실수(혹은 벡터) $x$, $y$와 스칼라(상수) $a$에 대하여 위 두 조건을 만족하는 함수를 선형 함수라고 부릅니다.

$f(ax) = af(x)$

### 일차함수

일차함수(Affine Function)는 보통 수학적 정의의 완전한 선형성(strict linearity)을 만족하지 않습니다.  

예시 #1  
$f(x) = a \times x + b$, ($b$가 $0$이 아닐 수도 있음)  
$b \neq 0$인 경우, $f(0) \neq  0$ 이므로 1차 동차성이 깨지기 때문입니다.

예시 #2  
$f(x) = 2x + 5$  
$f(x + y) = 2(x + y) + 5 = 2x + 2y + 5 \neq f(x) + f(y) = (2x + 5) + (2y + 5) = 2x + 2y + 10$

## 함수의 활용

게임에서 우리는 함수(메서드)를 정의합니다.  

함수(메서드)를 올바르게 작성하는 방법은 예상 가능한 정의역과 치역을 정의하고, 예상대로 동작하게 만드는 것입니다.  
예를 들어, 캐릭터의 체력(HP)은 0 이상이어야 하므로 함수의 정의역을 $\geq 0$로 설정해야 합니다.

```cpp
#include <iostream>
#include <algorithm> // std::max 사용

float ClampHealth(float health) {
    return std::max(0.0f, health); // 체력이 음수가 되지 않도록 제한
}

int main() {
    float playerHealth = -10.0f;
    playerHealth = ClampHealth(playerHealth);

    std::cout << "Player Health: " << playerHealth << "\n"; // 0 출력
    return 0;
}
```

위의 함수를 보면 체력이 음수로 내려가는 걸 방지하고 있습니다.

앞서 배운 개념과 연관지어 설명해보자면, 치역을 제한하여 특정 값이 논리적으로 유효한 범위 안에서만 동작하도록 보장한 것입니다.