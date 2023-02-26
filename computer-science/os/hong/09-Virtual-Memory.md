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
