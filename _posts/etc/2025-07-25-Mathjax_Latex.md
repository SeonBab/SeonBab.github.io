---
layout: single

title: "Latex 수식 문법"

categories:
    - etc
tag: [etc]

date: 2025-07-25
last_modified_at: 2025-07-25

order : 0
---

<style>
  table {
    width: 95%; /* 테이블 너비 조정 */
    margin: auto; /* 테이블 중앙 정렬 */
    border-collapse: collapse; /* 테두리 병합 */
    table-layout: fixed; /* 고정된 너비 적용 */
  }
  td {
    width: 180px; /* 셀 너비 고정 */
    height: 35px; /* 셀 높이 고정 */
    border: 1px solid black; /* 셀 테두리 */
    text-align: center; /* 텍스트 가운데 정렬 */
    padding: 5px; /* 여백 추가 */
    overflow: hidden; /* 내용이 넘칠 경우 숨김 */
    white-space: nowrap; /* 줄바꿈 방지 */
  }
</style>

# Latex 수식 문법

## 텍스트 및 크기 조절

$\textrm a_1, b^2$ `\textrm a_1, b^2` : 일반 텍스트  
$\mathcal{Apple}$ `\mathcal{텍스트}`: 캘리그래피체(필기체) 스타일  
$\mathbb{Apple}$ `\mathbb{Apple}` : 블랙보드 볼드체 스타일  
$\mathfrak{Apple}$ `\mathfrak{Apple}` : 고딕체 또는 독일식 필기체(프락투어) 스타일  
$\mathsf{Apple}$ `\mathsf{Apple}` : 고딕과 획 없는 글씨체로 일반 산세리프체 스타일  
$\mathbf{Apple}$ `\mathbf{Apple}` : 굵은 글씨체(볼드) 스타일  
$\mathscr{Apple}$ `\mathscr{Apple}` : 스크립트체 스타일

$\textstyle a_1, b^2$ `\textstyle a_1, b^2` : 텍스트 스타일  
$\displaystyle a_1, b^2$ `\displaystyle a_1, b^2` : 디스플레이 스타일  
$\scriptstyle a_1, b^2$ `\scriptstyle a_1, b^2` : 스크립트 스타일  
$\scriptscriptstyle a_1, b^2$ `\scriptscriptstyle a_1, b^2` : 스크립트스크립트 스타일

## 공백 및 줄바꿈

`\,` : 약 1/6 em, 한칸 띄어쓰기  
`\:` : 약 2/9 em, 두칸 띄어쓰기  
`\;` : 약 5/18 em, 세칸 띄어쓰기  
`\quad` : 1em, 네칸(= tab) 띄어쓰기  
`\qquad` : 2드, 여덟칸(= tab * 2) 띄어쓰기  
`\!` : 아주 작은 공백(간격이 거의 없음)  
`\hspace{길이}`, `\hspace*{길이}` : 임의 길이의 공백

`\\` : 줄바꿈  
`\newline` : 줄바꿈  
`\vspace{길이}`, `\vspace*{길이}` : 임의 줄간격

## 화살표

<table>
  <tr>
    <td> $\leftarrow$ </td> <td> \leftarrow </td>
    <td> $\rightarrow$ </td> <td> \rightarrow </td>
    <td> $\uparrow$ </td> <td> \uparrow </td>
    <td> $\downarrow$ </td> <td> \downarrow </td>
  </tr>
  <tr>
    <td> $\Leftarrow$ </td> <td> \Leftarrow </td>
    <td> $\Rightarrow$ </td> <td> \Rightarrow </td>
    <td> $\Uparrow$ </td> <td> \Uparrow </td>
    <td> $\Downarrow$ </td> <td> \Downarrow </td>
  </tr>
  <tr>
    <td> $\leftharpoonup$ </td> <td> \leftharpoonup </td>
    <td> $\rightharpoonup$ </td> <td> \rightharpoonup </td>
    <td> $\upharpoonleft$ </td> <td> \upharpoonleft </td>
    <td> $\downharpoonleft$ </td> <td> \downharpoonleft </td>
  </tr>
  <tr>
    <td> $\leftharpoondown$ </td> <td> \leftharpoondown </td>
    <td> $\rightharpoondown$ </td> <td> \rightharpoondown </td>
    <td> $\upharpoonright$ </td> <td> \upharpoonright </td>
    <td> $\downharpoonright$ </td> <td> \downharpoonright </td>
  </tr>
  <tr>
    <td> $\leftleftarrows$ </td> <td> \leftleftarrows </td>
    <td> $\rightrightarrows$ </td> <td> \rightrightarrows </td>
    <td> $\upuparrows$ </td> <td> \upuparrows </td>
    <td> $\downdownarrows$ </td> <td> \downdownarrows </td>
  </tr>
  <tr>
    <td> $\longleftarrow$ </td> <td> \longleftarrow </td>
    <td> $\longrightarrow$ </td> <td> \longrightarrow </td>
    <td> $\Lsh$ </td> <td> \Lsh </td>
    <td> $\Rsh$ </td> <td> \Rsh </td>
  </tr>
  <tr>
    <td> $\Longleftarrow$ </td> <td> \Longleftarrow </td>
    <td> $\Longrightarrow$ </td> <td> \Longrightarrow </td>
    <td> $\leftarrowtail$ </td> <td> \leftarrowtail </td>
    <td> $\rightarrowtail$ </td> <td> \rightarrowtail </td>
  </tr>
  <tr>
    <td> $\hookleftarrow$ </td> <td> \hookleftarrow </td>
    <td> $\hookrightarrow$ </td> <td> \hookrightarrow </td>
    <td> $\twoheadleftarrow$ </td> <td> \twoheadleftarrow </td>
    <td> $\twoheadrightarrow$ </td> <td> \twoheadrightarrow </td>
  </tr>
  <tr>
    <td> $\leftrightharpoons$ </td> <td> \leftrightharpoons </td>
    <td> $\rightleftharpoons$ </td> <td> \rightleftharpoons </td>
    <td> $\curvearrowleft$ </td> <td> \curvearrowleft </td>
    <td> $\curvearrowright$ </td> <td> \curvearrowright </td>
  </tr>
  <tr>
    <td> $\leftrightarrows$ </td> <td> \leftrightarrows </td>
    <td> $\rightleftarrows$ </td> <td> \rightleftarrows </td>
    <td> $\circlearrowleft$ </td> <td> \circlearrowleft </td>
    <td> $\circlearrowright$ </td> <td> \circlearrowright </td>
  </tr>
  <tr>
    <td> $\Lleftarrow$ </td> <td> \Lleftarrow </td>
    <td> $\Rrightarrow$ </td> <td> \Rrightarrow </td>
    <td> $\looparrowleft$ </td> <td> \looparrowleft </td>
    <td> $\looparrowright$ </td> <td> \looparrowright </td>
  </tr>
  <tr>
    <td> $\leftrightarrow$ </td> <td> \leftrightarrow </td>
    <td> $\Leftrightarrow$ </td> <td> \Leftrightarrow </td>
    <td> $\nleftarrow$ </td> <td> \nleftarrow </td>
    <td> $\nLeftarrow$ </td> <td> \nLeftarrow </td>
  </tr>
  <tr>
    <td> $\longleftrightarrow$ </td> <td> \longleftrightarrow </td>
    <td> $\Longleftrightarrow$ </td> <td> \Longleftrightarrow </td>
    <td> $\nrightarrow$ </td> <td> \nrightarrow </td>
    <td> $\nRightarrow$ </td> <td> \nRightarrow </td>
  </tr>
  <tr>
    <td> $\updownarrow$ </td> <td> \updownarrow </td>
    <td> $\Updownarrow$ </td> <td> \Updownarrow </td>
    <td> $\nleftrightarrow$ </td> <td> \nleftrightarrow </td>
    <td> $\nLeftrightarrow$ </td> <td> \nLeftrightarrow </td>
  </tr>
  <tr>
    <td> $\mapsto$ </td> <td> \mapsto </td>
    <td> $\longmapsto$ </td> <td> \longmapsto </td>
    <td> $\rightsquigarrow$ </td> <td> \rightsquigarrow </td>
    <td> $\leftrightsquigarrow$ </td> <td> \leftrightsquigarrow </td>
  </tr>
  <tr>
    <td> $\swarrow$ </td> <td> \swarrow </td>
    <td> $\searrow$ </td> <td> \searrow </td>
    <td> $\nwarrow$ </td> <td> \nwarrow </td>
    <td> $\nearrow$ </td> <td> \nearrow </td>
  </tr>
</table>

## 이항 연산자 및 관계연산자

<table>
  <tr>
    <td> $+$ </td> <td> + </td>
    <td> $-$ </td> <td> - </td>
    <td> $\times$ </td> <td> \times </td>
    <td> $\div$ </td> <td> \div </td>
    <td> $\wr$ </td> <td> \wr </td>
  </tr>
  <tr>
    <td> $\pm$ </td> <td> \pm </td>
    <td> $\mp$ </td> <td> \mp </td>
    <td> $\setminus$ </td> <td> \setminus </td>
    <td> $\leftthreetimes$ </td> <td> \leftthreetimes </td>
    <td> $\rightthreetimes$ </td> <td> \rightthreetimes </td>
  </tr>
  <tr>
    <td> $\ltimes$ </td> <td> \ltimes </td>
    <td> $\rtimes$ </td> <td> \rtimes </td>
    <td> $\oplus$ </td> <td> \oplus </td>
    <td> $\ominus$ </td> <td> \ominus </td>
    <td> $\otimes$ </td> <td> \otimes </td>
  </tr>
  <tr>
    <td> $\oslash$ </td> <td> \oslash </td>
    <td> $\cdot$ </td> <td> \cdot </td>
    <td> $\odot$ </td> <td> \odot </td>
    <td> $\circ$ </td> <td> \circ </td>
    <td> $\bullet$ </td> <td> \bullet </td>
  </tr>
  <tr>
    <td> $\land$ </td> <td> \land / \wedge </td>
    <td> $\lor$ </td> <td> \lor / \vee </td>
    <td> $\cap$ </td> <td> \cap </td>
    <td> $\cup$ </td> <td> \cup </td>
    <td> $\subset$ </td> <td> \subset </td>
  </tr>
  <tr>
    <td> $\subseteq$ </td> <td> \subseteq </td>
    <td> $\supset$ </td> <td> \supset </td>
    <td> $\supseteq$ </td> <td> \supseteq </td>
    <td> $\in$ </td> <td> \in </td>
    <td> $\notin$ </td> <td> \notin </td>

  </tr>
  <tr>
    <td> $\equiv$ </td> <td> \equiv </td>
    <td> $\neq$ </td> <td> \neq </td>
    <td> $\leq$ </td> <td> \leq </td>
    <td> $\geq$ </td> <td> \geq </td>
    <td> $\approx$ </td> <td> \approx </td>
  </tr>
  <tr>
    <td> $\sim$ </td> <td> \sim </td>
    <td> $\simeq$ </td> <td> \simeq </td>
    <td> $\cong$ </td> <td> \cong </td>
    <td> $\perp$ </td> <td> \perp </td>
    <td>  </td> <td>  </td>
  </tr>
</table>

## 기타 연산자 및 수학 기호

<table>
  <tr>
    <td> $\infty$ </td> <td> \infty </td>
    <td> $\sin$ </td> <td> \sin </td>
    <td> $\cos$ </td> <td> \cos </td>
    <td> $\bar{x}$ </td> <td> \bar{x} </td>
    <td> $\overline{x}$ </td> <td> \overline{x} </td>
  </tr>
  <tr>
    <td> $\frac {2x}{5y}$ </td> <td> \frac {2x}{5y} </td>
    <td> $\dfrac {2x}{5y}$ </td> <td> \dfrac {2x}{5y} </td>
    <td> $\tfrac {2x}{5y}$ </td> <td> \tfrac {2x}{5y} </td>
    <td> $a_0 + \cfrac{1}{a_1 + \cfrac{1}{\cdots}}$ </td> <td> a_0 + <br> \cfrac{1}{a_1 + <br> \cfrac{1}{\cdots}} </td>
    <td> ${2x} \over {5y}$ </td> <td> {2x} \over {5y} </td>
  </tr>
  <tr>
    <td> $\int$ </td> <td> \int </td>
    <td> $\int$ </td> <td> \oint </td>
    <td> $\sum$ </td> <td> \sum </td>
    <td> $\prod$ </td> <td> \prod </td>
    <td> $\lim_{h \to 0}$ </td> <td> \lim_{h \to 0} </td>
  </tr>
  <tr>
    <td> $\between$ </td> <td> \between </td>
    <td> $\natural$ </td> <td> \natural </td>
    <td> $\propto$ </td> <td> \propto </td>
    <td> $\therefore$ </td> <td> \therefore </td>
    <td> $\because$ </td> <td> \because </td>
  </tr>
  <tr>
    <td> $\diagup$ </td> <td> \diagup </td>
    <td> $\diagdown$ </td> <td> \diagdown </td>
    <td> $\asymp$ </td> <td> \asymp </td>
    <td> $\aleph$ </td> <td> \aleph </td>
    <td> $\imath$ </td> <td> \imath </td>
  </tr>
  <tr>
    <td> $\jmath$ </td> <td> \jmath </td>
    <td> $\ell$ </td> <td> \ell </td>
    <td> $\forall$ </td> <td> \forall </td>
    <td> $\exists$ </td> <td> \exists </td>
    <td> $\nexists$ </td> <td> \nexists </td>
  </tr>
  <tr>
    <td> $\neg$ </td> <td> \neg </td>
    <td> $\prime$ </td> <td> \prime </td>
    <td> $\backprime$ </td> <td> \backprime </td>
    <td> $\partial$ </td> <td> \partial </td>
    <td> $\emptyset$ </td> <td> \emptyset </td>
  </tr>
  <tr>
    <td> $\Re$ </td> <td> \Re </td>
    <td> $\Im$ </td> <td> \Im </td>
    <td> $\mho$ </td> <td> \mho </td>
    <td> $\curlywedge$ </td> <td> \curlywedge </td>
    <td> $\curlyvee$ </td> <td> \curlyvee </td>
  </tr>
  <tr>
    <td> $\barwedge$ </td> <td> \barwedge </td>
    <td> $\veebar$ </td> <td> \veebar </td>
    <td> $\bigtriangleup$ </td> <td> \bigtriangleup </td>
    <td> $\bigtriangledown$ </td> <td> \bigtriangledown </td>
    <td> $\LaTeX$ </td> <td> \LaTeX </td>
  </tr>
  <tr>
    <td> $\frown$ </td> <td> \frown </td>
    <td> $\smile$ </td> <td> \smile </td>
    <td> $\smallfrown$ </td> <td> \smallfrown </td>
    <td> $\smallsmile$ </td> <td> \smallsmile </td>
    <td> © </td> <td> \copyright </td>
  </tr>
  <tr>
    <td> $\centerdot$ </td> <td> \centerdot </td>
    <td> $\cdots$ </td> <td> \cdots </td>
    <td> $\vdots$ </td> <td> \vdots </td>
    <td> $\ddots$ </td> <td> \ddots </td>
    <td> $\ldots$ </td> <td> \ldots </td>
  </tr>
  <tr>
    <td> $\circledcirc$ </td> <td> \circledcirc </td>
    <td> $\circleddash$ </td> <td> \circleddash </td>
    <td> $\circledast$ </td> <td> \circledast </td>
    <td> $\diamondsuit$ </td> <td> \diamondsuit </td>
    <td> $\clubsuit$ </td> <td> \clubsuit </td>
  </tr>
  <tr>
    <td> $\heartsuit$ </td> <td> \heartsuit </td>
    <td> $\spadesuit$ </td> <td> \spadesuit </td>
    <td> $\bigcirc$ </td> <td> \bigcirc </td>
    <td> $\square$ </td> <td> \square </td>
    <td> $\Box$ </td> <td> \Box </td>
  </tr>
  <tr>
    <td> $\lozenge$ </td> <td> \lozenge </td>
    <td> $\Diamond$ </td> <td> \Diamond </td>
    <td> $\blacklozenge$ </td> <td> \blacklozenge </td>
    <td> $\diamond$ </td> <td> \diamond </td>
    <td> $\ast$ </td> <td> \ast </td>
  </tr>
  <tr>
    <td> $\star$ </td> <td> \star </td>
    <td> $\dagger$ </td> <td> \dagger </td>
    <td> $\ddagger$ </td> <td> \ddagger </td>
    <td> $\sharp$ </td> <td> \sharp </td>
    <td> $\flat$ </td> <td> \flat </td>
  </tr>
  <tr>
    <td> $\bot$ </td> <td> \bot </td>
    <td> $\top$ </td> <td> \top </td>
    <td>  </td> <td>  </td>
    <td>  </td> <td>  </td>
    <td>  </td> <td>  </td>
  </tr>
  <tr>
    <td> $\angle$ </td> <td> \angle </td>
    <td> $\measuredangle$ </td> <td> \measuredangle </td>
    <td> $\sphericalangle$ </td> <td> \sphericalangle </td>
    <td> $$ </td> <td> \ </td>
    <td> $$ </td> <td> \ </td>
  </tr>
</table>

## 그리스 문자

<table>
  <tr>
    <td> $\alpha$ </td> <td> \alpha </td>
    <td> $\beta$ </td> <td> \beta </td>
    <td> $\gamma$ </td> <td> \gamma </td>
    <td> $\delta$ </td> <td> \delta </td>
    <td> $\epsilon$ </td> <td> \epsilon </td>
  </tr>
  <tr>
    <td> $\zeta$ </td> <td> \zeta </td>
    <td> $\eta$ </td> <td> \eta </td>
    <td> $\theta$ </td> <td> \theta </td>
    <td> $\iota$ </td> <td> \iota </td>
    <td> $\kappa$ </td> <td> \kappa </td>
  </tr>
  <tr>
    <td> $\lambda$ </td> <td> \lambda </td>
    <td> $\mu$ </td> <td> \mu </td>
    <td> $\nu$ </td> <td> \nu </td>
    <td> $\xi$ </td> <td> \xi </td>
    <td> $o$ </td> <td> o (omicron) </td>
  </tr>
  <tr>
    <td> $\pi$ </td> <td> \pi </td>
    <td> $\rho$ </td> <td> \rho </td>
    <td> $\sigma$ </td> <td> \sigma </td>
    <td> $\tau$ </td> <td> \tau </td>
    <td> $\upsilon$ </td> <td> \upsilon </td>
  </tr>
  <tr>
    <td> $\phi$ </td> <td> \phi </td>
    <td> $\chi$ </td> <td> \chi </td>
    <td> $\psi$ </td> <td> \psi </td>
    <td> $\omega$ </td> <td> \omega </td>
    <td> </td> <td> </td>
  </tr>
  <tr>
    <td> $\varepsilon$ </td> <td> \varepsilon </td>
    <td> $\vartheta$ </td> <td> \vartheta </td>
    <td> $\varkappa$ </td> <td> \varkappa </td>
    <td> $\varpi$ </td> <td> \varpi </td>
    <td> $\varrho$ </td> <td> \varrho </td>
  </tr>
  <tr>
    <td> $\varphi$ </td> <td> \varphi </td>
    <td> $\varsigma$ </td> <td> \varsigma </td>
    <td>  </td> <td>  </td>
    <td>  </td> <td>  </td>
    <td>  </td> <td>  </td>
  </tr>
  <tr>
    <td> $A$ </td> <td> A </td>
    <td> $B$ </td> <td> B </td>
    <td> $\Gamma$ </td> <td> \Gamma </td>
    <td> $\Delta$ </td> <td> \Delta </td>
    <td> $E$ </td> <td> E </td>
  </tr>
  <tr>
    <td> $Z$ </td> <td> Z </td>
    <td> $H$ </td> <td> H </td>
    <td> $\Theta$ </td> <td> \Theta </td>
    <td> $I$ </td> <td> I </td>
    <td> $K$ </td> <td> K </td>
  </tr>
  <tr>
    <td> $\Lambda$ </td> <td> \Lambda </td>
    <td> $M$ </td> <td> \M </td>
    <td> $N$ </td> <td> \N </td>
    <td> $\Xi$ </td> <td> \Xi </td>
    <td> $O$ </td> <td> O </td>
  </tr>
  <tr>
    <td> $\Pi$ </td> <td> \Pi </td>
    <td> $P$ </td> <td> P </td>
    <td> $\Sigma$ </td> <td> \Sigma </td>
    <td> $T$ </td> <td> T </td>
    <td> $\Upsilon$ </td> <td> \Upsilon </td>
  </tr>
  <tr>
    <td> $\Phi$ </td> <td> \Phi </td>
    <td> $X$ </td> <td> X </td>
    <td> $\Psi$ </td> <td> \Psi </td>
    <td> $\Omega$ </td> <td> \Omega </td>
    <td>  </td> <td>  </td>
  </tr>
</table>

## 첨자

<table>
  <tr>
    <td> $\acute{a}$ </td> <td> \acute{a} </td>
    <td> $\grave{a}$ </td> <td> \grave{a} </td>
    <td> $\hat{a}$ </td> <td> \hat{a} </td>
    <td> $\check{a}$ </td> <td> \check{a} </td>
    <td> $\bar{a}$ </td> <td> \bar{a} </td>
  </tr>
  <tr>
    <td> $\dot{a}$ </td> <td> \dot{a} </td>
    <td> $\ddot{a}$ </td> <td> \ddot{a} </td>
    <td> $\tilde{a}$ </td> <td> \tilde{a} </td>
    <td> $\vec{a}$ </td> <td> \vec{a} </td>
    <td> $\breve{a}$ </td> <td> \breve{a} </td>
  </tr>
</table>

## 괄호

<table>
  <tr>
    <td> $($ </td> <td> ( </td>
    <td> $)$ </td> <td> ) </td>
    <td> $\lbrace$ </td> <td> \lbrace, \{ </td>
    <td> $\rbrace$ </td> <td> \rbrace, \} </td>
    <td> $[$ </td> <td> [ </td>
  </tr>
  <tr>
    <td> $]$ </td> <td> ] </td>
    <td> $\langle$ </td> <td> \langle </td>
    <td> $\rangle$ </td> <td> \rangle </td>
    <td> $\vert$ </td> <td> \vert </td>
    <td> $\Vert$ </td> <td> \Vert </td>
  </tr>
  <tr>
    <td> $/$ </td> <td> / </td>
    <td> $\backslash$ </td> <td> \backslash </td>
    <td> $\lfloor$ </td> <td> \lfloor </td>
    <td> $\rfloor$ </td> <td> \rfloor </td>
    <td> $\lceil$ </td> <td> \lceil </td>
  </tr>
  <tr>
    <td> $\rceil$ </td> <td> \rceil </td>
    <td> $\llcorner$ </td> <td> \llcorner </td>
    <td> $\lrcorner$ </td> <td> \lrcorner </td>
    <td> $\ulcorner$ </td> <td> \ulcorner </td>
    <td> $\urcorner$ </td> <td> \urcorner </td>
  </tr>
</table>