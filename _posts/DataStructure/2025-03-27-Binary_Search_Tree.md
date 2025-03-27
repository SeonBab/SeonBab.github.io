---
layout: single

title: "[C++ Data Structure] 이진 탐색 트리(Binary Search Tree)"

categories:
    - DataStructure
tag: [Cpp, 자료구조]

date: 2025-03-27
last_modified_at: 2025-03-27

order : 1020
---

# 이진 탐색 트리

이진 탐색 트리(Binary Search Tree, BST)는 이진 탐색(binary search)과 연결리스트(Linked list)를 결합한 자료구조의 한 종류입니다.  
탐색에 효율적이면서도, 효율적인 삽입과 삭제가 가능합니다.

최대 두 개의 자식을 가집니다.

BST에서 주요 연산은 다음과 같습니다.

+ 삽입(Insertion)
    - 새로운 노드가 부모 노드보다 값이 큰 경우 오른쪽 서브트리이며, 값이 작은 경우 왼쪽 서브트리로 설정됩니다.
+ 삭제(Deletion)
    - 삭제할 노드가 자식이 없는 경우 -> 바로 삭제
    - 삭제할 노드가 자식이 하나인 경우 -> 자식 노드를 부모와 연결 후 삭제
    - 삭제할 노드가 자식이 둘인 경우 -> 오른쪽 서브트리에서 최소값을 찾아 대체 후 삭제
+ 탐색(Search)
    - 루트 노드부터 시작하여, 찾고자 하는 값과 비교합니다.
    - 찾는 값이 현재 노드의 값보다 작으면 왼쪽 서브트리로 이동합니다.
    - 찾는 값이 현재 노드의 값보다 크면 오른쪽 서브트리로 이동합니다.
    - 이 과정을 반복하며, 값을 찾거나 없으면 탐색을 종료합니다.

평균적으로 탐색, 삽입, 삭제가 O(log N)의 시간 복잡도를 가집니다.  
트리가 한쪽으로 치우친 경우에는 최악의 경우 O(N)이 될 수 있습니다.

다음 링크에서 이진 탐색 트리가 생성되고, 삭제되는 모습들 이해하기 쉽게 볼 수 있습니다.  
[Binary Search Tree](https://www.cs.usfca.edu/~galles/visualization/BST.html){: target="_blank"}

## 구현 예시

```cpp
#include <iostream>

using namespace std;

// 노드 구조체
struct Node
{
    int key;     // 노드가 저장하는 값
    Node* left;  // 왼쪽 자식 노드의 포인터
    Node* right; // 오른쪽 자식 노드의 포인터

    // 생성자
    Node(int value) : key(value), left(nullptr), right(nullptr) {}
};

// 삽입 함수
Node* insert(Node* root, int key)
{
    // 노드가 비어있는 경우
    if (root == nullptr)
    {
        return new Node(key);
    }
    // 노드의 키가 더 큰 경우
    // 재귀호출을 통해 왼쪽 서브트리에 삽입
    if (key < root->key)
    {
        root->left = insert(root->left, key);
    }
    // 노드의 키가 더 작은 경우
    // 재귀호출을 통해 오른쪽 서브트리에 삽입
    else if (key > root->key)
    {
        root->right = insert(root->right, key);
    }

    // 같은 값은 무시됩니다.
    return root;
}

// 탐색 함수
bool search(Node* root, int key)
{
    if (root == nullptr)
    {
        return false;
    }

    if (root->key == key)
    {
        return true;
    }

    // key값이 현재 노드보다 값이 작은지 큰지 비교 후 재귀호출
    return (key < root->key) ? search(root->left, key) : search(root->right, key);
}

// 순회 함수
void inorder(Node* root)
{
    if (root == nullptr)
    {
        return;
    }

    // 재귀호출을 통해 왼쪽 서브트리 -> 현재 노드 -> 오른쪽 서브트리 순서로 순회하며 출력
    inorder(root->left);
    cout << root->key << " ";
    inorder(root->right);
}

// 최소값을 찾는 함수
Node* findMin(Node* root)
{
    // 왼쪽 끝 노드를 찾는 반복문
    while (root->left != nullptr)
    {
        root = root->left;
    }

    return root;
}

// 삭제 함수
Node* remove(Node* root, int key)
{
    // 삭제할 노드가 없는 경우
    if (root == nullptr)
    {
        return nullptr;
    }

    // 탐색을 통해 삭제할 노드를 찾음
    // 삭제할 값이 현재 노드보다 작으면 왼쪽 서브트리에서 탐색
    if (key < root->key)
    {
        root->left = remove(root->left, key); // 왼쪽에서 삭제
    }
    // 삭제할 값이 현재 노드보다 크면 오른쪽 서브트리에서 탐색
    else if (key > root->key)
    {
        root->right = remove(root->right, key); // 오른쪽에서 삭제
    }
    // 삭제할 노드를 찾은 경우
    else
    {
        // 자식이 없는 경우
        if (root->left == nullptr && root->right == nullptr)
        {
            delete root;
            return nullptr;
        }
        // 한 개의 자식이 있는 경우
        else if (root->left == nullptr)
        {
            // 오른쪽 자식을 새로운 루트로 설정하고 기존 노드를 삭제
            Node* temp = root->right;
            delete root;
            return temp;
        }
        else if (root->right == nullptr)
        {
            // 왼쪽 자식을 새로운 루트로 설정하고 기존 노드를 삭제
            Node* temp = root->left;
            delete root;
            return temp;
        }

        // 두 개의 자식이 있는 경우
        // 오른쪽 서브트리의 최소값으로 대체
        Node* temp = findMin(root->right);
        root->key = temp->key;
        root->right = remove(root->right, temp->key);
    }

    return root;
}

void deleteTree(Node* root)
{
    if (root == nullptr)
    {
        return;
    }

    deleteTree(root->left);
    deleteTree(root->right);
    delete root;
}

int main()
{
    Node* root = nullptr;

    root = insert(root, 50);
    root = insert(root, 30);
    root = insert(root, 70);
    root = insert(root, 20);
    root = insert(root, 40);
    root = insert(root, 60);
    root = insert(root, 80);

    cout << "순회 결과: ";
    inorder(root);

    cout << endl;

    cout << "탐색 40: " << (search(root, 40) ? "찾음" : "없음") << endl;
    cout << "탐색 90: " << (search(root, 90) ? "찾음" : "없음") << endl;

    deleteTree(root);
}
```