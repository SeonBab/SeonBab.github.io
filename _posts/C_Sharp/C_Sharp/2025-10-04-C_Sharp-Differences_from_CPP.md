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

C++은 컴파일 시점에 기계어로 변환되는 중간 수준 언어로, 저수준(Low-level)과 고수준(High-Level) 언어의 특성을 모두 가지고 있습니다.

성능 최적화가 가능하기 때문에 하드웨어 접근이 필요한 시스템 프로그래밍에 적합합니다.

컴파일러와 플랫폼에 따라 실행 결과나 바이너리 구조가 달라질 수 있습니다.

운영체제, 게임 개발, 임베디드 시스템 등에 사용됩니다.

C#은 고수준 언어로, 메모리 관리가 자동으로 수행됩니다.

CIL(Common Intermediate Language)로 컴파일 되며, 실행 시 JIT(Just-In-Time) 컴파일을 통해 네이티브 코드로 변환됩니다.

.NET 프레임워크로 윈도우에서 실행되며, .NET Core/Mono, Unity 등으로 다양한 플랫폼에서 실행됩니다.

윈도우 기반의 애플리케이션, 웹 서비스, 데스크톱 애플리케이션, Unity 기반 개발에 사용됩니다.

## 메모리 관리

C++에서는 개발자가 직접 메모리를 할당하고 해제해야 합니다.  
이로인해 메모리 누수, 댕글링 포인터가 발생하기도 합니다.

```cpp
int* ptr = new int(10);

delete ptr;
```

C#에서는 가비지 컬렉터가 자동으로 메모리 관리를 수행합니다.  
가비지 컬렉터의 실행으로 순간 성능이 지연되기도 합니다.

```csharp
int a = new int;
float[] b = new float[5];

// 가비지 컬렉터가 자동으로 정리
```

## 포인터

C++은 포인터를 직접적으로 사용할 수 있어 메모리 주소에 접근하고 조작할 수 있습니다.

```cpp
int number = 100;
int* ptr = &number;
```

C#은 보안상의 이유로 직접적인 포인터 사용을 제한합니다.

```csharp
int number = 100;
int* ptr = &number; // unsafe 블록 내에서만 사용 가능
```

## 객체지향

두 언어 모두 OOP를 지원하지만 다중상속에서 차이가 있습니다.

C++은 다중 상속이 가능합니다.

C#은 다중 상속이 불가능하며 인터페이스로 대체합니다.

## 문법

C++은 C 언어의 확장으로, C와 유사하며 더 복잡한 문법을 가집니다.

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

C#은 자바와 유사한 문법으로, C/C++에 비해 상대적으로 쉬운 문법을 가집니다.

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