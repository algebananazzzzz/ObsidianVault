# Concurrency

| Concept     | Difference                                                 | Implementation   |
| ----------- | ---------------------------------------------------------- | ---------------- |
| Concurrency | Multiple tasks are in progress but not necessarily running | `go func()`      |
| Parallelism | Multiple tasks run **simultaneously**                      | `GOMAXPROCS > 1` |
### Fork-Join Model
**Join**: use synchronization primitive like WaitGroup or channels to act as join points

![[Diagram 2.svg]]
**Without join:**

| Problem         | What happens                           |
| --------------- | -------------------------------------- |
| Goroutine leaks | Goroutines block forever, memory grows |
| Early exit      | `main` ends → work killed mid-flight   |
| Data races      | No happens-before guarantee            |
**Symptoms:**
- RSS slowly increases
- `runtime.NumGoroutine()` climbs

## Channels
**Channel != just a queue**
It is a **synchronization point** (between go routines ) with optional buffer managed by the **Go scheduler**.

| Unbuffered Channels                                                                                                                                                                                                                                                                                                                                                                                                                                                                     | Buffered Channels                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Synchronize execution: strict handoff between sender and receiver, instant backpressure                                                                                                                                                                                                                                                                                                                                                                                                 | Asynchronous execution until `n` buffers: use when work is bursty, delay backpressure                                                                                                                                                                                                                                                                                                                                                                                             |
| - Handshake (applying config, synchronous)<br>- Event loop (state ownership, K8s controller)<br>- Broadcast signal (StopEverything in K8s)                                                                                                                                                                                                                                                                                                                                              | - Worker pool / bounded parallel (I/O bound)<br>- Producer-Consumer (bursty loads)<br>- Pipelines (independent sequential, ETL)                                                                                                                                                                                                                                                                                                                                                   |
| **Sender** `ch <- v`<br>1. If there is a **waiting receiver** in `recvq`:<br>- direct handoff: copy v into the receiver dest<br>- wake receiver (mark as runnable)<br><br>2. Else (no receiver waiting):<br>- enqueue current goroutine into `sendq`<br>- park the goroutine (sleep)<br><br>**Receiver** `<- ch`<br>1. If there is a **waiting sender** in `sendq`:<br>- direct handoff: copy from sender to receiver<br>- wake sender<br><br>2. Else:<br>- enqueue into `recvq`, sleep | **Sender** `ch <- v`<br>1. If there is a **waiting receiver** in `recvq`:<br>- bypass buffer, direct handover<br><br>2. Elif buffer has space `qcount < dataqsiz`:<br>- copy v into `buf`<br><br>3. Else (buffer full):<br>- enqueue and park goroutine<br><br>**Receiver** `<- ch`<br>1. If buffer has data `qcount > 0`:<br>- copy out from `buf`<br>- if any sender waiting in `sendq`, move sender's value into `buf` and wake it<br><br>2. Else: same as unbuffered channels |
## Concurrency Patterns

| **Pattern name**                      | **Typical use case (real world)**                                                             | **Mechanism (how it works)**                                                                             |
| ------------------------------------- | --------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------- |
| **Handshake (Rendezvous)**            | Deployment orchestration, request–response between goroutines, ownership transfer (socket/FD) | **Unbuffered channel**; sender and receiver block until both are ready, enforcing strict synchronization |
| **Event Loop (Single Owner)**         | Kubernetes controllers, schedulers, game loops, state machines                                | **Unbuffered or small buffered channel** feeding a single goroutine that owns and mutates shared state   |
| **Broadcast Signal**                  | Graceful shutdown, leader election loss, config reload                                        | **Unbuffered channel + close**; closing the channel wakes all receivers simultaneously                   |
| **Worker Pool (Bounded Parallelism)** | Health checks, crawlers, rate-limited APIs                                                    | **Buffered job channel + fixed number of workers**; workers cap concurrency, buffer absorbs bursts       |
| **Producer–Consumer**                 | Log ingestion, metrics pipelines, async writes                                                | **Buffered channel**; decouples producer speed from consumer speed, buffer encodes backpressure          |
| **Pipeline (Stage-based)**            | ETL, video transcoding, streaming data processing                                             | **Buffered channels between stages**; each stage runs concurrently and passes results downstream         |
| **Fork–Join**                         | CI test execution, batch processing, parallel computation                                     | **Goroutines + WaitGroup or channels**; fork work in parallel, join at a barrier before proceeding       |
| **Semaphore (Concurrency Limiting)**  | API throttling, DB connection caps                                                            | **Buffered channel used as token bucket**; acquire token before work, release after completion           |

**Handshake**
```go
ready := make(chan struct{})

go func() {
	prepare()
	close(ready)
}()

<-ready // blocks
usePreparedState()
```

**Event Loop**
```go
events := make(chan Event)

go func() {
	for e := range events {
		handle(e)
	}
}()
```

**Broadcast**
```go
stop := make(chan struct{})

go worker(stop)
go worker(stop)

close(stop)
```

**Worker Pool**
```go
jobs := make(chan Job, 50)

for i := 0; i < workers; i++ {
	go func() {
		for j := range jobs {
			process(j)
		}
	}()
}
```

**Pipeline**
```go
func stage1(in <-chan A) <-chan B
func stage2(in <-chan B) <-chan C
```

**Fork-Join**
```go
var wg sync.WaitGroup
wg.Add(n)

for _, t := range tasks {
	go func(t Task) {
		defer wg.Done()
		work(t)
	}(t)
}

wg.Wait()
```

