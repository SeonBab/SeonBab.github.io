---
layout: single

title: "[Algorithm] 다익스트라(Dijkstra)"

categories:
    - Algorithm
tag: [알고리즘]

date: 2025-04-21
last_modified_at: 2025-10-21

order : 3000
---

# 다익스트라

다익스트라(Dijkstra)은 가중치가 있는 그래프에서 한 정점으로부터 다른 모든 정점까지의 최단 경로를 찾는 알고리즘 중 대표적인 알고리즘입니다.

그리디 알고리즘의 일종입니다.  
핵심은 가장 비용이 적은 경로부터 탐색하고, 탐색 과정에서 경로 비용을 업데이트하면서 최단 경로를 찾습니다.

다음과 같이 동작합니다.

1. 시작 정점의 거리를 0으로 설정하고, 나머지 정점은 무한대로 설정
2. 현재 방문하지 않은 정점 중, 최단 거리가 가장 짧은 정점을 선택
3. 그 정점에서 인접한 정점으로의 거리를 계산하고, 더 짧은 경로가 있다면 갱신
4. 모든 정점을 방문할 때까지 반복

네비게이션, 경로 탐색, 네트워크 지연 시간 계산 등에 사용됩니다.

![Dijkstra-Animation]({{site.url}}/images/Algorithm/2025-04-21-Algorithm-Dijkstra/Dijkstra-Animation.gif)  
<cite>이미지 제작자: Ibmua, [출처](https://commons.wikimedia.org/wiki/File:Dijkstra_Animation.gif){: target="_blank"}, public domain</cite>
{: .small}

가장 짧은 경로를 탐색하는 과정에서 최소 비용의 정점을 빠르게 선택하는 것인데, 이를 효율적으로 처리하기 위해 힙(우선순위 큐)을 사용합니다.

만약 힙을 사용하지 않는다면 각 단계에서 $O(n)$의 시간 복잡도로 모든 정점 중 최소 비용의 정점을 찾아야 하며, 모든 정점을 방문할 때마다 최소 비용의 정점을 찾아야 하기 때문에 일반적인 경우 $O(n^2)$의 시간 복잡도를 가집니다.  
만약 힙을 사용한다면 항상 최소값 혹은 최대값을 빠르게 꺼낼 수 있기 때문에 최소 비용의 정점을 찾는 작업이 $O(log \ n)$의 시간 복잡도를 가집니다.  
모든 정점을 비교할 필요 없이 최소값을 가져오는데에 $O(1)$의 시간 복잡도를 가지지만, `Heapify`에 $O(log \ n)$이 걸리기 때문에 $O(log \ n)$의 시간 복잡도를 가집니다.  
결과적으로 전체 알고리즘의 시간 복잡도는 $O(E \ log \ V)$가 됩니다. (V와 E는 각각 노드, 간선의 개수를 의미)

BFS는 가중치가 없거나 동일한 경우에 사용하며, 다익스트라 알고리즘은 가중치가 양수이면서 값이 다른 경우 사용합니다.  
다익스트라 알고리즘은 가중치가 음수인 경우 적합하지 않으며, 이 경우 벨만 포드 알고리즘을 사용하는 것이 좋습니다.

다익스트라 알고리즘을 잘 설명해주는 EBS의 영상입니다.  
로그인 하면 무료 공개되어 있기 때문에 시청 할 수 있습니다.

[EBS, 링크 소프트웨어 세상 10회](https://www.ebs.co.kr/tv/show?prodId=116896&lectId=10363413&pageNum=4&srchType=0&srchText=&srchYear=&srchMonth=&vodProdId=#none){: target="_blank"}

## 예시

<details>
<summary><h5 style="display: inline;">인접 리스트와 우선순위 큐 사용</h5></summary>
<div markdown="1">

사용자에게 값을 입력 받아 1번 노드부터 각 정점까지의 최단 거리를 구합니다.

```cpp
#include <iostream>
#include <vector>
#include <queue>
#include <limits>

using namespace std;

// 무한대를 표현하기 위한 상수값 (최단 거리 초기화에 사용)
const int INF = numeric_limits<int>::max();

// 다익스트라 알고리즘 함수
void dijkstra(int start, const vector<vector<pair<int, int>>>& graph, vector<int>& dist)
{
    int V = graph.size() - 1; // 정점 개수 (1-indexed 기준)

    dist.assign(V + 1, INF);  // 거리 배열 초기화
    dist[start] = 0;          // 시작 정점까지의 거리는 0으로 설정

    // 최소 힙 (거리, 정점 번호)
    priority_queue<pair<int, int>, vector<pair<int, int>>, greater<>> pq;

    // 시작 정점을 우선순위 큐에 넣음
    pq.push({ 0, start });

    // 우선순위 큐가 빌 때까지 반복
    while (!pq.empty())
    {
        // 현재 가장 가까운 정점을 꺼내 현재 정점까지의 최단 거리와 정점 번호를 저장한다.
        int currentDist = pq.top().first;
        int current = pq.top().second;
        pq.pop();

        // 이미 더 짧은 거리로 방문한 적이 있으면 스킵
        if (currentDist > dist[current])
        {
            continue;
        }

        // 현재 정점에서 인접한 모든 정점 순회하며 탐색
        for (const auto& [next, weight] : graph[current])
        {
            // 현재 정점까지 거리 + 간선 가중치
            int newDist = currentDist + weight;

            // 더 짧은 경로를 발견한 경우 갱신
            if (newDist < dist[next])
            {
                // 최단 거리를 갱신하고 큐에 넣어 다시 탐색한다.
                dist[next] = newDist;
                pq.push({ newDist, next });
            }
        }
    }
}

int main()
{
    // 사용자로부터 정점 개수, 간선 개수, 시작 정점을 입력 받는다.
    int V, E, start;
    cout << "순서대로 정점 개수, 간선 개수, 시작 정점 입력" << endl;
    cin >> V >> E >> start;

    // 그래프 초기화
    vector<vector<pair<int, int>>> graph(V + 1);

    cout << "순서대로 출발정점, 도착정점, 가중치 입력" << endl;

    // 간선 정보에 대해 입력 받는다.
    for (int i = 0; i < E; ++i)
    {
        int u, v, w;
        cin >> u >> v >> w;

        // 방향 그래프로 u -> v, 가중치 w
        graph[u].emplace_back(v, w); 
    }

    // 최단 거리 결과를 저장할 벡터
    vector<int> dist;

    // 다익스트라 알고리즘 실행
    dijkstra(start, graph, dist);

    // 결과 출력
    for (int i = 1; i <= V; ++i)
    {
        // 해당 정점에 도달할 수 없는 경우
        if (dist[i] == INF)
        {
            cout << "INF" << endl;
        }
        // 도달 가능한 경우
        else
        {
            cout << dist[i] << endl;
        }
    }
}
```

입력값 예시

```
5 6 1
1 2 2
1 3 3
2 3 4
2 4 5
3 4 6
4 5 1
```

출력 예시

```
0
2
3
7
8
```

</div>
</details>