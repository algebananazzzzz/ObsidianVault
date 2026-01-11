Imagine a school where the **CPU** is like the **Teacher**. All the student are extremely lazy, they will sleep if the teacher is not teaching them. There can be **multiple teachers (CPU cores)**.
# Process
Creating a **Process** is like opening a **new class**. 

| Analogy                                                        | Mapping                                                                                                                                             |
| ----------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------- |
| **High Setup Cost:** setup lesson plan, pr                     | The kernel has to create a new `task_struct`, a new `mm_struct` (virtual memory map) and a new `files_struct` (file descriptors: files and net **Independence and Security:** classes does not affect e.o. ct  ct  ct  | Unique address space: one process cannot override another process's data                                                                            |

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