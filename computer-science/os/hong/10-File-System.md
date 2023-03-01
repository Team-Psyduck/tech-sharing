# 10. File System
<!-- TOC -->

- [File System](#file-system)
    - [File](#file)
        - [Directory and Logical Disk](#directory-and-logical-disk)
            - [Directory](#directory)
            - [Partition](#partition)
        - [파일 시스템콜](#%ED%8C%8C%EC%9D%BC-%EC%8B%9C%EC%8A%A4%ED%85%9C%EC%BD%9C)
            - [open](#open)
            - [read](#read)
        - [File Protection](#file-protection)
        - [Access Methods](#access-methods)
    - [Allocation of File Data in Disk](#allocation-of-file-data-in-disk)
        - [Contiguous Allocation](#contiguous-allocation)
        - [Linked Allocation](#linked-allocation)
        - [Indexed Allocation](#indexed-allocation)
    - [파일 시스템의 구조](#%ED%8C%8C%EC%9D%BC-%EC%8B%9C%EC%8A%A4%ED%85%9C%EC%9D%98-%EA%B5%AC%EC%A1%B0)
        - [UNIX 파일 시스템 구조](#unix-%ED%8C%8C%EC%9D%BC-%EC%8B%9C%EC%8A%A4%ED%85%9C-%EA%B5%AC%EC%A1%B0)
        - [FAT File System](#fat-file-system)
    - [Free Space Management](#free-space-management)
    - [Directory Implementation](#directory-implementation)
    - [VFS and NFS](#vfs-and-nfs)
        - [VFS - Virtual File System](#vfs---virtual-file-system)
        - [NFS - Network File System](#nfs---network-file-system)
    - [Page Cache and Buffer Cache](#page-cache-and-buffer-cache)

<!-- /TOC -->

<br>

## File

> A named collection of related information

- 일반적으로 비휘발성의 보조기억장치에 저장
- OS 는 다양한 저장 장치를 file 이라는 동일한 논리적 단위로 볼 수 있게 해줌

File
- File attribute (=metadata)
    - 파일 자체의 내용이 아니라 파일을 관리하기 위한 각종 정보들
    - 접근 권한, 시간, 소유자 등
- File system
    - 운영체제에서 파일을 관리하는 부분

<br>

### Directory and Logical Disk

#### Directory

> 파일의 메타데이터 중 일부를 보관하고 있는 일종의 특별한 파일

- 파일을 찾거나, 생성, 삭제

<br>

#### Partition

> (= Logical Disk) 하나의 물리적 디스크 안에 여러 파티션을 두는게 일반적

- 여러 개의 물리적인 디스크를 하나의 파티션으로 구성하기도 함

<br>

### 파일 시스템콜

#### open()

> 디스크로부터 root 부터 해당 디렉토리까지의 메타데이터를 커널 메모리 영역으로 로드하는 시스템콜

- OS 가 커널메모리 영역에 copy 하고 사용자 메모리 영역에 전달해줌
    - 커널메모리 영역에 요청한 정보를 copy 해서 저장해두는 `buffer cache` 가 존재
    - 이미 메모리에 존재하는 데이터는 운영체제가 디스크를 거치지 않고 바로 전달

<br>

#### read()

> 커널메모리 영역에 존재하는 buffer cache 에 존재하는 데이터를 사용자 메모리 영역으로 가져오는 시스템콜

<br>

### File Protection

> 각 파일에 대한 접근 권한 (read / write/ execution)

- Access Control 방법
    - Access Control Matrix
        - Access control list : 파일별로 접근권한 표시
        - Capability : 사용자별로 접근권한 표시
    - **Grouping ⁂**
        - 전체 user 를 owner, group, public 의 세 그룹으로 구분
        - 각 파일에 대해 세 그룹의 접근 권한(rwx)을 3비트씩으로 표시
    - Password
        - 파일마다 password 를 두는 방법

<br>

### Access Methods

> 시스템이 제공하는 파일 정보의 접근 방식

- 순차 접근
    - 카세트 테이프를 사용하는 방식처럼 접근
    - 순서대로 접근 가능
- 직접 접근
    - LP 레코드 판과 같이 접근하도록 함
    - 특정 위치를 바로 접근 가능

<br>

## Allocation of File Data in Disk

### Contiguous Allocation

> 하나의 파일이 디스크에 연속해서 저장되도록 할당하는 방법

- 장점
    - Fast I/O
    - Direct Access 가능
- 단점
    - 외부 조각이 발생할 수 있음
    - File grow 가 어려움
        - File 생성 시 hole 을 미리 할당 (내부 조각 발생)

<br>

### Linked Allocation

> 디스크에 연속적으로 할당하지 않고, 빈 공간이 존재하면 어느 곳이든 저장하는 방법

- Linked List 의 형태로 저장
    - start 주소만 기록

- 장점
    - 외부 조각이 발생하지 않음
- 단점
    - 직접 접근이 불가능
        - 앞부분부터 순서대로 접근해야됨
    - Reliability 문제
        - 한 sector 가 고장나 pointer가 유실되면 많은 부분을 잃음
    - Pointer 를 위한 공간이 block 의 일부가 되어 공간 효율성 감소
- 변형
    - File-allocation table(FAT) 파일 시스템
        - 포인터를 별도의 위치에 보관하여 reliability 와 공간효율성 해결

<br>

### Indexed Allocation

> index block 하나에 각 index 의 주소를 한번에 저장하는 방법

- 장점
    - 직접 접근 가능
    - 외부 조각이 발생하지 않음
- 단점
    - 아무리 작은 파일이라도 index block 이 필요해서 공간낭비
    - Large file 의 경우 하나의 index block 에 저장하기 어려움
        - linked scheme : 마지막에 또다른 index block 의 주소 저장
        - multi-level index

<br>

## 파일 시스템의 구조

### UNIX 파일 시스템 구조

<img src="https://user-images.githubusercontent.com/107091097/222149575-22e6d795-e8df-4365-ab8d-805ea11eeb67.png" width="80%">

- Boot block
    - 부팅에 필요한 정보 (bootstrap loader)
- Super block
    - 파일 시스템에 관한 총체적인 정보 저장
- Inode
    - 파일 이름을 제외한 파일의 모든 메타데이터를 저장 (위치정보 등)
        - 파일은 indirect 를 통해 위치를 모두 저장
    - directory 에는 파일의 일부 메타데이터(file 이름)만 저장하고 나머지는 Inode list 에 저장
        - file 1개 당 Inode 1개 존재
- Data block
    - 파일의 실제 내용을 보관

<br>

### FAT File System

<img src="https://user-images.githubusercontent.com/107091097/222153534-af6480b5-dea2-41cd-9896-dadac3d96978.png" width="80%">

- Boot block
- **FAT**
    - Data block 의 크기만큼 배열을 가짐.
    - file의 다음 블럭 주소를 저장 (linked 방식)
    - 메모리에 저장해서, 접근하고 싶은 파일의 주소를 확인 후 디스크에서 해당 파일을 로드하여 직접 접근이 가능
    - 중요하기 때문에 여러개 copy
- Root directory
- Data block
    - directory file 의 이름과 주소 위치를 저장

<br>

## Free Space Management

> 비어있는 block 을 관리하는 방법

- Bit map or bit vector
    - 각각의 block 별로 사용 여부를 0과 1로 표시
    - bit map 은 부가적인 공간을 필요로 함
    - 연속적인 n 개의 free block을 찾는데 효과적

- Linked list
    - 모든 free block 들을 링크로 연결
    - 연속적인 가용공간을 찾는 것이 어려움

- Grouping
    - linked list 방법의 변형 (indexed allocation 방식과 유사)
    - 첫번째 free block이 n 개의 pointer 를 가짐

- Counting
    - 프로그램들이 종종 여러개의 연속적인 block 을 할당하고 반납한다는 성질에 착안
    - first free block, # of contiguous free blocks 을 유지

<br>

## Directory Implementation

> directory file 의 내용을 저장하는 방법

<img src="https://user-images.githubusercontent.com/107091097/222162872-8a83cb61-ad6d-4fcb-a293-051d9103d182.png" width="80%">

- Linear list
    - file name, file의 metadata 의 list
    - 디렉토리 내에 파일이 있는지 찾기 위해서는 linear search 필요
- Hash Table
    - linear list + hashing
    - file name 을 파일의 linear list 의 위치로 바꿔줌
    - search time 을 없앰
    - Collision 발생 가능

<br>

- File 의 metadata의 보관 위치
    - 디렉토리 내에 직접 보관
    - 디렉토리에는 포인터를 두고 다른 곳에 보관
        - inode, FAT 등

- Long file name 의 지원
    - \<file name, file의 metadata\> 의 list 에서 각 entry 는 일반적으로 고정 크기
    - file name 이 길어지는 경우 entry 마지막 부분에 포인터를 두는 방법

<br>

## VFS and NFS

### VFS - Virtual File System

> 사용자가 file system 을 접근할 때 동일한 시스템 콜 인터페이스를 통해 접근할 수 있게 해주는 System

<br>

### NFS - Network File System

> 원격의 다른 컴퓨터의 file system 에 접근할 때 Network 를 통해 접근할 수 있게 해주는 System

- RPC 통신

<br>

## Page Cache and Buffer Cache

- Page Cache
    - paging system 에서 사용하는 page frame 을 caching 의 관점에서 설명하는 용어
    - Memory-Mapped I/O 를 쓰는 경우 file의 I/O 에서도 page cache 사용
- Memory-Mapped I/O
    - File 을 접근할 때, File 의 일정부분을 virtual memory 에 mapping 시키는 방식
- Buffer Cache
    - 파일시스템을 통한 I/O 연산은 메모리의 특정 영역인 buffer cache 사용
- Unified Buffer Cache
    - 최근 OS에서는 buffer cache 와 page cache 를 같이 관리