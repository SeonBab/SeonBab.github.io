---
layout: single

title: "[C++] 참조자"

categories:
    - Cpp
tag: [Cpp]

date: 2024-12-31
last_modified_at: 2024-12-31

mermaid: true

order : 165
---

# 참조자

참조자(Reference)는 변수에 대한 별칭을 제공하는 개념입니다.

변수를 직접 복사하지 않고 원래 변수에 접근할 수 있도록 합니다.  
포인터와 유사하지만, 사용법과 특성이 약간 다릅니다.

보통 코드의 가독성을 높이고, 복사 오버헤드를 줄이기 위해 사용됩니다.  
일반적으로 함수 인자로 사용하거나 함수의 반환 값으로 사용합니다.

참조자의 타입은 참조하려는 변수의 타입과 일치해야합니다.

참조자가 소멸된 객체를 참조하려 하면 오류가 발생하므로 주의해야합니다.  
이 경우를 댕글링 참조(Dangling Reference)라고 합니다.

## 참조자 선언

참조자는 선언 시 `&`연산자를 사용하여 생성합니다.  
또한 초기화가 반드시 되어야 하며, 초기화되지 않은 상태일 수 없습니다.

참조자를 선언하는 방법은 다음과 같습니다.

```cpp
int a = 10;
int& ref = a;
int& refError; // 에러
```

`ref`라는 변수는 `a`변수의 메모리 공간을 참조하게 됩니다.  
이 경우 `ref`를 통해 값을 바꾸면 같은 메모리 공간을 사용하는 `a`의 값 또한 바뀌게 됩니다.

참조자는 한번 선언된 후에는 다른 변수로 참조 대상을 변경할 수 없습니다.

```cpp
int a = 10, b =20;
int& ref = a;
ref = b;
```

`ref`는 `a`를 참조합니다.  
이때, `b`변수를 참조하려고 한다면 참조가 되는 것이 아닌 `b`변수의 값이 `a`변수에 복사가 됩니다.  
즉 `a`의 값이 `b`의 20으로 값이 변경됩니다.  
그렇게 때문에 위의 코드는 에러가 나지 않습니다.

rvalue에 대한 참조도 선언 및 초기화가 가능합니다.

```cpp
int&& a = 3;
```

이 경우 `3`을 참조합니다.

## 포인터와의 차이점

포인터는 참조 대상을 변경 할 수 있지만, 참조자는 참조 대상을 변경할 수 없습니다.
참조 대상을 변경 할 수 없으므로, 선언 시 반드시 초기화를 해주어야 합니다.

참조자는 포인터처럼 메모리 주소를 노출하지 않고, 사용자가 직접 메모리 주소를 조작할 필요가 없습니다.  

포인터처럼 `*`나 `&`를 사용해 접근할 필요가 없기 때문에 기본 자료형의 변수처럼 사용합니다.

## 참조자의 활용

참조자는 일반적으로 함수에서 많이 사용하게 됩니다.

가장 먼저 함수의 인자로 사용 될 경우를 살펴보겠습니다.

```cpp
void func1(int num);
void func2(int& num);
```

`func1`처럼 매개변수에 참조자가 없는 경우 함수가 호출 될 때 변수가 복사가 되며, 원본 데이터에 접근 할 수 없습니다.  
하지만 `func2`의 경우 변수의 복사가 되지 않으며, 원본 데이터를 수정할 수 있게 됩니다.

함수의 반환 값으로 사용 될 경우 다음과 같습니다.

```cpp
int func1(int& num);

int& func2(int& num)
{
    return num;
}
```

`func1`의 경우 반환 값에 참조자가 없는데, 이 경우 반환 값은 값의 복사가 이루어 진것으로 `rvalue`가 옵니다.  
`func2`의 경우 반환 값에 참조자가 있으므로, 값의 복사는 이루어지지 않습니다.  
만약 해당 반환값에 접근한다면 원본 데이터에 접근하거나 수정할 수 있습니다.

참조자를 반환 값에 사용하면 함수 호출 후에도 원본 데이터에 접근하거나 수정할 수 있습니다.

이 외에도 상수 참조나 임시 객체에 대한 참조를 할 때에도 사용합니다.

```cpp
void func1(const int& num); // 상수 참조
void func2(const int& num); // 임시 객체 참조

main()
{
    func2(2);
}
```

임시 객체를 참조 할 경우 `const`키워드를 사용해야합니다.  
임시 객체가 함수 호출 후 소멸할 수 있기 때문에, 값을 변경하지 않겠다고 명시적으로 표시합니다.