## Reduce Latency

Parallelization aims to reduce the **wall-clock execution time**:
 $$T_{p}(n)$$

Where:
- n = problem size
- p = number of processing units

### Speedup
Ratio of how fast the parallel program runs compared to the best sequential version.
$$S_{p}(n)=T_{*}(n) / T_{p}(n)$$
**Superlinear Speedup**
Sometimes speedup exceeds the number of processors. This may occur due to:
- Improved cache utilization
- Reduced memory contention
- Better branch prediction behavior

### Cost
Total amount of work performed by all processors combined.
$$C_{p}(n)=p \times T_{p}(n)$$
A parallel algorithm is **cost-optimal** if it performs the same total work as the fastest sequential algorithm.

### Efficiency
Efficiency measures how well the processors are utilized.
$$E_{p}(n)=S_{p}(n) / p = )=T_{*}(n) / C_{p}(n)$$
- $E_{p}(n) = 1$ → perfect linear speedup
- $E_{p}(n) < 1$ → some overhead or idle time
- $E_{p}(n) > 1$  → superlinear speedup