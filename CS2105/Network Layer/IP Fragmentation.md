## IP Datagram Format
**Properties**
1. **Identification**
   16-bit unique number shared across fragments
2. **Flags**
	- First bit is reserved and unused
	- Second bit is the **Do Not Fragment (DF)** bit. If datagram exceeds size, drop.
	- Third bit is the **More Fragments (MF)** bit. Indicates if more fragments are arriving.
3. **Fragmentation Offset**
   Offset measured in units of 8 bytes.
   E.g. if fragment carries bytes 1400-1600, offset $=\frac{1400}{8}=175$.
4. **Header Checksum:** 16 bits header checksum for checking errors in the datagram header
5. **Time to live:** Decrements by 1 per hop to prevent infinite loops
6. **Protocol:** Name of the **transport-layer protocol** (1: ICMP, 6: TCP, 17: UDP)

![[IP Datagram.png]]

### Purpose of Fragmentation
When data travels through a network, every network has a Maximum Transmission Unit (MTU) which is inclusive of the header + data.