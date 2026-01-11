## Primary Responsibilities:
Transfer the datagram across a single link between single / many nodes. 
1. **Framing** (Addressing)
2. **Medium Access Control (MAC)**
   If multiple nodes share a **broadcast link**, how do they avoid speaking at the same time?
3. **Error Detection and Correction**
   Noise corrupts bits → must detect (and correct) errors.
#### Implementation:
Performed in **NIC (Network Interface Card)** → often hardware for speed.
#### Types of Link Technologies
1. **Point-to-Point Links**: Only 2 nodes share the medium.
	- **PPP**, **SLIP**, fiber links between switches/routers
	- No collisions → no MAC needed
2. **Broadcast Links**: Many nodes share one channel.
	- Classic **Ethernet bus**, 802.11 **Wi-Fi**