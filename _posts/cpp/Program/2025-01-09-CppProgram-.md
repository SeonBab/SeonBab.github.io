---
layout: single

title: "[C++ 프로그램] 계산기"

categories:
    - CppProgram
tag: [Cpp, CppProgram]

date: 2025-01-09
last_modified_at: 2025-01-09

order : 10
---

# 계산기

일반적으로 계산기의 기능은 다음과 같습니다.

사용자가 두 숫자를 입력하고 원하는 연산을 선택하도록 구현합니다.

연산은 덧셈, 뺄셈, 곱셈, 나눗셈, 나머지, 제곱, 제곱근(루트)이 있습니다.

숫자 입력 및 결과가 출력됩니다.

## 구현 코드

```cpp
#include <iostream>
#include <string>
#include <cmath>

// 계산을 하는 함수를 가진 클래스 정의
class Calculator {
public:
    // 계산기 연산들
    static double add(double a, double b) {
        return a + b;
    }

    static double subtract(double a, double b) {
        return a - b;
    }

    static double multiply(double a, double b) {
        return a * b;
    }

    static double divide(double a, double b) {
        if (0 == b) {
            throw std::runtime_error("0으로 나눌 수 없습니다.");
        }

        return a / b;
    }

    static double modulo(double a, double b) {
        if (0 == b) {
            throw std::runtime_error("0으로 나눌 수 없습니다.");
        }

        return std::fmod(a, b);
    }
    
    static double pow(double a, double b) {
        return std::pow(a, b);
    }

    static double sqrt(double a) {
        if (0 > a) {
            throw std::runtime_error("제곱근을 구할 떄에는 음수를 사용할 수 없습니다.");
        }

        return std::sqrt(a);
    }
};

int main() {
    while (true) {

        // 왼쪽 연산자와 오른쪽 연산자가 될 값
        double num1 = 0, num2 = 0;
        // 연산하려는 연산자를 문자열로 받는다.
        std::string op{};

        // 왼쪽 연산자 될 숫자 입력 받기
        std::cout << "첫 번째 숫자를 입력하세요: ";
        if (!(std::cin >> num1)) {
            std::cout << "잘못된 입력입니다. 숫자를 입력하세요." << std::endl;
            // 입력 스트림 상태 초기화 및 잘못된 입력 제거
            std::cin.clear();
            std::cin.ignore(std::numeric_limits<std::streamsize>::max(), '\n');
            continue;
        }

        // 어떤 연산을 할지 연산자 입력 받기
        std::cout << "연산자를 입력하세요(+, -, *, /, %, pow, sqrt): ";
        std::cin >> op;

        if (op != "sqrt") {
            // 오른쪽 연산자가 될 숫자 입력 받기
            std::cout << "두 번쨰 숫자를 입력하세요: ";
            if (!(std::cin >> num2)) {
                std::cout << "잘못된 입력입니다. 숫자를 입력하세요." << std::endl;
                // 입력 스트림 상태 초기화 및 잘못된 입력 제거
                std::cin.clear();  
                std::cin.ignore(std::numeric_limits<std::streamsize>::max(), '\n');
                continue;
            }
        }

        try {
            double result = 0;
            if (op == "+") {
                result = Calculator::add(num1, num2);
            }
            else if (op == "-") {
                result = Calculator::subtract(num1, num2);
            }
            else if (op == "*") {
                result = Calculator::multiply(num1, num2);
            }
            else if (op == "/") {
                result = Calculator::divide(num1, num2);
            }
            else if (op == "%") {
                result = Calculator::modulo(num1, num2);
            }
            else if (op == "pow") {
                result = Calculator::pow(num1, num2);
            }
            else if (op == "sqrt") {
                result = Calculator::sqrt(num1);
            }
            else {
                std::cout << "잘못된 연산자입니다." << std::endl;
            }

            std::cout << "결과값: " << result << std::endl;
        }
        catch (std::runtime_error& e) {
            std::cout << e.what() << std::endl;
        }
    }
}
```