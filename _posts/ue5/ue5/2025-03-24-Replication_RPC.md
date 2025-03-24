---
layout: single

title: "[UE5] Replication과 RPC"

categories:
    - UE5
tag: [Unreal Engine, UE5]

date: 2025-02-23
last_modified_at: 2025-02-23

order : 500060
---

# Replication과 RPC

`Server-Client`간 통신을 위한 두 가지 핵심 메커니즘이 `Replication`과 `RPC`(Remote Procedure Call)입니다.  
이들은 목적과 동작이 다르므로 구분해서 이해해야 합니다.

간단하게 비유하면 `Replication`은 값 동기화, `RPC`는 원격 함수 호출입니다.  
필요에 따라 병행하여 사용합니다.

## Replication

`Replication`은 `Network Framework API`를 상속받아 구현된 기능으로, 서버에서 클라이언트로 객체 상태를 자동 동기화해주는 기능입니다.

`Property`나 `Actor`의 `Replicates`을 `true`로 설정하면, 엔진의 네트워킹 시스템이 주기적으로 해당 데이터를 관련된 클라이언트들에게 보냅니다.

자동으로(프레임마다/필요한 시점마다) 값을 보내주며 내부적으로 최적화됩니다.  
여러 변수의 `Replication`은 한 번에 묶어서 전송되며, 특정 순서를 보장되지 않습니다.

단방향으로 서버에서 클라이언트로만 동기화가 가능합니다.  
만약 특정 권한이 있는 정보를 변경하려면, 클라이언트에서 서버로 요청해야 합니다.

일반적으로 상태(State) 기반이기 때문에 `Property`의 현재 값을 보냅니다.  
이벤트 자체를 보내지 않기 때문에 동기화되는 시점에 특정 작업을 하려한다면 `ReNotify`를 사용할 수 있습니다.

캐릭터의 위치, 체력, 문이 열렸는지/닫혔는지 등 계속 변할 수 있는 상태를 동기화하는데 적합합니다.  
예를 들어 FPS 게임에서 플레이어의 총알 갯수는 (계속 변할 수 있고 현재 값을 유지해야 하는 정보)은 `Replicated Property`로 만듭니다.

언리얼 엔진의 네트워크 시스템은 한 번에 전송할 수 있는 데이터 크기에 제한(약 64KB)이 있습니다.  
대용량 데이터를 한 번에 복제하려 하면, 네트워크 패킷을 초과하거나, 전송이 실패할 위험이 있습니다.  
Reliable RPC를 여러 개 호출하여 데이터를 나누어 보내고, 클라이언트에서 재조립하는 방법을 시도할 수 있습니다.  
그러나 RPC는 일정량의 버퍼를 차지하므로, 너무 많은 Reliable RPC를 보내면 네트워크가 과부하(Reliable Buffer Overflow)될 수 있습니다.  
소켓 통신(TCP/UDP) 또는 Online Subsystem의 파일 전송 기능을 사용하여 데이터를 따로 전송하고, 상태만 간단히 복제(Replication)하여 동기화하는 방식을 시도할 수 있습니다.

거대한 데이터 자체를 보내는 대신 Reference나 ID만 보내고 클라이언트가 자체적으로 준비하게 할 수 있습니다.  
예를 들어, 수백 개의 오브젝트 위치를 가진 레벨 정보를 통째로 보내는 대신, 서버가 전체 데이터를 직접 보내는 대신 ID(참조 값) 만 보내고, 클라이언트가 이를 이용해 로컬에서 데이터를 로드하게 할 수 있습니다.  
또는 아주 많은 하위 객체들이 있는 경우, 한 액터에 모두 넣어서 복제하는 대신 여러 액터/컴포넌트로 나눠서 각각 복제하게 할 수도 있습니다.  
그러면 엔진이 그것들을 개별적으로 관리하고, 필요하면 일부만 보낼 수도 있습니다.

### Replication Conditions

Replication Conditions는 네트워크 환경에서 특정 변수를 어떤 조건에서 클라이언트로 전송할지를 결정하는 중요한 요소입니다.  
즉, 특정 조건을 만족할 때만 해당 변수가 클라이언트로 복제됩니다.

불필요한 데이터 전송을 줄이고, 네트워크 트래픽을 감소시켜 성능을 최적화 할 수 있습니다.  
소유자만 볼 수 있는 정보와 다른 플레이어도 볼 수 있는 정보를 분리 할 수 있습니다.  
특정 조건에서만 데이터를 전송하면 예측할 수 없는 동기화 문제를 방지할 수 있습니다.

![Replication_RPC-Replication_Conditions]({{site.url}}/images/Unreal/ue5/2025-03-24-Replication_RPC/Replication_RPC-Replication_Conditions.PNG)

+ `COND_None`: 기본값으로, 모든 클라이언트에 항상 복제
+ `COND_InitialOnly`: 객체가 생성될 때 복제(이후 값이 변경되어도 복제되지 않음)
+ `COND_OwnerOnly`: 해당 객체의 소유자(Client Owner)에게만 복제
+ `COND_SkipOwner`: 객체의 소유자(Client Owner)에게는 복제되지 않고, 나머지 클라이언트에게만 복제
+ `COND_SimulatedOnly`: 시뮬레이션 클라이언트(즉, 로컬 컨트롤이 없는 클라이언트)에게만 복제
+ `COND_AutonomousOnly`: 자율 클라이언트(즉, 로컬 컨트롤이 있는 플레이어)에게만 복제
+ `COND_SimulatedOrPhysics`: 시뮬레이션 클라이언트이거나 물리적으로 활성화된 경우에만 복제
+ `COND_InitialOrOwner`: 객체가 생성될 때 모든 클라이언트에 복제되며, 이후에는 소유자에게만 복제
+ `COND_Custom`: 사용자 정의 논리에 따라 복제 여부를 결정
+ `COND_ReplayOrOwner`: 리플레이 시스템을 사용할 때 또는 소유자에게만 복제
+ `COND_ReplayOnly`: 리플레이 시스템에서만 복제
+ `COND_SkipReplay`: 리플레이에서는 복제되지 않음
+ `COND_Never`: 절대 복제되지 않음 (클라이언트에 전송되지 않음)

```cpp
UPROPERTY(ReplicatedUsing = OnRep_Health, ReplicationCondition = COND_OwnerOnly)
float Health;

UPROPERTY(Replicated, ReplicationCondition = COND_SkipOwner)
float Speed;
```

### Actor Relevancy

`Actor Relevancy`는 시스템을 사용하여 각 클라이언트가 어떤 액터의 정보를 받을지 결정합니다.  
즉, 각 클라이언트가 관련된지 여부를 판단하고, 관련 있는 액터를 `Replication` 받도록 필터링하는 메커니즘입니다.

모든 네트워크 액터(Actor)를 모든 클라이언트에 전송하는 것은 비효율적이므로, 효율화하기 위한 방법입니다.

![Replication_RPC-Actor_Relevancy]({{site.url}}/images/Unreal/ue5/2025-03-24-Replication_RPC/Replication_RPC-Actor_Relevancy.PNG)

기본 규칙은 거리 기반입니다.  
`NetCullDistanceSquared`라는 속성의 값 이내에 있는 플레이어들에게만 해당 액터를 복제시킵니다.

만약 `bAlwaysRelevant`속성을 활성화하는 경우 거리와 상관없이 모든 클라이언트에 복제됩니다.  
`GameMode` 등의 전역 객체에 사용합니다.

`NetDormancy`는 네트워크 관점에서 액터를 휴면 시키는 기능입니다.  
액터가 `Dormant`로 표시되면, 해당 액터의 연결을 깨울 때까지 업데이트를 보내지 않습니다.  
`SetNetDormancy(DORM_DormantAll)`을 호출하면 해당 액터가 모든 클라이언트에서 휴면 상태가 됩니다.  
`DORM_DormantPartial`은 부분 휴면이 가능합니다.

`Replicatie`된 프로퍼티를 업데이트하면 액터가 자동으로 `AActor::FlushNetDormancy`를 호출하며 다시 `Awake` 상태가 됩니다.

예를 들어, 문이 닫힌 상태는 처음 모든 클라이언트에 전달되고, 더 이상 변하지 않는다면 문을 `Dormant`로 설정합니다.  
이 상태에서 무작위 플레이어가 문을 열면 서버에서 액터를 깨우고(`FlushNetDormancy`) 최신 상태를 보내며, 필요하다면 다시 `Dormant`를 설정합니다.

`NetUpdateFrequency`는 초당 최대 업데이트 횟수입니다.  
값이 높을 수록 자주 업데이트에 고려되어 간격이 짧아지고, 값이 낮을 수록 업데이트의 간격이 커집니다.

`NetPriority`는 중요도를 나타내며, 네트워크 대역폭(`Bandwidth`)이 부족할 때 어떤 액터를 우선 복제할지 결정하는 값입니다.  
값이 높을수록 우선적으로 복제되며, 값이 낮아 중요도가 낮은 액터의 업데이트는 건너뛰기도 합니다.

### Adaptive Frequency

`Adaptive Frequency`는 엔진이 네트워크 `Replication` 업데이트 빈도를 동적으로 조절하는 기능입니다.  
즉, 필요한 순간에는 자주 업데이트하고, 필요하지 않을 때는 업데이트를 줄여서 네트워크 부하를 최적화하는 방식입니다.

액터의 `Replication` 속성에서 최솟값(`MinNetUpdateFrequency`)과 최댓값(`NetUpdateFrequency`)을 설정해 주면, 그 사이에서 현재 액터에 맞는 `NetUpdateFrequency`로 자동으로 설정합니다.

`DefaultEngine.ini`에서 설정할 수 있습니다.

```cpp
// DefaultEngine.ini
[SystemSettings]
net.UseAdaptiveNetUpdateFrequency=1
```

### RepNotify

`RepNotify`는 변수(프로퍼티)가 `Replication`될 때 자동으로 실행되는 콜백 함수입니다.  
즉, 서버에서 변경된 변수가 클라이언트로 전파될 때, 클라이언트에서 특정 함수를 호출하도록 설정하는 기능입니다.

UI 갱신, 애니메이션 재생, 사운드 출력 등의 추가적인 반응을 쉽게 구현할 수 있습니다.

```cpp
// 헤더 파일 (MyCharacter.h)
UPROPERTY(ReplicatedUsing = OnRep_Health)
float Health;

UFUNCTION()
void OnRep_Health();

// CPP 파일 (MyCharacter.cpp)
void AMyCharacter::OnRep_Health()
{
    UE_LOG(LogTemp, Warning, TEXT("Health 변경됨: %f"), Health);
    UpdateHealthUI(); // UI 갱신
}
```

## RPC

`RPC`(Remote Procedure Call)는 네트워크를 통해 클라이언트 또는 서버가 원격 시스템에서 함수를 호출하는 명시적인 방법입니다.  
즉, 특정 순간에 실행되는 동작/이벤트를 원격으로 호출하는 것입니다.

이를 통해 클라이언트와 서버가 서로 다른 머신에서 실행되더라도, 마치 로컬에서 함수 호출을 하는 것처럼 쉽게 통신할 수 있습니다.

멀티플레이어 환경에서 서버와 클라이언트 간의 데이터를 동기화, 명령 전달, 상태 변경 등을 위해 사용됩니다.

개발자가 필요한 순간에 직접 호출해야 하고, 호출할 때마다 네트워크 `Packet`을 생성합니다.  
`Reliable`와 `Unreliable`를 선택할 수 있는데, 같은 종류의 `RPC`들끼리는 호출 순서가 보장됩니다.  
별도의 키워드를 지정하지 않으면 기본적으로 `Unreliable`로 간주됩니다.

`UFUNCTION` 매크로에 `Server`, `Client`, `NetMulticast` 같은 지정자를 붙여 만들며, 이를 호출하면 설정된 대상(Server 또는 Client들)에서 함수가 실행됩니다.  

|RPC 종류|호출 방향|실행 위치|주요 용도|
|---|---|---|---|
|Server RPC|클라이언트 -> 서버|서버|총알 발사, 체력 감소, 이동, 아이템 습득 등 유저 요청 전달|
|Client RPC|서버 -> 클라이언트|클라이언트|UI 업데이트, 효과음 재생, 알림 등 개인화된 피드백 전달|
|Multicast RPC|서버 -> 모든 클라이언트|서버(Optional) 및 모든 클라이언트|애니메이션, 폭발 이펙트, 사운드 등 모든 플레이어가 공유하는 이벤트|

```cpp
UFUNCTION(Server, Reliable)
void FireWeapon();

UFUNCTION(NetMulticast, Unreliable)
void MulticastPlayExplosionEffect();
```

플레이어의 행동(공격, 이동, 상호작용 등)을 서버에 전달하여 게임 상태를 업데이트하거나, 서버에서 결정된 결과를 클라이언트에 알려주는 경우에 사용됩니다.  
예를 들어, 클라이언트에서 `FireWeapon`(플레이어가 총을 쐈다는 행위)이라는 `Server RPC`를 호출하면, 그 함수가 서버에서 실행되며, 총알 발사 처리를 합니다.

### Reliable

Reliable로 지정된 RPC는 패킷이 반드시 전송 대상에게 도달하는 것이 보장됩니다.  
즉, 패킷이 손실되더라도 재전송되어 수신 측에 도달하게 합니다.

엔진이 해당 RPC를 수신 측에서 확인할 때까지 재전송을 시도합니다.

동일 채널의 Reliable RPC들은 보낸 순서가 유지됨을 보장합니다.

중요한 이벤트를 전송할 경우 필수입니다.

패킷을 재전송하기 때문에 네트워크 상태가 좋지 않을 경우 딜레이가 발생할 수 있습니다.

어떤 패킷이 유실되면, 그 RPC가 재전송되어 도착할 때까지 이후에 보내진 다른 Reliable RPC들도 대기하게 됩니다.  
Reliable RPC를 너무 많이 사용하면 네트워크 Packet Queue(Buffer)를 채워 다른 메시지를 막아버릴 수 있습니다.  
그러므로 RPC를 Reliable로 지정하는 것은 반드시 도달해야 하는 이벤트에만 사용하는 것이 좋습니다.

기본 Reliable 버퍼 한도는 256개입니다.

### Unreliable

Unreliable RPC는 패킷이 전송 대상에게 도달하는 것을 보장하지 않습니다.  
네트워크 상황에 따라 패킷이 손실될 수 있으며, 패킷의 도착 순서도 보장하지 않습니다.

Unreliable RPC는 오버헤드가 적습니다.

네트워크가 해당 패킷을 Drop하면 재전송하지 않습니다.  
패킷을 재전송하지 않기 때문에 지연이 거의 없으며 다른 Unreliable RPC가 대기하지 않습니다.

동일 채널에선 발신 순서를 유지하지만, 도착은 보장되지 않으므로 결과적으로 일부 누락되거나 순서가 보장되지 않습니다.

그러나 많은 경우(특히 빈번한 업데이트)에서 한두 번 메시지가 떨어져도 큰 문제는 없습니다.

중요도가 낮고, 빈도가 높은 이벤트나 약간 손실되어도 빠르게 갱신되는 데이터를 동기화하는 경우에 적합합니다.