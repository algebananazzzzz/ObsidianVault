Imagine a 
# Process
**Process = Cubicle**

| Analogy                                                 | Mapping                                                                                                                          | Significance                                                      |
| ------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------- |
| Each cubicle has its own dedicated toilet, toilet paper | The kernel has to create a new `task_struct`, a new `mm_struct` (virtual memory map) and a new `files_struct` (file descriptors) | **Independence and Security:** processes do not affect each other |
**Problem:** when the next person goes into the toilet, he needs to 
# Thread
A **Thread** is a **Table** inside that class.

| Analogy                                                                  | Mapping                                                                                      |
| ------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------- |
| **Sharing Class:** teacher no need to memorize lesson plan again         | Lower context switching cost as `mm_struct` (memory map) remains unchanged (no flushing TLB) |
| **Independent Stack:** progress on one table is independent from another | Each thread has its own private stack i.e. `thread_struct`                                   |
| **Risk:** students share the same whiteboard                             | Threads share memory, need to resolve race conditions.                                       |

# Coroutine
A **Coroutine** are **Students** on a **Table**

| Analogy                                                                 | Mapping                                                    |
| ----------------------------------------------------------------------- | ---------------------------------------------------------- |
| **Teacher decides:** teacher decides how students interact on the table | Managed on the **User Space**, language specific generally |
e.g. **Golang** uses the **GPM** model
- **G:** Goroutine (Coroutine)
- **P:** Processor (scheduling context with a LRQ)
- **M:** Machine (actual OS thread)