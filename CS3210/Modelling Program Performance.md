## Reduce Latency

Parallelization aims to reduce the **wall-clock execution time**:
 $$T_{p}(n)$$

Where:
- n = problem size
- p = number of processing units

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
$$E_{p}(n)=S_{p}(n) / p = )=T_{*}(n) / C_{p}(n)$$
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
**V1**
$$Time_{user}(A)=N_{cycle}(A) \times Time_{cycle}$$

| $Time_{user}(A)$ | User CPU time of program A                               |
| ---------------- | -------------------------------------------------------- |
| $N_{cycle}(A)$   | Total number of CPU cycles required for all instructions |
| $Time_{cycle}$   | 1 / CPU clock cycle frequency                            |
**V2**

## Amdahl's Law
Speedup of parallel execution is limited by the sequential fraction of the code (parts that cannot be paralleized)
f (0 <= f <= 1) sequential fraction
fixed workload performance

Sequential execution time:
$$T_{seq} = f \times T_{*}(n)$$
Parallel execution time:
$$T_{par} = (1-f) \times T_{*}(n)$$
