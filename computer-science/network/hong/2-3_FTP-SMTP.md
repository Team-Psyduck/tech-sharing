# Chapter 2-3. FTP, Electronic mail:SMTP
<!-- TOC -->

- [Chapter 2-3. FTP, Electronic mail:SMTP](#chapter-2-3-ftp-electronic-mailsmtp)
    - [User-server state: cookies](#user-server-state-cookies)
        - [Cookies](#cookies)
    - [Web caches proxy server](#web-caches-proxy-server)
        - [More about Web caching](#more-about-web-caching)
        - [Conditional Get](#conditional-get)

<!-- /TOC -->

<br>

## User-server state: cookies

> HTTP 가 stateless 이므로 과거 요청정보를 유지하기 위해 사용

- 서버측에서 response message 에 cookie header 를 전달
    - 이후부터 client 에서 request message 에 cookie 를 담아서 요청
- Cookie file 은 user's browser 에 유지

<br>

### Cookies

- 쿠키 역할
    - authorization
    - shopping carts
    - recommendations
    - user session state

<br>

## Web caches (proxy server)

> origin server 의 개입 없이 클라이언트 요청을 충족하기 위한 목적

- browser 가 모든 HTTP request 를 cache (proxy server)로 전달
    - cache 서버에서 request objects 를 갖고있으면 origin server 까지 거치지 않고 HTTP response 전달

<br>

### More about Web caching

- cache 는 client 와 server 역할을 둘다 수행
    - origin server 로 요청
    - client 로 응답
- client request 에 대한 응답 시간 단축
- server 에 대한 access link utilization 감소
    - local web cache 를 사용함으로써 적은비용으로 origin server 에 대한 네트워크 사용률 감소

<br>

### Conditional Get

> 캐시된 정보가 최신 정보인지 아닌지 server 에 보내는 요청

- web cache 를 사용함으로써 응답 지연 감소 및 링크 사용률 감소
    - but, 캐시된 정보가 최신 상태가 아닐경우 문제가 발생

