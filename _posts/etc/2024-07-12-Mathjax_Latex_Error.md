---
layout: single

title: "Mathjax의 LaTex 태그의 시그마 중첩 렌더링 문제"

categories:
    - etc
tag: [etc]

date: 2024-07-12
last_modified_at: 2024-07-12

mermaid: true
---

# Mathjax의 LaTex 렌더링 문제

Mathjax는 거의 모든 LaTex 태그를 잘 표현하지만 가끔씩 수식 렌더링이 되지 않는 문제가 있습니다.

[이 문제는 Mathjax에 이슈로 등록되어 있습니다.](https://github.com/mathjax/MathJax/issues/984)

```
$
\mathcal{L}_{MSE} = \frac{1}{N} \sum_{i=1}^N (y_i - \hat{y_i})^2
$

```

$
\mathcal{L}_{MSE} = \frac{1}{N} \sum_{i=1}^N (y_i - \hat{y_i})^2
$

이는 아래첨자 `_`가 이탤릭체가 아니라 아래 첨자임을 확실하게 알지 못하기 때문에 랜더링에 실패해 문제가 되는 것입니다.

## 해결

시그마를 사용할 때 이탤릭체가 아닌 아래 첨자를 명확하게 표시해주면 됩니다.  
그 방법은 이스케이프(`\_`)를 사용해 명확하게 표시해 줄 수 있습니다.

이 방식을 사용할 경우 위에서 렌더링 문제가 있던 LaTex코드가 정상적으로 출력되는 것을 알 수 있습니다.

```
$
\mathcal{L}\_{MSE} = \frac{1}{N} \sum \_{i=1}^N (y\_i - \hat{y\_ i})^2
$
```

$
\mathcal{L}\_{MSE} = \frac{1}{N} \sum \_{i=1}^N (y\_i - \hat{y\_ i})^2
$
