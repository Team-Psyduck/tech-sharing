# 11. Disk Management & Scheduling

<!-- TOC -->

- [Disk Management & Scheduling](#disk-management--scheduling)
    - [Disk Structure](#disk-structure)
    - [Disk Management](#disk-management)
    - [Disk Scheduling](#disk-scheduling)
        - [Disk Scheduling Algorithm](#disk-scheduling-algorithm)
            - [SCAN](#scan)

<!-- /TOC -->

<br>

## Disk Structure

- logical block
    - 디스크의 외부에서 보는 디스크의 단위 정보 저장 공간들
- Sector
    - Logical block이 물리적인 디스크에 매핑된 위치
    - Sector 0 은 최외곽 실린더의 첫 트랙에 첫 번째 섹터

## Disk Management

- physical formatting
    - 디스크를 컨르롤러가 읽고 쓸 수 있도록 섹터들로 나누는 과정
    - 각 섹터는 `header + 실제 data + trailer` 로 구성
- Partitioning
    - 디스크를 하나 이상의 실린더 그룹으로 나누는 과정
    - OS 는 독립적 disk로 취급
- Logical formatting
    - 파일시스템을 만드는 것
    - FAT, inode, free space 등의 구조 포함
- Booting
    - ROM 에 있는 `small bootstrap loader`의 실행
    - sector 0 (boot block)을 load하여 실행

<br>

## Disk Scheduling

- Access time 의 구성
    - Seek time
        - 헤드를 해당 트랙(=실린더)으로 움직이는데 걸리는 시간
    - Rotational latency
        - 헤드가 원하는 섹터에 도달하기까지 걸리는 회전지연시간
    - Transfer time
        - 실제 데이터 전송 시간
- Disk bandwidth
    - 단위 시간당 전송된 바이트 수

<br>

- Disk Scheduling
    - seek time을 최소화하는 것이 목표

<br>

### Disk Scheduling Algorithm

> OS 에서는 logical block 을 통해 스케줄링을 한다.

- FCFS (First Come Fist Service)
- SSTF (Shortest Seek Time First)
    - Starvation 문제

#### SCAN

- SCAN
    - disk arm 이 디스크의 한쪽 끝에서 다른쪽 끝으로 이동하며 가는 길목에 있는 모든 요청을 처리
    - ex) 엘리베이터
    - 문제점 : 실린더 위치에 따라 대기 시간이 다름
- C-SCAN
    - 헤드가 한쪽으로 이동할 때만 모든 요청을 처리하고 다시 돌아감
    - SCAN 보다 균일한 시간을 제공
- N-SCAN
    - SCAN 의 변경 알고리즘
    - 지나가는 도중에 들어오는 요청은 처리하지 않는다.
- LOOK
    - SCAN 처럼 헤드가 이동하다가 처리할 요청이 없으면 끝까지 이동하지 않고 그냥 돌아감
- C-LOOK
    - C-SCAN 의 응용버전

<br>
