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

## Allocation of Physical Memory

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

### Contiguous allocation

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