# 9. Virtual Memory
<!-- TOC -->

- [Virtual Memory](#virtual-memory)
    - [Demand Paging](#demand-paging)
        - [Page Fault](#page-fault)
        - [Page Replacement](#page-replacement)
            - [Optimal Algorithm](#optimal-algorithm)
            - [FIFO Algorithm](#fifo-algorithm)
            - [LRU - Least Recently Used](#lru---least-recently-used)
            - [LFU - Least Frequently Used](#lfu---least-frequently-used)
    - [다양한 Caching 환경](#%EB%8B%A4%EC%96%91%ED%95%9C-caching-%ED%99%98%EA%B2%BD)
        - [Clock Algorithm](#clock-algorithm)
    - [Page Frame 의 Allocation](#page-frame-%EC%9D%98-allocation)
        - [Global vs Local Replacement](#global-vs-local-replacement)
        - [Thrashing](#thrashing)
        - [Working-Set Model](#working-set-model)
            - [Working-set Algorithm](#working-set-algorithm)
            - [Page-Fault Frequency Scheme - PFF](#page-fault-frequency-scheme---pff)
        - [Page Size 의 결정](#page-size-%EC%9D%98-%EA%B2%B0%EC%A0%95)

<!-- /TOC -->

<br>

## Demand Paging

> 요청이 있으면 해당 page 를 메모리에 올리는 방법

<img src="https://user-images.githubusercontent.com/107091097/221399641-d92a25b2-f7ce-4571-8826-85be33d5b773.png" width="80%">

- 장점
    - I/O 양의 감소
    - Memory 사용량 감소
    - 빠른 응답 시간
- Valid / Invalid bit 사용
    - address translation 시에 invalid bit 인 경우 `Page Fault` 발생

<br>

### Page Fault

> invalid page 를 접근하면 MMU 가 trap 을 발생시킴

- Kernel mode 로 들어가서 page fault handler 가 invoke 됨
- Disk I/O 를 통해 read 가 끝나면 메모리에 저장하고 valid/invalid bit 를 valid 로 변경
    - disk 에 접근해서 값을 read 해오는 과정은 굉장히 오래걸림

<br>

### Page Replacement

<img src="https://user-images.githubusercontent.com/107091097/221400214-7ad17e1b-1d33-4f05-9238-a000906bce6e.png" width="80%">

- 어떤 frame 을 빼앗아올지 결정해야 함
- 곧바로 사용되지 않을 page 를 쫓아내는 것이 좋음

<br>

#### Optimal Algorithm

> Offline Optimal Algorithm. 가장 먼 미래에 참조되는 page 를 replace

- 미래를 미리 알고 replace 한다고 가정하기 때문에, 현실적으로 사용하기 어려움.
    - page fault 를 최소화하는 최적의 알고리즘
    - 다른 알고리즘과의 성능 비교용도로 사용됨

<br>

#### FIFO Algorithm

> 먼저 들어온 것을 먼저 내쫓는 방법

- 메모리 크기를 늘려주면 성능이 더 나빠지는 현상 발생
    - page fault 증가

<br>

#### LRU - Least Recently Used

> 가장 오래 전에 참조된 것을 지움

- 최근에 참조된 page 가 다시 참조될 확률이 높다고 예측
- page 들을 참조시간 기준으로 정렬해서 관리
    - 시간복잡도 : O(1)
    - LinkedList 로 저장

<br>

#### LFU - Least Frequently Used

> 참조 횟수가 가장 적은 page 를 교체

- page 들을 참조 횟수 기준으로 정렬해서 관리
    - 시간복잡도 : O(log n)
    - Heap 으로 저장

<br>

- 최저 참조 횟수인 page 가 여럿 있는 경우
    - 여러 page 중 임의로 선정
    - LRU 방식을 같이 사용할 수도 있음

<br>

## 다양한 Caching 환경

- Caching 기법
    - 한정된 캐시메모리에 요청된 데이터를 저장해 두었다가 후속 요청시 캐시로부터 직접 서비스하는 방식
    - cache memory, buffer caching, Web caching
        - buffer, Web caching 은 O(1) ~ O(log n) 정도 까지 가능

<br>

### Clock Algorithm

> LRU 의 근사 알고리즘

- reference bit 이 존재
    - 시계바늘이 한바퀴 도는 동안 한번이라도 **참조**된 적이 있는 경우 1 로 저장
    - 시계바늘이 돌면서 0인 것은 교체하고, 1인 것은 0으로 변경
- modified bit 이 존재
    - 최근에 **변경**된 적이 있는 경우 1 로 저장
        - 지울 때 backing store 에 수정된 내용을 반영 후 교체해야됨.
        - 디스크까지 접근하는 것은 오래걸리므로 modified bit 이 0인 경우만 교체하는 방법이 있음

<br>

## Page Frame 의 Allocation

> 각 process 에 얼마만큼의 page frame 을 할당할 것인가?

- Allocation Scheme
    - Equal allocation : 모든 프로세스에 똑같은 갯수 할당
    - Proportional allocation : 프로세스 크기에 비례해서 할당
    - Priority allocation : 프로세스의 우선순위에 따라 다르게 할당

> page fault 를 최소화하는 것이 중요하다.

<br>

### Global vs Local Replacement

- Global replacement
    - replace 시 다른 process에 할당된 frame 을 빼앗아 올 수 있다.
    - Process 별 할당량을 조절할 수 있는 또 다른 방법임

- Local replacement
    - 자신에게 할당된 frame 내에서만 replacement

<br>

### Thrashing

> 프로세스의 원활한 수행에 필요한 최소한의 page frame 수를 할당받지 못한 경우

- Page fault rate 가 매우 높아짐
    - CPU utilization 이 낮아짐
- OS 는 MPD (multiprogramming degree) 를 조절해서 Thrashing 을 방지해야됨

<br>

### Working-Set Model

- Locality of reference
    - 프로세스는 특정 시간 동안 일정 장소만을 집중적으로 참조
    - locality set
        - 집중적으로 참조되는 해당 page 들의 집합

- Working-set Model
    - Locality에 기반하여 프로세스가 일정 시간 동안 원할하게 수행되기 위해 한꺼번에 메모리에 올라와 있어야 하는 page 들의 집합을 Working Set 이라고 정의
    - but, MPD 가 너무 높으면 Working-Set 을 보장할 수 없음
        - 보장할 수 없는 경우 모든 frame 을 반납하여 Thrashing 방지

<br>

#### Working-set Algorithm

> MPD 를 조절하면서 Working-set 을 메모리에 보장

- Working set 은 Working set window 를 통해 알아냄
    - 참조된 page 를 특정 시간 동안 메모리에 유지한 후 버림
- process 들의 working set size 의 합이 page frame 보다 큰 경우
    - 일부 process 를 swap out 시켜 남은 process 의 working set을 우선적으로 충족 (= MPD를 줄임)

<br>

#### Page-Fault Frequency Scheme - PFF

> page fault 를 많이 일으키는 process 에 frame 을 더 할당하는 방식

- 반대로 page fault 를 너무 적은 process 는 frame 을 줄인다.

=> page fault 를 일정수준으로 유지하도록 한다.

<br>

### Page Size 의 결정

- Page size 를 감소시키면
    - page 수 증가
    - page table 크기 증가
    - internal fragmentation 감소
    - Disk transfer 효율성 감소
    - 필요한 정보만 메모리에 올라와 메모리 이용이 효율적
        - Locality 활용 측면에서는 좋지 않음