---
layout: single

title: "[Algorithm] 백트래킹(Backtracking)"

categories:
    - Algorithm
tag: [알고리즘]

date: 2025-03-18
last_modified_at: 2025-03-18

order : 1020
---

# 백트래킹

백트래킹(Backtracking)은 모든 경우의 수를 탐색하면서 조건을 만족하지 않는 경우 되돌아가는 알고리즘입니다.

일반적으로, 재귀(Recursion)을 사용하며, 상태를 저장했다가 조건이 맞지 않으면 이전 상태로 돌아가는 방식으로 동작합니다.

동작 원리는 다음과 같습니다.

1. 현재 단계에서 가능한 모든 선택(분기)을 탐색
2. 각 선택에 대해, 유효성 검사
    + 조건을 만족하지 않으면 더 깊이 탐색하지 않고 돌아감 (가지치기)
4. 유효하다면 다음 단계로 진행 (재귀 호출)
5. 모든 선택을 시도한 뒤, 이전 단계로 돌아가서 다른 경우의 수 탐색

N-Queen, 미로 탐색, 수열 문제에서 활용할 수 있습니다.

가지치기, 비트마스크, 메모제이션을 사용해서 좀 더 효율적으로 사용할 수 있습니다.

## 가지치기

일반적으로 가지치기(Pruning)를 포함하여 구현합니다.  
가지치기란 백트래킹에서 불필요한 탐색을 미리 차단하는 기법입니다.

해가 될 가능성이 없는 경로는 더 이상 탐색하지 않고 바로 돌아가는 것을 의미합니다.

가능성이 없는 경로를 조기에 중단하여 탐색 속도를 향상시킵니다.  
불필요한 경우를 배제하여 탐색하는데 할당되는 공간을 줄입니다.

## 예시

순열 생성 및 사용한 문자를 다시 사용하지 않는 방법

```cpp
#include <iostream>
#include <vector>
#include <string>

using namespace std;

// 문자열의 순열을 만드는 함수
void func(const string& str, string& curStr, vector<bool>& used)
{
	// 순열이 만들어진 경우
	if (str.size() == curStr.size())
	{
		cout << curStr << endl;

		return;
	}

	// 순열을 만들기 위한 글자를 찾는 반복문
	for (int i = 0; i < str.size(); ++i)
	{
		// 사용하지 않은 글자라면
		if (!used[i])
		{
			// 사용 및 처리
			curStr.push_back(str[i]);
			used[i] = true;

			// 함수 호출
			func(str, curStr, used);

			// 더이상 사용하지 않으므로 수정
			used[i] = false;
			curStr.pop_back();
		}
	}
}

// 사용 예시
int main()
{
	string str = "ABC";
	string curStr = "";
	vector<bool> used(str.size(), false);

	func(str, curStr, used);
}
```

---

N-Queen을 구하는 방법

```cpp
#include <iostream>
#include <vector>

using namespace std;

void printBoard(const vector<vector<int>>& board, int n)
{
    cout << "====" << endl;

    for (int i = 0; i < n; i++)
    {
        for (int j = 0; j < n; j++)
        {
            if (board[i][j] == 1)
            {
                cout << "Q ";
            }
            else
            {
                cout << ". ";
            }
        }

        cout << endl;
    }

    cout << "====" << endl;
}

// 현재 행과 열에 배치 할 수 있는지 확인하는 함수
// 한 행에 하나의 퀸만 배치하고 다음 행으로 이동하므로 검사할 필요가 없음
// 좌하향 및 우하향 대각선은 퀸이 배치되지 않았으므로 검사할 필요가 없음
bool canPlaceQueen(vector<vector<int>>& board, int row, int col, int n)
{
    // 같은 열 검사
    for (int i = 0; i < row; ++i)
    {
        if (board[i][col] == 1)
        {
            return false;
        }
    }

    // 좌상향 대각선 검사
    for (int i = row, j = col; i >= 0 && j >= 0; --i, --j)
    {
        if (board[i][j] == 1)
        {
            return false;
        }
    }

    // 우상향 대각선 검사
    for (int i = row, j = col; i >= 0 && j < n; --i, ++j)
    {
        if (board[i][j] == 1)
        {
            return false;
        }
    }

    // 현재 위치에 퀸 배치 가능
    return true;
}

// 백트래킹을 이용해 N-Queen 해를 찾는 함수
void solveNQueens(vector<vector<int>>& board, int row, int n)
{
    // n개의 퀸이 모두 배치된 경우 해를 출력
    if (row == n)
    {
        printBoard(board, n);
        return;
    }

    // 열 순회
    // 현재 행에서 가능한 모든 열에 배치 시도
    for (int col = 0; col < n; ++col)
    {
        // 현재 위치에 퀸을 배치 할 수 있는지 확인
        if (canPlaceQueen(board, row, col, n))
        {
            board[row][col] = 1;    // 퀸 배치
            solveNQueens(board, row + 1, n);    // 다음 행
            board[row][col] = 0;    // 퀸 제거 후 다른 경우 탐색 (백트래킹)
        }
    }
}

// 사용 예시
int main()
{
	int n;
	std::cin >> n;

    // 보드
	vector<vector<int>> board(n, vector<int>(n, 0));

    solveNQueens(board, 0, n);
}
```