---
layout: single

title: "[C++] 문자열"

categories:
    - Cpp
tag: [Cpp]

date: 2024-05-02
last_modified_at: 2024-05-02

order : 80
---

# 문자열

C++의 문자열 표현 방식은 2가지가 있습니다.  
첫번째로 마지막 문자 배열에 `\0`이 있는 C-스트링 방식이 있습니다.  
두번째로 string 클래스를 이용한 방식이 있습니다.

## C-스트링 방식

C-스트링 방식은 C언어에서 사용하던 방식입니다.  

배열의 마지막 원소에 널(NULL) 종료자를 가지는 문자 배열입니다.  
널 종료자는 문자열의 끝을 나타내는 데 사용하는 특수 문자 `\0`입니다.

C-스트링으로 문자열을 정의하려면 다음과 같습니다.
```cpp
char myString1[8]{ 's', 't', 'r', 'i', 'n', 'g', '1', '\0' };
char myString2[8] = "string2";
char myString3[] = "string3";
```

`myString1`은 크기가 8인 문자 배열을 선언하고, 각 인덱스에 문자 상수를 순서대로 지정해주었습니다.  
`myString2`는 크기가 8인 문자 배열을 선언하고, 문자열 상수를 대입하는 방법입니다.  
`myString3`은 문자열 상수로 초기화하지만 배열의 크기를 명시하지 않았습니다. 이런 경우 문자열 상수의 크기에 따라 배열의 크기가 자동적으로 정해집니다.

`myString1`과 `myString2`는 배열의 크기가 명시적으로 정해졌기 때문에 크기를 넘어가는 문자열 상수는 대입 할 수 없어 컴파일 오류가 발생하므로 주의해주어야 합니다.

위의 예시로 만든 변수를 출력해볼 경우 다음과 같습니다.

```cpp
#include <iostream>

int main()
{
	char myString1[8]{ 's', 't', 'r', 'i', 'n', 'g', '1', '\0' };
	char myString2[8] = "string2";
	char myString3[] = "string3";

	std::cout << "myString1의 크기 :" << sizeof(myString1) << std::endl;
	std::cout << "myString2의 크기 :" << sizeof(myString2) << std::endl;
	std::cout << "myString3의 크기 :" << sizeof(myString3) << std::endl;

	std::cout << myString1 << std::endl;
	std::cout << myString2 << std::endl;
	std::cout << myString3 << std::endl;
}
```

![CstyleString_Print]({{site.url}}/images/cpp/cpp/2024-05-02-CPP-String/CstyleString_Print.png)

출력하고자 하는 대상이 C-스트링이므로 `\0`를 만날때까지 배열의 각 인덱스에 접근해 문자를 출력합니다.  
예시의 경우 `\0`이 배열의 마지막에 붙어있으므로 배열에 들어있는 문자가 모두 출력됩니다.  
하지만 `\0`이 만약 문자 배열의 중간에 있다면 다음과 같습니다.

```cpp
#include <iostream>

int main()
{
	char myString1[8]{ 's', 't', 'r', '\0', 'i', 'n', 'g', '1' };

	std::cout << myString1 << std::endl;

	std::cout << "myString1의 크기 :" << sizeof(myString1) << std::endl;
}
```

![CstyleString_PrintNull]({{site.url}}/images/cpp/cpp/2024-05-02-CPP-String/CstyleString_PrintNull.png)

이처럼 `string1`이 출력되지 않고 `str`까지만 출력된 것을 볼 수 있습니다.

그렇다면 `\0`이 마지막에도 없고, 중간에도 없어서 문자 배열에 포함되어있지 않다면 어떻게 출력되는지 다음과 같습니다.

```cpp
#include <iostream>

int main()
{
	char myString1[7]{ 's', 't', 'r', 'i', 'n', 'g', '1' };

	std::cout << "myString1의 크기 :" << sizeof(myString1) << std::endl;

	std::cout << myString1 << std::endl;
}
```

![CstyleString_PrintNullNotInclude1]({{site.url}}/images/cpp/cpp/2024-05-02-CPP-String/CstyleString_PrintNullNotInclude1.png)

`\0`를 제외하면서 배열의 크기가 작아져 7이 됐습니다.

`\0`를 찾을때까지 배열에 접근하게 되면서 배열의 크기를 넘어서도 출력을 하게 됐습니다.  
그 결과 배열의 크기를 넘어선 인덱스의 경우 메모리에 알 수 없는 쓰레기 값이 들어있어 이상한 문자가 출력되게 됐습니다.  
이 경우 프로그램을 다시 한번 실행시켜 출력을 확인해보게 되면 코드는 달라지지 않았지만 의도하지 않은 메모리를 접근하게 되면서 뒤의 이상한 문자는 출력이 달라지는 것을 볼 수 있습니다.

![CstyleString_PrintNullNotInclude2]({{site.url}}/images/cpp/cpp/2024-05-02-CPP-String/CstyleString_PrintNullNotInclude2.png)

### 문자 상수와 문자열 상수

문자 상수는 `'s'`와 같이 1글자씩있는 단일 문자를 말합니다.  
작은따옴표(`''`)를 사용합니다.

문자열 상수는 `"string1"`과 같이 문자들의 배열을 말합니다.  
큰따옴표(`""`)를 사용합니다.  
문자열 상수를 사용하면 메모리 어딘가에 문자열을 위한 공간이 생기고, 별도의 이름을 가지지 않은체 저장되어있습니다.  
어딘가에 저장되어있으므로, 주소를 가지는데 이 주소를 문자열의 시작주소로 사용합니다.

## string 클래스

C++ STL에서 제공하는 문자열을 다루는 클래스입니다.  

C-스트링과 다르게 문자열의 끝에 `\0`가 들어가지 않습니다.  
배열처럼 한 문자씩 다룰 수 있게 되어있습니다.  
C-스트링보다 사용하기 편리하고, 안전합니다.

string 클래스를 사용하기 위해서는 헤더 파일을 포함시켜야 합니다.  

헤더 파일은 다음과같이 포함합니다.

```cpp
#include <string>
```

문자열을 선언하고 초기화하는 방법은 다음과 같습니다.

```cpp
std::string myString1{ 's', 't', 'r', 'i', 'n', 'g', '1' };
std::string myString2{ "string2" };
std::string myString3 = "string3";
```
C-스트링에서 `myString1`을 초기화 했던 방법도 사용 할 수 있습니다.

위의 예시로 만든 변수를 출력해볼 경우 다음과 같습니다.

```cpp
#include <iostream>
#include <string>

int main()
{
	std::string myString1{ 's', 't', 'r', 'i', 'n', 'g', '1' };
	std::string myString2{ "string2" };
	std::string myString3 = "string3";

	std::cout << "myString1의 크기 :" << sizeof(myString1) << std::endl;
	std::cout << "myString2의 크기 :" << sizeof(myString2) << std::endl;
	std::cout << "myString3의 크기 :" << sizeof(myString3) << std::endl;

	std::cout << myString1 << std::endl;
	std::cout << myString2 << std::endl;
	std::cout << myString3 << std::endl;
}
```

string 클래스를 사용해 출력을 했는데 스트링의 크기가 각각 40이 나옵니다.  
std::string 타입의 크기를 출력했기 때문인데, std::string은 대략 다음과 같은 구조를 가집니다.  
SSO(Small(또는 short) String Optimization)를 사용하고, 그리고 문자열의 크기와 Capacity를 저장합니다.

저는 마이크로소프트 비주얼 스튜디오를 64비트로 사용한 환경이므로, 포인터는 8byte의 크기를 가집니다.  
SSO 24byte + 문자열의 크기 8byte + Capacity 8byte로 총 40byte가 나옵니다.

메모리의 크기에 대해서는 큰 차이가 있지만 출력되는 문자의 결과는 같습니다.

std::string의 장점인 간편한 관리를 위해 다양하게 사용 할 수 있는 멤버함수들이 존재하고, 연산자가 오버라이딩 돼있습니다.  
예를 들어 함수로는 문자열의 크기나 길이를 반환해주고, 가장 앞 문자를 반환하거나, 가장 뒤 문자를 반환하기도 합니다. 연산자로는 문자열을 비교해주기위해 비교 연산자가 오버로딩 되어있고, 두 문자열을 붙여 이어주는 `+` 연산자가 오버로딩 돼있습니다.

### SSO

SSO(Small(또는 short) String Optimization)는 문자열의 작은 크기를 저장하기 위한 최적화 기법입니다. 

문자열의 길이가 짧다면 char형 배열로 저장하고, 길다면 char형 배열을 동적으로 할당해 주소를 저장합니다.

이 기법을 사용하는 이유는 힙 할당을 줄이기 위함입니다.  
작은 문자열을 다룰 때 정적으로 저장한다면 메모리를 동적으로 할당 및 해제를 안해도 되므로 이에 대한 오버헤드를 피할 수 있기 때문에 사용합니다.

### Capacity

메모리의 크기를 가변적으로 사용할 경우 문자열을 변경할 때마다 메모리의 크기를 바꿔줘야 하는데, 메모리를 할당받고 지울 경우 이 연산은 컴퓨터에서 굉장히 느린 연산에 속합니다.  
그러므로 string 클래스에서는 최대한 메모리를 재할당 하는 경우가 없도록 넉넉하게 메모리를 할당 받고 사용합니다.
이때 얼만큼 할당 받았는지 byte 크기로 저장하는게 Capacity입니다.

예를들어 string 객체에 Capacity의 값이 20이 되도록 메모리의 크기를 할당 받았고, 이 메모리의 크기가 부족하지 않았다면 메모리를 다시 할당받는 일이 없습니다.  
하지만 만약 메모리가 40만큼 부족해 재할당 받아 사용해야 한다면 부족한 40만큼 할당받아 60이 되는게 아닌 여유가 있도록 넉넉하게 100정도로 할당을 받습니다. 그리고 할당된 메모리의 크기가 변경됐으므로 Capacity의 값이 변경됩니다.