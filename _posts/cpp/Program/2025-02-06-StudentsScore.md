---
layout: single

title: "[C++ 프로그램] 학생들의 성적 관리 시스템"

categories:
    - CppProgram
tag: [Cpp, CppProgram]

date: 2025-02-06
last_modified_at: 2025-02-06

order : 20
---

# 학생들의 성적 관리 시스템

## 문제

1. **학생 성적 추가 기능**
    - `학생 ID(int)`, `과목 이름(string)`, `점수(int)`를 저장한다.
    - 한 학생은 여러 과목을 수강할 수 있다.
        - (예: "1001번 학생이 'C++' 과목에서 85점, '알고리즘'에서 90점을 받음")
    - 동일 학생의 동일 과목을 입력하는 경우엔 최신 점수로 갱신을 한다.
    - 점수는 0 ~ 100점까지만 유효한 범위

2. **학생의 전체 성적 조회 기능**
    - 특정 학생의 모든 과목 성적을 출력한다.
        - (예: "1001번 학생: C++(85점), 알고리즘(90점)")
    - 과목명은 알파벳 순으로 정렬하여 출력한다.
    - 또한, 존재하지 않는 학생 ID 입력 시 예외 처리를 하도록 한다.

3. **전체 학생의 평균 점수 출력 기능**
    - 전체 학생들의 각 과목별 평균 점수를 출력한다.
    - 평균 점수는 소수점 둘째 자리까지 표시한다.
        - (예: "C++ 과목 평균 점수: 87.5, 알고리즘 과목 평균 점수: 92.3")

4. **과목별 최고 점수 학생 조회 기능**
    - 특정 과목에서 가장 높은 점수를 받은 학생들을 찾는다.
    - 동점자가 있을 경우 학생 ID를 오름차순으로 정렬해서 모두 출력한다.
        - (예: "알고리즘 최고 점수: 95점 (학생 ID: 1003, 1005)")

## 구현 요구 사항

- 학생 ID를 기준으로 데이터를 효율적으로 관리할 것.
- 과목별 성적을 효율적으로 조회할 수 있도록 설계할 것.
- `map`, `multimap`, `vector`, `set`, `multiset` 등의 컨테이너를 조합하여 사용할 것.
- 적절한 STL 알고리즘(`sort`, `find_if`, `accumulate` 등)을 활용할 것.

## 입출력 예시

### 입력

1. 학생 성적 추가 (1001, "C++", 85)
2. 학생 성적 추가 (1001, "알고리즘", 90)
3. 학생 성적 추가 (1002, "C++", 92)
4. 학생 성적 추가 (1002, "알고리즘", 78)
5. 학생 성적 추가 (1003, "C++", 95)
6. 학생 성적 추가 (1003, "알고리즘", 95)
7. 학생 성적 조회 (1001)
8. 전체 평균 점수 출력
9. 과목별 최고 점수 학생 조회 ("알고리즘")

### 출력

학생 ID 1001의 성적:
- C++: 85점
- 알고리즘: 90점

전체 과목 평균 점수:
- C++: 90.67점
- 알고리즘: 87.67점

알고리즘 최고 점수: 95점
- 학생 ID: 1003

## 작성한 코드

```cpp
#include <iostream>
#include <string>
#include <regex>
#include <sstream>
#include <unordered_map>
#include <map>
#include <numeric>
#include <algorithm>

// 가독성 문제로 사용
using namespace std;

string ExtractStudentInfo(const string& StudentInfo)
{
	// 매개변수로 받은 문자열을 '(' 문자부터 분리		
	string substr = StudentInfo.substr(StudentInfo.find('('));

	// 괄호, 쉼표, 따옴표를 ""으로 대체
	regex re("[(),\"]");

	return regex_replace(substr, re, "");
}

void AddStudentScore(map<int, map<string, int>>& students, string& StudentInfo)
{
	string replaceStr = ExtractStudentInfo(StudentInfo);
	stringstream ss(replaceStr);
	
	// 학생 ID 확인
	int studentID = -1;
	ss >> studentID;
	if (ss.fail())
	{
		cout << "잘못된 입력" << endl;
		return;
	}

	// 과목 확인
	string subject;
	ss >> subject;
	if (ss.fail())
	{
		cout << "잘못된 입력" << endl;
		return;
	}

	// 점수 확인
	int score;
	ss >> score;
	if (ss.fail())
	{
		cout << "잘못된 입력" << endl;
		return;
	}

	// 학생 ID를 찾고 해당 학생의 과목을 찾아서 접근
	students[studentID][subject] = score;
}

void PrintStudentScore(const map<int, map<string, int>>& students, string& StudentInfo)
{
	string replaceStr = ExtractStudentInfo(StudentInfo);
	stringstream ss(replaceStr);

	// 학생 ID 확인
	int studentID = -1;
	ss >> studentID;
	if (ss.fail())
	{
		cout << "잘못된 입력" << endl;
		return;
	}

	cout << "학생 ID " << studentID << "의 성적:" << endl;
	// students의 자료형이 const이므로 at을 통해 접근
	for (const auto& subject : students.at(studentID))
	{
		cout << "- " << subject.first << ": " << subject.second << "점" << endl;
	}
}

void PrintAverageScore(const map<int, map<string, int>>& students)
{
	// 과목에 대한 합계 점수와 학생 수
	map<string, pair<int, int>> subjectScores;

	// 학생 순회
	for (const auto& student : students)
	{
		// 각 과목 순회
		for (const auto& subject : student.second)
		{
			// 현재 과목에 대한 합계 점수
			subjectScores[subject.first].first += subject.second;
			// 현재 과목에 대한 학생 수
			subjectScores[subject.first].second += 1;
		}
	}

	cout << "전체 과목 평균 점수:" << endl;
	// 합계 점수와 학생 수가 저장된 맵 순회
	for (const auto& subject : subjectScores)
	{
		// 과목에 대한 점수와 학생수에 접근 후 평균 값 구하기
		double average = static_cast<double>(subject.second.first) / subject.second.second;

		// 소수점 이후 2번째까지의 값을 출력하도록 설정
		cout << fixed;
		cout.precision(2);
		// 출력
		cout << "- " << subject.first << ": " << average << "점" << endl;
		cout.unsetf(ios::fixed);
	}
}

void PrintHighScore(const map<int, map<string, int>>& students, const string& StudentInfo)
{
	string replaceStr = ExtractStudentInfo(StudentInfo);
	stringstream ss(replaceStr);

	// 과목 확인
	string subjectName;
	ss >> subjectName;
	if (ss.fail())
	{
		cout << "잘못된 입력" << endl;
		return;
	}

	// 최고 점수와 최고 점수의 학생 ID
	int HighScore = 0;
	vector<int> StudentID;
	// 학생 순회
	for (const auto& student : students)
	{
		// 현재 학생에 특정 과목의 점수가 저장된 이터레이터
		auto it = student.second.find(subjectName);
		
		// 현재 학생에 특정 과목의 점수가 있는지 확인
		if (it != student.second.end())
		{
			if (HighScore == it->second)
			{
				StudentID.push_back(student.first);
			}

			// 현재 학생에 특정 과목의 점수가 가장 높은 점수인지 확인
			if (HighScore < it->second)
			{
				HighScore = it->second;
				StudentID.clear();
				StudentID.push_back(student.first);
			}
		}
	}

	// 출력
	cout << subjectName << " 최고 점수: " << HighScore << "점" << endl;
	for (const int& ID : StudentID)
	{
		cout << "- 학생 ID: " << ID << endl;
	}
}

void PrintatLeastScore(const map<int, map<string, int>>& students, const string& StudentInfo)
{
	string replaceStr = ExtractStudentInfo(StudentInfo);
	stringstream ss(replaceStr);

	// 과목 확인
	string subjectName;
	ss >> subjectName;
	if (ss.fail())
	{
		cout << "잘못된 입력" << endl;
		return;
	}

	// 점수 확인
	int score;
	ss >> score;
	if (ss.fail())
	{
		cout << "잘못된 입력" << endl;
		return;
	}

	cout << subjectName << " " << score << "점 이상 조회:" << endl;
	// 학생 순회
	for (const auto& student : students)
	{
		// 해당 과목이 있는지 찾기
		auto it = student.second.find(subjectName);
		
		// 해당 과목이 있는 경우
		if (it != student.second.end())
		{
			// 학생의 점수가 특정 점수 이상인지 확인
			if (score <= it->second)
			{
				cout << "- 학생 ID: " << student.first << endl;
			}
		}
	}
}

void PrintatMostScore(const map<int, map<string, int>>& students, const string& StudentInfo)
{
	string replaceStr = ExtractStudentInfo(StudentInfo);
	stringstream ss(replaceStr);

	// 과목 확인
	string subjectName;
	ss >> subjectName;
	if (ss.fail())
	{
		cout << "잘못된 입력" << endl;
		return;
	}

	// 점수 확인
	int score;
	ss >> score;
	if (ss.fail())
	{
		cout << "잘못된 입력" << endl;
		return;
	}

	cout << subjectName << " " << score << "점 이하 조회:" << endl;
	// 학생 순회
	for (const auto& student : students)
	{
		// 해당 과목이 있는지 찾기
		auto it = student.second.find(subjectName);

		// 해당 과목이 있는 경우
		if (it != student.second.end())
		{
			// 학생의 점수가 특정 점수 이상인지 확인
			if (score >= it->second)
			{
				cout << "- 학생 ID: " << student.first << endl;
			}
		}
	}
}

void PrintSubjectInfo(const map<int, map<string, int>>& students, const string& StudentInfo)
{
	string replaceStr = ExtractStudentInfo(StudentInfo);
	stringstream ss(replaceStr);

	// 과목 확인
	string subjectName;
	ss >> subjectName;
	if (ss.fail())
	{
		cout << "잘못된 입력" << endl;
		return;
	}

	// 최고 및 최저 점수
	int highScore = 0;
	int lowScore = 100;
	// 점수 합계
	int totalScore = 0;
	// 수강 인원
	int StudentCount = 0;

	// 학생 순회
	for (const auto& student : students)
	{
		// 해당 과목이 있는지 찾기
		auto it = student.second.find(subjectName);

		// 해당 과목이 있는 경우
		if (it != student.second.end())
		{
			// 현재 학생의 점수
			int CurScore = it->second;

			// 학생의 점수가 최고 점수인지 최저 점수인지 확인 및 수정
			highScore = max(highScore, CurScore);
			lowScore = min(lowScore, CurScore);

			// 점수 합계에 값 추가
			totalScore += CurScore;

			// 수강 인원 증가
			++StudentCount;
		}
	}

	cout << subjectName << " " << "점수 및 수강 인원 조회:" << endl;
	cout << "- 최고 점수: " << highScore << endl;
	cout << "- 최저 점수: " << lowScore << endl;
	// 소수점 이후 2번째까지의 값을 출력하도록 설정
	cout << fixed;
	cout.precision(2);
	cout << "- 평균 점수: " << static_cast<double>(totalScore) / StudentCount << endl;
	cout.unsetf(ios::fixed);
	cout << "- 수강 인원: " << StudentCount << endl;
}

const static unordered_map<string, int> functionIndexList
{
	{ "학생 성적 추가", 0 },
	{ "학생 성적 조회", 1 },
	{ "전체 평균 점수 출력", 2 },
	{ "과목별 최고 점수 학생 조회", 3 }, 
	{ "과목별 점수 이상 조회", 4 },
	{ "과목별 점수 이하 조회", 5 },
	{ "과목별 점수 및 수강 인원 조회", 6 }
};

int main()
{
	map<int, map<string, int>> students;

	// 프로그램이 꺼지지 않게 무한 실행
	while (true)
	{
		string inputStr;
		getline(cin, inputStr);

		// 빈 값을 입력했다면
		if (inputStr.empty())
		{
			cout << "잘못된 입력" << endl;
			continue;
		}

		// 기능 찾기
		int FuncIndex = -1;	// 기능의 인덱스
		auto it = find_if(functionIndexList.begin(), functionIndexList.end(),
			[&inputStr](const auto& pair) { return inputStr.find(pair.first) != string::npos; });

		// 기능 찾기
		if (it != functionIndexList.end())
		{
			FuncIndex = it->second;
		}
		// 기능을 찾지 못했다면
		else
		{
			cout << "잘못된 입력" << endl;
			continue;
		}
		
		// 실행할 함수 선택
		switch (FuncIndex)
		{
		case 0:
			AddStudentScore(students, inputStr);
			break;
		case 1:
			PrintStudentScore(students, inputStr);
			cout << endl;
			break;
		case 2:
			PrintAverageScore(students);
			break;
		case 3:
			PrintHighScore(students, inputStr);
			break;
		case 4:
			PrintatLeastScore(students, inputStr);
			break;
		case 5:
			PrintatMostScore(students, inputStr);
			break;
		case 6:
			PrintSubjectInfo(students, inputStr);
			break;
		default:
			break;
		}
	}
};
```

사용한 컨테이너의 종류는 다음과 같습니다.

1. map<int, map<string, int>>
    + 학생 ID를 키로하고, 과목명을 키로, 과목의 점수를 값으로 하는 `map`을 값으로 사용합니다.
    + 학생 ID로 여러 과목의 점수를 관리합니다.
    + Key인 학생 ID를 기준으로 자동으로 정렬합니다.
2. unordered_map<string, int>
    + 각 기능에 해당하는 명령어 문자열을 키로, 해당 명령어가 실행될 때의 인덱스를 값으로 가지는 맵입니다.
3. vector<int>
    + 특정 과목에서 최고 점수를 받은 학생들의 ID를 저장하는 데 사용됩니다. 
    + 최고 점수가 동일한 학생들이 여러 명일 수 있기 때문에 배열을 사용했습니다.
4. string
    + 입력이나 출력 값을 처리하기 위해 사용됩니다.
    + 과목 이름, 임력 명령어 등을 처리하는 데 사용했습니다.

사용한 STL 알고리즘의 종류는 다음과 같습니다.

1. std::find_if
    + 입력 문자열에 해당하는 기능을 찾기 위해서 사용됐습니다.
    + `find_if`는 주어진 범위에서 특정 조건을 만족하는 첫 번째 요소를 찾는 알고리즘입니다.

효율적으로 데이터 관리를 위한 설계 방식

1. 코드 가독성을 위해 ``using namespace std``를 사용했습니다.
2. 학생들의 성적 데이터를 효율적으로 저장하기 위해 이중 맵을 사용했습니다.
    + ``students[학생 ID][과목] = 점수``로 데이터를 저장하고 접근할 수 있어 검색과 수정이 빠릅니다.
3. 하드 코딩을 줄이기 위해 unordered_map<string, int>을 사용해 실행할 함수를 선택했습니다.
4. 입력된 기능 인덱스에 따라 `switch`문을 사용해 함수를 실행합니다.
5. `regex`를 사용해 입력 문자열을 관리했습니다.
    + `regex_replace`함수를 사용해 입력 문자열에서 불필요한 문자를 제거했습니다.
6. `stringstream`을 사용해 공백 단위로 문자열을 분할해 필요한 데이터를 추출했습니다.
7. 특정 과목의 평균 점수 계산 시 ``map<string, pair<int, int>> subjectScores``를 사용해 ``subjectScores[과목] = (총점, 학생 수)``형태로 저장했습니다.
8. ``cout << fixed``와 ``cout.precision(2)``를 사용해 소수점 2번째 자리까지 출력하도록 설정했습니다.
    + 출력 후 ``cout.unsetf(ios::fixed)``로 출력을 원래대로 돌려놓습니다.