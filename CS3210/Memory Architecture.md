
**Concurrency** = multiple tasks can _make progress_ (interleaving is allowed).
**Parallelism** = tasks _actually run at the same time_ (needs hardware).

## Bit Level Parallelism

### 1. Increasing Word Size (e.g., 32-bit → 64-bit)
- A **word** is the natural unit of data the processor can handle in one operation and transfer between CPU and memory.
- Larger word size allows processing **more bits per instruction**.
- E.g. A 64-bit processor can add two 64-bit integers in **one instruction**, whereas a 32-bit processor would need two operations.
### 2. Single-Instruction Multiple Data (SIMD) registers
- Use **wide vector registers** (e.g., 128-bit, 256-bit, 512-bit).
- Executes **one instruction on multiple data elements simultaneously**.
- E.g. A 512-bit register can hold 8 64-bit values and process them with a single instruction.

**Limitation:**
- Only effective when data is **highly parallelizable**

## Instruction-Level Parallelism (ILP)

**Instruction-Level Parallelism (ILP)** allows a **single CPU core** to execute **multiple independent instructions in parallel.
### Key Mechanisms

#### 1. Pipelining
- Instruction execution is split into multiple stages, e.g.:
    - **Fetch → Decode → Execute → Memory → Write Back**
- Different instructions can occupy **different pipeline stages in the same clock cycle**
    - **Maximum throughput ≈ number of pipeline stages**
    - (One instruction completes per cycle after the pipeline is full)
#### 2. Superscalar
- Uses **multiple parallel execution units (pipelines)** e.g. 4 decoders, 5 ALU etc.
- Executes **multiple instructions per clock cycle**, Increases **Instructions Per Cycle (IPC)**


**Limitations**
- **Data hazards**: one instruction depends on the result of another
- **Control hazards**: branches change the instruction flow

**Mitigation techniques**

- **Branch prediction** – guess control flow to avoid stalls
- **Out-of-order execution** – execute independent instructions early
- **Instruction prefetching** – reduce fetch latency

**Pipeline stall** (conditional branches): when branch predictor guessed wrong, flush incorrect instructions


## Thread-Level Parallelism
**Allows a single physical core to maintain multiple hardware execution contexts and issue instructions from multiple threads.**
- **Instruction-Level Parallelism (ILP)** is limited (typical IPC ≈ 1–3 i.e. max 1-3 parallelism)
### Key Mechanisms

### 1. Simultaneous Multithreading
- Each hardware thread has its own:
    - **Program Counter (PC)**
    - **Register state**
    - Architectural execution context
- Threads **share core resources**:
    - Execution units (ALU, FPU, Load/Store)
    - Caches
    - Branch predictors
- If one thread stalls (e.g., cache miss), the other thread can use the execution units.
- E.g. Intel **Hyper-Threading** — one physical core exposes **two logical cores**.
- **SMT does NOT double performance.** It increases core utilization.

## **Processor-Level Parallelism (PLP)**

**Improves performance by adding more physical cores to a processor.**
- Each core can execute its own thread(s) independently.
- Enables true parallel execution across cores.

## Flynn's Parallel Architecture Taxonomy
 
### 1. Single Instruction, Single Data (SISD)
- One instruction stream
- Each instruction operates on a single data element
### 2. Single Instruction, Multiple Data (SIMD)
- One instruction stream 
- Same instruction applied to multiple data elements simultaneously
- Data-parallel execution
### 3. Multiple Instruction, Multiple Data (MIMD)
- Multiple processing units (cores)
- Each core fetches its own instruction stream
- Each core operates on its own data
- Most modern multi-core systems follow this model
### 4. Variant: SIMD + MIMD
- Combines **MIMD across cores** and **SIMD within each core**.
- Each core executes its own instruction stream (**MIMD**).
- Within a core, a single instruction can operate on multiple data elements (**SIMD**)

## Memory Hierachy
1. Registers (~1 ns)
2. L1 / L2 / L3 cache  (~1–5 ns)
3. Main memory (RAM) (~100 ns)
4. Disk storage (💀 millions of ns)



### Uniform Memory Access (UMA)
- All processors share a **single main memory**.
- Memory access latency is **uniform** for every core.
### Non-Uniform Memory Access (NUMA)
- Each processor has **its own local memory**.
- Memory access time depends on location:
    - Local memory → faster
    - Remote memory → slower
### Cache-Only Memory Architecture (COMA)
- No fixed “main memory” per node.
- All memory behaves like a large distributed cache.
- Data migrates dynamically to where it is accessed.