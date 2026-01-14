### 1. DNS Resolution
- Browser checks local DNS cache
- Forward query to OS stub resolver
- If not cached, queries passed to ISP recursive resolver, which performs hierarchical lookup
- root nameserver (regional) → TLD nameserver (.com) → authoritative nameserver

### 2. TCP Three-Way Handshake
Network layer provides **best effort delivery** i.e. packets may be dropped, reordered, duplicated.

| Guarantee                        | Mechanism                |
| -------------------------------- | ------------------------ |
| Guaranteed delivery and ordering | Sequence number and ACKs |
| Loss recovery                    | Retransmission           |
| Flow and congestion control      | Sliding window           |
- Client sends SYN packet and opens TCB locally
- Server kernel receives SYN, creates entry in **SYN queue**, responds with SYN-ACK
- Client sends ACK, server kernel moves connection from SYN queue to accept queue
- Application process calls `accept()` syscall to retrieve socket file descriptor from accept queue
- apache prefork (1 process per Conn), thread-per-request, nginx worker process

However, we do not need TCP to guarantee delivery, order etc.
- My personal belief is that managing these concerns in the application layer provides more control to the application. 
- QUIC is a good example of this. It runs using UDP, but reintroduces the TCP guarantees in user space. A benefit of QUIC over TCP is the elimination of HOL blocking.
### 3. Load Balancer
Problem: 
- Memory overloaded in handling concurrent connections (factor of 100x after `accept` call) 
- accept queue has max limit (default 4096 in `net.core.somaxconn`) 
- SYN queue can overflow (SYN flood attacks)

**Solution:** 
- Load balancers distribute millions of connections across multiple backend servers
- Optimized kernel tuning (larger SYN/accept queues)
- Maintain persistent TCP connections to servers to avoid repeated TCP handshakes
- Enable horizontal scaling beyond single-machine limits     

### 4. Geographic Latency: CDN
- Routing all global traffic to single US data center causes high latency for Singapore users (RTT)
- CDNs cache content at edge locations worldwide and serve from geographically nearest PoP
- Authoritative nameserver uses GeoDNS to return IP of nearest edge server based on resolver location

### 5. GeoDNS Limitations: Anycast Routing
GeoDNS unreliable because: 
- Many ISPs don't implement EDNS Client Subnet (forward partial IP subnet info) properly
- Users configure third-party resolvers like 1.1.1.1 in `/etc/resolv.conf` or `systemd-resolved`, masking true client location

Solution:
- Anycast announces same IP prefix from multiple global PoPs via BGP
- Network-layer routing automatically directs packets to topologically nearest location based on number of hops
- Instant route updates via BGP convergence (seconds vs DNS TTL minutes)
### 6. TLS Handshake
**TLS 1.2:** Two round trips required
- ClientHello → 
- ServerHello/Certificate/Key Exchange → 
- Client Key Exchange/Change Cipher Spec → 
- Server Change Cipher Spec.
**TLS 1.3:** One round trip only, based on Zero Knowledge mathematical proof

### 7. HTTP Request/Response

**HTTP/1.1 persistent connections by default.** Client sends `GET /` for HTML document over single TCP connection. Browser parses HTML incrementally, discovers subresources (CSS, JS, images). 

**Same origin**: Reuses existing TCP connection for additional GET requests, no new handshake needed. **Cross-origin**: Requires new TCP handshake + TLS handshake to different domain.

**HTTP/2 improvement:** Multiplexes multiple requests over single connection as parallel streams.

**HTTP/3:** Uses **QUIC** that runs on UDP, removes Head-of-Line blocking of TCP.
- **Head-of-Line Blocking:** TCP intentionally waits for current in-order segment before delivering it to the application, even when future segments arrived.
- What TCP provides is a standardization with the kernel stack, easier to troubleshoot with `ss -t`, `tcpdump` and other linux commands.