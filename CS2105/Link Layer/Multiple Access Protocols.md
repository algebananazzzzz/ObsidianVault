# Desired Properties
1. **Collision-free**
2. **Efficient**
3. **Fair**
	- M active nodes → each gets ≈ **R/M** on average
4. **Fully decentralized**
    - No central coordinator
    - No global clock
    - No special “control” channel
**Mandatory: No out-of-band signaling**: Coordination must use the **shared channel itself**
# CLASS 1 — Random Access Protocols
## 1. Pure / Slotted ALOHA

| Pure ALOHA                                                                                                                                                                                                                              | Slotted ALOHA                                                                                                                                                                                                                                |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Protocol:**<br>1. When you have a frame → transmit <br>immediately at full channel data rate **R**.<br>2. Retransmit frame with probability **p** until success<br><br>**Vulnerable Period** = 2 frame time<br>**Efficiency** = 18.4% | **Protocol:**<br>1. Time is divided into slots of **L/R**.<br>2. Nodes may transmit **only at slot boundaries**.<br>3. Collisions when **≥ 2** nodes select same slot.<br><br>**Vulnerable Period** = 1 frame time<br>**Efficiency** = 36.8% |
| ![[ALOHA collision.svg]]                                                                                                                                                                                                                | ![[Slotted ALOHA collision.svg]]                                                                                                                                                                                                             |
**❌ Collision-free**
**❌ Efficient (but high if only one node)*
**✔Fair**
**✔Decentralized**
## 2. CSMA — Carrier Sense Multiple Access

| CSMA                                                                                                                                                                                                                            | CSMA/CD — Collision Detection (Ethernet)                                                                                                                                                                                                                                                                                                                         |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Protocol:**<br>1. If channel **idle** → transmit.<br>2. If channel **busy** → defer until idle.<br>3. Collisions still possible due to **propagation delay**<br>4. Problem: transmission does not <br>stop until end of frame | **Protocol:**<br>1. Sense medium. If idle → transmit.<br>2. During transmission, listen for energy from another sender.<br>3. If collision detected → abort immediately.<br>4. Run binary exponential backoff:<br>- After **m-th** collision, choose <br>  $K \in \{ 0, 1, 2, \dots, 2^m - 1 \}$.<br>- Wait **K backoff slots**, where slot = **512 bit times**. |
**Limitation of CSMA/CD: Requires wired medium →** radio can't detect while transmitting
#### Minimum Ethernet Frame Size
If frame too small → sender could finish transmitting before collision is sensed → **undetected collision** → **no retransmission** → corrupted data.

For **Ethernet**, minimum frame size = **64 bytes**.
$$
\text{Minimum frame size} \geq 2 \times \text{propagation delay} \times \text{link rate}
$$
**❌ Collision-free**
**✔ Efficient**
**✔Fair**
**✔Decentralized**
# CLASS 2 — Taking Turns Protocols

| Polling (Bluetooth)                                                                                                 | Token Passing (Token Ring)                                                                                                             |
| ------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------- |
| **Protocol**<br>One master node invites slaves to transmit **n** number of frames in **round-robin**.               | **Protocol**<br>A special frame “token” circulates; node transmit **n** frames and pass the token to next node.                        |
| **Pros:**<br>- No collisions, Controlled access<br><br>**Cons:**<br>- Polling overhead<br>- Single point of failure | **Pros:**<br>- No collisions, Deterministic access<br><br>**Cons:**<br>- Token loss → recovery needed<br>- Failures can break the ring |
| **✔ Collision-free**<br>**~ Efficient (Polling overhead)**<br>**✔Fair**<br>**❌ Decentralized (needs master)**       | **✔ Collision-free**<br>**~ Efficient (overhead not efficient at low load)**<br>**✔Fair**<br>**~ Decentralized**                       |
# CLASS 3 — Channel Partitioning Protocols

| TDMA — Time Division Multiple Access                                       | FDMA — Frequency Division Multiple Access                                                 |
| -------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------- |
| **Protocol**<br>Divide timeline into slots; each node gets dedicated slot. | **Protocol**<br>Divide frequency band into subchannels; each node gets dedicated channel. |
**✔ Collision-free**
**❌ Efficient (slots / channels idle at low load: max efficiency = R/N)**
**✔Fair**
**❌ Decentralized (needs global clock / central allocation of bands)**