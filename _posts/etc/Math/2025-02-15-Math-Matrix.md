---
layout: single

title: "[수학] 행렬"

categories:
    - Math
tag: [수학]

date: 2025-02-15
last_modified_at: 2025-02-15

order : 150
---

# 행렬

행렬(Matrix)은 숫자나 기호를 직사각형 형태로 배열한 수학적 개체입니다. 

행렬을 사용하면 복잡한 변환을 간단한 연산으로 수행할 수 있으며, 여러 개의 변환을 하나의 행렬로 합쳐서 최적화할 수 있습니다.

게임에서 컴퓨터 그래픽스, 물리 시뮬레이션(객체의 이동, 회전, 크기 변환) 등에서 중요하게 사용됩니다.
이외에서도 선형대수학, 기계 학습(딥러닝), 공학 및 경제학 등에서 사용됩니다.

## 변환

이동(Translation), 회전(Rotation), 크기 조절(Scaling), 반사(Reflection), 투영(Projection) 등의 변환을 단순한 행렬 연산으로 표현할 수 있습니다.

## 이동 변환

2D 공간에서  $(x, y)$ 좌표를 이동할 경우 이동 변환 공식은 다음과 같습니다.  
$(x\prime, y\prime) = (x + dx, y + dy)$

이를 이동 변환 행렬로 표현하면 다음과 같이 나타낼 수 있습니다.

$$
T =
\begin{bmatrix}
    1 & 0 & dx \newline 
    0 & 1 & dy \newline
    0 & 0 & 1
\end{bmatrix}
$$

이 행렬을 점 $(x, y)$에 적용하면 새로운 위치 $(x\prime, y\prime)$를 구할 수 있습니다.

$$
\begin{bmatrix}
    x\prime \newline 
    y\prime \newline
    1
\end{bmatrix}

=

T

\times

\begin{bmatrix}
    x \newline 
    y \newline
    1
\end{bmatrix}
$$

2D를 표현하는데 $3 \times 3$행렬을 사용한 이유는 행렬 곱셉을 덧셈으로 표현하기 위한 방법이기 때문에 그렇습니다.  
이동 변환 공식과 같이 특정 좌표를 이동시키는 것이기 때문에 기존의 좌표에 값을 더하는 방식입니다.  
행렬 곱셈은 곱셈 연산만 지원하고 덧셈을 직접 표현할 수 없으므로 좌표를 한 차원 증가시켜 연산을 수행하게 됩니다.  
이를 동차 좌표계(Homogeneous Coordinates)라고 합니다.

기존 좌표계가 $(x, y)$일 경우 동차 좌표계는 $(x, y, 1)$입니다.

결과적으로 다음과 같은 계산 과정을 가지게 됩니다.

$$
\begin{bmatrix}
    1 & 0 & dx \newline 
    0 & 1 & dy \newline
    0 & 0 & 1 
\end{bmatrix}

\times

\begin{bmatrix}
    x \newline 
    y \newline
    1 
\end{bmatrix}

=

\begin{bmatrix}
    (1 \cdot x) + (0 \cdot y) + (dx \cdot 1) \newline
    (0 \cdot x) + (1 \cdot y) + (dy \cdot 1) \newline
    (0 \cdot x) + (0 \cdot y) + (1 \cdot 1) 
\end{bmatrix}
$$

이를 정리하면 다음과 같습니다.

$$
\begin{bmatrix}
    x + dx \newline
    y + dx \newline
    1 
\end{bmatrix}
$$

각 항목을 개별적으로 보면 다음과 같습니다.

$x\prime = (1 \cdot x) + (0 \cdot y) + dx = x + dx$  
$y\prime = (0 \cdot x) + (1 \cdot y) + dy = y + dy$  
$1 = (0 \cdot x) + (0 \cdot y) + (1 \cdot 1) = 1$

예시  
점 $P(3, 4)$를 $(dx = 2, dy = 5)$만큼 이동하는 것을 행렬로 표현하면 다음과 같습니다.

초기 좌표(동차 좌표계 적용)

$$
P =
\begin{bmatrix}
    3 \newline
    4 \newline
    1 
\end{bmatrix}
$$

이동 변환 행렬

$$
T =

\begin{bmatrix}
1 & 0 & 2 \newline
0 & 1 & 5 \newline
0 & 0 & 1 
\end{bmatrix}
$$

행렬 연산 수행

$$
\begin{bmatrix}
    x\prime \newline
    y\prime \newline
    1 
\end{bmatrix}

=

\begin{bmatrix}
    1 & 0 & 2 \newline
    0 & 1 & 5 \newline
    0 & 0 & 1
\end{bmatrix}

\begin{bmatrix}
    3 \newline
    4 \newline
    1 
\end{bmatrix}
$$

계산 과정

$x\prime = (1 \times 3) + (0 \times 4) + (2 \times 1) = 3 + 2 = 5$  
$y\prime = (0 \times 3) + (1 \times 4) + (5 \times 1) = 4 + 5 = 9$

결과 좌표

$P\prime = (5,9)$  
즉, 점 $(3,4)$ 가 $(5,9)$ 로 이동하였습니다.

## 회전 변환

회전 변환은 객체를 원점을 기준으로 특정 각도 $\theta$만큼 회전하는 변환입니다.  
즉, 좌표를 특정 각도만큼 회전시키는 연산입니다.

기존의 좌표 $(x, y)$를 극좌표계로 나타내면, 점 $(x, y)$는 다음과 같이 반지름 $r$과 각도 $\theta_0$로 표현할 수 있습니다.  
$x = r \cos \theta_0, \quad y = r \sin \theta_0$

여기서, $r$은 원점에서 점까지의 거리 $r = \sqrt{x^2 + y^2}$, $\theta_0$은 기존 점이 형성하는 각도입니다.  
이제 이 점을 원점을 기준으로 $\theta$만큼 회전한다고 가정하겠습니다.  
회전 후의 새로운 각도는 $\theta_0 + \theta$가 됩니다.

이를 바탕으로 회전 후의 좌표 $(x\prime, y\prime)$를 극좌표계에서 표현하면 다음과 같습니다.  
$x\prime = r \cos(\theta_0 + \theta), \quad y\prime = r \sin(\theta_0 + \theta)$

각도의 합에 대한 공식은 다음과 같습니다.  
코사인 덧셈 공식  
$\cos(A + B) = \cos A \cos B - \sin A \sin B$  
사인 덧셈 공식  
$\sin(A + B) = \sin A \cos B + \cos A \sin B$

이를 $x\prime, y\prime$에 적용하면 다음과 같습니다.  
$x\prime = r (\cos \theta_0 \cos \theta - \sin \theta_0 \sin \theta)$  
$y\prime = r (\sin \theta_0 \cos \theta + \cos \theta_0 \sin \theta)$

$r \cos\theta_0 = x ,  r \sin\theta_0 = y$ 를 대입하면 다음과 같은 회전 변환 공식을 얻을 수 있습니다.  
$x\prime = x \cos \theta - y \sin \theta$  
$y\prime = x \sin \theta + y \cos \theta$

이 공식을 회전 변환 행렬로 표현하면 다음과 같습니다.

$$
R(\theta) =
\begin{bmatrix}
    \cos \theta & -\sin \theta & 0 \newline 
    \sin \theta & \cos \theta & 0 \newline
    0 & 0 & 1
\end{bmatrix}
$$

이 행렬을 적용하면 회전 후의 값을 구할 수 있습니다.

$$
\begin{bmatrix}
    x\prime \newline 
    y\prime \newline
    1
\end{bmatrix}

=

R(\theta)

\times

\begin{bmatrix}
    x \newline 
    y \newline
    1
\end{bmatrix}
$$

예시  
점  $P(3,4)$를 $90^\circ$회전하는 것을 행렬로 표현하면 다음과 같습니다.  
일반적으로 컴퓨터 그래픽에서는 반시계 방향이 양의 회전으로 간주됩니다.

초기 좌표(동차 좌표계 적용)

$$
P =
\begin{bmatrix}
    3 \newline
    4 \newline
    1 
\end{bmatrix}
$$

회전 행렬(90도)

$$
R(90^\circ) =
\begin{bmatrix}
    0 & -1 & 0 \newline
    1 & 0 & 0 \newline
    0 & 0 & 1 
\end{bmatrix}
$$

행렬 연산 수행

$$
\begin{bmatrix}
    x\prime \newline
    y\prime \newline
    1 
\end{bmatrix}

=

\begin{bmatrix}
    0 & -1 & 0 \newline
    1 & 0 & 0 \newline
    0 & 0 & 1 
\end{bmatrix}

\begin{bmatrix}
    3 \newline
    4 \newline
    1 
\end{bmatrix}
$$

계산 과정  
$x\prime = (0 \times 3) + (-1 \times 4) + (0 \times 1) = -4$  
$y\prime = (1 \times 3) + (0 \times 4) + (0 \times 1) = 3$

결과 좌표  
$P\prime = (-4,3)$  
즉, 점 $(3,4)$ 가 90도 회전 후 $(-4,3)$으로 이동하였습니다.

![Matrix-Rotation]({{site.url}}/images/etc/Math/2025-02-15-Math-Matrix/Matrix-Rotation.PNG)

만약 시계 방향 회전을 원한다면 $-\theta$를 적용해야합니다.

$$
R(-\theta) = 
\begin{bmatrix}
    \cos(-\theta) & -\sin(-\theta) & 0 \newline
    \sin(-\theta) & \cos(-\theta) & 0 \newline
    0 & 0 & 1
\end{bmatrix}

=

\begin{bmatrix}
    \cos \theta & \sin \theta & 0 \newline
    -\sin \theta & \cos \theta & 0 \newline
    0 & 0 & 1
\end{bmatrix}
$$

위의 예시를 시계 방향으로 회전한다면, $(4, -3)$이 됩니다.

## 크기 변환

크기 변환은 객체의 크기를 특정 배율만큼 확대 또는 축소하는 변환입니다.

원점 기준으로 좌표를  $sx, sy$배만큼 변경하는 연산입니다.

크기 변환 공식은 다음과 같습니다.  
$x\prime = sx \cdot x$  
$y\prime = sy \cdot y$

크기 변환 행렬

$$
S =
\begin{bmatrix}
    sx & 0 & 0 \newline
    0 & sy & 0 \newline
    0 & 0 & 1
\end{bmatrix}
$$

행렬 연산 적용

$$
\begin{bmatrix}
    x\prime \newline
    y\prime \newline
    1
\end{bmatrix}

=

S

\times

\begin{bmatrix}
    x \newline
    y \newline
    1
\end{bmatrix}
$$

계산 과정  
$x\prime = sx \cdot x$
$y\prime = sy \cdot y$

예시  
점 $P(3,4)$을 $x$축 방향으로 2배, $y$축 방향으로 3배 확대하면 다음과 같습니다.

초기 좌표(동차 좌표계 적용)

$$
P =
\begin{bmatrix}
    3 \newline
    4 \newline
    1 
\end{bmatrix}
$$

회전 행렬(90도)

$$
S =
\begin{bmatrix}
    2 & 0 & 0 \newline
    0 & 3 & 0 \newline
    0 & 0 & 1 
\end{bmatrix}
$$

행렬 연산 수행

$$
\begin{bmatrix}
    x\prime \newline
    y\prime \newline
    1 
\end{bmatrix}

=

\begin{bmatrix}
    2 & 0 & 0 \newline
    0 & 3 & 0 \newline
    0 & 0 & 1 
\end{bmatrix}

\begin{bmatrix}
    3 \newline
    4 \newline
    1 
\end{bmatrix}
$$

계산 과정  
$x\prime = 2 \times 3 = 6$  
$y\prime = 3 \times 4 = 12$

결과 좌표  
$P\prime = (6,12)$

## 이동, 회전, 크기 변환을 하나의 행렬로 결합하기

이동(T), 회전(R), 크기 변환(S)을 조합하여 하나의 행렬로 표현하는 방법은 다음과 같습니다.

변환 순서  
$M = S \times R \times T$

예를 들어, 점 $(3,4)$를 $(dx = 2, dy = 5)$ 이동 → 90도 회전 → 2배 확대하면 다음과 같이 표현 할 수 있습니다.

$$
M =
\begin{bmatrix}
    2 & 0 & 0 \newline
    0 & 2 & 0 \newline
    0 & 0 & 1
\end{bmatrix}

\times

\begin{bmatrix}
    0 & -1 & 0 \newline
    1 & 0 & 0 \newline
    0 & 0 & 1
\end{bmatrix}

\times

\begin{bmatrix}
    1 & 0 & 2 \newline
    0 & 1 & 5 \newline
    0 & 0 & 1
\end{bmatrix}
$$

이것을 각각으로 표현하는 것이 아니라 다음과 같이 변환 행렬을 곱하여 하나의 변환 행렬로 만들면 연산이 단순해지고 최적화됩니다.

$$
M =
\begin{bmatrix}
    0 & -2 & 2 \newline
    2 & 0 & 5 \newline
    0 & 0 & 1
\end{bmatrix}
$$

이 하나의 행렬을 사용하여 점에 곱하면, 이동 → 회전 → 크기 변환이 한 번의 연산으로 적용됩니다.