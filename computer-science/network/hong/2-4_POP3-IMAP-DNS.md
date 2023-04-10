# Chapter 2-4. Electronic Mail

<!-- TOC -->

- [Chapter 2-4. Electronic Mail](#chapter-2-4-electronic-mail)
    - [Mail access protocols](#mail-access-protocols)
        - [POP3 protocol](#pop3-protocol)
        - [POP3 and IMAP](#pop3-and-imap)
    - [DNS : domain name system](#dns--domain-name-system)
        - [DNS : services, structure](#dns--services-structure)
        - [DNS : a distributed, hierarchical database](#dns--a-distributed-hierarchical-database)
        - [TLD, authoritative servers](#tld-authoritative-servers)
        - [DNS name resolution example](#dns-name-resolution-example)
        - [DNS : caching, updating records](#dns--caching-updating-records)
        - [DNS protocol, messages](#dns-protocol-messages)
        - [DNS records](#dns-records)
        - [DNS server 는 각각 무엇을 저장하는가?](#dns-server-%EB%8A%94-%EA%B0%81%EA%B0%81-%EB%AC%B4%EC%97%87%EC%9D%84-%EC%A0%80%EC%9E%A5%ED%95%98%EB%8A%94%EA%B0%80)

<!-- /TOC -->

<br>

## Mail access protocols

- SMTP : delivery/storage to receiver's server
- mail access protocol
    - POP : Post Office Protocol
    - IMAP
    - HTTP

<br>

### POP3 protocol

1. authorization phase
    - client commands
        - username
        - password
    - server responses
        - +OK
        - -ERR
2. transaction phase
    > 메시지 리스트를 받아서 검색하고, 삭제하는 과정
    - list : list message numbers
    - retr (num) : retrieve message by number
    - dele (num) : delete
    - quit

<br>

### POP3 and IMAP

- more about POP3
    - 과거에 POP3 는 "download and delete" mode 로 동작했다.
    - POP3 는 sessions 간에 stateless

- IMAP
    - 모든 메시지는 local message 로 가져오는게 아니라 server 의 mail box 에서 메시지를 관리
    - sessions 간에 유저 정보가 유지

<br>

## DNS : domain name system

> 호스트 네임을 IP 주소로 매핑해주는 시스템

- distributed database 를 이용해서 매핑정보를 저장

<br>

### DNS : services, structure

- DNS services
    - host aliasing
    - mail server
    - load distribution
        - replicated Web servers

- why not centralize DNS?
    - single point of failure
    - traffic volume
    - distant centralized database
        - Network resource 낭비
    - maintenance

<br>

### DNS : a distributed, hierarchical database

1. Root DNS Servers
    > root 하위에는 TLD 서버들이 존재
    - TLD 에 대한 정보를 알고있음
2. TLD
3. authoritative DNS server

- Local DNS name server
    > = default name server, proxy server
    - Web cache 처럼 모든 host DNS query 가 Local DNS server 로 요청이 전달된다.
    - local site 내에서 proxy 역할
    - 도메인 정보가 존재하지 않으면 DNS hierakey system 로 전달해주는데 이 hierakey system 의 제일 최상단에 존재하는 root name server 로 전달된다.

<br>

### TLD, authoritative servers

- top-level domain server
    - com, org, net, edu, kr ...
    - 각 도메인마다 관리하는 기관들이 나눠져 있음
    - 도메인네임을 담당하고 있는 authoritative DNS server 정보를 알고있음
- authoritative DNS server
    - 각 기관마다 기관내에서 관리하는 host 에 대한 hostname to IP address 매핑 정보를 갖고있음

<br>

### DNS name resolution example

- iterated query
    1. request host
    2. local DNS server
        - 도메인 정보를 모르면 root DNS 에 요청
    3. root DNS server
        - TLD server 정보를 local DNS server 에 전달
    4. TLD DNS server
        - authoritative DNS 정보를 local DNS server 에 전달
    5. local DNS server 가 최종적으로 요청 DNS query 에 해당하는 서버를 requesting host 에 연결
- recursive query
    - iterated query 와 달리 root DNS server 가 직접 TLD 에게, TLD 가 authoritative DNS server 에 요청하는 방식
    - DNS system 의 상위일수록 부하가 증가함

<br>

### DNS : caching, updating records

- DNS mapping 정보를 캐싱하면 응답시간 및 부하 감소
    - local DNS server 는 TLD server 정보를 캐싱하여 root 를 거치지 않아도 된다.
- out-of-date 의 위험이 있음

<br>

### DNS protocol, messages

- query 와 reply 는 동일한 message format 을 사용
    - query 와 reply 를 하나로 묶기위해
- msg header
    - identification
        - 16 bit for query, reply to query uses same
    - flags
        - query or reply
        - 여러가지 정보 제공
- body
    - questions : name, type query 정보
    - answers : RRs in response
    - authority : records of authoritative servers
    - additional info : 추가정보
<br>

### DNS records

- name
- value
- type
    - type=A
        - name = hostname
        - value = IP address
    - type=CNAME
        - name = alias name (=www.ibm.com)
        - value = canonical name
    - type=MX
        - name = domain
        - value = name of mail server
    - type=NS
        - name = domain
        - value = hostname of authoritative name

<br>

### DNS server 는 각각 무엇을 저장하는가?

- Local name server
    - caching DNS query resource
- Root
    - TLD server for each domain
- TLD
    - authoritative server for each organization in the domain
- Authoritative
    - hostname to IP address mapping
