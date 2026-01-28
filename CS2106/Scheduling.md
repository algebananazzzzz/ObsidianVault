
| Non-preemptive                                                     | Preemptive                                                        |
| ------------------------------------------------------------------ | ----------------------------------------------------------------- |
| Process stays scheduled until it blocks or give up CPU voluntarily | Process is given a fixed time quota to run before being suspended |


### Batch Processing
- No user interaction
- Typically non-preemptive -> run job until completion/block/exit

**Key metrics:**
**Turnaround time**
- Finish time - arrival time
- Includes waiting time

**Throughput:** rate of task completion

**CPU utilization:** % time CPU is working on task

| Algorithm                              | Process                                             | Guarantee                                               | Limitation                                                          |
| -------------------------------------- | --------------------------------------------------- | ------------------------------------------------------- | ------------------------------------------------------------------- |
| **First-Come First-Served (FCFS)**     | Jobs stored in FIFO queue → pick first job to run   | No starvation → every job eventually runs               | Convoy effect → long CPU-bound job delays many short/I/O-bound jobs |
| **Shortest Job First (SJF)**           | Select job with smallest total CPU time             | Minimizes average waiting time if job lengths are known | Starvation of long jobs; hard to predict CPU time                   |
| **Shortest Remaining Time Next (SRT)** | Preemptive variant of **SJF:** remaining time known | Better response for short jobs than SJF                 | Higher overhead of keeping track of preemptive time quota           |
SJF exponential average:
  $$\text{Predicted}_{n+1}=\alpha\text{Actual}_{n}+(1-\alpha)\text{Predicted}_{n}$$


### Interactive environment
- Timer interrupt fires (~1–10ms) -> forces entry into kernel -> scheduler runs
- Time quantum q -> max CPU time a task gets before preemption

**Key metrics:**
- **Response time:** time between request and response by system
- **Rredictability:** variation in response time

| Algorithm                             | Process                                                                                                                                                                | Guarantee                                                        | Limitation                                             |
| ------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------- | ------------------------------------------------------ |
| **Round Robin (RR)**                  | Store tasks in FIFO queue → dispatch head until fixed time quantum elapsed → place task at end of queue / blocked queue                                                | Bounded response time: wait ≤ (n−1)q                             | Large _q_ → FCFS behavior; small _q_ → high overhead   |
| **Priority Scheduling**               | Assign priority per task → dispatch highest priority task<br><br>**Preemptive variant:** preempt current lower-priority task                                           | High-priority tasks receive fast service                         | Starvation of low priority; priority inversion         |
| **Multi-Level Feedback Queue (MLFQ)** | Multiple ready queues `Q0...Qk` using RR→ tasks move based on observed behavior                                                                                        | Favors interactive/I-O bound tasks; approximates SJF dynamically | Parameter-sensitive; can be gamed without strict rules |
| **Lottery Scheduling**                | Each task holds `T` tickets → scheduler draws random ticket per quantum<br><br>**Compensation ticket / transfer ticket:** transfer tickets to priority / starved tasks | Probabilistic fairness in scheduling                             | Weak short-term latency guarantees due to randomness   |

