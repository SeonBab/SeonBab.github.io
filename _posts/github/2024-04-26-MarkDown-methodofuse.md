---
layout: single

title: "GitHub MarkDown 사용법/문법"

categories:
    - GitHub
tag: [GitHub]

date: 2024-04-26
last_modified_at: 2024-05-02

order : 1

mermaid: true
---
# GitHub MarkDown

MarkDown은 텍스트 기반의 마크업언어로 2004년 존 그루버에 의해 만들어졌습니다.  

특수기호와 문자를 이용한 간단한 구조의 문법을 사용해 웹에서도 빠르게 컨텐츠를 작성하고 보다 직관적으로 인식 할 수 있습니다.
MarkDown은 HTML로 변환이 가능합니다.

마크다운의 장점은 다음과 같습니다.
+ 문법이 쉽고 관리가 간편합니다.
- 다양한 형태로 변환이 가능합니다.
+ 텍스트(text)로 저장되기 때문에 용량이 적어 보관이 용이합니다.
- 지원하는 프로그램과 플랫폼이 다양합니다.

단점은 다음과 같습니다.
- 표준이 없기 때문에 도구에 따라서 문법이 다를 수 있습니다.
+ 모든 HTML 마크업을 대신하지 못합니다.

## 사용법(문법)

본 문서에서는 GitHub에서의 MarkDown의 사용법을 다뤘습니다.

### 제목

**h1**부터 **h6**까지 #을 이용해 제목(Header)을 나타낼 수 있습니다.

```
# h1 제목 1
## h2 제목 2
### h3 제목 3
#### h4 제목 4
##### h5 제목 5
###### h6 제목 6
####### This is a H7(지원하지 않음)
```

# h1 제목 1
{: .no_toc}
## h2 제목 2
{: .no_toc}
### h3 제목 3
{: .no_toc}
#### h4 제목 4
{: .no_toc}
##### h5 제목 5
{: .no_toc}
###### h6 제목 6
{: .no_toc}

####### This is a H7(지원하지 않음)

```
h1: 문서 제목
=============
```

h1: 문서 제목
=============
{: .no_toc}

=의 갯수는 중요하지 않습니다.

```
h2: 문서 제목
---------------
```

h2: 문서 제목
---------------
{: .no_toc}

-의 갯수는 중요하지 않습니다.

### 수평선

수평선을 만들려면 한 줄에 3개 이상의 *, -, _를 한가지만 사용합니다.

```
***

---

___
```

***

---

___

### toc를 사용한 목차

목차를 만드는 방법 중 하나인 toc를 사용하면 다음과같이 사용합니다.

```
# 순서가 있는 목록
0. TOC 
{:toc}

# 순서가 없는 목록
* TOC
{:toc}

# 목록 스타일 없는 목록
# 제목으로만 목차를 구성할 경우 no-style을 추가합니다.
* TOC 
{:toc}
{: .no-style }
```

특정 제목을 목차에서 제외하는 방법은 다음과 같습니다.  
이 경우 제목은 문서에 정상적으로 출력되지만 목차에 포함시키지 않습니다.

```
{: .no_toc}
```

### 텍스트 스타일 지정

|스타일|구문|예시|출력|
|---|---|---|---|
|굵게|`** **` 또는 `__ __`|`**bold text**` </br> `__bold text__`|**bold text**|
|기울임|`* *` 또는 `_ _`|`*italicized text*` </br> `_italicized text_`|*italicized text*|
|취소선|`~~ ~~`|`~~Strikethrough~~`|~~Strikethrough~~|
|모두 굵게 및 기울임|`*** ***`|`***bold and Italic***`|***bold and Italic***|
|아래 첨자|`<sub> </sub>`|`H<sub>2</sub>O`|H<sub>2</sub>O|
|윗 첨자|`<sup> </sup>`|`X<sup>2</sup>`|X<sup>2</sup>|

### 이모지 사용

이모지를 그대로 복사해와서 사용 할 수 있습니다.  
혹은 콜론, 이모지 이름 순서`:EMOJICODE:`로 입력해 글에 이모지를 추가할 수 있습니다.

이모지의 이름은 [emojipedia](https://emojipedia.org/ko)를 참고할 수 있습니다.

```
:rice:
:cooked_rice:
```

### 각주

각주를 추가하려면 `[^1]`, `[^2]`등을 사용해서 추가할 수 있습니다.

```
각주1[^1]
  
각주2[^2]

[^1]: 각주1의 내용
  
[^2]: 각주2의 내용
```

각주1[^1]
각주2[^2]

[^1]: 각주1의 내용
[^2]: 각주2의 내용

### 줄 바꿈

줄 바꿈을 하려면 두 개 이상의 공백으로 줄을 끝내거나 `\`를 줄 끝에 붙여줍니다.

```
This is the first line.  
And this is the second line.

This is the first line.\
And this is the second line.
```

`\`를 사용한 방법은 모든 MarkDown 응용 프로그램이 지원하지는 않으므로 호환성 관점에서 좋은 옵션이 아닙니다.  
GitHub에서는 지원하고 있습니다.

### 단락

새 단락을 만들려면 빈 줄을 사용해 하나 이상의 텍스트 줄을 구분합니다.
```
I really like using Markdown.

I think I'll use it to format all of my documents from now on.
```

### 인용문

인용문을 만들려면 `>`단락을 앞에 추가합니다.

```
> Single BlockQuote
```

> Single BlockQuote

```
> This is nested blockqute.
> > This is nested blockqute.
> > > This is nested blockqute.
```

> This is nested blockqute.
> > This is nested blockqute.
> > > This is nested blockqute.

### 목록

순서가 있는 목록은 `1.`, `2.`, `3.` 등을 사용합니다.  
순서가 없는 목록은 `*`,`+`,`-`를 사용합니다.

```
1. 순서가 있는 항목
1. 순서가 있는 항목
    + 순서가 없는 항목
    + 순서가 없는 항목
1. 순서가 있는 항목
1. 순서가 있는 항목

* 순서가 없는 항목
+ 순서가 없는 항목
    * 순서가 없는 항목
- 순서가 없는 항목
```

1. 순서가 있는 항목
1. 순서가 있는 항목
    + 순서가 없는 항목
    + 순서가 없는 항목
1. 순서가 있는 항목
1. 순서가 있는 항목

* 순서가 없는 항목
+ 순서가 없는 항목
    * 순서가 없는 항목
- 순서가 없는 항목

### 작업 목록

작업 목록은 접두사로 `- [ ]`를 사용합니다. 작업을 완료로 표시할 경우 `- [x]`를 사용합니다.

```
- [ ] 작업 목록A
- [ ] 작업 목록B
- [x] 작업 목록C
```

- [ ] 작업 목록A
- [ ] 작업 목록B
- [x] 작업 목록C

### 경고

경고는 중요한 정보를 강조하는 데 사용할 수 있는 blockquote 구문을 기반으로 한 Markdown 확장입니다. GitHub에서는 콘텐츠의 중요도를 나타내기 위해 고유한 색과 아이콘으로 표시됩니다.

```
> [!NOTE]
> Useful information that users should know, even when skimming content.

> [!TIP]
> Helpful advice for doing things better or more easily.

> [!IMPORTANT]
> Key information users need to know to achieve their goal.

> [!WARNING]
> Urgent info that needs immediate user attention to avoid problems.

> [!CAUTION]
> Advises about risks or negative outcomes of certain actions.
```

![경고](https://docs.github.com/assets/cb-50447/mw-1440/images/help/writing/alerts-rendered.webp)

### 인라인 코드

강조할 코드를 백틱`` ` ``으로 감싸 인라인(InLine)코드로 표현합니다.

코드로 표시하려는 문구에 백틱이 하나 이상 포함된 경우 백틱`` ` ``을 두개 사용해서 감싸줍니다.

```
`a = 1 + 2`

백틱 `` ` ``
```

`a = 1 + 2`

백틱 `` ` ``

### 코드 블록

강조할 코드를 삼중 백틱을 사용해 고유한 블록 안으로 지정할 수 있습니다.

코드 하이라이트를 넣고 싶은 경우 백틱 뒤에 언어 이름을 넣어주면 됩니다.

````
```
int a = 1 + 2;
```

``` Cpp
int a = 1 + 2;
```
````

```
int a = 1 + 2;
```

``` Cpp
int a = 1 + 2;
```

코드블럭 안에 코드블럭을 표시하고 싶은 경우 4중 백틱 안에 3중 백틱을 넣어줍니다.
`````
````
```
int a = 1 + 2;
```

``` Cpp
int a = 1 + 2;
```
````
`````

### 링크

인라인 링크는 다음과 같습니다.

```
사용문법: [Title](link)
적용예: [GitHub-Get-Started](https://docs.github.com/ko/get-started)
```

[GitHub-Get-Started](https://docs.github.com/ko/get-started)  

url 링크는 다음과 같습니다.  
이메일에도 사용 가능합니다.

```
<https://docs.github.com/ko/get-started>
<tjsnr3180@gmail.com>
```

<https://docs.github.com/ko/get-started>  
<tjsnr3180@gmail.com>

참조 링크는 다음과 같습니다.

```
[link keyword][id]

[id]: URL "Optional Title here"

// code
[GitHub-Get-Started][GitHub-Get-Startedlink]

[GitHub-Get-Startedlink]: https://docs.github.com/ko/get-started
```

[GitHub-Get-Started][GitHub-Get-Startedlink]

[GitHub-Get-Startedlink]: https://docs.github.com/ko/get-started

### 이미지

이미지는 `!`를 추가하고 `[ ]`에 대체 텍스트를 넣어 이미지를 표시할 수 있습니다. 그런 다음 이미지에 대한 링크를 괄호 `()`에 넣습니다.  
대체 텍스트는 이미지의 정보와 동일한 짧은 텍스트입니다. 

```
![Screenshot of a comment on a GitHub issue showing an image, added in the Markdown, of an Octocat smiling and raising a tentacle.](https://myoctocat.com/assets/images/base-octocat.svg)
```

![Screenshot of a comment on a GitHub issue showing an image, added in the Markdown, of an Octocat smiling and raising a tentacle.](https://myoctocat.com/assets/images/base-octocat.svg)

이미지에 링크를 추가하고 싶은 경우 아래와 같습니다.

```
[![대체 텍스트](이미지에 대한 링크)](걸고자 하는 링크)

[![Screenshot of a comment on a GitHub issue showing an image, added in the Markdown, of an Octocat smiling and raising a tentacle.](https://myoctocat.com/assets/images/base-octocat.svg)](https://myoctocat.com/assets/images/base-octocat.svg)
```

[![Screenshot of a comment on a GitHub issue showing an image, added in the Markdown, of an Octocat smiling and raising a tentacle.](https://myoctocat.com/assets/images/base-octocat.svg)](https://myoctocat.com/assets/images/base-octocat.svg)

### 표

표을 추가하려면 3개 이상의 하이픈`-`을 사용해 각 열의 헤더를 생성하고 버티컬바`|`를 사용해 각 열을 구분합니다.  
호환성을 위해서는 행의 양쪽 끝에 버티컬바 추가해야 합니다. GitHub에서는 양쪽 끝에 버티컬바가 없어도 표가 추가됩니다.

```
| 헤더 | 헤더 | 헤더 |
|---|---|---|
| 셀 | 셀 | 셀 |
| 셀 | 셀 | 셀 |

헤더 | 헤더 | 헤더
---|---|---
셀 | 셀 | 셀
셀 | 셀 | 셀
```

| 헤더 | 헤더 | 헤더 |
|---|---|---|
| 셀 | 셀 | 셀 |
| 셀 | 셀 | 셀 |

헤더를 구분해 각 열의 내용을 정렬할 수 있습니다.

```
| 헤더 | 헤더 | 헤더 | 헤더 |
|---|:---|:---:|:---:|
| 좌측정렬 | 좌측정렬 | 가운데정렬 |우측정렬|
```

| 헤더 | 헤더 | 헤더 | 헤더 |
|---|:---|:---:|:---:|
| 좌측정렬 | 좌측정렬 | 가운데정렬 |우측정렬|

### 콜랩스

섹션을 확장하거나 축소하는 기능을 추가해 긴 문서의 가독성을 향상시킵니다.

```
<details>
  <summary>클릭하여 섹션을 펼치거나 접기</summary>

  내용이 들어갑니다.  

</details>
```

<details>
  <summary>클릭하여 섹션을 펼치거나 접기</summary>

  내용이 들어갑니다.

</details>

### 다이어그램 만들기

텍스트를 다이어그램으로 렌더링해주는 Mermaid를 사용합니다.

순서도, 시퀀스 다이어그램, 원형 차트 등을 렌더링 할 수 있습니다.

[Mermaid](https://mermaid.js.org/#/)에 자세한 내용이 있습니다.

````
```mermaid
graph TD;
    A-->B;
    A-->C;
    B-->D;
    C-->D;
```

```mermaid
graph LR
A(입력)-->B[연산]
B-->C(출력)
```
````

```mermaid
graph TD;
    A-->B;
    A-->C;
    B-->D;
    C-->D;
```

```mermaid
graph LR
A(입력)-->B[연산]
B-->C(출력)
```

### 수학 식

GitHub는 MarkDown 내에서 LaTeX 형식의 수학을 지원합니다.  
자세한 내용은 [LaTeX/Mathematics](https://en.wikibooks.org/wiki/LaTeX/Mathematics)를 참조하세요.

GitHub의 수학 렌더링 기능은 오픈 소스 JavaScript 기반 디스플레이 엔진인 MathJax를 사용합니다.  
자세한 내용은 [MatJax설명서](https://docs.mathjax.org/en/latest/input/tex/index.html#tex-and-latex-support)를 참조하세요.

식을 `$`로 감싸거나, `` $` ``로 시작하고 `` `$ ``로 끝낼 수 있습니다.

```
$\sqrt{3x-1}+(1+x)^2$
```

$\sqrt{3x-1}+(1+x)^2$

식을 블록으로 작성할 경우 다음과 같습니다.

```
$$\left( \sum_{k=1}^n a_k b_k \right)^2 \leq \left( \sum_{k=1}^n a_k^2 \right) \left( \sum_{k=1}^n b_k^2 \right)$$
```

$$\left( \sum_{k=1}^n a_k b_k \right)^2 \leq \left( \sum_{k=1}^n a_k^2 \right) \left( \sum_{k=1}^n b_k^2 \right)$$

또는 다음과 같은 방법으로 식을 작성할 수 있습니다.  
위의 방법과 동일하게 렌더링 됩니다.

````
```math
\left( \sum_{k=1}^n a_k b_k \right)^2 \leq \left( \sum_{k=1}^n a_k^2 \right) \left( \sum_{k=1}^n b_k^2 \right)
```
````

### Markdown 서식 무시

문법 앞에 \를 사용하여 형식을 무시(또는 이스케이프)하도록 GitHub에 지시할 수 있습니다.

```
\*\*bold text**
```

\*\*bold text**

참조  
https://www.markdownguide.org/  
https://docs.github.com/ko/get-started
