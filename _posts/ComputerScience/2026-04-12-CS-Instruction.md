---
layout: single

title: "[ComputerScience] 명령어"

categories:
    - ComputerScience
    
tag: [컴퓨터 과학, ComputerScience]

date: 2026-04-12
last_modified_at: 2026-04-12

order : 2100
---

# 명령어

컴퓨터에서 명령어(Instruction)는 수행할 동작(Operation)과 수행할 대상(Operand)으로 구성됩니다.  
프로그램 실행의 가장 기본적인 단위입니다.

![Instruction_Example1]({{site.url}}/images/ComputerScience/2026-04-12-CS-Instruction/Instruction-Instruction_Example1.PNG)  
<cite>출처: 강민철, 『혼자 공부하는 컴퓨터 구조+운영체제』, 한빛미디어, 2022, 91쪽 </cite>
{: .small}


![Instruction_Example2]({{site.url}}/images/ComputerScience/2026-04-12-CS-Instruction/Instruction-Instruction_Example2.PNG)  
<cite>출처: 강민철, 『혼자 공부하는 컴퓨터 구조+운영체제』, 한빛미디어, 2022, 91쪽 </cite>
{: .small}

## 연산 코드

연산 코드(Opcode)는 명령어가 수행할 동작 또는 연산의 종류를 지정합니다.  
연산 코드를 연산자라고 부르기도 합니다.

연산 코드의 종류는 크게 4가지로 데이터 전송, 산술/논리 연산, 제어 흐름 변경, 입출력 제어 등의 유형이 있습니다.

연산 코드가 담긴 영역을 연산코드 필드라고 부릅니다.

## 오퍼랜드

오퍼랜드(Operand)는 연산에 사용될 데이터 또는 데이터가 저장된 주소를 나타냅니다.  
즉, 연산 대상(피연산자)를 의미하며 그 자체가 값일 수도 있고, 메모리 상의 주소 혹은 레지스터 주소일 수도 있습니다.

오퍼랜드는 명령어 안에 하나도 없을 수도 있고, 한 개 혹은 두 개 이상일 수 있습니다.  
오퍼랜드의 명령어 수마다 0-주소 명령어, 1-주소 명령어, 2-주소 명령어, 3-주소 명령어라고 부릅니다.

![Instruction-Format]({{site.url}}/images/ComputerScience/2026-04-12-CS-Instruction/Instruction-Format.PNG)  
<cite>출처: 강민철, 『혼자 공부하는 컴퓨터 구조+운영체제』, 한빛미디어, 2022, 93쪽 </cite>
{: .small}

오퍼랜드가 위치한 메모리 주소를 오퍼랜드 필드 혹은 주소 필드(Address Field)라고 부릅니다.

명령어에서 오퍼랜드 필드를 살펴보면, 실제 데이터가 아닌 메모리 주소나 레지스터 주소가 들어가는 경우가 많습니다.  
이는 명령어 길이(비트 수)에 제한이 있기 때문입니다.  
예를 들어, 전체 명령어 크기가 16비트이고 연산 코드가 4비트라면, 오퍼랜드에 사용할 수 있는 비트는 12비트가 됩니다.  
그런데, 2-주소 명령어의 경우, 각 오퍼랜드에는 6비트가 할당되며, 3-주소 명령어의 경우, 각 오러팬드에는 4비트가 할당됩니다.

![Bit_Layout]({{site.url}}/images/ComputerScience/2026-04-12-CS-Instruction/Instruction-Bit_Layout.PNG)  
<cite>출처: 강민철, 『혼자 공부하는 컴퓨터 구조+운영체제』, 한빛미디어, 2022, 95쪽 </cite>
{: .small}

이처럼 각 오퍼랜드가 표현할 수 있는 값의 범위가 너무 작으면, 데이터 자체를 저장하기보다 주소를 저장하고 이를 통해 데이터를 간접적으로 다루게 됩니다.

여기서 오퍼랜드의 값이 직접 데이터인지, 아니면 주소를 통한 간접 접근인지는 주소 지정 방식으로 정의됩니다.

오퍼랜드가 직접 값일 경우 즉시(Inmediate), 메모리 주소일 경우 주소를 통해 메모리에서 인출(Fetch)해야 합니다.

### 주소 지정 방식

주소 지정 방식(Addressing Mode)은 오퍼랜드 필드에 데이터가 저장된 위치를 명시할 때 사용할 데이터 위치를 찾는 방법을 의미합니다.

연산 코드에 사용될 데이터가 저장된 위치, 즉 연산의 대상이 되는 데이터가 저장된 위치를 유효 주소(Effective Address)라고 합니다.

#### 즉시 주소 지정 방식

즉시 주소 지정 방식(Immediate Addressing Mode)는 연산에 사용할 데이터를 오퍼랜드 필드에 직접 명시하는 방식입니다.

![Immediate_Addressing]({{site.url}}/images/ComputerScience/2026-04-12-CS-Instruction/Instruction-Immediate_Addressing.PNG)  
<cite>출처: 강민철, 『혼자 공부하는 컴퓨터 구조+운영체제』, 한빛미디어, 2022, 97쪽 </cite>
{: .small}

가장 간단한 형태의 주소 지정 방식입니다.

데이터의 크기가 작아지는 단점이 있지만, 연산에 사용할 데이터를 메모리나 레지스터로부터 찾는 과정이 없기 때문에 다른 주소 지정 방식들보다 빠릅니다.

#### 직접 주소 지정 방식

직접 주소 지정 방식(Direct Addressing Mode)는 오러팬드 필드에 유효 주소를 직접적으로 명시하는 방식입니다.

![Direct_Addressing]({{site.url}}/images/ComputerScience/2026-04-12-CS-Instruction/Instruction-Direct_Addressing.PNG)  
<cite>출처: 강민철, 『혼자 공부하는 컴퓨터 구조+운영체제』, 한빛미디어, 2022, 97쪽 </cite>
{: .small}

오퍼랜드 필드에서 표현할 수 있는 데이터의 크기는 즉시 주소 지정 방식보다 더 커졌지만, 여전히 유효ㅛ 주소를 표현할 수 있는 범위가 연산 코드의 비트 수만큼 줄어들었습니다.  
이는 표현할 수 있는 오퍼랜드 필드의 길이가 연산 코드의 길이만큼 짧아져 표현할 수 있는 유효 주소에 제한이 생길 수 있습니다.

#### 간접 주소 지정 방식

간접 주소 지정 방식(Indirect Addresing Mode)는 유효 주소의 주소를 오퍼랜드 필드에 명시합니다.

![Indirect_Addressing]({{site.url}}/images/ComputerScience/2026-04-12-CS-Instruction/Instruction-Indirect_Addressing.PNG)  
<cite>출처: 강민철, 『혼자 공부하는 컴퓨터 구조+운영체제』, 한빛미디어, 2022, 98쪽 </cite>
{: .small}

직접 주소 지정 방식보다 표현할 수 있는 유효 주소의 범위가 더 넓어졌지만 두 번의 메모리 접근이 필요하기 때문에 상대적으로 느린 방식입니다.

#### 레지스터 주소 지정 방식

레지스터 주소 지정 방식(Register Addressing Mode)은 직접 주소 지정 방식과 비슷하게 연산에 사용할 데이터를 저장한 레지스터를 오퍼랜드 필드에 직접 명시하는 방법입니다.

![Register_Addressing]({{site.url}}/images/ComputerScience/2026-04-12-CS-Instruction/Instruction-Register_Addressing.PNG)  
<cite>출처: 강민철, 『혼자 공부하는 컴퓨터 구조+운영체제』, 한빛미디어, 2022, 98쪽 </cite>
{: .small}

CPU 외부의 메모리에 접근하는 것보다 CPU 내부의 레지스터에 접근하는 것이 더 빠르기 때문에 직접 주소 지정 방식보다 빠르게 데이터에 접근할 수 있습니다.

직접 주소 지정 방식과 비슷한 문제를 공유하는데, 표현할 수 있는 레지스터 크기에 제한이 생길 수 있다는 문제가 있습니다.

#### 레지스터 간접 주소 지정 방식

레지스터 간접 주소 지정 방식(Register Indirect Addressing Mode)는 연산에 사용할 데이터를 메모리에 저장하고, 그 주소(유효 주소)를 저장한 레지스터를 오퍼랜드 필드에 명시하는 방법입니다.

![Register_Indirect_Addressing]({{site.url}}/images/ComputerScience/2026-04-12-CS-Instruction/Instruction-Register_Indirect_Addressing.PNG)  
<cite>출처: 강민철, 『혼자 공부하는 컴퓨터 구조+운영체제』, 한빛미디어, 2022, 99쪽 </cite>
{: .small}

간접 주소 지정 방식과 비슷하지만, 메모리에 접근하는 횟수가 한 번으로 줄어든다는 차이이자 장점을 가집니다.

메모리에 접근하는 것보다 레지스터에 접근하는 것이 더 빠르기 때문에 레지스터 간접 주소 지정 방식은 간접 주소 지정 방식보다 빠릅니다.
