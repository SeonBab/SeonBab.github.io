---
layout: single

title: "[Algorithm] 알고리즘이란"

categories:
    - Algorithm
tag: [알고리즘]

date: 2024-12-26
last_modified_at: 2024-12-26

order : 1
---

# 알고리즘

알고리즘이란 문제를 해결하기 위해 컴퓨터가 이해하고 실행할 수 있도록 문제 해결 방법을 명확하고 논리적으로 정의한 것입니다.

알고리즘의 효율성은 문제 해결에 드는 시간과 메모리 사용량에 직접적인 영향을 미칩니다.  
대규모 데이터 처리나 실시간 시스템에서는 효율적인 알고리즘이 필수적입니다.

주로 수학, 컴퓨터 과학, 인공지능, 데이터 분석 등에서 활용됩니다.

## 알고리즘 특성

알고리즘은 다음과 같은 특성을 가집니다.

유한성(Finiteness)  
유한한 단계 내에 종료되어야 합니다.  
즉, 무한히 반복되거나 끝나지 않는 과정이 되어서는 안됩니다.

명확성(Definiteness)  
알고리즘의 각 단계는 모호하지 않게 명확하게 정의되어야 합니다.  
각 단계가 무엇을 해야 하는지, 그 결과가 무엇인지 분명히 이해할 수 있어야 합니다.

입력(Input)  
외부로부터 문제를 해결하기 위한 입력을 받습니다.

출력(Output)  
특정 문제에 대한 해결책을 출력으로 제공합니다.

효율성(Efficiency)  
좋은 알고리즘은 시간과 공간을 효율적으로 사용해야 합니다.  
문제 해결에 필요한 자원을 최소화하는 것이 중요합니다.

## 알고리즘 표현

알고리즘은 여러 방식으로 표현됩니다.

자연어  
일상 언어로 알고리즘을 설명하는 방법입니다.  
쉽게 이해할 수 있지만 정확성이 떨어질 수 있습니다.

의사 코드(Pseudocode)  
알고리즘을 일반적인 언어처럼 표현한 것으로, 프로그램 코드와 유사하게 작성되지만 실제 프로그래밍 언어는 아닙니다.

흐름도(Flowchart)
알고리즘의 흐름을 시각적으로 표현한 도식입니다.  
각 단계를 도형으로 나타내고, 흐름을 화살표로 연결합니다.

## 알고리즘 평가

알고리즘을 평가하는 일반적인 방법으로 시간 복잡도와 공간 복잡도를 사용합니다.

해당 복잡도를 표현 할 때 점근 표기법을 일반적으로 사용합니다.  
점근 표기법은 알고리즘의 성능을 수학적으로 표현하는 방법입니다.  
입력 크기가 커질수록 알고리즘의 실행 시간이 어떻게 변화하는지 나타냅니다.

주요 표기법은 다음과 같습니다.

빅-오 표기법(Big-O) : 최악의 경우를 나타냅니다.  
빅-오메가 표기법(Big-Omega) : 최선의 경우를 나타냅니다.  
빅-세타 표기법(Big-Theta) : 평균적인 경우를 나타냅니다.

본 글에서는 빅-오 표기법만 알아보겠습니다.  
오메가와 세타 표기법은 특정 상황에서는 유용하지만 일반적으로 잘 사용하지 않습니다.

### 시간 복잡도

시간 복잡도(Time Complexity)는 알고리즘이 문제를 해결하는 데 걸린 시간을 평가하는 척도입니다.

알고리즘이 처리하는 입력의 크기(n)에 따라 얼마나 시간이 증가하는지를 나타냅니다.  

시간을 측정한다는 것은 많은 변수가 있습니다.  
하드웨어 혹은 OS차이로 인한 실행시간의 차이가 있을 수 있고, 같은 컴퓨터라 하더라도 매 순간 상태가 달라 같은 실행시간을 보장할 수 없습니다.

### 공간 복잡도

공간 복잡도(Space Complexity)는 알고리즘이 문제를 해결하는 데 사용한 메모리의 양을 평가하는 척도입니다.

알고리즘이 처리하는 입력의 크기(n)에 따라 얼마나 많은 추가 메모리가 필요한지 나타냅니다.

시간 복잡도와 비슷하게 언어나 OS마다 변수하나를 저장하는데 사용하는 메모리양이 다를 수 있습니다.

### 빅-오 표기법

빅-오 표기법(Big-O Notation)은 시간, 공간 복잡도를 수학적으로 표기하는 방법입니다.

입력 크기(n)이 증가할 때, 알고리즘의 성능이 어떻게 변하는지를 나타내며, 최악의 경우를 표현합니다.

입력 크기에 따라 알고리즘이 얼마나 효율적인지 비교할 때 사용합니다.  
대규모 데이터에서 얼마나 잘 작동하는지 예측할 때 사용합니다.

입력 크기를 `n`으로 표현합니다.

알고리즘의 영향을 끼치는 모든 연산을 분석하지 않고, 실행 시간에 가장 큰 영향을 미치는 연산만 분석합니다.  
$O(2n) \rightarrow O(n)$ 상수 배율을 제거합니다.  
$O(n^2 + n) \rightarrow O(n^2)$ 낮은 차수는 무시합니다.

주요 시간 복잡도는 다음과 같습니다.  
가장 빠른 순서부터 느린 순서로 정렬 된 상태입니다.

|시간|표기법|
|---|---|
|상수 시간|$O(1)$|입력 크기와 관계없이 실행 시간이 일정합니다.|
|로그 시간|$O(log n)$|입력 크기를 반씩 줄이는 알고리즘에서 나타나 실행 시간이 천천히 증가합니다.|
|선형 시간|$O(n)$|입력 크기에 비례하여 실행 시간이 증가합니다.|
|로그 선형 시간|$O(n log n)$|입력 크기를 반씩 줄이면서 각 단계에서 선형 작업을 수행합니다.|
|이차 시간|$O(n^2)$|입력 크기가 증가할수록 시간이 제곱으로 증가합니다. 중첩 루프에서 나타납니다.|
|지수 시간|$O(2^n)$|입력 크기에 따라 실행 시간이 지수적으로 증가합니다.|
|팩토리얼 시간|$O(n!)$|입력 크기가 n개일 때 n!개의 가능한 경우를 모두 탐색하는 매우 비효율적인 알고리즘에서 나타납니다.|

빅 오 표기법에는 한계가 있습니다.  
상수를 무시하기 때문에 실제 실행 시간과는 차이가 있을 수 있습니다.  
주로 최악의 경우만 분석하며, 평균적인 경우는 차이가 있을 수 있습니다.  
알고리즘이 실행되는 하드웨어나 시스템 환경에 따라 달라지는 성능 차이를 반영하지 않습니다.

## 알고리즘 유형

정렬 알고리즘  
데이터를 정렬하는 알고리즘으로, 버블 정렬, 병합정렬, 퀵 정렬 등이 있습니다.

탐색 알고리즘  
주어진 데이터 구조에서 특정 데이터를 찾는 알고리즘으로, 이진 탐색, 선형 탐색 등이 있습니다.

그래프 알고리즘  
그래프 구조에서 경로를 찾거나 탐색하는 알고리즘으로, 너비 우선 탐색(BFS), 깊이 우선 탐색(DFS), 다익스트라 알고리즘 등이 있습니다.

그리디 알고리즘  
각 단계에서 가장 좋은 선택을 하는 방식으로, 최적해를 구하는 데 사용됩니다.  
예를 들어, 동전 거스름돈 문제 등이 그리디 알고리즘을 사용한 예입니다.

동적 계획법  
문제를 작은 부분 문제로 나누어 해결하는 방식으로, 피보나치 수열, 최단 경로 문제 등이 동적 계획법을 활용합니다.