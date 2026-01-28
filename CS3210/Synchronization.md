## Data Race
A **data race** exists when:
- Two threads access the **same memory location** concurrently
- At least one access is a **write**
- There is **no synchronization** that creates a _happens-before_ relationship between those accesses
## Deadlock

Deadlock exists **iff all four hold simultaneously**:
1. **Mutual exclusion**  
    At least one resource is non-shareable (only one holder at a time)
2. **Hold and wait**  
    A process holds resource(s) and waits for additional resource(s)    
3. **No preemption**  
    Resources can’t be forcibly taken; only released voluntarily
4. **Circular wait**  
    There is a cycle: P1 waits for P2, P2 waits for P3, …, Pn waits for P1

**Solutions:**
1. **Ignore it (ostrich):**  assume rare; recover by restarting when it happens  
    When used: many general-purpose systems for some deadlock classes because prevention overhead is high
2. **Prevention (break a Coffman condition):** Make at least one of the 4 conditions impossible:
	- Break mutual exclusion -> use shareable resources (rarely possible for locks)
	- Break hold-and-wait -> request all resources at once (bad utilization)
	- Break circular wait -> enforce global ordering of resource acquisition
3. **Avoidance (control allocation):** Only grant requests if system stays in a “safe state”  
    - Banker’s algorithm (needs max demands known in advance)  
4. **Detection and recovery:** Let deadlock happen, then recover by preempting (DB transactions)

## Livelock



## Starvation

A thread is **prevented from making progress** for an unbounded time because other threads “keep winning”.
- Scheduling: high priority always runs -> low priority never gets CPU    
- Locks: one thread repeatedly reacquires lock faster than another, especially with unfair locks

## Locks
 Protects a **critical section** and shared state: 
`acquire() -> enter critical section -> release()`

## Mutex
**Binary lock with ownership**: at most 1 thread holds it at a time

**Mechanism**
**CAS (Compare and Swap):** a CPU atomic operation, if val = expected val, change val
-  Fast path: CAS succeeds → enter without blocking
- Slow path: CAS fails → syscall `futex` (put thread on wait queue)
- Wake: Mutex unlocks → syscall `futex` (wake thread)

## Semaphore
- Provide mutual exclusion via atomic counters (`count >= 0` at all times)
- `wait()` consumes a permit (decrement)
- `signal()` adds a permit (increment) and wakes a waiter if needed

**Mechanism**
- Fast path: decrement `counter` with CAS if `counter > 0`
- Slow path: if `count == 0`, enqueue thread on semaphore wait queue
- `post()`: wake thread on wait queue

**Drawbacks**
- **No tight coupling** between the semaphore and _critical section_.
- With a mutex, mutex belongs to structure.
- With semaphores, it’s easy to `post()`/`wait()` in the wrong place, order, number of times.

**Mutex Semaphore**
- When `count` is set to `1`
- However, it is shared globally; any thread can `post` even if it didn’t `wait`
- Easy to "unlock twice"
