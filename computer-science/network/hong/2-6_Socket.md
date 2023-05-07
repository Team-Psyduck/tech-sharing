# Chapter 2-6. Socket programming with UDP and TCP
<!-- TOC -->

- [Chapter 2-6. Socket programming with UDP and TCP](#chapter-2-6-socket-programming-with-udp-and-tcp)
    - [Socket programming](#socket-programming)
        - [Socket programming with UDP](#socket-programming-with-udp)
            - [Client/server socket interaction: UDP](#clientserver-socket-interaction-udp)
        - [Socket programming with TCP](#socket-programming-with-tcp)
            - [Client/server socket interaction: TCP](#clientserver-socket-interaction-tcp)

<!-- /TOC -->

<br>

## Socket programming

- UDP
- TCP

<br>

### Socket programming with UDP

> no connection between client & server

- no handshaking before sending data
- client explicitly attaches IP destination address and port

- UDP : transmitted data may be lost or received out-of-order

<br>

#### Client/server socket interaction: UDP

- server
    1. create serverSocket + port binding
    2. read datagram from serverSocket
    3. read datagram from clientSocket

- client
    1. create clientSocket (server IP and port)
    2. send datagram via clientSocket
    3. write reply to serverSocket specifying client address, port number
        => 데이터를 전달할 때 server 주소를 명시

<br>

### Socket programming with TCP

- client must contact server
    - server process must first be running
    - server must have created socket that

- client contacts server by
    - Creating TCP socket, specifying IP address, port number of server process
    - client TCP establishes connection to server TCP
    - client 와 접촉했을 때, server TCP 는 새로운 소켓을 생성
        - TCP는 각 connection 마다 new socket 생성
        > TCP 는 byte-stream 을 전송하기 때문

<br>

#### Client/server socket interaction: TCP

- server
    1. create socket
    2. wait for incoming connection request
    3. read request from connectionSocket
    4. write reply to connectionSocket
    5. close connectionSocket

- client
    1. create socket
    2. TCP connect to hostid, port
    3. send request using clientSocket
    4. read reply from clientSocket
    5. close clientSocket