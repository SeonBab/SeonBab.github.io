---
layout: single

title: "[Design Pattern] 싱글톤 패턴"

categories:
    - DesignPattern
tag: [디자인 패턴]

date: 2025-01-09
last_modified_at: 2025-01-09

order : 10

mermaid: true
---

# 싱글톤 패턴

싱글톤(Singleton) 패턴은 하나의 클래스에 대해 단 하나의 인스턴스만 생성되도록 보장하고, 이 인스턴스에 전역적으로 접근할 수 있도록 하는 디자인 패턴입니다.

주로 애플리케이션에서 전역적으로 관리해야 하는 설정 클래스, 로깅 클래스, 리소스 관리자 등에서 사용됩니다.

단일 인스턴스 보장으로 클래스에 대한 인스턴스가 하나만 생성되도록 제한합니다.

인스턴스 생성 시점을 제어해서 필요할 때까지 인스턴스를 생성하지 않을 수도 있습니다.  
지연 초기화라고 합니다.

프로그램 내 어디서나 동일한 인스턴스를 참조할 수 있습니다.

전역적으로 하나의 객체만 존재하므로 메모리와 리소스를 절약할 수 있습니다.

싱글톤 패턴은 프로그램 설계에 있어서 테스트와 확장이 어려워 집니다.

다중 스레드 환경에서 초기화나 접근에 스레드 안전성을 고려하지 않으면 문제가 발생할 수 있습니다.

## 싱글톤 패턴 예시

로그 시스템을 구현한 코드입니다.

```cpp
#include <iostream>
#include <fstream>
#include <string>

class Logger {
private:
    // 정적 멤버 변수로 싱글톤 인스턴스를 저장
    static Logger* instance;

    std::ofstream logFile; // 로그를 기록할 파일 스트림, 로그를 기록할 공간이라고 생각하면 쉽습니다.
    
    // private 생성자 (외부에서 인스턴스 생성 불가)
    Logger() {
        logFile.open("log.txt", std::ios::app); // 로그 파일 열기 (append 모드)
        if (!logFile.is_open()) {
            throw std::runtime_error("Unable to open log file.");
        }
    }

    // private 소멸자 (외부에서 delete 불가)
    ~Logger() {
        if (logFile.is_open()) {
            logFile.close();
        }
    }

    // 복사 생성자와 대입 연산자 삭제 (복사 방지)
    Logger(const Logger&) = delete;
    Logger& operator=(const Logger&) = delete;

public:
    // 싱글톤 인스턴스를 반환하는 정적 메서드
    static Logger* GetInstance() {
        if (instance == nullptr) {
            instance = new Logger();
        }
        return instance;
    }

    // 로그 메시지를 파일에 기록
    void Log(const std::string& message) {
        if (logFile.is_open()) {
            logFile << message << std::endl;
        }
    }
};

// 정적 멤버 변수 초기화
Logger* Logger::instance = nullptr;

int main() {
    // 싱글톤 객체를 통해 로그 기록
    Logger* logger = Logger::GetInstance();
    logger->Log("Application started.");
    logger->Log("Performing some operations...");
    logger->Log("Application ended.");

    // 동일한 인스턴스를 반환
    Logger* logger2 = Logger::GetInstance();
    logger2->Log("This message uses the same logger instance.");

    return 0;
}
```

`static Logger* instance`는 클래스의 유일한 인스턴스를 저장합니다.  
`static Logger* GetInstance()`는 `instance`를 초기화하고 반환합니다.

생성자와 소멸자는 `private`로 선언되어 외부에서 직접 호출할 수 없습니다.  
이를 통해 외부에서 `new`나 `delete`를 사용할 수 없게 합니다.

복사 생성자와 대입 연산자를 `delete`하여 객체가 복사되지 않도록 방지합니다.

`Log()` 메서드를 통해 로그 메시지를 기록합니다.

위의 코드를 멀티스레드 환경에서 사용할 경우 두 스레드가 동시에 `GetInstance()`를 호출하면, 두 번의 `new Logger()`를 호출하기 때문에 두 개의 인스턴스가 생성될 수 있습니다.  
이를 방지하려면 추가적인 동기화 코드가 필요합니다.  
동기화 코드 대신 이를 방지하는 기능인 C++11의 magic static방식을 사용할 수 있습니다.

예시는 다음과 같습니다.

```cpp
    // 싱글톤 인스턴스를 반환 (magic static 사용)
    static Logger& GetInstance() {
        static Logger instance; // 정적 지역 변수 사용 (스레드 안전)
        return instance;
    }
```

`GetInstance()` 메서드에서 정적 지역 변수를 사용해서 C++11 이상에서 스레드 안전을 보장할 수 있습니다.  
전역 정적 포인터(`static Logger* instance`)와 다르게 수동 메모리 관리가 필요하지 않다는 장점이 있습니다.

C++11이후 magic static을 사용하는 것이 권장 방식입니다.