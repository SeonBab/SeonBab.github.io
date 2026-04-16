---
layout: single

title: "[C++] Name Hiding"

categories:
    - Cpp
tag: [Cpp]

date: 2026-04-16
last_modified_at: 2026-04-16

order : 256
---

# Name Hiding

Name Hiding, 이름 숨김은 C++에서 자식 클래스(파생 클래스)가 부모 클래스(기반 클래스)의 같은 이름을 가진 멤버를 가려버리는 현상을 말합니다.  
즉, 부모 클래스의 동일한 이름의 모든 멤버가 이름 탐색에서 제외됩니다.

Name Hiding은 파생 클래스의 인터페이스를 명확하게 하고, 기반 클래스의 모든 오버로드를 자동으로 노출하지 않도록 하여 의도하지 않은 함수 호출을 방지하기 위한 설계입니다.

클래스의 멤버에서만 발생하며, 컴파일 단계에서 이름 탐색(Name Lookup) 과정에서 결정됩니다.

## 예시

### 오버로딩이 가려지는 경우

예시 코드는 다음과 같습니다.

```cpp
#include <iostream>

using namespace std;

class Base
{
public:
	void func()
	{
		cout << "Base func()" << endl;
	}

	void func(int x)
	{
		cout << "Base func(int)" << endl;
	}
};

class Derived : public Base
{
public:
	void func()
	{
		cout << "Derived func()" << endl;
	}
};

int main()
{
	Derived d;
	
	d.func(); // 출력: Derived func()

	d.func(5); // 컴파일 에러
}
```

`Derived`에서 `func()`를 선언하면서 `Base`의 `func()`와 `func(int)`까지 모두 가려지게 되는 상태입니다.  
그렇기 때문에 `func(int)`의 경우 호출이 불가능하여 컴파일 에러까지 발생합니다.

이 문제를 해결하기 위해서는 `using` 키워드를 사용하여 가려진 이름을 가져오는 방법이 있습니다.

```cpp
class Derived : public Base
{
public:
	using Base::func;

	void func()
	{
		cout << "Derived func()" << endl;
	}
};
```

자식 클래스 내부에서 `using`키워드를 사용해 가려진 이름을 가져올 수 있는데, 다루고 있는 문제 코드에서 `func(int)`를 호출하면 컴파일 에러가 발생하지 않고, 접근이 가능합니다.

다른 방법으로는 함수를 호출 할 때, `d.Base::func(5);` 이러한 방법으로 가려진 이름을 찾아 호출할 수 있습니다.

### virtual 키워드를 사용해도 발생하는 경우

```cpp
#include <iostream>

using namespace std;

class Base
{
public:
	virtual void func(int);
};

class Derived : public Base
{
public:
	void func(); // 오버라이딩 아님 + Name Hiding 발생
};
```

`virtual`키워드를 사용하여도 매개변수가 다르기 때문에 다른 함수이며, 이름이 같기 때문에 가려집니다.

따라서 상속 구조에서 오버로딩을 유지하려면 `using`선언이 필수적입니다.

### 멤버 변수에서 발생하는 경우

```cpp
#include <iostream>

using namespace std;

class Base
{
public:
	int value = 10;
};

class Derived : public Base
{
public:
	int value = 5; // Name Hiding
};

int main()
{
	Derived d;

	cout << d.value << std::endl;		// 출력: 5
	cout << d.Base::value << std::endl; // 출력: 10
}
```

파생 클래스에서 동일한 이름의 멤버 변수를 선언하면, 부모 클래스의 멤버 변수 또한 숨겨지게 됩니다.

### 접근 지정자로 인해 발생하는 경우

```cpp
#include <iostream>

using namespace std;

class Base
{
public:
	void func();
};

class Derived : public Base
{
private:
	void func();
};
```

파생 클래스에서 동일 이름을 다른 접근 지정자로 선언하면 부모의 멤버는 숨겨지고 새로운 접근 규칙이 적용됩니다.

## 오버라이딩과 비교

오버라이딩은 `virtual`함수를 통해 부모의 함수를 재정의하는 것이지만, Name Hiding은 단순히 이름이 같다는 이유로 부모의 멤버를 가려버리는 현상입니다.

비교한 것을 표로 나타내면 다음과 같습니다.

|구분|Name Hiding|Overriding|
|---|---|---|
|목적|이름 가림|부모 함수 재정의|
|키워드 여부|사용하지 않음|virtual|
|결과|부모 함수 숨겨짐|부모 함수 대체|

## 오버로딩과 비교

오버로딩과 비교하면 다음과 같습니다.

|구분|Name Hiding|Overloading|
|---|---|---|
|발생 위치|상속 관계|같은 클래스|
|기준|이름만 같으면 발생|이름만 같고, 매개변수 다르면 허용|
|결과|부모 함수 숨김|함수 공존|
