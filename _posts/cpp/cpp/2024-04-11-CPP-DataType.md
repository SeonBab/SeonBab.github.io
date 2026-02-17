---
layout: single

title: "[C++] 자료형"

categories:
    - Cpp
tag: [Cpp]

date: 2024-04-11
last_modified_at: 2024-04-26

order : 1
---

# 자료형

자료형(Data Type)은 특정 목적에 맞는 자료를 저장하기 위해 만든 형식(format, layout)을 말하며, 프로그래머가 자료(data)를 사용하려는 의도를 컴파일러나 인터프리터에게 알려주는 자료의 속성입니다.

자료형은 크게 두가지로 구분한다. 하나는 프로그래밍 언어가 제공하는 기본 자료형(char, int, double, ...) 다른 하나는 개발자가 필요에 의해 만드는 사용자 정의 자료형입니다. 사용자 정의 자료형은 기본 자료형 외 class, struct, union 등으로 만든 모든 것입니다.

+ 수(수치, 값)는 양을 말하고, 숫자는 수를 나타내는 기호입니다.
- C++에서는 일반적으로 기본 자료형으로 정의한 것을 변수, 사용자 정의 자료형으로 정의한 것을 객체라고 합니다.
  - 하지만 객체는 변수를 포함하므로 일반적인 설명을 위해 객체라는 용어를 더 많이 사용합니다.

## 기본 자료형

하드웨어의 자원(resources)이 유한하기 때문에 프로그래밍 언어에서 사용하는 자료형으로 표현 가능한 수치 또한 유한합니다.

기본 자료형(primitive data types)은 다음과 같습니다.

<table>
    <tr>
        <td>데이터 형태(자료형)</td>
        <td>자료형</td>
        <td>설명</td>
        <td>메모리 공간</td>
        <td>범위</td>
    </tr>
    <tr>
        <td>문자형(정수형)</td>
        <td>char (character)</td>
        <td>문자 및 정수</td>
        <td>1바이트</td>
        <td>-128 ~ 127</td>
    </tr>
    <tr>
        <td rowspan="4">정수형</td>
        <td>short</td>
        <td>정수값 저장</td>
        <td>2바이트</td>
        <td>-32,768 ~ 32,767</td>
    </tr>
    <tr>
        <td>int (integer)</td>
        <td>정수값 저장</td>
        <td>4바이트</td>
        <td>-2,147,483,648 ~ 2,147,483,647</td>
    </tr>
    <tr>
        <td>long</td>
        <td>정수값 저장</td>
        <td>8바이트</td>
        <td>-9,223,372,036,854,775,808 ~ 9,223,372,036,854,775,807</td>
    </tr>
    <tr>
        <td>long long</td>
        <td>정수값 저장</td>
        <td>8바이트</td>
        <td>-9,223,372,036,854,775,808 ~ 9,223,372,036,854,775,807</td>
    </tr>
    <tr>
        <td rowspan="3">실수형</td>
        <td>float</td>
        <td>단일 정밀도 부동소수점</td>
        <td>4바이트</td>
        <td>1.17549e-38 ~ 3.40282e+38</td>
    </tr>
    <tr>
        <td>double</td>
        <td rowspan="2">두배 정밀도 부동 소수점</td>
        <td>8바이트</td>
        <td>2.22507e-308 ~ 1.79769e+308</td>
    </tr>
    <tr>
        <td>long double</td>
        <td>16바이트</td>
        <td>3.3621e-4932 ~ 1.18973e+4932</td>
    </tr>
    <tr>
        <td>불형</td>
        <td>bool (boolean)</td>
        <td>참/거짓 표현</td>
        <td>1바이트</td>
        <td>true,false</td>
    </tr>
</table>

### 비트와 바이트

비트(bit)란 컴퓨터가 데이터를 처리하기 위해 사용하는 데이터의 최소 단위입니다.
이러한 비트에는 2진수의 값을 단 하나만 저장할 수 있습니다.
바이트(byte)란 위와 같은 비트가 8개 모여서 구성되며, 한 문자를 표현할 수 있는 최소 단위입니다.

## 사용자 정의 자료형

사용자 정의 자료형(User-Defined Types)은 용어 그대로 개발자가 직접 자료형을 선언하고 정의하여 사용하는 자료형입니다.

사용자 정의 자료형은 enum, enum class, struct, union, class 그리고 자료형의 별칭을 지정하는 typedef와 using이 있습니다.  
C++11부터는 typedef를 using으로 사용하기를 권장합니다.

C++에서 struct와 class는 동일하게 취급하지만 C 스타일 struct를 지원하는 기능을 유지하고 있어 다른 부분이 있습니다.