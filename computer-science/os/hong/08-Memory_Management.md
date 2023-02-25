# 8. Memory Management

<!-- TOC -->

- [Memory Management](#memory-management)
    - [Logical vs Physical Address](#logical-vs-physical-address)
        - [주소 바인딩](#%EC%A3%BC%EC%86%8C-%EB%B0%94%EC%9D%B8%EB%94%A9)
        - [MMU - Memory Management Unit](#mmu---memory-management-unit)
    - [메모리 관리와 관련된 용어](#%EB%A9%94%EB%AA%A8%EB%A6%AC-%EA%B4%80%EB%A6%AC%EC%99%80-%EA%B4%80%EB%A0%A8%EB%90%9C-%EC%9A%A9%EC%96%B4)
        - [Dynamic Loading](#dynamic-loading)
        - [Overlays](#overlays)
        - [Swapping](#swapping)
        - [Dynamic Linking](#dynamic-linking)
- [Allocation of Physical Memory](#allocation-of-physical-memory)
    - [Contiguous allocation](#contiguous-allocation)
    - [Noncontiguous allocation](#noncontiguous-allocation)
        - [Paging](#paging)
            - [Page table](#page-table)
            - [TLB](#tlb)
        - [Two-Level Page Table](#two-level-page-table)

<!-- /TOC -->

<br>

## Logical vs Physical Address

- Logical Address
    - 프로세스마다 독립적으로 가지는 주소 공간
    - CPU가 보는 주소는 Logical Address
- Physical Address
    - 메모리에 실제 올라가는 위치

> 주소 바인딩 : Symbolic Address -> Logical Address -> Physical Address

### 주소 바인딩

- Compile time binding
    - 물리적 메모리 주소가 컴파일 시 정해짐
    - 시작 위치 변경시 재컴파일
    - 컴파일러는 absolute code 를 생성
- Load time binding
    - Loader 의 책임하에 물리적 메모리 주소 부여
    - 컴파일러가 재배치 가능코드(relocatable code) 를 생성한 경우 가능
- Run time binding
    - 실행된 이후에도 프로세스의 메모리 상 위치를 옮길 수 있음
        - 하드웨어적인 지원이 필요 (MMU)
    - CPU 가 주소를 참조할 때마다 binding 을 점검해야함.

<br>

### MMU - Memory Management Unit

> Logical address 를 physical address 로 매핑해주는 Hardware device

- registre 2 개를 통해서 주소를 변환한다.
    - relocation register (=base register) : 시작위치
    - limit register : 시작위치부터 최대 사이즈
        - 프로그램이 limit 을 넘기는 접근을 하면 `Trap 발생`

- user program 은 logical address 만 다룸
    - 실제 physical address 를 볼 수 없다.

<br>

## 메모리 관리와 관련된 용어

### Dynamic Loading

> 프로세스 전체를 메모리에 미리 다 올리는 것이 아니라 실행되는 부분만 메모리에 load 하는 것

- memory utiilization 향상
- 운영체제의 특별한 지원없이 프로그램 자체에서 구현 가능
    - OS 는 라이브러리를 통해 지원 가능

<br>

### Overlays

> 메모리에 프로세스의 부분 중 실제 필요한 정보만을 올림

- 사용자에 의해 구현
- 프로세스의 크기가 메모리보다 클 때 유용

<br>

### Swapping

> 프로세스를 일시적으로 메모리에서 backing store 로 쫓아내는 것

- Backing store
    - 디스크 : 많은 사용자의 프로세스 이미지를 담을 만큼 충분히 빠르고 큰 저장 공간
- Swap in / Swap out
    - 일반적으로 중기 스케줄러에 의해 swap out 시킬 프로세스 선정
    - run time binding 에서는 추후 빈 메모리 영역에 자유롭게 올릴 수 있기 때문에 적합

<br>

### Dynamic Linking

> Linking 을 실행시간까지 미루는 기법

- Static linking
    - 라이브러리가 프로그램의 실행 파일 코드에 포함됨
    - 동일한 코드가 각각의 프로세스에 포함되므로 메모리 낭비
- Dynamic linking
    - 라이브러리가 실행시 연결(link)
    - 라이브러리를 메모리에 올려서 라이브러리를 사용하는 모든 코드와 연결
    - 운영체제의 도움이 필요

<br>

# Allocation of Physical Memory

> 메모리는 일반적으로 두 영역으로 나뉘어 사용

1. OS 상주 영역
2. 사용자 프로세스 영역

- 사용자 프로세스 영역의 할당 방법
    - Contiguous allocation (연속할당)
        - Fixed partition allocation (고정 분할 방식)
        - Variable partition allocation (가변 분할 방식)
    - Noncontiguous allocation (불연속할당)
        - Paging
        - Segmentation
        - Paged Segmentation

<br>

## Contiguous allocation

> 물리적 메모리에 프로세스를 연속적으로 할당하는 방법

- 고정 분할 방식
- 가변 분할 방식
    - First-fit
        - 프로세스보다 큰 최초 hole 에 할당
    - Best-fit
        - 가장 적합한 hole 에 할당
        - 모든 hole 의 리스트를 탐색해야됨.
    - Worst-fit
        - 가장 큰 hole 에 할당
- compaction
    - 사용 중인 메모리 영역을 한 군데로 몰아 모든 hole 을 한곳으로 모으는 것
    - 매우 비용이 많이 드는 방법
    - run time binding 이 가능해야 한다.

<br>

## Noncontiguous allocation

### Paging

> Program 의 논리적 메모리를 동일한 크기로 잘라서 각각의 페이지를 물리적 메모리에 비어있는 위치에 적재하는 방법

#### Page table

> 페이징 기법에서 주소를 변환할 때, 논리적 메모리의 페이지들이 물리적 메모리의 어느 부분에 올라가있는지 주소를 저장하는 테이블

- 인덱스를 이용해서 물리적 메모리에 적재된 위치를 빠르게 찾을 수 있음.
- 프로그램마다 page table 이 별도로 존재한다.
- 메모리에 저장한다.
    - 메모리 접근 연산에는 2번의 memory access 필요
        - page table 1번, data/instruction 1번

구성요소
- PTBR(Page-table base register)
- PTLR(Page-table limit register)

<br>

#### TLB

> 속도 향상을 위해 `associative register` 혹은 `translation look-aside buffer(TLB)` 라 불리는 고속의 lookup hardware cache 사용

- Main memory 와 CPU 사이에 존재
    - 주소 변환을 위한 별도의 캐시를 저장
    - page table 에서 빈번하게 참조되는 일부 entry 를 저장

- page table 을 참조하기 전에 TLB 를 먼저 조회하고 존재하면 바로 메모리에 접근
- parallel search 가 가능

<br>

### Two-Level Page Table

> 공간 확보를 위해 Page Table 이 내부, 외부로 나눈다.

- 현대 컴퓨터는 address space 가 매우 큰 프로그램 지원
    - 32 bit (=4GB), 64 bit
    - page size가 4KB 시 1M 개의 page table entry 필요
    - 각 page entry 가 4 Byte 시 프로세스 당 4M page table 필요

> 대부분 프로그램은 4GB 의 주소 공간 중 일부만 사용하므로 page table 공간이 심하게 낭비됨.

=> page table 자체를 page 로 구성, 사용되지 않는 주소 공간에 대한 outer page table 엔트리 값은 NULL

- 내부 page table 의 크기는 page size 랑 같다.
- 외부 page table 은 page 수 만큼 생성되지만, 내부 page table 은 실제 사용되는 page 만 생성된다.

<br>

### Multi-level Paging

> Address Space 가 더 커지면 다단계 페이지 테이블이 필요

- TLB 를 통해 메모리 접근 시간을 줄일 수 있어서 Multi-level Pagin 기법을 사용해도 크게 오버헤드가 발생하지 않음

- Logical memory 의 페이지 갯수만큼 page table 이 생성되는데, 물리적 메모리에 올리지 않을 페이지는 value를 Invalid 로 저장

#### Memory Protection

> Page table 의 각 entry 마다 아래의 bit 을 둔다.

- Protection bit
  - page 에 대한 접근 권한 (read/write/read-only)
  - 프로세스에 접근 권한을 둬서 프로그램 내부 코드가 변경되는 것을 방지
- Valid-invalid bit
  - invalid : 해당 주소의 frame 에 유효한 내용이 없음을 뜻함 (접근불허)
    - 프로세스가 그 주소 부분을 사용하지 않는 경우

### Inverted Page Table

> Multi-level paging 으로 인해 page table 의 사이즈가 커지면서 공간 오버헤드 증가를 방지하기 위한 방법

- 프로세스마다 page table 이 존재하는 것이 아니라 물리적 메모리의 frame 갯수만큼 page table 의 entry 가 존재
  - address 뿐만 아니라 어떤 프로세스인지도 entry에 저장해야함

- physical address 를 바탕으로 logical address 를 탐색하는 역방향 방식
  - page table 을 전부 탐색해야 주소 변환이 가능해지는 단점 (시간적 오버헤드 발생)
    - associative register 라는 하드웨어를 이용하여 병렬탐색을 가능하도록 보완

<br>

#### Shared Page

> 여러 프로세스가 같은 Page 를 사용하는 경우 별도로 나눠서 올리는 것이 아니라 하나의 frame 을 공유

- 반드시 read-only 로 설정
- 모든 프로세스의 logical address space 의 동일한 위치에 존재해야 함

<br>

### Segmentation

> 의미단위로 (code, data, stack) 메모리를 나누는 기법

- 의미단위로 나누기때문에 segment 마다 크기가 동일하지 않음
  - Sharing 하는 경우에는 용이
- Segment 의 길이를 저장
  - 물리적 주소의 base, limit 저장
