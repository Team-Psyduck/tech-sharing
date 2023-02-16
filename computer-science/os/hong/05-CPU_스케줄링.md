# 5. CPU 스케줄링
<!-- TOC -->

- [CPU 스케줄링](#cpu-%EC%8A%A4%EC%BC%80%EC%A4%84%EB%A7%81)
    - [CPU 스케줄러](#cpu-%EC%8A%A4%EC%BC%80%EC%A4%84%EB%9F%AC)
        - [CPU 스케줄링 방식](#cpu-%EC%8A%A4%EC%BC%80%EC%A4%84%EB%A7%81-%EB%B0%A9%EC%8B%9D)
    - [Dispacher](#dispacher)
    - [스케줄링의 성능 평가](#%EC%8A%A4%EC%BC%80%EC%A4%84%EB%A7%81%EC%9D%98-%EC%84%B1%EB%8A%A5-%ED%8F%89%EA%B0%80)
        - [성능 평가요소](#%EC%84%B1%EB%8A%A5-%ED%8F%89%EA%B0%80%EC%9A%94%EC%86%8C)
    - [스케줄링 알고리즘](#%EC%8A%A4%EC%BC%80%EC%A4%84%EB%A7%81-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98)
        - [FCFS - First Come First Served](#fcfs---first-come-first-served)
        - [SJF - Shortest Job First](#sjf---shortest-job-first)
        - [우선순위 스케줄링](#%EC%9A%B0%EC%84%A0%EC%88%9C%EC%9C%84-%EC%8A%A4%EC%BC%80%EC%A4%84%EB%A7%81)
        - [Round Robin 스케줄링](#round-robin-%EC%8A%A4%EC%BC%80%EC%A4%84%EB%A7%81)
        - [Multi-level Queue](#multi-level-queue)
        - [Multilevel Feedback Queue](#multilevel-feedback-queue)
        - [Multi-processor system](#multi-processor-system)
        - [Real-time system](#real-time-system)
    - [스케줄링 알고리즘의 평가](#%EC%8A%A4%EC%BC%80%EC%A4%84%EB%A7%81-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98%EC%9D%98-%ED%8F%89%EA%B0%80)

<!-- /TOC -->

<br>

> 프로그램의 수행은 CPU burst 와 I/O burst 가 번갈아가면서 이루어진다.

- CPU bound process
    - I/O 작업을 거의 수행하지 않아 CPU burst 가 길게 나타나는 프로세스
    - 계산 위주의 프로그램
- I/O bound process
    - I/O 요청이 빈번해 CPU burst가 짧게 나타나는 프로세스
    - 대화형 프로그램

> CPU 스케줄링은 CPU 사용패턴이 상이한 여러 프로그램이 동일한 시스템 내부에서 함께 실행되기 때문에 필요하다.

<br>

## CPU 스케줄러

> ready 상태에 있는 프로세스들 중 어떠한 프로세스에게 CPU를 할당할지 결정하는 운영체제의 Code 다.

스케줄링이 필요한 경우
1. running -> blocked 상태로 변경 (I/O 요청 등)
2. running -> ready 상태로 변경 (timer interrupt)
3. blocked -> ready 상태로 변경 (I/O 완료 후 인터럽트 발생)
4. running -> terminate (종료)

<br>

### CPU 스케줄링 방식

- nonpreemptive(비선점형)
    - CPU를 획득한 프로세스가 스스로 CPU 를 반납하기 전까지 빼앗기지 않는 방법
- preemptive(선점형)
    - CPU를 강제로 빼앗을 수 있는 스케줄링 방법

<br>

## Dispacher

> CPU 스케줄러가 어떤 프로세스에게 CPU를 할당할지 결정한 후, 새롭게 선택된 프로세스가 CPU를 할당받고 작업을 수행할 수 있도록 환경설정을 하는 운영체제의 Code

- Dispacher 는 수행중이던 프로세스의 문맥을 PCB로부터 복원한 후, 새로운 프로세스에게 CPU를 넘기는 과정을 수행

- `Dispacher latency(지연시간)`
    - Dispacher 가 하나의 프로세스를 정지시키고 다른 프로세스에게 CPU를 전달하기까지 걸리는 시간
    - 대부분은 Context Switch 오버헤드에 해당

<br>

## 스케줄링의 성능 평가

### 성능 평가요소
- 시스템 관점
    - CPU 이용률
        - 전체 시간 중 CPU가 일을 한 시간의 비율
    - 처리량
        - 주어진 시간동안 ready queue 에서 기다리고 있는 프로세스 중 완료한 갯수

- 사용자 관점
    - 소요시간
        - 프로세스가 CPU를 요청한 시점부터 원하는 만큼 다 쓰고 `CPU burst`가 끝날 때까지 걸린 시간
    - 대기시간
        - CPU burst 기간 중 프로세스가 ready queue에서 CPU를 얻기 위해 기다린 시간의 합
    - 응답시간
        - 프로세스가 ready queue에 들어온 후 첫번째 CPU를 획득하기까지 기다린 시간
        - 사용자 입장에서 가장 중요한 척도

<br>

## 스케줄링 알고리즘

### FCFS - First Come First Served

> 프로세스가 ready queue 에 도착한 시간 순서대로 CPU를 할당하는 방식

- CPU burst 가 긴 프로세스를 먼저 처리할 경우, 뒤에 CPU burst 가 짧은 I/O bound process 들이 계속 기다려야하는 비효율 발생
    - 평균 대기시간(average waiting time) 증가
    - I/O 장치들의 이용률 하락

<br>

### SJF - Shortest Job First

> CPU burst 가 가장 짧은 프로세스부터 CPU를 할당하는 방식

- 평균 대기시간을 가장 짧게하는 최적 알고리즘

1. 비선점형 방식
    - 현재 CPU에서 실행 중인 프로세스의 남은 CPU burst 시간보다 더 짧은 프로세스가 도착해도 그냥 진행
2. 선점형 방식
    - 현재 CPU에서 실행 중인 프로세스의 남은 CPU burst 시간보다 더 짧은 프로세스가 도착할 경우 CPU 를 강탈해서 더 짧은 프로세스에게 할당

- 프로세스의 CPU burst 시간을 미리 알 수 없어서 예측을 통해 CPU burst 시간을 구해서 CPU를 할당해야함.

<br>

### 우선순위 스케줄링

> ready queue 에서 기다리는 프로세스들 중 우선순위가 가장 높은 프로세스부터 CPU를 할당하는 방식

- 우선순위는 우선순위 값을 표시하며 값이 작을수록 높은 우선순위를 가짐.

1. 비선점형 방식
2. 선점형 방식
    - 우선순위가 높은 프로세스가 계속 도착하는 상황에서 우선순위가 낮은 프로세스는 계속 기다려야 한다.
    - 이러한 문제점을 해결하기 위해 `Aging(노화)` 기법을 사용하여 대기시간이 길수록 우선순위를 높여준다. (=경로우대)

<br>

### Round Robin 스케줄링

> - 시분할 시스템의 성질을 가장 잘 활용한 스케줄링 방식
> - 각 프로세스가 CPU를 연속적으로 사용할 수 있는 시간이 특정 시간으로 제한되며, 이 시간이 경과되면 해당 프로세스로부터 CPU를 회수해 ready queue에 있는 다른 프로세스에게 할당하는 방식

- `time quantum`(할당시간)
    - 각 프로세스가 한 번에 CPU를 연속적으로 사용할 수 있는 최대시간
    - 할당시간이 너무 짧으면 프로세스간에 빈번한 context switch 으로 오버헤드 증가
    - 너무 길면 FCFS 와 같은 문제 발생

- 여러 종류의 이질적인 프로세스가 같이 실행되는 환경에서 효과적

<br>

### Multi-level Queue

> ready queue 를 여러 개로 분할해 관리하는 스케줄링 기법

- 프로세스들이 CPU를 기다리기 위해 여러 줄로 서는 것
- 성격이 다른 프로세스들을 별도로 관리하고, 성격에 맞는 스케줄링을 적용하기 위한 기법
    1. 대화형 프로세스는 Round robin
    2. 계산위주 프로세스는 FCFS

Multi-level Queue 에서는 Queue 자체에 대한 스케줄링이 필요
- 고정 우선순위 방식
- time slice 방식

<br>

### Multilevel Feedback Queue

> CPU 를 기다리는 프로세스를 여러 큐에 줄 세운다는 측면에서 Multi-level queue 와 동일하나, 프로세스가 다른 큐로 이동 가능한 방식

- 우선순위 스케줄링에서 Starvation(기아) 현상을 해결하기 위해 등장한 Aging(노화) 기법을 구현할 수 있다.
- 상위 큐일수록 우선순위가 높으며, 상위 2개 큐는 Round robin, 세번째 큐는 FCFS 기법을 사용

<br>

### Multi-processor system

> CPU 가 여러개인 시스템

- CPU 가 하나인 시스템에서보다 스케줄링이 더 복잡하다.
    - 프로세스를 준비큐에 한 줄로 세워서 각 CPU가 알아서 다음 프로세스를 꺼내가도록 할 수 있다.
    - 여러 줄로 세울 경우, 일부 CPU에 작업이 편중되는 현상이 발생
        - 부하균형 매커니즘 필요

<br>

- 스케줄링 방식
    - 대칭형 다중처리
        - 각 CPU가 각자 알아서 스케줄링
    - 비대칭형 다중처리
        - 하나의 CPU가 다른 모든 CPU의 스케줄링 및 데이터 접근 책임

<br>

### Real-time system

> 각 작업마다 주어진 deadline 이 존재할 때 사용하는 기법

1. Hard real-time system
    - 미사일 발사, 원자로 제어 등 시간을 정확히 지켜야 하는 시스템
2. Soft real-time system
    - deadline 이 존재하지만, 못지켜도 큰 위험 없음

> Real-time system 에서는 EDF(Earlist Deadline First) 스케줄링 기법을 널리 사용

<br>

## 스케줄링 알고리즘의 평가

알고리즘 성능 평가 방법
- queueing model
    - 확률 분포를 통해 프로세스들의 도착률, CPU 처리율을 입력값으로 성능계산
- simulation
    - 가상으로 CPU 스케줄링 프로그램을 통해 성능 측정
    - 실제 시스템에서 추출한 CPU 요청 입력값을 트레이스(Trace)
- implementation & measurement
    - 운영체제 커널의 소스 코드 중 CPU 스케줄링 코드를 수정하고 커널을 컴파일하여 시스템에 설치한 후 실제 성능 측정