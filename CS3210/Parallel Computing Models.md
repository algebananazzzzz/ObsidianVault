| **Data Parallelism**                                                             | **Task Parallelism**                                                                               |
| -------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------- |
| Divide the **data set** among processing units (PUs).                            | Divide the **work (tasks/functions)** among processing units.                                      |
| Each PU performs the **same operation** on different portions of data.           | Each PU may perform **different operations**, possibly on the same or different data.              |
| Best when operations are independent across elements.                            | Represented using a **Task Dependence Graph**. <br>- **Critical path length:** max completion time |
| Example: Split an array into chunks; each PU processes its own subset.           | Example: One PU sorts data, another compresses it, another writes to disk.                         |
| Loop parallelism: If loop iterations are independent, they can run concurrently. | Pipeline parallelism: Different stages of computation run in parallel.                             |

## Foster's Design Methodology (PCAM)
![[Pasted image 20260206165253.png]]

### 1. Partitioning
**Divide conputation and/or data into independent pieces to discover opportunity for parallelism**
- At least 10x more primitive tasks than processing units
- Minimize redundant computations and data storage
- Should be roughly uniform size
- Numbers of tasks being proportional to problem size
### 2. Communication
**Design Local and Global communication between tasks**
- **Local:** communicate with a small group of neighbours
- **Global:** require centralized (no distribution) or sequential (no overlap) computation and communications. Likely a **limiting factor** for parallelism.
### 3. Agglomeration
**Combine tasks into larger tasks to reduce task creation and communication cost**
- Increase the locality of parallel algorithm
- Tradeoff between agglomeration and code modification costs is reasonable
### 4. Mapping
**Map tasks to processors (cores)**
- Maximize processor utilization: place tasks on different processing units
- Minimize IPC: place tasks that communicate frequently on the same processing units 

# Parallel Programming Pattern
| **Pattern Name**                            | **Typical Use Case (Real World)**                                                  | **Mechanism (How It Works)**                                                                                                                         |
| ------------------------------------------- | ---------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Fork–Join**                               | CI test execution, batch jobs, parallel computation (e.g., map-reduce style tasks) | Main thread **forks** multiple parallel subtasks, waits for them to complete (**join**), then continues. Often recursive (e.g., parallel quicksort). |
| **Parbegin–Parend**                         | Simple structured parallel sections, academic models, OpenMP sections              | Multiple blocks start in parallel at `parbegin` and synchronize at `parend`. All threads begin and end together in a single structured region.       |
| **Single Instruction Multiple Data (SIMD)** | Image processing, matrix operations, cryptography, ML inference                    | One instruction operates on **multiple data elements simultaneously** using vector registers. Hardware-level data parallelism.                       |
| **Single Program Multiple Data (SPMD)**     | HPC simulations, distributed computing (MPI), multi-process workloads              | All processors run the **same program**, but operate on different subsets of data. May diverge via conditionals (e.g., rank-based logic).            |
| **Master–Worker**                           | Distributed job schedulers, task orchestration systems                             | A master node assigns tasks to workers, collects results, and coordinates execution. Workers do not coordinate directly.                             |
| **Task (Worker) Pool**                      | Web servers, RPC handlers, background job systems                                  | A fixed set of worker threads repeatedly pull tasks from a shared queue. Queue access must be synchronized to avoid race conditions.                 |
| **Producer–Consumer**                       | Log ingestion, metrics pipelines, async logging, message queues                    | Producers generate data and push to a buffer/queue; consumers pull and process it. Requires synchronization and possibly bounded buffers.            |
| **Pipeline (Stage-Based)**                  | ETL pipelines, video transcoding, streaming data processing                        | Work is divided into stages; each stage processes data and passes it to the next stage. Different stages run concurrently.                           |
