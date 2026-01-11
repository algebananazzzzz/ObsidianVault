# TCP vs UDP
Network layer provides **best effort delivery** i.e. packets may be dropped, reordered, duplicated. Some modern day applications require ordered, guaranteed delivery.

**TCP prioritizes reliability and ordering.**

| Guarantee                        | Mechanism                |
| -------------------------------- | ------------------------ |
| Guaranteed delivery and ordering | Sequence number and ACKs |
| Loss recovery                    | Retransmission           |
| Flow and congestion control      | Sliding window           |
**Head-of-Line Blocking:** TCP intentionally waits for current in-order segment before delivering it to the application, even when future segments arrived.


**UDP prioritizes performance. It only does a basic 1s-complement checksum.**

However, we do not need TCP to guarantee delivery, order etc.
- My personal belief is that managing these concerns in the application layer provides more control to the application. 
- QUIC is a good example of this. It runs using UDP, but reintroduces the TCP guarantees in user space. A benefit of QUIC over TCP is the elimination of HOL blocking.
- What TCP provides is a standardization with the kernel stack, easier to troubleshoot with `ss -ti`, `tcpdump` and other linux commands.


TCP Three-Way Handshake

- Client sends SYN packet and opens TCB locally

- Server kernel receives SYN, creates entry in **SYN queue**, responds with SYN-ACK

- Client sends ACK, server kernel moves connection from SYN queue to accept queue

- Application process calls `accept()` syscall to retrieve socket file descriptor from accept queue

- apache prefork (1 process per Conn), thread-per-request, nginx worker process