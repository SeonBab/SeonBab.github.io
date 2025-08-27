---
layout: single

title: "[Unity6] 자료형의 종류"

categories:
    - Unity6
tag: [Unity, Unity6]

date: 2025-08-27
last_modified_at: 2025-08-27

order : 10
---

# 자료형의 종류

유니티에서 사용하는 자료형은 크게 C#에서 기본적으로 제공하는 자료형과 유니티 엔진이 제공하는 고유 자료형 두 가지로 나눌 수 있습니다.

## 기본 자료형

C# 언어에서 제공하는 기본적인 자료형이며, 유니티 스크립트에서 동일하게 사용됩니다.

### 정수형

- `sbyte`: 8비트 정수 (-128 ~ 127)
- `byte`: 8비트 정수 (0 ~ 255)
- `short`: 16비트 정수
- `int`: 32비트 정수
- `long`: 64비트 정수
- `uint`, `ulong`, `ushort`: 부호 없는 정수형

### 실수형

- `float`: 32비트 부동소수점
- `double`: 64비트 부동소수점
- `decimal`: 128비트 부동소수점

### 문자/문자열

- `char`: 문자
- `string`: 문자열

### 논리형

- `bool`: 8비트, `true` 혹은 `false`를 저장

### 컬렉션

- `List<T>`: 가변 길이 리스트
- `Dictionary<TKey, TValue>`: 키-값 쌍
- `Queue<T>`, `Stack<T>` 등등

## 고유 자료형

유니티 엔진 내부에 정의된 구조체나 클래스입니다.

제 기준 자주 사용한다고 판단되는 자료형을 정리했습니다.

### 벡터/위치/방향

- `Vector2`: 2차원 벡터
- `Vector3`: 3차원 벡터
- `Vector4`: 4차원 벡터
- `Quaternion`: 회전을 표현하는 단위 사원수
- `Matrix4x4`: 4x4 행렬

### 좌표/공간

- `Rect`: 2D 사각형 영역
- `Bounds`: 3D 경계 영역
- `Transform`: 위치, 회전, 크기를 표현하는 컴포넌트
- `Ray` : 광선(시작점 + 방향)
- `RaycastHit`: 레이캐스트 충돌 정보

### 색상

- `Color`: RGBA 색상(0 ~ 1)
- `Color32`: RGBA 색상(0 ~ 255 정수)

### 시간

- `Time.deltaTime`: 한 프레임에 걸린 시간
- `Time.time`: 게임 시작 후 경과 시간

### 게임 객체

- `GameObject`: 유니티 씬 안의 모든 오브젝트를 표현하는 클래스
- `Component`: `GameObject`에 붙는 구성 요소의 기본 클래스
- `MonoBehaviour`: 스크립트가 상속받는 기본 클래스

### 컬렉션

- `NativeArray<T>`: 고정 크기 배열, Job System, Brust 등에 사용
- `NativeList<T>`: 가변 길이 배열
- `NativeQueue<T>`, `NativeStack`, `NativeHashMap<TKey, TValue>` 등등
- `ScriptableObject`: 데이터 보관을 위한 오브젝트