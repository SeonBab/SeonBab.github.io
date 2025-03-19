---
layout: single

title: "[C++] 템플릿"

categories:
    - Cpp
tag: [Cpp]

date: 2025-01-02
last_modified_at: 2025-01-02

order : 290
---

# 템플릿

템플릿(Template)은 코드의 재사용성과 일반성을 높이기 위해 도입된 기능입니다.

템플릿은 호출 시 전달된 인자의 타입에 따라 컴파일러가 적절한 타입으로 구체화된 코드를 생성합니다.

템플릿을 사용하면 클래스나 함수에서 데이터 타입에 종속되지 않고 다양한 타입에 대해 동작하는 일반화된 코드를 작성할 수 있습니다.  
즉, 동일한 논리를 여러 데이터 타입에 대해 중복해서 작성하지 않아도 됩니다.

템플릿은 코드 재사용성을 증가시키고, 유지 보수성을 향상시킵니다.

컴파일러가 코드를 구체화하여 처리하므로 컴파일러에 따라 결과가 달라질 수 있습니다.  
각 타입에 대해 별도의 인스턴스를 생성하므로 바이너리 크기가 증가될 수 있습니다.

템플릿을 구체화하는 과정에서 발생하는 에러 메시지가 복잡하게 보일 수 있다는 단점이 있습니다.

함수 템플릿과 클래스 템플릿으로 나눌 수 있습니다.

템플릿 정의는 다음과 같습니다.

```cpp
template <typename 타입이름>
class MyClass {};
template <typename A, typename B>
class MyClass2 {};

template <typename 타입이름>
void func() {}
template <typename T>
void func2() {}
```

`template <typename T>`가 템플릿 정의 구문입니다.  
`T`는 타입 이름을 나타냅니다.

`MyClass`클레스에 대한 템플릿을 정의한 것입니다.  
`func`함수에 대한 템플릿을 정의한 것입니다.

`template <class T>`가 올 수 도 있습니다.  
이것은 위에서 알아본 `typename T`와 같은 의미를 가지며 같은 작업을 수행합니다.  
하지만 `typename`을 사용하는 것이 권장됩니다.


## 함수 템플릿

함수 템플릿(Function Template)은 데이터 타입에 의존하지 않아 동일한 로직을 여러 타입에 대해 사용할 수 있는 일반화된 함수 정의입니다.

함수 호출 시 전달된 인자의 타입을 기반으로 함수 템플릿을 구체화합니다.

---

함수 템플릿을 사용하는 예시는 다음과 같습니다.

템플릿으로 구현한 두 수를 더하는 함수입니다.

```cpp
template <typename T>
T add(T x, T y)
{
    return x + y;
}

int main()
{
    add<int>(3, 4);
    add(3.3, 4.2);
}
```

``add<int>(3, 4);``는 명시적으로 자료형을 지정한 호출입니다.  
``add(3.3, 4.2);``는 컴파일러가 타입을 추론합니다.

---

템플릿으로 구현한 배열의 합을 구하는 함수입니다.

```cpp
template <typename T>
T sumArray(T arr[], int size)
{
    T sum = 0;
    for (int i = 0; i < size; ++i) {
        sum += arr[i];
    }
    return sum;
}

int main()
{
    int intArr[] = {1, 2, 3, 4, 5};
    double doubleArr[] = {1.1, 2.2, 3.3, 4.4};

    sumArray(intArr, 5);
    sumArray(doubleArr, 4);
}
```

---

템플릿으로 구현한 두 값이 같은지 확인하는 함수입니다.

```cpp
template <typename T>
bool isEqual(T a, T b)
{
    return a == b;
}

int main()
{
    isEqual(10, 10);
    isEqual(3.5, 2.7);
    isEqual('a', 'b');
}
```


## 클래스 템플릿

클래스 템플릿(Class Template)은 데이터 타입에 의존하지 않아 다양한 데이터 타입을 처리하며, 재사용 가능한 클래스를 작성할 수 있습니다.

컴파일러는 인스턴스화를 통해 타입에 맞는 클래스 정의를 생성합니다.

---

클래스 템플릿을 사용하는 예시는 다음과 같습니다.

벡터를 직접 구현해보면서 작성했던 코드입니다.  
해당 클래스는 템플릿을 사용합니다.

```cpp
#include <iostream>
#include <algorithm>

template <typename T>
class SimpleVector
{
private:
	T* startPoint;
	int currentSize;
	int currentCapacity;

	void reSize(int newCapacity)
	{
		T* newData = new T[newCapacity];
		std::copy(startPoint, startPoint + currentSize, newData);
		delete[] startPoint;
		startPoint = newData;
		currentCapacity = newCapacity;
	}

public:
	SimpleVector(int size = 10) : startPoint(new T[size]), currentSize(0), currentCapacity(size) {}

	SimpleVector(const SimpleVector& other) : startPoint(nullptr), currentSize(other.currentSize), currentCapacity(other.currentCapacity)
	{
		this->startPoint = new T[other.currentCapacity];
		std::copy(other.startPoint, other.startPoint + other.currentSize, this->startPoint);
	}

	~SimpleVector()
	{
		// 동적 배열 메모리 해제
		delete[] startPoint;
	}

	void push_back(const T& value)
	{
		// 배열이 가득 찼는지 확인
		if (currentSize == currentCapacity)
		{
			int newCapacity = currentCapacity + 5;
			reSize(newCapacity);
		}

		// 배열의 마지막에 값 대입 및 사이즈 변경
		startPoint[currentSize++] = value;

		// 대입된 값 출력
		std::cout << "push_back : " << startPoint[currentSize - 1] << std::endl;
	}

	void pop_back()
	{
		// 배열에 값이 있는지 확인
		if (currentSize <= 0)
		{
			return;
		}

		T deleteValue = startPoint[currentSize - 1];

		// 배열의 크기 감소
		--currentSize;

		// 삭제된 값 출력
		std::cout << "pop_back : " << deleteValue << std::endl;
	}

	int size() const { return currentSize; }

	int capacity() const { return currentCapacity; }

	void sortData()
	{
		std::sort(startPoint, startPoint + currentSize);
	}
};
```

클래스 탬플릿을 사용할 때 `SimpleVector<int>`처럼 명시적으로 타입을 지정해주어야 합니다.  
C++17 이후 함수 템플릿처럼 클래스 템플릿도 타입을 자동으로 추론할 수 있게 됐습니다.

## 템플릿 특수화

템플릿 특수화(Template Specialization)는 특정 타입에 대해 별도의 동작을 정의하고 싶을 때 사용합니다.

템플릿 특수화를 정의하려면 템플릿 인수에서 특수화 하려는 인수를 비우고, 클래스 이름 옆에 자료형을 명시하면 됩니다.

템플릿 특수화의 예시는 다음과 같습니다.

```cpp
template <typename T>
class MyClass {}; // 기본 템플릿 정의

template <>
class MyClass<int> {}; // int 타입에 대한 특수화

template <typename A, typename B>
class MyClass2 {}; // 기본 템플릿 정의

template <typename B>
class MyClass2<int, B> {}; // A가 int일 경우에 대한 특수화
```

`MyClass`는 템플릿 인수가 하나이므로, `template <>`로 인수를 비워 템플릿을 선언합니다.  
클래스 이름 옆에 특수화 하고싶은 자료형인 `int`를 지정해 특수화를 선언합니다.

`MyClass2`는 두 개의 템플릿 인수를 받는 클래스입니다.  
`template <typename B>`와 같이 나머지 템플릿 인수는 남겨두고, 특수화 하려는 타입인 `int`를 지정해줍니다.  
해당 특수화 예시는 부분 특수화를 사용해 첫 번째 템플릿 인수인 `A`가 `int`일 때에만 별도의 처리를 하도록 템플릿 특수화를 선언했습니다.

## 비 타입 템플릿 파라미터

비-타입(non-type) 템플릿 파라미터는 데이터 타입이 아닌 정수나 포인터같은 값을 받는것입니다.

예시는 다음과 같습니다.

```cpp
template <typename T, int size>
class Array {};

int main() {
    Array<int, 5> obj;
}
```

## 가변 템플릿

가변 템플릿(Variadic Templates)은 함수의 가변인자처럼 여러 개의 템플릿 파라미터를 처리할 수 있습니다.  
C++11부터 사용 가능합니다.

```cpp
template <typename... Args>
void print(Args... args) {}

int main() {
    print(1, 2.5, "Hello", 'a');
}
```