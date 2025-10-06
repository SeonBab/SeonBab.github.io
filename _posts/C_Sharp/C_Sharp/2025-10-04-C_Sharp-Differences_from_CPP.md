---
layout: single

title: "[C#] C++과의 차이점"

categories:
    - C_Sharp
tag: [C#]

date: 2025-10-04
last_modified_at: 2025-10-04

order : 1
---

# C++과의 차이점

## 개발 환경 및 용도

- C++
    + 컴파일 시점에 기계어로 변환되는 고성능 언어로, 저수준(Low-level) 제어와 고수준(High-Level) 추상화를 제공합니다.
    + 하드웨어 접근과 성능 최적화가 가능하기 때문에 시스템 프로그래밍에 적합합니다.
    + 운영체제, 드라이버, 렌더링 엔진, 게임 개발, 임베디드 시스템, 고성능 네트워크 등 다양한 분야에 사용됩니다.

- C#
    + 고수준 언어로, C++에 비해 문법이 간단합니다.
    + CIL(Common Intermediate Language)로 컴파일 된 후, 실행 시 JIT(Just-In-Time) 또는 AOT(Ahead-of-Time)을 통해 네이티브 코드로 변환됩니다.
    + 네트워크, 파일 입출력, 쓰레드, 윈도우 기반의 애플리케이션, 웹 서비스, Unity 기반 개발에 사용됩니다.
    + 마이크로소프트에서 개발했으며, 현재는 .NET 재단을 통해 오픈소스로 관리됩니다.

## 플랫폼 종속성

- C++
    + 소스 코드 수준의 이식성(Portability)은 높습니다.
    + 컴파일러와 플랫폼별 ABI(Application Binary Interface)차이로 인해 바이너리 호환성이 낮습니다.

- C#
    + 과거에는 윈도우와 .NET Framework에 종속적이었습니다.
    + 현재는 .NET Core, .NET 5+와 Mono, Unity 덕분에 Linux, macOS, 모바일 등으로 확장될 수 있습니다.

## 실행 성능

- C++
    + 컴파일 시 기계어로 직접 변환되므로 네이티브 성능을 그대로 발휘합니다.
    
- C#
    + JIT와 가비지 컬렉터로 인한 오버헤드가 존재합니다.
    + 최신 .NET 런타임은 JIT 최적화, AOT(Ahead-of-Time Compoilation), NativeAOT 등을 통해 성능 격차가 크게 줄었습니다.

## 예외 처리 및 안정성

- C++
    + 예외 처리(try-catch)를 지원하지만, 대부분의 함수는 에러 코드를 반환하는 방식을 많이 사용합니다.
    + 잘못된 메모리 접근, 버퍼 오버플로우 등으로 인해 프로그램이 크래시가 발생할 수 있습니다.

- C#
    + 모든 에러 처리는 예외(Exception) 기반으로 이루어집니다.
    + 타입 안전성과 메모리 안정성을 보장하여 상대적으로 런타임에서 좀 더 안전한 동작을 보장합니다.

## 객체지향

두 언어 모두 OOP를 지원하지만 다중상속에서 차이가 있습니다.

- C++
    + 다중 상속이 가능합니다.

- C#
    + 다중 상속이 불가능하며 인터페이스로 대체합니다.

## 라이브러리 및 프레임워크

- C++
    + 표준 라이브러리(STL)와 Boost같은 서드파티 라이브러리를 주로 사용합니다.
    + GUI나 웹 관련 기능은 기본적으로 지원하지 않아 직접 구현하거나 외부 라이브러리를 이용해야 합니다.

- C#
    + .NET Framework/.NET Core에 방대한 표준 라이브러리를 제공합니다.
    + 네트워크, 데이터베이스, UI, 웹 등 대부분의 기능을 내장 프레임워크로 지원합니다.

## 메모리 관리

- C++
    + 개발자가 직접 메모리를 할당하고 해제해야 합니다.
    + 메모리 누수, 댕글링 포인터가 발생하기도 합니다.
    + RAII(Resource Acquisition Is Initialization) 패턴을 활용해 스마트 포인터로 메모리 안전성을 보완합니다.

```cpp
int* ptr = new int(10);

delete ptr;
```

- C#
    + 가비지 컬렉터가 자동으로 메모리 관리를 수행합니다.
    + 가비지 컬렉터의 실행으로 순간 성능이 지연되기도 합니다.
    + 가비지 컬렉션 외에도 `usin` 구문을 통해 `IDisposable` 패턴으로 리소스 관리가 가능합니다.

```csharp
int a = new int;
float[] b = new float[5];

// 가비지 컬렉터가 자동으로 정리
```

## 포인터

- C++
    + 포인터를 직접적으로 사용할 수 있어 메모리 주소에 접근하고 조작할 수 있습니다.

```cpp
int number = 100;
int* ptr = &number;
```

- C#
    + 보안상의 이유로 직접적인 포인터 사용을 제한합니다.

```csharp
int number = 100;
int* ptr = &number; // unsafe 블록 내에서만 사용 가능
```

## 문법

- C++
    + C 언어의 확장으로, C 와 유사하며 더 복잡한 문법을 가집니다.

```cpp
#include <iostream>
#include <vector>

int main()
{
	std::vector<int> numbers = { 1, 2, 3, 4, 5 };

	for (int num : numbers)
	{
		std::cout << num << " ";
	}
}
```

- C#
    + 자바와 유사한 문법으로, C/C++에 비해 상대적으로 쉬운 문법을 가집니다.

```csharp
using System;
using System.Collections.Generic;

class Program
{
    public static void Main(string[] args)
    {
        List<int> numbers = new List<int> {1, 2, 3, 4, 5};

        foreach (int num in numbers)
        {
            Console.Write(num + " ");
        }
    }
}
```

## 병렬 처리

- C++
    + `std::thread`, OpenMP, TBB(Intel Threading Building Blocks)등을 활용합니다.

- C#
    + `async/await`, `Task Parallel Library(TPL)`, LINQ와 결합된 병렬처리 지원 등 고수준 추상화를 활용합니다.