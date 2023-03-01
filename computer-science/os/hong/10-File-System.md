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