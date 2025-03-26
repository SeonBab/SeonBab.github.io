---
layout: single

title: "[C++ Data Structure] 레드-블랙 트리"

categories:
    - DataStructure
tag: [Cpp, 자료구조]

date: 2025-02-05
last_modified_at: 2025-02-05

order : 1100
---

# 레드-블랙 트리

레드-블랙 트리(Red-Black Tree)는 자가 균형 이진 탐색 트리(self-balancing Binary Search Tree)의 한 종류입니다.

삽입 및 삭제 연산 후에도 트리의 균형을 유지하는 특성을 가집니다.  
탐색, 삽입, 삭제 연산이 평균 및 최악의 경우에도 $O(log N)$의 시간 복잡도를 가집니다.

![Red-Black_Tree]({{site.url}}/images/cpp\DataStructure\2025-02-05-RedBlackTree\Red-Black_Tree.PNG)

## 특징

일반적인 이진 탐색 트리(BST)를 따르면서, 5가지 규칙을 가집니다.

1. 각 노드는 빨간색 또는 검은색 중 하나입니다.
2. 루트(Root)노드는 항상 검은색입니다.
3. 모든 리프 노드는 검은색 입니다.
    + 리프 노드(NIL/null leaf 노드): 자료를 갖지 않고 트리의 끝을 나타내는 노드
4. 빨간색 노드의 자식 노드는 항상 검은색입니다.
    + 빨간색 노드는 연속해서 존재할 수 없습니다.
5. 어떤 노드에서든 루트 노드에서 시작하여 리프 노드까지 가는 모든 경로에는 같은 개수의 검은색 노드가 존재해야합니다.

## 데이터 추가

기본적으로 이진 트리 삽입 방식을 따릅니다.  
루트 노드보다 작다면 왼쪽 노드로 이동하고, 왼쪽 노드보다 크다면 오른쪽 자식으로 삽입됩니다.

새로운 노드를 삽입할 때에는 항상 빨간색으로 삽입됩니다.  
하지만 루트는 검은색이어야 하므로, 새 노드가 루트인 경우 검은색으로 변경합니다.

만약 삽입된 노드의 부모가 검은색일 경우 트리 규칙이 깨지지 않으므로 그대로 둡니다.

![Red-Black_Tree_Insert]({{site.url}}/images/cpp\DataStructure\2025-02-05-RedBlackTree\Red-Black_Tree_Insert.PNG)

이 경우 빨간색 노드가 2번 연속으로 나타나 빨간색 노드의 자식이 항상 검은색이어야한다는 규칙에서 벗어나게 됩니다.

이러한 `Double Red` 문제를 해결하기 위해 2가지 방법이 있습니다.

![Red-Black_Tree_DoubleRed]({{site.url}}/images/cpp\DataStructure\2025-02-05-RedBlackTree\Red-Black_Tree_DoubleRed.PNG)

조상 노드를G(Grand Parent), 부모 노드를 P(Parent), 삼촌 노드를 U(Uncle), 새로 삽입할 노드를 N(New)라고 하겠습니다.

삼촌 노드가 검은색이라면 `Restructuring`를 수행합니다.  
삼촌 노드가 빨간색이라면 `Recoloring`를 수행합니다.

### Restructuring

`Restructuring`은 트리의 균형을 맞추기 위해 특정 노드를 회전(Rotation)하여 재구정하는 과정을 말합니다.

조부모 노드, 부모 노드, 삽입된 노드의 상대적인 위치에 따라서 단순 회전(Single Rotation)과 이중 회전(Double Rotation)을 사용하게 됩니다.

단순 회전은 노드가 일직선 형태일 때 수행됩니다.

부모 노드가 오른쪽 자식을 가지는 경우로, 조부모, 부모, 삽입된 노드가 모두 오른쪽 방향(Right Rotation)으로 일직선이라면 수행됩니다.

부모 노드가 왼쪽 자식을 가지는 경우로, 조부모, 부모, 삽입된 노드가 모두 왼쪽 방향(Left Rotation)으로 일직선이라면 수행됩니다.

![Red-Black_Tree_Single_Rotation]({{site.url}}/images/cpp\DataStructure\2025-02-05-RedBlackTree\Red-Black_Tree_Single_Rotation.PNG)

영상으로는 다음과 같습니다.

![Red-Black_Tree_Single_Rotation2]({{site.url}}/images/cpp\DataStructure\2025-02-05-RedBlackTree\Red-Black_Tree_Single_Rotation2.gif)

이중 회전은 트리가 일직선이 아닌 지그재그(꺾인)인 형태일 때 수행됩니다.  
한 번의 회전만으로 균형을 맞출 수 없을 때 두 번의 회전을 수행하는 방식입니다.

![Red-Black_Tree_Double_Rotation]({{site.url}}/images/cpp\DataStructure\2025-02-05-RedBlackTree\Red-Black_Tree_Double_Rotation.PNG)

### Recoloring

`Recoloring`은 트리의 구조는 그대로 유지하면서 색상만 변경하여 균형을 맞추는 작업입니다.  
조부모 노드, 부모 노드, 삼촌 노드의 색상을 조정하여 트리 속성을 복원하는 과정입니다.

부모와 삼촌을 검은색으로 변경하고, 조부모를 빨간색으로 변경합니다.

![Red-Black_Tree_Recoloring]({{site.url}}/images/cpp\DataStructure\2025-02-05-RedBlackTree\Red-Black_Tree_Recoloring.PNG)

## 데이터 삭제

노드를 삭제하면 균형이 깨질 수 있으며, 이를 복구하기 위해 추가적인 색 변환과 회전이 필요합니다.  
삭제 시에도 데이터 추가와 같은 회전과 색 변환이 이루어집니다.