# Link Layer Implementation — NIC

NIC handles:
- **Framing**
- **MAC & addressing**
- **CSMA/CD**
- **CRC generation/checking**
- Interacts directly with **physical layer**
# 1. Framing
Encapsulate IP packet with:
![[Framing.svg]]
**Ethernet frame**
![[Ethernet Frame.svg]]
1. **Preamble:**
   Provides a “square wave” pattern that tells the receiver the sender’s clock rate
   - 7 bytes with pattern 10101010 (AA Hex )
   - Followed by 1 start-of-frame byte with pattern 10101011 (AB Hex )
2. **Src / Dest MAC Address**
   If NIC receives frame with **broadcast / matching destination address**, pass data in frame to network layer protocol. Else, discard frame.
3. **Type:** indicate higher layer protocol e.g. **IPv4, IPv6, ARP, PTP**
4. **Data**
   - **Max size:** link **MTU** (see IP fragmentation)
   - **Min size:** 46 bytes to ensure collision is detected
5. **CRC:** drop corrupted frames
# 2. Error Detection

| Parity Checking                                                                                                                                        | Cyclic Redundancy Check (CRC)                                                                                                                                                                                                                                                                                                                                                    |
| ------------------------------------------------------------------------------------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Single bit**<br>![[Parity (single bit).svg]]<br>- Undetected errors can reach 50% if clustered in "bursts"<br><br>**2D**<br>![[Parity (2D).svg]]<br> | **D: data bits**<br>**G: generator bit pattern of r bits** (e.g. 101 → $x^2+1$)<br><br>**Sender:**<br>1. Add **r - 1** bits of zeroes to **D**<br>2. Divide by **G** using **mod-2** (XOR) division<br>3. Obtain remainder **R** → set **CRC**<br><br>**Receiver:**<br>1. Receive **D\|\|CRC**<br>2. Perform same check with **G**<br>3. If remainder = 0 → accept. Else → ERROR |
| - Can detect and correct single/two-bit errors<br>- Single-bit error detection only                                                                    | - Easy to implement on hardware (XOR)<br>- Can detect all odd number of single-bit errors, r-bit burst errors, > r-bit burst with probability ($1-0.5^r$)                                                                                                                                                                                                                        |

**XOR division**
Same: 0; Different: 1
