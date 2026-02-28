Cache Coherence Overhead
- Multiple cores writing to same memory location or cache line

### Cache Coherence Properties  
  
1. **Program Order**  → Own order correct
	A core sees its **own reads/writes in order**.  
	→ If it writes X, later reads must see that write. 
2. **Write Propagation** → Eventually visible
	A write becomes **visible to other cores eventually**.  
	→ Other cores will eventually read the new value.  
3. **Transaction Serialization**  → Same global order
	All cores see **writes in the same order**.  
	→ If X=1 then X=2, no core can see 2 before 1. 

### Cache Coherence Implementations  

1. **Snooping-based**  
- Caches **broadcast and monitor bus**  
- All cores see all writes  
- Simple, but high bus traffic  
  
2. **Directory-based**  
- **Central directory tracks cache states**  
- No broadcast → less congestion  
- Scales better (NUMA systems)

**Granularity**
- Coherence maintained **per cache block**
- Typical size: **64 bytes**

**Issues**
1. Cache ping-pong  
	- Multiple cores access **same cache line**  
	- Causes **repeated invalidations**  
	- Leads to **slow memory fetches**  
2. False sharing (important)  
	- Cores access **different variables in same cache line**  
	- Causes unnecessary invalidations  
	- **Solution: padding**

**Coherence vs Consistency**
Coherence = Everyone agrees on ONE variable
Consistency = Everyone agrees on the ORDER of multiple variables

Optimizations
Sequential Consistency
Relaxed Consistency 


|                 | Sequential Consistency | Total Store Ordering | Processor Consistency | Partial Store Ordering |
| --------------- | ---------------------- | -------------------- | --------------------- | ---------------------- |
| Program order   | ✅                      | ❌                    | ❌                     | ❌                      |
| R → R / R → W   | ✅                      | ✅                    | ✅                     | ✅                      |
| W → R           | ✅                      | ❌                    | ❌                     | ❌                      |
| W → W           | ✅                      | ✅                    | ✅                     | ❌                      |
| Write Atomicity | ✅                      | ✅                    | ❌                     | ✅                      |
**Total Store Ordering**
Store enters store buffer, load executes immediately (before store is globally visible)

| Operation             | Behaviour                                                                           |
| --------------------- | ----------------------------------------------------------------------------------- |
| A = 1;  <br>print(B); | Store A → store buffer<br>Load B → executes immediately<br>Store A propagates later |
**Processor Consistency**
Writes can become visible to cores at different times.

**Partial Store Ordering**
Writes can bypass earlier writes (non FIFO write buffer).