
**Concurrency** = multiple tasks can _make progress_ (interleaving is allowed).
**Parallelism** = tasks _actually run at the same time_ (needs hardware).

Bit Level Parallelism
word size: unit of transfer between processor and memory
impact: lesser operations e.g. 64-bit system only requires 1 instruction to add

Single-Instruction Multiple Data (SIMD) registers
64-bit processors can support 512-bit registers + instructions (load 8 words at a time into register)

Limitation: stop increasing CPU word size due to trade off 


## Instruction-Level Parallelism (ILP)

**Instruction-Level Parallelism (ILP)** allows a **single CPU core** to execute **multiple independent instructions in parallel.
### Key Mechanisms

#### 1. Pipelining
- Instruction execution is split into multiple stages, e.g.:
    - **Fetch → Decode → Execute → Memory → Write Back**
- Different instructions can occupy **different pipeline stages in the same clock cycle**
    - **Maximum throughput ≈ number of pipeline stages**
    - (One instruction completes per cycle after the pipeline is full)

**Limitations**
- **Data hazards**: one instruction depends on the result of another
- **Control hazards**: branches change the instruction flow

**Mitigation techniques**

- **Branch prediction** – guess control flow to avoid stalls
- **Out-of-order execution** – execute independent instructions early
- **Instruction prefetching** – reduce fetch latency

However, **pipeline stall** can be caused by some conditional branches: when branch predictor guessed wrong, flush incorrect instructions
  

**Memory Hierachy**
1. Registers (~1 ns)
2. L1 / L2 / L3 cache  (~1–5 ns)
3. Main memory (RAM) (~100 ns)
4. Disk storage (💀 millions of ns)

Parallel programs often stall not on _compute_, but on:
- cache misses
- contention
- coherence traffic

Flynn's Parallel Architecture Taxonomy

Memory Organization
- distributed memory
- shared memory
- hybrid