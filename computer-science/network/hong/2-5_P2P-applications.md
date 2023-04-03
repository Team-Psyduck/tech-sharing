# Chapter 2-5. P2P applications
<!-- TOC -->

- [Chapter 2-5. P2P applications](#chapter-2-5-p2p-applications)
    - [Pure P2P architecture](#pure-p2p-architecture)
        - [client-server vs P2P](#client-server-vs-p2p)
        - [P2P file distribution : BitTorrent](#p2p-file-distribution--bittorrent)
            - [BitTorrent: requesting, sending file chunks](#bittorrent-requesting-sending-file-chunks)

<!-- /TOC -->

<br>

## Pure P2P architecture

- no always-on server
- end systems directly communicate
- example
    - file distribution
    - skype

<br>

### client-server vs P2P

- file distribution 서비스에서 upload rate
    - P2P : time to distribute F to N clients using P2P approach
        > D<sub>P2P</sub> >= max(F/u<sub>s</sub>, F/d<sub>min</sub>, N*F/(u<sub>s</sub>+sum(u<sub>i</sub>)))
        - Client 의 수 N 이 늘어날수록 시간 증가량이 작다.
    - Client-Server
        > D<sub>C-S</sub> >= max(N*F/u<sub>s</sub>, F/d<sub>min</sub>)
        - Client 의 수 N 이 늘어나는만큼 시간이 증가한다.

<br>

### P2P file distribution : BitTorrent

- tracker
    - tracks peers participating in torrent
- torrent
    - requesting chunks
    - sending chunks: tit-for-tat

<br>

#### BitTorrent: requesting, sending file chunks

> 개인적인 파일에 대해서도 공유 될 위험성이 있음.

- requesting chunks
    - rarest first 원칙으로 peers 에게 missing chunks 를 우선적으로 요청한다.
- sending chunks: tit-for-tat
    - peers 중 Top 4 제공자에게 chunks 를 전송
        - 10초마다 top 4 를 갱신
    - 30초마다 랜덤하게 peers 에게 sending
        - 랜덤하게 보낸 peers 가 새롭게 Top 4 에 합류할 수 있음

<br>
