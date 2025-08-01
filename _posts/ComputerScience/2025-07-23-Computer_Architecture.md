---
layout: single

title: "[ComputerScience] 컴퓨터 구조"

categories:
    - ComputerScience
    
tag: [컴퓨터 과학, ComputerScience]

date: 2025-07-23
last_modified_at: 2025-07-23

order : 1000
---

# 컴퓨터 구조

컴퓨터 구조는 CPU, 메모리, 저장장치, 버스, 캐시 등 하드웨어의 동작 원리에 대한 지식으로 구성됩니다.

효율적인 프로그램과 성능 최적화를 위해 필수적으로 알고있어야하는 지식입니다.

## 컴퓨터가 이해하는 정보

소스 코드는 컴퓨터가 이해할 수 없기 때문에, 컴파일러나 인터프리터를 거쳐 컴퓨터가 이해할 수 있는 기계어(Machine Code)나 중간 언어로 변환됩니다.  

컴퓨터가 실제로 이해하는 것은 0과 1로 이루어진 명령어(Instruction)이며, 이 명령어는 보통 두 가지 요소로 구성됩니다.

1. 수행할 동작(Operation Code, Opcode)
    - 데이터를 활용하는 정보로 어떤 연산을 할지 결정
    - CPU가 명령어를 이해하고 실행하는 주체
    - 명령어 사이클이 존재하며, CPU가 명령어를 처리하는 순서를 의미
2. 수행할 대상(Operands)
    - 연산에 사용할 데이터나 메모리 주소, 레지스터 등(예: 숫자, 문자, 이미지, 동영상과 같은 정보)
    - 컴퓨터와 주고받는 정보나 컴퓨터에 저장된 정보 자체를 데이터라고 통칭