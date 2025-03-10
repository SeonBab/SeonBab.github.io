---
layout: single

title: "[Algorithm] 퀵 정렬"

categories:
    - Algorithm
tag: [알고리즘]

date: 2025-03-10
last_modified_at: 2025-03-10

order : 30
---

# 퀵 정렬

퀵 정렬(Quick Sort)은 분할 후 정복(Divide and Conquer) 방식으로 데이터를 나누고 정렬하여 전체를 정리하는 알고리즘입니다.

다음과 같은 과정으로 동작합니다.

1. 기준 원소(Pivot)을 선택합니다.
2. 피벗보다 작은값은 왼쪽, 큰 값은 오른쪽으로 배치하여 두 개의 부분 배열을 만들어 분할(Partitioning)해줍니다.
3. 왼쪽 부분 배열과 오른쪽 부분 배열에 대해 각각 퀵 정렬을 재귀적으로 수행합니다.

![Quick_sort-example]({{site.url}}/images/Algorithm/2025-03-10-Algorithm-Quick_Sort/Quick_sort-example.gif)

시간 복잡도는 최선 $\Omega(n \ log \ n)$, 평균 $\theta(n \ log \ n)$ 최악 $O(n^2)$입니다.  
퀵 정렬은 평균적으로 가장 빠른 정렬 알고리즘 중 하나지만, 최악의 경우 삽입 정렬과 같은 O(n²)이 될 수 있습니다.

공간 복잡도는 최선 $\Omega(log \ n)$, 평균 $\theta(log \ n)$ 최악 $O(n)$입니다.  
추가적인 배열을 사용하지 않고 입력 배열을 직접 수정하는 제자리 정렬(In-place sorting)이지만, 재귀 호출로 인한 스택 공간을 주로 사용합니다.

퀵 정렬은 요소들을 교환(Swap)하면서 정렬을 수행하기 때문에, 같은 값을 가진 요소들이 원래의 상대적인 순서를 유지하지 못할 수 있는 불안정 정렬입니다.  
구현 방법에 따라 안정 정렬로도 만들 수 있지만, 추가적인 메모리 사용이 필요하고, 제자리 정렬이라는 장점이 사라질 수 있습니다.

## 파티셔닝 방식

로무토(Lomuto)와 호어(Hoare)라는 파티셔닝 방식의 두 가지 주요 알고리즘이 있습니다.  
이 두 알고리즘은 파티셔닝을 하는 방식에 차이가 있습니다.

### 로무토 파티셔닝

로무토 파티셔닝(Lomuto Partitioning)은 퀵 정렬에서 가장 직관적인 방식 중 하나입니다.

파티션을 만들기 위해 하나의 피벗을 선택하고, 배열에서 피벗을 기준으로 두 부분으로 나누는 방식입니다.

동작은 다음과 같습니다.

1. 한쪽 끝을 선택하는데, 보통 배열의 마지막 요소(오른쪽 끝)을 피벗으로 선택합니다.
2. 배열을 처음부터 끝까지 탐색하면서 피벗보다 작은 값들을 왼쪽으로, 큰 값들을 오른쪽으로 배치합니다.
3. 피벗이 제 위치로 오도록, 피벗과 배열의 마지막에 위치한 값을 교환합니다.
4. 피벗을 기준으로 왼쪽과 오른쪽 배열에 대해 재귀적으로 퀵 정렬을 수행합니다.

+ 장점
    + 코드가 간단하고 이해하기 쉽습니다.
    + 구현이 비교적 직관적입니다.

+ 단점
    + 최악의 경우 시간 복잡도가 $O(n^2)$이 될 수 있습니다.

### 호어 파티셔닝

호어 파티셔닝(Hoare Partitioning)은 피벗을 선택하고, 배열을 왼쪽과 오른쪽으로 나누기 위해 두 개의 인덱스를 사용합니다.

로무토 파티셔닝보다 더 효율적일 수 있습니다.

동작은 다음과 같습니다.

1. 왼쪽이나 오른쪽 한쪽 끝을 피벗으로 선택합니다.
2. 배열의 양쪽 끝에서 시작하는 인덱스를 설정해주어 왼쪽에서 피벗보다 큰 값을 찾아 인덱스를 설정하고, 오른쪽에서 피벗보다 작은 값을 찾아 인덱스를 설정합니다.
3. 큰 값과 작은 값을 찾았다면 서로 교환합니다.
4. 교환 후, 인덱스가 교차될 때까지 반복합니다.
5. 인덱스가 교차된다면 피벗을 제 위치로 이동시키고, 피벗을 기준으로 두 부분 배열을 정렬합니다.

+ 장점
    + 스왑의 횟수가 적기 때문에 로무토 파티셔닝 방식보다 더 빠른 성능을 제공할 수 있습니다.

+ 단점
    + 코드가 조금 더 복잡하기 때문에 이해하기 어려울 수 있습니다.

## 예시

피벗을 랜덤한 위치로 결정한 퀵 정렬 구현 예시입니다.

```cpp
#include <iostream>
#include <vector>
#include <cstdlib>
#include <ctime>

using namespace std;

int partition(vector<int>& arr, int left, int right)
{
    // 랜덤한 피벗 위치 결정하기
    int randomIndex = left + rand() % (right - left + 1);
    // 랜던한 위치의 피벗 값 가져오기
    int pivot = arr[randomIndex];
    
    int i = left;   // 정렬 대상의 가장 왼쪽
    int j = right;  // 정렬 대상의 가장 오른쪽
    
    while (true)
    {
        // 왼쪽에서 피벗보다 큰 요소 찾기
        while (arr[i] < pivot) { ++i; }
        // 오른쪽에서 피벗보다 작은 요소 찾기
        while (arr[j] > pivot) { --j; }
    
        // 두 포인터가 교차되면 분할 종료
        if (i >= j) break;
    
        // 작은 값과 큰 값의 위치 교환
        swap(arr[i], arr[j]);
        // 교환 이후 다음 위치로 이동
        ++i, --j;
    }
    
    // 교차된 위치 반환
    return j;
}

void quickSort(vector<int>& arr, int left, int right)
{
    // 정렬할 요소가 없는 경우
    if (left >= right)
    {
        return;
    }

    int pivotIndex = partition(arr, left, right);
    quickSort(arr, left, pivotIndex);   // 왼쪽 부분 정렬
    quickSort(arr, pivotIndex + 1, right);  // 오른쪽 부분 정렬
}

int main()
{
    srand(time(0));

    vector<int> arr{ 10, 7, 8, 9, 1, 5, 3, 6, 10 };

    quickSort(arr, 0, arr.size() - 1);

    for (int e : arr)
    {
        cout << e << " ";
    }
}
```