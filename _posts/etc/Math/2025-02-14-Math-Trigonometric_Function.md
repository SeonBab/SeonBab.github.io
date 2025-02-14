---
layout: single

title: "[수학] 삼각함수"

categories:
    - Math
tag: [수학]

date: 2025-02-14
last_modified_at: 2025-02-14

order : 110
---

# 삼각함수

삼각함수(Trigonometric Function)는 직각삼각형을 기반으로 정의됩니다. 

직각삼각형에는 세 변이 있으며, 특정한 각도를 기준으로 다음과 같이 명명됩니다.

- 빗변(Hypotenuse): 직각삼각형에서 가장 긴 변(직각을 이루는 두 변이 아닌 나머지 변)
- 밑변(Adjacent): 기준 각도에 인접한 변
- 높이(Opposite): 기준 각도에 마주보는 변

이러한 세 변의 비율을 통해 삼각비(삼각함수)를 정의합니다.  
삼각비는 삼각형을 기반으로 하지만, 이를 모든 실수로 확장하여 정의할 수도 있습니다.  
이 개념을 이용하면 삼각함수가 정의됩니다.

![Trigonometric_Function-Trigonometric_Function]({{site.url}}/images/etc/Math/2025-02-14-Math-Trigonometric_Function/Trigonometric_Function-Trigonometric_Function.PNG)

$\cos^2{\theta} + \sin^2{\theta} = 1$

$\tan{\theta} = \frac{\sin{\theta}}{\cos{\theta}}$

$\cos(A + B) = \cos A \cos B - \sin A \sin B$

$\sin(A + B) = \sin A \cos B + \cos A \sin B$

## 정의역과 공역

정의역: 모든 실수 $\mathbb{R}$

$sin$, $cos$의 공역: $[−1,1]$

![](https://upload.wikimedia.org/wikipedia/commons/thumb/3/38/Sine_cosine_plot.svg/800px-Sine_cosine_plot.svg.png)  
<cite>이미지 출처 위키피디아</cite>
{: .small}

$tan$의 공역: $\mathbb{R}$  
(단, 특정 값에서 정의되지 않음 $\theta = \frac{\pi}{2} + k\pi$)

![](https://upload.wikimedia.org/wikipedia/commons/thumb/d/d3/Tangent.svg/738px-Tangent.svg.png)  
<cite>이미지 출처 위키피디아</cite>
{: .small}

## 단위 원

삼각비를 직관적으로 이해하기 위해, 중심이 원점(0, 0)이고 반지름이 1인 단위 원(Unit Circle)을 사용합니다.

단위 원의 방정식은 $x^2 + y^2 = 1$입니다.  
해당 방정식을 기준으로 반지름이 1인 원에서 임의의 점 $(x, y)$는 $x=cosθ$, $y=sinθ$로 나타낼 수 있습니다.

즉, 각도를 기준으로 하면 $cosθ$은 $x$좌표, $sinθ$은 $y$좌표와 동일합니다.

## 원의 반지름과 좌표 계산

반지름 $r$인 원에서 점의 좌표는 $(x,y)=(r cosθ,r sinθ)$와 같이 표현됩니다.

이는 삼각비가 벡터와 연결될 수 있음을 의미합니다.

## 각도법

각도법(Degree)은 원을 360개의 동일한 부분으로 나누고, 이를 기준으로 각을 측정하는 방식입니다.  
각의 단위로 $°$(degree, 도)를 사용합니다.

고대 바빌로니아인들은 1년을 약 360일로 계산했고, 이 때문에 원을 360도로 나누는 개념이 생겼습니다.  
360은 약수가 많아 계산하기 편리합니다.

예시  
직각: $90^\circ$  
반원: $180^\circ$  
원 전체: $360^\circ$

## 호도법

호도법(Radian)은 원의 반지름을 이용해서 각도를 측정하는 방법입니다.  
원의 반지름과 같은 길이의 원호가 만드는 중심각을 1 라디안(rad)이라고 정의합니다.

원의 반지름과 같은 길이의 원호가 만드는 각을 1 라디안으로 정의하면, 반원의 중심각은 $\pi$이며, 이는 $180^\circ$에 해당합니다.

각도법은 직관적으로 이해하기가 쉽지만, 호도법은 수학적 계산에서 훨씬 편리합니다.  
미적분, 물리학, 공학에서는 대부분 호도법을 사용합니다.

다음과 같은 관계식을 가집니다.

$180^\circ = \pi \ rad $  
$1^\circ = \frac{\pi}{180} \ rad $  
$1 \ rad = \frac{180^\circ}{\pi}$

## 삼각함수의 활용

삼각함수는 게임에서 캐릭터 이동, 회전, 충돌 감지 등 다양한 곳에서 활용됩니다.

대부분의 수학적 계산을 언리얼 엔진이 제공하는 기능을 활용하여 처리할 수 있습니다.  
하지만 아래와 같은 경우 수학적인 이해를 바탕으로 직접 기능을 구현해야할 수도 있습니다.

1. 복잡한 물리 엔진을 직접 만들 때
    - 언리얼의 `Physics` 시스템을 사용하지 않고, 직접 구현해야 하는 경우

2. 커스텀 수학적 계산이 필요한 특수한 시스템
    - AI가 특정 각도로 시야를 계산하는 경우, 삼각함수를 직접 사용해야 할 수도 있음

3. 성능 최적화가 필요할 때
    - 기본 제공 함수가 너무 느리거나, 불필요한 연산이 많다면 직접 최적화된 수식을 적용할 수 있음

4. 언리얼 기본 기능이 제공하지 않는 특별한 기능이 필요할 때
    - 언리얼이 지원하지 않는 특정한 비선형 궤적(곡선 이동)을 구현할 때