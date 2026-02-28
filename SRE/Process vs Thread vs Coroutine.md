# Process
**Process = Cubicle** (closed space)

| Analogy                                                     | Mapping                                                                                                                          | Significance                                                      |
| ----------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------- |
| Each cubicle has its own privacy and dedicated toilet paper | The kernel has to create a new `task_struct`, a new `mm_struct` (virtual memory map) and a new `files_struct` (file descriptors) | **Independence and Security:** processes do not affect each other |
![[SRE/images/Diagram 1.svg]]

**Problem:** 
1. **Cannot run multiple tasks at the same time:** multiple CPU cores, process only utilizes one
2. **Cannot share address space, file descriptors**
3. **Inter Process Communication requires kernel switch:** requires syscalls (gRPC)
# Thread
**Thread = multiple People sharing a Cubicle**

| Analogy                                      | Mapping                                                                                      |
| -------------------------------------------- | -------------------------------------------------------------------------------------------- |
| Everyone know what's going on in the cubicle | Lower context switching cost as `mm_struct` (memory map) remains unchanged (no flushing TLB) |
| Each person has its own state                | Each thread has its own stack i.e. `thread_struct`                                           |
| **Risk:** everyone shares same space         | Threads share memory, concurrency and security risk.                                         |
**Problem:**
1. **Race Condition:** use mutex, atomic operations
2. **Deadlock:** two threads waiting for mutex to be released by each other

**Resolution:**
1. **Mutex:** Sleep until lock is released (different from `await`)
	- **CAS (Compare and Swap):** CPU atomic operation, if val = expected val, change val
	- Fast path: CAS succeeds → enter without blocking
	- Slow path: CAS fails → syscall `futex` (put thread on wait queue)
	- Wake: Mutex unlocks → syscall `futex` (wake thread)
2. **Spinlock:** Keep checking in a loop (burns clock cycles)
3. **Atomic operations:** Same concept as SQL, use Golang sync/atomic package
4. **Semaphore:** Same as mutex, but allows a max of **n** threads
# Coroutine
**Coroutine = same Person multitasking**
Personal opinion: I really don't see a benefit threads provide, just use coroutines (just like Golang).

**Golang** uses the **GPM** model
- **G:** Goroutine (lightweight function)
- **P:** Processor (scheduling context, binds to each **M** thread with a Local Run Queue)
- **M:** Machine (actual OS thread, set by `GOMAXPROCS`)

- **P** creates **G** and places it in its local run queue (LRQ)
- If LRQ > 256 tasks, put in global run queue (GRQ)
- Each **M** thread continuously pop LRQ / GRQ for tasks
- Yielded G open file descriptor to epoll, special G polls epoll for completion to place original G in queue
- If queues empty, steal G from other P's LRQ