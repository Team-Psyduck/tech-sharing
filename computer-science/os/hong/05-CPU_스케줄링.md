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

### 우선순위 스케줄링

### Round Robin 스케줄링

### Multi-level Queue

### Multilevel Feedback Queue

### Multi-processor system

### Real-time system

1. Hard real-time system
2. Soft real-time system

## 스케줄링 알고리즘의 평가