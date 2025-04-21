---
layout: single

title: "[Algorithm] 힙 정렬"

categories:
    - Algorithm
tag: [알고리즘]

date: 2025-04-21
last_modified_at: 2025-04-21

order : 100
---

# 힙 정렬

힙 정렬(Heap Sort)은 힙을 사용해서 데이터를 정렬하는 알고리즘입니다.  
최대 힙 또는 최소 힙을 이용해 데이터를 정렬하는데, 일반적으로 최대 힙을 사용해 오름차순 정렬을 수행합니다.

완전 이진 트리 구조를 사용해 정렬을 수행합니다.

정렬을 위한 추가 메모리 공간이 필요하지 않아 $O(1)$의 공간 복잡도를 가지는 제자리 정렬입니다.

안정 정렬은 아니기 때문에 같은 값을 가진 요소의 상대 순서가 바뀔 수 있습니다.

시간 복잡도는 최선, 평균, 최악 모두 $O(n \ log \ n)$입니다.

정렬되는 과정을 시각적으로 살펴보면 다음과 같습니다.

![Heap_Sort-example]({{site.url}}/images/Algorithm/2025-04-21-Algorithm-Heap_Sort/Heap_Sort-example.gif)  
<cite>이미지 제작자: Nagae, [출처](https://commons.wikimedia.org/wiki/File:Heap_sort_example.gif){: target="_blank"}, CC BY 3.0</cite>
{: .small}

## 예시

<details>
<summary><h5 style="display: inline;">벡터를 힙 정렬하는 예시</h5></summary>
<div markdown="1">

벡터를 힙 구조로 변환하고, 힙 정렬을 하는 예시는 다음과 같습니다.

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

int main()
{
    std::vector<int> v = { 10, 30, 5, 15, 20, 25 };

    // 벡터를 힙으로 만들기
    std::make_heap(v.begin(), v.end());

    // 힙 상태 출력
    std::cout << "힙 상태: ";
    for (int num : v)
    {
        std::cout << num << " ";
    }
    std::cout << std::endl;

    // 힙 정렬 수행
    std::sort_heap(v.begin(), v.end());

    // 정렬된 결과 출력
    std::cout << "정렬된 상태: ";
    for (int num : v)
    {
        std::cout << num << " ";
    }
}
```

</div>
</details>

<details>
<summary><h5 style="display: inline;">힙 정렬 직접 구현 예시</h5></summary>
<div markdown="1">

```cpp
#include <iostream>
#include <vector>

using namespace std;

// i를 루트로 하는 서브트리를 힙 조건을 만족하도록 조정
void heapify(vector<int>& arr, int n, int i)
{
    int largest = i;          // 루트 인덱스
    int left = 2 * i + 1;     // 왼쪽 자식 인덱스
    int right = 2 * i + 2;    // 오른쪽 자식 인덱스

    // 왼쪽 자식이 루트보다 크면 largest 갱신
    if (left < n && arr[left] > arr[largest])
    {
        largest = left;
    }

    // 오른쪽 자식이 가장 크면 largest 갱신
    if (right < n && arr[right] > arr[largest])
    {
        largest = right;
    }

    // 루트가 가장 큰 값이 아니라면 swap하고 재귀적으로 heapify
    if (largest != i)
    {
        swap(arr[i], arr[largest]);
        heapify(arr, n, largest);
    }
}

// 힙 정렬 함수
void heapSort(vector<int>& arr)
{
    int n = arr.size();

    // 1. 최대 힙 구성 (Build Heap)
    for (int i = n / 2 - 1; i >= 0; i--)
    {
        heapify(arr, n, i);
    }

    // 2. 하나씩 요소를 힙에서 꺼내 정렬
    for (int i = n - 1; i > 0; i--)
    {
        // 현재 루트(최댓값)를 맨 뒤와 교환
        swap(arr[0], arr[i]);

        // 줄어든 힙에 대해 heapify 수행
        heapify(arr, i, 0);
    }
}

int main()
{
    vector<int> arr = { 12, 11, 13, 5, 6, 7 };

    cout << "정렬 전 배열: ";
    for (int val : arr)
    {
        cout << val << " ";
    }
    cout << endl;

    // 힙 정렬
    heapSort(arr);

    cout << "정렬 후 배열: ";
    for (int val : arr)
    {
        cout << val << " ";
    }
}
```

</div>
</details>