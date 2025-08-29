---
layout: single

title: "[Unity6] 오브젝트 이동시키기"

categories:
    - UnityPractice
tag: [Unity6, UnityPractice]

date: 2025-08-29
last_modified_at: 2025-08-29

order : 100
---

# 오브젝트 이동시키기

`Vector3` 구조체에서 제공하는 여러 보간(Interpolation) 함수를 활용해 오브젝트를 이동시킬 수 있습니다.

대표적으로 `MoveTowards`, `SmoothDamp`, `Lerp`, `Slerp` 네 가지 함수가 있습니다.

## MoveTowards

`Vector3.MoveTowards` 함수는 현재 위치에서 목표 위치로 일정한 속도만큼 움직이도록 하는 함수입니다.  
즉, 매 프레임마다 일정한 거리를 이동하기 때문에 직선 경로로 가속과 감속이 없이 움직입니다.

```C#
Vector3.MoveTowards(Vector3 current, Vector3 target, float maxDistanceDelta);
```

- current: 현재 위치
- target: 목표 위치
- maxDistanceDelta: 한 번 호출될 때 이동할 최대 거리(속도)

```C#
Vector3 targetPosition = new Vector3(10.f, 5.f, 0.f);

void Update()
{
    transform.position = Vector3.MoveTowards(
        transform.position, 
        targetPosition, 
        1.f * Time.deltaTime);
}
```

하드웨어 성능 등 다양한 요인으로 프레임이 달라질 수 있기 때문에 속도에 `Time.deltaTime`을 곱해 프레임 보정하여 이동을 균일하게 해줄 수 있습니다.

## SmoothDamp

`Vector3.SmoothDamp`함수는 목표 지점에 가까워질수록 속도를 줄이면서 부드럽게 도달하도록 만드는 함수입니다.

물리적으로 감속 운동에 가깝습니다.

카메라 이동이나 자연스러운 오브젝트 추적에 사용되기도 합니다.

```C#
Vector3.SmoothDamp(Vector3 current, Vector3 target, ref Vector3 currentVelocity, float smoothTime);
```

- current: 현재 위치
- target: 목표 위치
- currentVelocity: 현재 속도를 저장하는 참조 변수
- smoothTime: 목표 지점에 도달하는 데 걸리는 예상 시간(값이 작을수록 빠르게 이동)

```C#
Vector3 targetPosition = new Vector3(10.f, 5.f, 0.f);
Vector3 velocity = Vector3.up * 10.f;

void Update()
{
    transform.position = Vector3.SmoothDamp(
        transform.position, 
        targetPosition, 
        ref velocity, 
        1.f);
}
```

내부적으로 `deltaTime`을 고려하기 때문에 별도로 곱하지 않습니다.

## Lerp

`Vector3.Lerp` 함수는 현재 위치와 목표 위치 사이를 0 ~ 1 사이의 비율로 보간합니다.  
예를 들어, 0.5를 넣으면 두 위치의 중간으로 이동합니다.

```C#
Vector3.Lerp(Vector3 a, Vector3 b, float t);
```

- a: 시작 위치
- b: 목표 위치
- t: 보간 비율(0 ~ 1)

```C#
Vector3 targetPosition = new Vector3(10.f, 5.f, 0.f);

void Update()
{
    transform.position = Vector3.Lerp(
        transform.position, 
        targetPosition, 
        0.1f * Time.deltaTime); // 매번 10%씩 가까워진다.
}
```

보간 비율을 사용하지만, 프레임 단위로 실행되기 때문에 보정을 위해 `deltaTime`을 곱해줍니다.

## Slerp

`Vector3.Slerp` 함수는 `Lerp` 함수와 유사하지만, 두 벡터를 원호를 그리며 이동합니다.

회전 방향 보간이나 곡선 궤적이 필요한 경우에 사용되기도 합니다.

```C#
Vector3.Slerp(Vector3 a, Vector3 b, float t);
```

- a: 시작 위치
- b: 목표 위치
- t: 보간 비율(0 ~ 1)

```C#
Vector3 targetPosition = new Vector3(10.f, 5.f, 0.f);

void Update()
{
    transform.position = Vector3.Slerp(
        transform.position, 
        targetPosition, 
        1.f * Time.deltaTime);
}
```

Lerp와 같은 이유로 `deltaTime`을 곱해줍니다.