## Why Threads Exist

Processes are expensive because:

- Creating a process duplicates memory and resources (`fork()` is costly)
- Process context switch is heavy (must switch memory, registers, OS state)
- Processes cannot share memory easily → need IPC (slow and complex)

**Threads solve this by allowing multiple execution flows inside ONE process.**

|      | Kernel Thread                                                                                      | User Thread                                                                                                                   |
| ---- | -------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| Pros | Kernel can schedule threads simultaneously on multiple PUs                                         | Just library calls  → runs on any kernel, lightweight, flexible (custom scheduling policy)                                    |
| Cons | Thread operations are syscalls → slower, resource intensive<br>Less flexible: controlled by kernel | Kernel not aware → one thread `syscall` and blocks → process blocks → all threads blocked<br>Process scheduled on one PU only |


![[CS2106/assets/Diagram 1.svg]]

**Simultaneous multi-threading**
It makes **1 physical core appear as 2 logical cores** to run multiple threads in **parallel**.
- CPU duplicates **thread_struct** (registers, program counter, thread state)
- But shares **hardware** (ALU, cache)
- **Without hyperthreading:** compute → wait memory → compute → wait
- **With hyperthreading:** compute a → compute b while waiting memory a

