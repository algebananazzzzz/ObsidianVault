## Special IP Ranges

**Private**  
**Class A:** 10.0.0.0 to 10.255.255.255 (10/8)
**Class B:** 172.16.0.0 to 172.31.255.255 (172.16/12)
**Class C:** 192.168.0.0 to 192.168.255.255 (192.168/16)

**Default route:** 0.0.0.0/8
**Loopback:** 127.0.0.0/8 (never forwarded)
**Broadcast:** 255.255.255.255/32
- Delivered to **every host on the local link** (e.g. DHCP request)
- Encapsulated in an Ethernet broadcast frame (FF:FF:FF:FF:FF:FF)
- Routers **never forward** them — **must drop**

**Obtaining IP Addresses**
1. Lease from ISP's larger block of IP addresses
2. ICANN allocate addresses to regional internet registries, which handles allocation/management of IP addresses in the region

## Dynamic Host Configuration Protocol (DHCP)

Static IP: client is assigned IP address manually. Problem: IP conflict  
DHCP servers assigns client IP leases from its scope

| Illustration                         | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| ------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| ![[Pasted image 20251127211203.png]] | Options:<br>- IP address<br>- Subnet mask<br>- Default gateway<br>- DNS server<br>- Maximum Transmission Unit (link layer)<br><br>**Discover**<br>Client broadcast to discover DHCP servers<br><br>**Offer**<br>DHCP server unicasts an offer to client MAC; otherwise broadcast if not supported<br><br>**Request**<br>Client selects an offer and broadcasts request so non selected servers return offered IPs to their scope<br><br>**Ack**<br>Server acknowledges the offer |
## The Internet and Autonomous Systems

**Autonomous System (AS)**
Collection of IP networks and routers under a single administrative entity that presents a unified routing policy to the external Internet. 

Each AS is assigned a globally unique Autonomous System Number (ASN) by regional Internet registries, which serves as its identifier in BGP routing. 1 to 64511: global use. 64512 to 65535: private.

## Routing Policies
| Feature            | **Distance Vector**                                                 | **Link State**                                                        |
| ------------------ | ------------------------------------------------------------------- | --------------------------------------------------------------------- |
| Core idea          | Routers tell neighbors **“my best distance to every destination.”** | Routers flood **full link information**; **complete topology** (LSDB) |
| Router’s algorithm | **Bellman–Ford** (iterative, distributed).                          | **Dijkstra’s SPF** run locally on LSDB.                               |
| Convergence        | **Slow** (can count to infinity).                                   | **Fast** (converges after LSA flood).                                 |
| Scalability        | Small to medium networks.                                           | Large enterprise/service providers.                                   |

**Intra-As policies**

| Protocol  | Type / Mechanism | Metric                  | Strengths                                          | Weaknesses                                |
| --------- | ---------------- | ----------------------- | -------------------------------------------------- | ----------------------------------------- |
| **RIP**   | Distance Vector  | Hop count (max 15)      | Simple, low overhead                               | Slow convergence, loops                   |
| **OSPF**  | Link State       | Cost based on bandwidth | Fast convergence, hierarchical areas               | Complex configuration; larger LSDB        |
| **IS-IS** | Link State       | Cost (configurable)     | Scales very well, widely used in service providers | Less common in enterprises, harder syntax |
**Inter-AS Protocols**

| Protocol                          | Type / Mechanism | Metric / Decision Basis            | Strengths                              | Weaknesses                                   |
| --------------------------------- | ---------------- | ---------------------------------- | -------------------------------------- | -------------------------------------------- |
| **BGP (Border Gateway Protocol)** | Path Vector      | LOCAL_PREF, AS-PATH, MED, policies | Internet-scale routing, policy control | Slow convergence, very complex, large tables |
