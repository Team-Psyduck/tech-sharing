# Chapter 3-1. Connectionless transport: UDP, Connection-oriented transport: TCP

<!-- TOC -->

- [Chapter 3-1. Connectionless transport: UDP, Connection-oriented transport: TCP](#chapter-3-1-connectionless-transport-udp-connection-oriented-transport-tcp)
    - [Connection-oriented demux](#connection-oriented-demux)
    - [UDP](#udp)
        - [UDP: segment header](#udp-segment-header)
            - [UDP checksum](#udp-checksum)
    - [Summary of Reliable Data Transfer Mechanism](#summary-of-reliable-data-transfer-mechanism)
    - [connection-oriented: TCP](#connection-oriented-tcp)
        - [TCP segment structure](#tcp-segment-structure)

<!-- /TOC -->

<br>

## Connection-oriented demux

- TCP socket identified by 4-tuple
    - source IP address
    - source port number
    - destination IP address
    - destination port number

- connectionless transport 에서는 server process 가 하나의 socket만 열어두기때문에 destination 정보만 있으면 되지만, connection-oriented transport 에서는 여러개의 socket 을 열어두기 때문에 source 정보도 필요하다.

<br>

## UDP

- no frills
- best effort
    - packet loss
    - out-of-order
- connectionless
    - no handshaking between UDP sender, receiver
    - each UDP segment handled independently of others
- use
    - streaming multimedia
        - loss tolerant
        - rate sensitive
    - DNS
    - SNMP
    - reliable transfer 가 굉장히 중요한 경우 UDP 를 사용하고 에러를 처리하는 시스템을 같이 사용

<br>

### UDP: segment header

> header 가 작으므로 오버헤드가 작다.

- header
    - source port
    - destination port
    - length
    - checksum
- payload
    - application data

<br>

#### UDP checksum

> UDP 에서 전송 중에 발생할 수 있는 오류를 감지하기 위한 방법

- sender가 전송되는 데이터의 (header 와 payload 를 포함) 합을 16-bit 에 담아서 전송
- receiver 는 sender 가 보낸 checksum 가 receiver 가 직접 계산한 checksum 이 일치하는지 확인하여 오류 확인

<br>

## Summary of Reliable Data Transfer Mechanism

- Checksum
- Acknowledgement
- Negative acknowledgement
- Timer
- Window, pipelining
- Sequence number

<br>

## connection-oriented: TCP

- reliable, in-order byte stream
- flow controlled
- connection-oriented
    - handshaking
- point-to-point
    - one sender, one receiver
- congestion controlled
- full duplex data

<br>

### TCP segment structure

> header 가 가변적

- header (20 byte 는 고정)
    - source port (16 bits)
    - destination port (16 bits)
    - sequence number (32 bits)
    - ACK number
    - header length, URG, ACK, PSH, RST, SYN, FIN
    - receiver window
    - checksum
    - Urg data pointer