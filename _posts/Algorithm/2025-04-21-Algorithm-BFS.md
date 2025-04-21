---
layout: single

title: "[Algorithm] 너비 우선 탐색(Breadth First Search)"

categories:
    - Algorithm
tag: [알고리즘]

date: 2025-04-21
last_modified_at: 2025-04-21

order : 2010
---

# 너비 우선 탐색

너비 우선 탐색(Breadth First Search, BFS)은 그래프나 트리에서 같은 레벨에 있는 노드들을 먼저 방문한 후에 다음 레벨로 이동해 탐색하는 방식입니다.  
즉, 트리의 너비를 먼저 탐색하고, 그다음에 깊이를 탐색합니다.

탐색하는 과정을 시각화한다면 다음과 같습니다.

![Breadth_First_Search-BFS]({{site.url}}/images/Algorithm/2025-04-21-Algorithm-BFS/Breadth_First_Search-BFS.gif)  
<cite>이미지 제작자: Blake Matheny, [출처](https://commons.wikimedia.org/wiki/File:Animated_BFS.gif){: target="_blank"}, CC BY 3.0</cite>
{: .small}

일반적으로 BFS는 큐(Queue) 자료구조를 사용하여 구현합니다.  
큐는 내부적으로 `std::deque`를 사용하기 때문에 `std::deque`로 구현할 수도 있습니다.

BFS에서의 핵심은 노드를 탐색할 때는 먼저 방문한 노드의 자식들을 모두 큐에 넣고 그 자식들을 차례차례 탐색한다는 점입니다.  
이 과정에서 같은 레벨에 있는 노드들을 먼저 탐색하는 방식과 일치하게 됩니다.  
이를 큐로 구현하면 자연스럽게 노드가 레벨 순서대로 탐색됩니다.

- 장점
    - 모든 간선의 가중치가 동일한 경우 최단 경로를 보장합니다.
    - 레벨(거리)을 쉽게 계산할 수 있습니다.
    - 큐와 방문 배열을 사용하기 때문에 구현하기 쉽고, 구조가 직관적입니다.

- 단점
    - 큐에 노드를 계속 저장해야 하므로, 메모리 사용량이 많습니다.
    - 깊이가 깊은 문제에는 비효율적입니다.
    - 가중치가 있는 그래프에는 적합하지 않습니다.

대표적으로 인접 리스트나 인접 행렬을 표현할 때 사용할 수 있습니다.  
이외에는 다음과 같은 경우 효율적으로 사용할 수 있습니다.

+ 최단 거리 구하기 및 최단 이동 횟수 계산
+ 미로 탐색 문제
+ 그래프에서 연결 요소 찾기

최단 거리 및 최단 이동 횟수 계산에 용이한 이유는 다음과 같습니다.  
BFS는 루트에서 가장 가까운 노드부터 탐색하는 방법이기 때문에 루트에서부터 노드까지의 레벨이 낮을수록 경로가 짧습니다.  
다시 말해, 루트에서 해당 노드까지 가는 길의 깊이가 짧을수록 그 경로가 더 짧은 경로입니다.  
트리 기준으로 루트에서 레벨 1에 있는 노드들이 가장 가까운 경로를 가지며, 레벨 2에 있는 노드들는 그보다 더 먼 경로를 가지고 있습니다.

## 예시

### 무방향 그래프

```cpp
#include <iostream>
#include <vector>
#include <queue>

using namespace std;

void bfs(int start, const vector<vector<int>>& graph, vector<bool>& visited)
{
    // BFS를 위한 큐
    queue<int> q;

    // 시작 위치 저장 및 방문 처리
    q.push(start);
    visited[start] = true;

    // BFS 시작
    while (!q.empty())
    {
        // 현재 위치의 값
        int current = q.front();
        q.pop();

        // 출력
        cout << current << " ";

        // 그래프에 연결된 값 순회
        for (int next : graph[current])
        {
            // 방문하지 않은 경우
            if (!visited[next])
            {
                q.push(next);
                visited[next] = true;
            }
        }
    }
}

int main()
{
    int n = 6; // 노드 개수
    vector<vector<int>> graph(n + 1);

    // 무방향 그래프 간선 추가
    graph[1] = { 2, 3 };
    graph[2] = { 1, 4, 5 };
    graph[3] = { 1, 6 };
    graph[4] = { 2 };
    graph[5] = { 2 };
    graph[6] = { 3 };

    vector<bool> visited(n + 1, false);

    cout << "BFS 시작:" << std::endl;
    bfs(1, graph, visited); // 1번 노드부터 BFS 시작
}
```