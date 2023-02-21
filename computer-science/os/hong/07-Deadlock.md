# 7. Deadlock
<!-- TOC -->

- [Deadlock](#deadlock)
    - [Deadlock](#deadlock)
    - [Deadlock 발생 4가지 조건](#deadlock-%EB%B0%9C%EC%83%9D-4%EA%B0%80%EC%A7%80-%EC%A1%B0%EA%B1%B4)
        - [Resource-Allocation Graph](#resource-allocation-graph)
    - [Deadlock 처리 방법](#deadlock-%EC%B2%98%EB%A6%AC-%EB%B0%A9%EB%B2%95)
        - [Deadlock Prevention](#deadlock-prevention)
        - [Deadlock Avoidance](#deadlock-avoidance)
        - [Deadlock Detection and recovery](#deadlock-detection-and-recovery)

<!-- /TOC -->

<br>

## Deadlock

> 일련의 프로세스들이 서로가 가진 자원을 기다리며 block된 상태

- 프로세스가 자원을 사용하는 절차
    - Requeset
    - Allocate
    - Use
    - Release

<br>

## Deadlock 발생 4가지 조건

1. Mutual exclusion (상호 배제)
    - 매 순간 하나의 프로세스만이 자원을 사용할 수 있음
2. No preemption (비선점)
    - 프로세스는 자원을 스스로 내어놓을 뿐 강제로 빼앗기지 않음
3. Hold and wait (보유대기)
    - 자원을 가진 프로세스가 다른 자원을 받을 때까지 보유 자원을 놓지 않고 계속 가지고 있음
4. Circular wait (순환대기)
    - 자원을 기다리는 프로세스간에 사이클이 형성되어야 함

<br>

### Resource-Allocation Graph

- 그래프에 cycle 이 없다면 deadlock 이 아니다
- cycle 이 있다면
    - 자원의 인스턴스가 하나이면, deadlock 이다.
    - 자원의 인스턴스가 여러개면, deadlock 이 아닐수도 있다.

<br>

## Deadlock 처리 방법

> 위로 갈수록 Deadlock 을 방지하지만, 오버헤드가 상당히 크다. 
- Deadlock 이 빈번하게 발생하는 것이 아니기때문에 방지하기 위해 제약을 두는 것은 비효율적이다.

1. Deadlock Prevention
2. Deadlock Avoidance
3. Deadlock Detection and recovery
4. Deadlock Ignorance

<br>

### Deadlock Prevention

> 자원 할당 시 Deadlock의 4가지 필요 조건 중 어느 하나가 만족되지 않도록 하는 것

- Mutual Exclusion
    - 공유해서는 안되는 자원의 경우 반드시 성립해야 함
- Hold and Wait
    - 프로세스가 자원을 요청할 때 다른 어떤 자원도 가지고 있지 않아야 한다.
- No Preemption
    - process가 어떤 자원을 기다려야 하는 경우 이미 보유한 자원이 선점됨
    - 모든 필요한 자원을 얻을 수 있을 때 프로세스가 다시 시작된다.
- Circular Wait
    - 모든 자원 유형에 할당 순서를 정하여 정해진 순서대로만 자원 할당

> Utilization 저하, throughtput 감소, starvation 문제

<br>

### Deadlock Avoidance

> 자원 요청에 대한 부가적인 정보를 이용해서 deadlock의 가능성이 없는 경우에만 자원을 할당

- 자원 요청시 safe 상태를 유지할 경우에만 자원을 할당

- 방법
    1. 자원 인스턴스가 1개일때
        - Resource Allocation Graph algorithm 사용
            - cycle 이 생기지 않는 경우에만 요청 자원을 할당
    2. 자원 인스턴스가 여러개일때
        - Banker's Algorithm 사용
            - 프로세스의 최대 요청 자원이 현재 가용 자원보다 작은 경우에만 요청 자원을 할당
<br>

### Deadlock Detection and recovery

> Deadlock 발생은 허용하되 그에 대한 detection 루틴을 두어 deadlock 발견시 recovery

- Detection
    - Resource 당 instance 가 한개인 경우
        - cycle 이 발생하면 deadlock 발생
    - Resource 당 instance 가 여러개인 경우
        - Banker's Algorithm 과 유사

- Recovery
    - Process termination
        - 관련된 모든 프로세스를 제거
    - Resource Preemption
        - 비용을 최소화할 목표물을 선정하고 프로세스를 하나씩 제거
        - Starvation 문제 우려
            - 동일한 프로세스만 계속해서 victim 으로 선정되는 경우