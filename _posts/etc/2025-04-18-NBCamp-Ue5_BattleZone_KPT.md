---
layout: single

title: "내배캠 언리얼 엔진 프로그래밍 대난투 기반 팀 프로젝트 KPT"

categories:
    - etc
tag: [etc]

date: 2025-04-18
last_modified_at: 2025-04-18

order : 50
---

# KPT

KPT 회고란 다음과 같습니다.

+ Keep: 현재 만족하고 있고, 계속 이어갔으면 하는 부분
+ Problem: 불편하게 느낀 부분, 개선이 필요하다고 생각되는 부분
+ Try:
    - 잘하고 있는 부분을 더 잘하거나 유지하기 위한 액션
    - Problem에 대한 해결책
    - 다음 회고에서 해결 여부를 확인할 수 있는 기준
    - 당장 실행 가능한 액션

해당 프로젝트를 진행한 팀원들은 김선국, 김강현, 김인겸, 박선우, 이수빈님으로 각각 KPT를 하나씩 제시했습니다.

## Keep - 현재 만족하고 있는 부분

- 김선국
    - 부족한 시간과 인력인데, 노력해서 최선을 다해준 팀원들이 고맙습니다.
    - 전체적인 구상과 팀원 일정 및 작업 관리와 문서 작성 경험은 좋은 경험이었습니다.
    - 다양하게 버그를 수정해보고, 최적화에 고민을 해볼 수 있는 좋은 프로젝트 였습니다.
- 김강현
    - 혼자 하는 것보다는 계속해서 주변에 물어보면서 한 것
    - 디버그 실험과 테스트를 통해 해결 방향 탐색
- 김인겸
    - 팀원들 간 소통이 잘 이루어졌고, 서로의 작업에 관심을 가지고 도우려는 분위기가 좋았음.
    - 특히 어려운 문제가 생겼을 때 함께 모여서 고민하고 해결책을 찾는 협업 태도가 돋보였음.
    - 작업 중에도 지속적인 의견 교환이 이루어져 전반적인 협력 과정이 원활하게 진행되었음.
- 박선우
    - 팀원 간 의사소통이 잘 진행되었음
    - 협업이 잘 이루어진 것 같음. 
    - 서로 역할을 잘 나누고 피드백도 주고받으며 무리 없이 진행됨.
- 이수빈
    - 분야별로 서로를 도우며 협업을 원활하게 진행했습니다.
    - 작업 중 오류가 생겼을 때 팀원들이 함께 해결하려는 태도를 보였고, 필요한 경우 튜터님께 바로 도움을 요청했습니다.
    - 일정 관리와 작업 분배가 비교적 잘 이루어져 마감기한을 지킬 수 있었습니다.

## Problem - 불편하게 느끼는 부분

- 김선국
    - 코드 컨벤션과 애셋 명명 규칙 등의 프로젝트 컨벤션이 잘 지켜지지 않았던 부분이 있어서 아쉽습니다.
    - 팀 전체적으로 기술적인 부족함이 느껴져서 아쉽습니다.
    - 팀의 인력에 공백이 크게 느껴져서 아쉽습니다.
- 김강현
    - 팀원 간 의사소통이 지연되면서 구현 속도에 영향
    - 기술적 문제로 지속적으로 튜터님한테 의지한 것
    - Run on Server, Multicast 사용해도 동기화가 안되었던 점
    - 할게 많아서 문서 작성을 소홀히 한점
- 김인겸
    - 일부 기능에서 해결되지 않은 버그가 남아 있었고, 이를 마감 직전까지 완전히 잡지 못한 부분이 있었음.
    - 테스트가 충분히 이루어지지 않아 사용 중 발견되는 오류가 있었고, 이로 인해 완성도에 영향을 줌.
    - 디버깅 과정에서 시간 소요가 컸고, 예상보다 작업 흐름에 지장을 준 경우가 있었음.
- 박선우
    - 튜터님께 많이 의지하게 된 모습이 아쉬움(조금 더 공부할 필요)
    - 초반 기획을 철저하게 빨리 끝내고 작업에 더 집중했으면 좋았을 것 같음
    - 기술적인 지식이 부족한 만큼 개발 단계에서 시간을 더 사용해야 할 것 같음.
- 이수빈
    - 팀 내 소통이 부족했던 시점이 있었고, 특히 초반 기획 단계에서 의사결정이 늦어지는 문제가 있었음.
    - 역할 분담 후 각자의 작업에 집중하다 보니 중간 점검과 피드백이 부족했던 부분이 있었음.
    - 일부 기술적인 문제나 기능 구현 방향에 대해 명확히 정리되지 않아 혼선이 있었음.

## Try - Problem에 대한 해결책, 당장 실행 가능한 것

- 김선국
    - 다음에는 코드 컨벤션을 조금만 더 신경써서 작업하면 해결 할 수 있다고 생각합니다.
    - 다른 사람의 코드를 보며 코드 리뷰 시간을 가져 코드를 개선하는 것도 좋을 것 같습니다.
    - 팀 프로젝트 기간에는 프로젝트에 집중하면 인력적으로나 부족한 기술을 어느정도 보완할 수 있을 것 같습니다.
- 김강현
    - 반복되는 로직을 함수화 또는 매크로로 분리하여 유지보수 및 디버깅 효율 향상할 것
    - 다음부터 각종 흐름을 시각화하여 팀원들과 공유
    - 리눅스를 사용한 자체적인 서버 구축
    - 블루프린트가 아닌 c++ 로직 구현
- 김인겸
    - 초기 단계에서 역할을 좀 더 명확히 나누고, 각자 맡은 부분에 대한 책임 범위를 확실히 정할 필요가 있음.
    - 파트별 연결되는 작업은 사전에 조율해 충돌이 없도록 하고, 중간 점검을 통해 분담이 잘 작동하는지 확인할 것.
    - 전체 일정 내에서 역할 분담 외에도 크로스체크할 여유 시간을 확보해 협업의 빈틈을 줄여갈 계획임.
- 박선우
    - 다음에는 더 구체적인 일정 계획이 필요함(맡은 파트 안에서도 자세한 계획)
    - 본인 파트 진행 상황에 대한 세세한 보고서를 작성해보고싶음
- 이수빈
    - 기획 초반에 전체적인 흐름과 역할을 명확히 정하고, 각 파트별 연결 지점을 구체적으로 정리할 것.
    - 정기적인 중간 점검 시간을 마련해 진행 상황을 공유하고 즉각적인 피드백을 주고받을 것.
    - 주요 기술 이슈나 문제 상황은 슬랙/노션 등을 활용해 문서화하고, 해결 방법을 공유하는 문화 만들기.