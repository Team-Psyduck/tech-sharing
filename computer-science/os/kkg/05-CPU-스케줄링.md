# 05. CPU 스케줄링

## CPU Scheduler

> Ready 상태에 있는 프로세스 중 이번에 CPU를 줄 프로세스를 고른다.

## Dispatcher

> CPU의 제어권을 Scheduler가 선택한 프로세스에 넘긴다.

## CPU Scheduling이 필요한 경우

- Running → Blocked (I/O 요청 인터럽트)
- Running → Ready (Timer Interrupt)
- Running → Ready (I/O 완료 후 우선순위가 높을 때)
- Terminate

비선점형 - Nonpreemptive (강제로 빼앗지 않음)

선점형 - Preemptive (강제로 빼앗음)

---

CPU Burst

I/O Burst

CPU Bound Job

I/O Bound Job

## Scheduling Criteria

> = Performance Index, Performance Measure, Performance Criteria, 성능 척도

### CPU Utilization (이용률)

> Keep the CPU as Busy as Possible

### Throughput (처리량)

> \# of Processes that Complete their Execution per time unit

### Turnaround Time (소요 시간, 반환 시간)

> Amount of Time to Execute a Particular Process

### Wating Time (대기 시간)

> Amount of Time a Process has been wating in the ready queue

### Response Time (응답 시간)

> Amount of Time it takes from when a Request was Submitted until the first Response is produced, not output (for time-sharing environment)

## Scheduling Algorithm

### Nonpreemptive

- FCFS (First-Come First-Served)
  - Convoy Effect: Short Process behind Long Process

### Preemptive

- RR (Round Robin)
  - 각 프로세스는 동일한 크기의 할당 시간(Time Quantum)을 가진다.
  - 할당시간이 너무 크면 FCFS, 너무 작으면 Context Switch 오버헤드가 커진다.
  - 일반적으로 SJF보다 Average Turnaround Time이 길지만 Response Time은 더 짧다.

### Nonpreemptive And Preemptive

- SJF (Shortest-Job-First)

  - CPU Burst Time을 가지고 스케줄링에 활용
  - 주어진 프로세스들에 대해 Minimum Average Wating Time을 보장 (Preemptive, SRTF)
  - Starvation 문제, CPU Burst Time을 정확히 알 수 없다.

- Priority Scheduling

  - Highest Priority를 가진 프로세스에게 CPU 할당
  - SJF는 Priority Scheduling의 일종
  - Starvation 해결 → Aging

- Multilevel Queue

  - Ready Queue를 여러 개로 분할

    - Foreground (Interactive)
    - Background (Batch - No Human Interaction)

  - 독립적인 스케줄링 알고리즘

    - Foreground - RR
    - Background - FCFS

  - 큐에 대한 스케줄링 필요

    - Fixed Priority Scheduling
    - Time Slice

- Multilevel Feedback Queue

  - 프로세스가 다른 큐로 이동 가능

### CPU가 여러 개인 경우

- Homogeneous Processor인 경우

  - Queue에 한줄로 세워서 각 프로세서가 알아서 꺼내가게 할 수 있다.
  - 반드시 특정 프로세서에서 수행되어야 하는 프로세스가 있는 경우에는 문제가 더 복잡해짐

- Load Sharing

  - 일부 프로세서에 job이 몰리지 않도록 부하를 적절히 공유하는 메커니즘 필요
  - 별개의 큐를 두는 방법 vs 공동 큐를 사용하는 방법

- Symmetric Multiprocessing (SMP)

  - 각 프로세서가 각자 알아서 스케줄링 결정

- Asymmetric Multiprocessing

  - 하나의 프로세서가 시스템 데이터의 접근과 공유를 책임지고 나머지 프로세서는 거기에 따름

### Hard Real-Time Systems

> 정해진 시간 안에 반드시 끝내도록 스케줄링해야 함

### Soft Real-Time Computing

> 일반 프로세스에 비해 높은 Priority를 갖도록 해야 함

### Local Scheduling

> User Level Thread의 경우 사용자 수준의 Thread Library에 의해 어떤 Thread를 스케줄할지 결정

### Global Scheduling

> Kernel Level Thread의 경우 일반 프로세스와 마찬가지로 커널의 단기 스케줄러가 어떤 Thread를 스케줄할지 결정

## Algorithm Evaluation

- Queueing Models

  - 확률 분포로 주어지는 arrival rate와 service rete등을 통해 각종 performance index 값을 계산

- Implementation(구현) & Measurement(성능 측정)

  - 실제 시스템에 알고리즘을 구현하여 실제 작업(workload)에 대해서 성능을 측정 비교

- Simulation(모의 실험)

  - 알고리즘을 모의 프로그램으로 작성 후 trace를 입력으로 하여 결과 비교
