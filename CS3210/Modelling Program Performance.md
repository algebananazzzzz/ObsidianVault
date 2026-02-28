## Reduce Latency

Parallelization aims to reduce the **wall-clock execution time**:
 $$T_{p}(n)$$

Where:
- n = problem size
- p = number of processing units

$$\text{Maximum Degree of Concurrency}=\frac{\text{Total\ Work}}{\text{Critical\ Path\ Length}}$$
#### Speedup
Ratio of how fast the parallel program runs compared to the best sequential version.
$$S_{p}(n)=T_{*}(n) / T_{p}(n)$$
**Superlinear Speedup**
Sometimes speedup exceeds the number of processors. This may occur due to:
- Improved cache utilization
- Reduced memory contention
- Better branch prediction behavior

#### Cost
Total amount of work performed by all processors combined.
$$C_{p}(n)=p \times T_{p}(n)$$
A parallel algorithm is **cost-optimal** if it performs the same total work as the fastest sequential algorithm.

#### Efficiency
Efficiency measures how well the processors are utilized.
$$E_{p}(n)=S_{p}(n) / p=T_{*}(n) / C_{p}(n)$$
- $E_{p}(n) = 1$ → perfect linear speedup
- $E_{p}(n) < 1$ → some overhead or idle time
- $E_{p}(n) > 1$  → superlinear speedup

## Amdahl’s Law (1967)
**Speedup of parallel execution is limited by the sequential fraction of the code** ( f ).
#### Execution Time
**Sequential**
$$T_{*}(n)=T_{seq}+T_{par}=f \cdot T^*(n)+(1-f) \cdot T^*(n)$$

**Parallel**
$$T_{p}(n)=T_{seq}+T_{par}=f \cdot T^*(n)+(1-f) \cdot \frac{T^*(n)}{p} $$
**Speedup**
$$S_{p}(n)=\frac{T^*(n)}{T_{p}(n)}=\frac{1}{f + \frac{1-f}{p}}$$
As $p \rightarrow \infty$, $S_{p}(n) \rightarrow \frac{1}{f}$, thus max speedup is $\frac{1}{f}$.

## Gustafson’s Law

Gustafson’s Law assumes **scaled workload** instead of fixed workload. The intuition is that serial fraction become insignificant for larger problems, thus speedup is only limited by p.

Instead of fixing the problem size, we **fix the parallel execution time** and scale the problem as the number of processors increases.
#### Execution Time
Let $f'$ be the sequential fraction for the chosen problem size n
**Parallel**
$$T_{p}(n)=f' \times T_{p}(n)+(1-f') \times T_{p}(n)$$
**Sequential**
$$T_{p}(n)=f' \times T_{p}(n)+(1-f') \times T_{p}(n) \times p$$
**Speedup**
$$S_{p}(n)=f' + (1-f')p$$
## Amdahl vs Gustafson

![[Pasted image 20260209034828.png]]

**Amdahl:** Pessimism - same problem size, f limits speedup
**Gustafson:** Optimism - solve bigger problems that parallelize better

**Assumptions:**
- **Amdahl:** serial fraction is fixed
- **Gustafson:** serial fraction is 0 if problem is large enough
- How about memory bottleneck, communication between processors etc.?
## User CPU Time
Can be modeled with the sum of these components
$$
\mathrm{Time}_{\mathrm{user}}(A)=(\text{sum of components})\times \mathrm{Time}_{\mathrm{avg\_cycle}}
$$
### 1. Instruction Time
$$
N_{\mathrm{instr}}(A)\times \mathrm{CPI}(A)\times \mathrm{Time}_{\mathrm{avg\_cycle}}
$$

| Term                                  | Meaning                                                                                           |
| ------------------------------------- | ------------------------------------------------------------------------------------------------- |
| $N_{\mathrm{instr}}(A)$               | Total number of instructions in program A *(architecture and compiler dependent)*                 |
| $\mathrm{CPI}(A)$                     | Average cycles per instruction of all instructions in program A *(mostly architecture dependent)* |
| $\mathrm{Time}_{\mathrm{avg\_cycle}}$ | Average time per CPU cycle *(1 / average CPU frequency)*                                          |

### 2. Average Memory Access Time
$$
N_{\text{rw\_ops}}(A)\times R_{\text{miss\_rate}}
$$
$$
T_{\text{read\_access}}(A)=T^{L1}_{\text{read\_hit}}(A)+\left(R^{L1}_{\text{read\_miss\_rate}}(A)\times T^{L1}_{\text{read\_miss}}(A)\right)
$$
Continuing on,
$$
T^{L1}_{\text{read\_miss}}(A)=T^{L2}_{\text{read\_hit}}(A)+\left(R^{L2}_{\text{read\_miss\_rate}}(A)\times T^{L2}_{\text{read\_miss}}(A)\right)
$$
Global miss rate:
$$
\text{Global miss rate}=R^{L1}_{\text{read\_miss\_rate}}(A)\times R^{L2}_{\text{read\_miss\_rate}}(A)
$$

### Throughput

| Metric             | Formula                                              | Meaning                                      | Notes                                          |
| ------------------ | ---------------------------------------------------- | -------------------------------------------- | ---------------------------------------------- |
| MIPS<br>(BogoMIPS) | Clock Rate / (CPI × 10⁶)                             | Million Instructions Per Second              | Depends on instruction set, easy to manipulate |
| MFLOPS             | Floating point instructions / (Execution Time × 10⁶) | Million Floating Point Operations Per Second | Standardize to FP64/32                         |
**Roofline Model (Contention)**
$\mathrm{Arithmetic\ Intensity}=\frac{\mathrm{Number\ of\ FLOP\ instructions}}{\mathrm{Amount\ of\ data\ moved\ (bytes)}}$
High arithmetic intensity → compute bound
Low arithmetic intensity → memory bound

**Solutions**
1. **Exploit Temporal Locality**  
	- Co-locate tasks that reuse the same data.  
	- Schedule threads that operate on the same data structure **close in time** and preferably on the **same core / socket** to maximize cache reuse (warm caches).  

2. **Exploit Spatial Locality**  
- Split / tile work so threads touch data that is **contiguous in memory** (better cache-line utilization).  
- Use **padding / alignment** to avoid false sharing when multiple threads update nearby fields.

