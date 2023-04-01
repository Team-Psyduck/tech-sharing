# Ch 2-1. Application Layer
<!-- TOC -->

- [Ch 2-1. Application Layer](#ch-2-1-application-layer)
    - [Application architectures](#application-architectures)
        - [Client-server architecture](#client-server-architecture)
        - [P2P architecture](#p2p-architecture)
        - [Processes communicating](#processes-communicating)
    - [Sockets](#sockets)
    - [Addressing processes](#addressing-processes)
        - [What transport service does an app need?](#what-transport-service-does-an-app-need)
        - [Internet transport protocols services](#internet-transport-protocols-services)
        - [Application Layer protocol defines](#application-layer-protocol-defines)
    - [Web and HTTP](#web-and-http)
        - [HTTP](#http)
            - [uses TCP](#uses-tcp)
            - [HTTP is "stateless"](#http-is-stateless)
        - [HTTP connections](#http-connections)

<!-- /TOC -->

<br>

## Application architectures

- client-server
- peer-to-peer

<br>

### Client-server architecture

- server
    - 고정된 IP 주소
    - always-on
    - data centers for scaling
- clients
    - communicate with server
    - dynamic IP addresses
    - may be intermittently connected

<br>

### P2P architecture

- no always-on server
- peers request service from other peers
- self scalability
    - 별도의 서버가 없고, peer 들끼리 통신
- peers are intermittently connected and change IP address
    - complex management

<br>

### Processes communicating

- 커뮤니케이션의 주체는 host 내부의 process
    - 같은 host 의 inter-process communication 은 OS 에 의해 관리
    - 다른 host 의 process 들간의 통신을 의미

- but, peer process 는 같은 host 내부에서 통신

## Sockets

> process 는 socket 과 메시지를 주고받는다.

- 소켓은 서로 다른 프로세스 간의 통신을 가능하게 하는 소프트웨어 endpoint
- socket 은 door 역할을 한다.

<br>

## Addressing processes

- host device 는 32-bit `IP address` 로 식별
- process 는 `port number` 로 식별

<br>

### What transport service does an app need?

- data integrity
    - file transfer
    - email, text messaging
- timing
    - low delay 를 보장
- throughput
    - require minimum amount of throughput
    - elastic apps
- security

<br>

### Internet transport protocols services

> TCP 가 다양한 기능을 제공하지만, connection 을 setup 하기때문에 오버헤드가 크다.

- `TCP service`
    - reliable transport
        - 3way handshake
    - flow control
        - receiver's buffer 를 overwhelm 방지
    - connection-oriented
    - congestion control
        - Network's congestion 방지
    - does not provide
        - timing
        - minimun throughput guarantee
        - security
- `UDP service`
    - unreliable data trnasfer
    - does not provide
        - reliability
        - flow control
        - congestion control
        - timing
        - throughput guarantee
        - security
        - connection setup

<br>

### Application Layer protocol defines

- types of messages exchanged
- message syntax
- message semantics
- rules

<br>

- open protocols
    - defined in RFC
    - allows for interoperability
- proprietary protocols
    - Skype

<br>

## Web and HTTP

> web page consists of objects

- Web page 는 base HTML-file 로 구성
    - several referenced objects 포함
- 각 object 는 URL 로 주소화됨

<br>

### HTTP

> Hypertext transfer protocol

- Web's application Layer protocol
- client/server model
    - client : browser
    - server : objects in response to requests

<br>

#### uses TCP

- client initiates TCP connection to server, port 80
    - creates socket
- server accepts TCP connection from client
- HTTP messages exchanged between brower and Web server
- TCP connection closed

<br>

#### HTTP is "stateless"

> server 는 과거 클라이언트 요청을 저장하지 않는다.

<br>

### HTTP connections

- non-persistent HTTP
    - TCP connection 을 요청 1개 보낼때마다 연결하고 닫음
    - 굉장히 비효율적

- persistent HTTP