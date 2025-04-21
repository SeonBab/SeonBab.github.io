---
layout: single

title: "[Algorithm] 깊이 우선 탐색(Depth First Search)"

categories:
    - Algorithm
tag: [알고리즘]

date: 2025-04-10
last_modified_at: 2025-04-21

order : 2000
---

# 깊이 우선 탐색

깊이 우선 탐색(Depth First Search, DFS)는 트리나 그래프를 최대한 깊은 노드를 먼저 탐색하고, 더 이상 갈 곳이 없을 때 되돌아오면서 다른 경로를 찾아 탐색하는 방법입니다.  
즉, 아래의 그림처럼 한 경로를 끝까지 탐색한 후에 되돌아와 다른 경로로 이동합니다.

![Depth-First-Search_DFS]({{site.url}}/images/Algorithm/2025-04-10-Algorithm-DFS/Depth-First-Search_DFS.gif)  
<cite>이미지 제작자: Mre, [출처](https://commons.wikimedia.org/wiki/File:Depth-First-Search.gif){: target="_blank"}, CC BY 3.0</cite>
{: .small}

DFS는 재귀 호출이나 스택 자료구조를 이용해서 구현할 수 있습니다.  
트리에서 DFS를 수행할 때는 루트 노드에서 시작해서 한쪽으로 최대한 깊이 들어간 후에 자식 노드를 모두 방문한 후에 다시 부모로 돌아와 다른 자식을 방문하는 방식입니다.  
이런 순회 방식을 전위 순회(Pre-order traverse)라고 합니다.

그래프의 모든 노드과 간선을 한 번씩 방문하므로 $O(V + E)$의 시간 복잡도를 가집니다.  
V는 노드 수, E는 간선 수를 의미합니다.

트리의 깊이를 먼저 탐색하기 때문에 깊이 우선 탐색이 유리한 문제에 적합하며, 다음과 같은 경우에 적합합니다.

+ 모든 경우의 수(혹은 모든 경로)를 찾는 경우
    - 백트래킹 기반의 조합/순열
+ 미로 탐색을 해결하는 데 유리합니다.
    - 처리 순서가 매우 직관적이어서 전위 순회를 많이 사용합니다.
+ 위상 정렬
+ 트리 순회

+ 장점
    - 정답이 그래프의 깊은 지점에 존재하는 경우 효율적입니다.
    - 재귀 깊이가 너무 깊지 않은 경우 메모리 사용량이 적어 효율적입니다.
+ 단점
    - 최단 경로가 필요한 문제에는 BFS가 적합합니다.
    - 재귀로 구현할 시 트리의 깊이가 매우 깊으면 스택 오버플로우가 발생할 수 있습니다.
    - 방문한 노드를 중복 방문하지 않도록 처리를 해주어야 하며, 이를 위해 추가적인 배열을 사용하기도 합니다.

## 예시

### 재귀 호출

```cpp
#include <iostream>
#include <vector>

using namespace std;

// 그래프를 인접 리스트 형태로 저장할 벡터
vector<vector<int>> graph;
// 방문한 노드를 표시하는 배열
vector<bool> visited;

// 깊이 우선 탐색 함수
void dfs(int node)
{
    // 현재 노드를 방문 처리
    visited[node] = true;

    cout << node << ' ';

    // 현재 노드와 연결된 인접 노드들을 순회
    for (int next : graph[node])
    {
        // 아직 방문하지 않은 노드라면 재귀적으로 DFS 수행
        if (!visited[next])
        {
            dfs(next);
        }
    }
}

int main()
{
    int n = 6; // 노드 개수

    // 그래프와 방문 배열의 크기를 초기화
    graph.resize(n + 1);
    visited.resize(n + 1, false);

    // 예시 그래프 구성 (무방향)
    graph[1] = {2, 3};
    graph[2] = {1, 4, 5};
    graph[3] = {1};
    graph[4] = {2};
    graph[5] = {2, 6};
    graph[6] = {5};

    cout << "DFS 탐색 순서: ";
    dfs(1); // 1번 노드부터 탐색 시작
}
```

출력은 다음과 같습니다.

```
DFS 탐색 순서: 1 2 4 5 6 3
```

### 스택

```cpp
#include <iostream>
#include <vector>
#include <stack>

using namespace std;

// 스택을 사용한 깊이 우선 탐색 함수
void dfsIterative(int start, vector<vector<int>>& graph)
{
    // 방문 여부를 저장하는 배열
    vector<bool> visited(graph.size(), false);
    // DFS에 사용할 스택 선언
    stack<int> s;

    // 시작 노드를 스택에 삽입
    s.push(start);

    // 스택이 비게될 때까지 반복
    while (!s.empty())
    {
        // 현재 탐색할 노드를 꺼낸다.
        int current = s.top();
        // 스택에서 제거
        s.pop();

        // 아직 방문하지 않은 노드인 경우
        if (!visited[current])
        {
            // 현재 노드를 방문 처리
            visited[current] = true;

            cout << current << ' ';

            // 인접 노드들을 스택에 넣는다.
            // 스택은 후입선출(LIFO) 구조이므로, 인접 노드들을 역순으로 넣어야 원래 연결 순서대로 탐색됩니다.
            for (int i = graph[current].size() - 1; i >= 0; --i)
            {
                int next = graph[current][i];
                // 아직 방문하지 않은 노드인 경우
                if (!visited[next])
                {
                    s.push(next);
                }
            }
        }
    }
}

int main()
{
    int n = 6; // 노드 개수

    // 그래프를 인접 리스트 형태로 저장할 벡터
    vector<vector<int>> graph(n + 1);

    // 예제 그래프 구성 (무방향)
    graph[1] = { 2, 3 };
    graph[2] = { 1, 4, 5 };
    graph[3] = { 1 };
    graph[4] = { 2 };
    graph[5] = { 2, 6 };
    graph[6] = { 5 };

    cout << "DFS Iterative 탐색 순서: ";
    dfsIterative(1, graph); // 1번 노드에서 시작
}
```

출력은 다음과 같습니다.

```
DFS Iterative 탐색 순서: 1 2 4 5 6 3
```