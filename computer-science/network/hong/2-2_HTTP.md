# Chapter 2-2. HTTP

<!-- TOC -->

- [Chapter 2-2. HTTP](#chapter-2-2-http)
    - [](#)
        - [](#)
            - [<br>](#br)
        - [HTTP message](#http-message)
    - [- host : value](#--host--value)
        - [- Accept : value](#--accept--value)
            - [- PUT](#--put)
        - [](#)
            - [](#)

<!-- /TOC -->

<br>

## HTTP connections

### Non-persistent HTTP

- HTTP 1.0
- 요청을 1개 보내고 받을때마다 TCP 가 닫힘
    - 10개의 요청을 보내야되면 TCP connection 을 10번 해야됨

#### response time

> 사용자가 browser 를 통해 요청을 보내고 응답을 받아서 Web page 가 나타날때까지 걸리는 시간

- RTT (Round Trip Time)
    - 하나의 packet 이 client 에서 server 까지 전달되고 돌아오는 시간
    - Non-persistent HTTP 는 1개의 요청마다 2RTT 사이클 필요
    > response time = 2RTT + file transmission time

<br>

### Persistent HTTP

> 요청의 수 만큼 소켓을 생성하고, request message 를 동시에 전달

- 서버는 응답을 보낸후에도 connection 을 open 상태로 유지
- 각 TCP connection 마다 buffer 와 Socket 을 할당해야한다.

<br>

## HTTP message

### HTTP request message

- request line
    - HTTP Method
    - request URL
    - HTTP version
- header lines
    - host : value
    - User-agent : value
    - Accept : value
- body
    - form input

<br>

#### Method types

- HTTP/1.0
    - GET
    - POST
    - HEAD : server 에 요청한 파일은 빼고 응답해달라
        - 테스트 용도
- HTTP/1.1
    - GET, POST, HEAD
    - PUT
    - DELETE

<br>

### HTTP response message

- status line
    - HTTP version
    - HTTP 응답코드, 메시지
- header lines
- data

<br>

#### HTTP response status codes

- 200 OK
- 301 Moved Permanently
    - 요청된 객체가 경로가 변경되었다.
- 400 Bad Request
- 404 Not Found
- 505 HTTP Version Not Supported