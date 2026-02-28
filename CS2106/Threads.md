## Why Threads Exist

Processes are expensive because:

- Creating a process duplicates memory and resources (`fork()` is costly)
- Process context switch is heavy (must switch memory, registers, OS state)
- Processes cannot share memory easily → need IPC (slow and complex)

**Threads solve this by allowing multiple execution flows inside ONE process.**