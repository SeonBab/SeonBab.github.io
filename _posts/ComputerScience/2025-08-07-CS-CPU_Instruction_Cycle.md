---
layout: single

title: "[ComputerScience] CPU 명령어 사이클"

categories:
    - ComputerScience
    
tag: [컴퓨터 과학, ComputerScience]

date: 2025-08-07
last_modified_at: 2026-04-07

order : 2700
---

# CPU 명령어 사이클

명령어 사이클(Instruction Cycle)은 CPU가 하나의 명령어를 처리하는 전체 과정을 의미합니다.  

하나의 명령어는 일반적으로 다음과 같은 단계들을 거칩니다.

1. 인출 사이클(Fetch Cycle)
    - 메모리에 저장된 명령어를 CPU로 가져오는 단계입니다.
    - 프로그램 카운터(PC)에 의해 지정된 명령어 주소에서 명령어를 읽어옵니다.
2. 간접 사이클(Indirect Cycle)
    - 명령어에 포함된 오퍼랜드가 메모리 주소를 나타낼 경우, 해당 주소를 통해 데이터를 한 번 더 메모리에서 읽어오는 단계입니다.
3. 실행 사이클(Execution Cycle)
    - CPU가 명령어를 해석하고, 지정된 연산을 수행하는 단계입니다.
4. 인터럽트 사이클(Interrupt Cycle)
    - 프로그램 실행 도중 인터럽트가 발생한 경우, CPU가 현재 작업을 일시 중지하고 인터럽트를 처리하는 단계입니다.

명령어 사이클은 컴퓨터 아키텍처 및 명령어 형식(1주소, 2주소, 3주소 등)에 따라 세부 구조가 달라질 수 있습니다.
