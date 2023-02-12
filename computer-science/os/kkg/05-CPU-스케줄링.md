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
