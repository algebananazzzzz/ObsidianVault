| Mechanism       | Type         | OS involvement  | Speed   | Complexity |
| --------------- | ------------ | --------------- | ------- | ---------- |
| Shared Memory   | Memory-based | Low after setup | Fastest | Hard       |
| Message Passing | Kernel-based | High            | Slower  | Easier     |
| Pipe            | Kernel-based | Medium          | Medium  | Easy       |
| Signal          | Notification | Low             | Fast    | Limited    |
## 1. Shared Memory

Shared memory is when the OS creates a region of memory that **multiple processes can access directly**. Each process attaches this region into its own address space, and from then on, reading or writing to it is just like using a normal variable.

**How it works (conceptually):**
- Process A creates shared memory.
- Process B attaches to the same memory.
- Both can read/write the same data.
- Detach from memory space after use.
- Destroy M (if not attached to any process)

**Key characteristics:**

|Aspect|Explanation|
|---|---|
|OS involvement|Only during create/attach/detach|
|Speed|Fastest IPC (no kernel in normal access)|
|Synchronization|Must be handled manually|
|Data size|No practical restriction|

**Main advantage:** Low OS overhead, ideal for large data.  
**Main disadvantage:** Risk of race conditions if both processes access simultaneously without synchronization, harder implementation.
## 2. Message Passing

Message passing is when processes communicate by **sending messages through the OS kernel**. The OS acts as an intermediary that receives the message from one process and delivers it to another.

**How it works:**
- Process A calls send() syscall
- OS stores message in kernel memory space
- Process B calls receive() syscall
- OS delivers message 

**Direct vs Indirect Communication:**

| Type     | Behaviour                                                                                          |
| -------- | -------------------------------------------------------------------------------------------------- |
| Direct   | Sender / Receiver explicitly names the other party<br>One link per pair of communicating processes |
| Indirect | Messages are sent / received from a message store                                                  |

**Blocking vs non-blocking:**

| Type         | Behaviour                           |
| ------------ | ----------------------------------- |
| Blocking     | Wait until message is sent/received |
| Non-blocking | Continue immediately                |

**Main advantage:** Portable (across distributed systems / WAN) and easier synchronization.  
**Main disadvantage:** Inefficient due to kernel overhead, messages must be structured.

## 3. Pipe

A pipe is a **kernel-managed buffer that connects the output of one process to the input of another**. One process writes data into the pipe, and another reads from it.

**How it works:**
- OS creates pipe
- One process writes
- Another process reads
- Data flows in order.

**Important behaviour:**
Pipe functions as **circular bounded byte buffer** with **implicit synchronization**
- If buffer is empty → reader waits
- If buffer is full → writer waits

**Main advantage:** Simple and automatically synchronized.  
**Main disadvantage:** Limited flexibility and usually between related processes.

## 4. Signal

A signal is a **asynchronous notification sent to a process to inform it that an event occurred**. It does not carry meaningful data, only the event type.

**How it works:**
- OS or another process sends signal
- Process stops normal execution    
- Signal handler runs

Examples:
- SIGKILL → terminate
- SIGSTOP → pause
- SIGSEGV → memory error

**Main advantage:** Fast and simple.  
**Main disadvantage:** Cannot transfer actual data.
