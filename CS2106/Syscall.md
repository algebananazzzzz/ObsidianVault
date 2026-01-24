### Why
- Enforce isolation between untrusted user code and trusted kernel
- Ensure hardware shared by all processes cannot be freely accessed by one process (e.g. modifying page tables)
### When it is used
- File and device I/O (read, write, open, close)
- Process/thread control (fork, exec, exit, wait)
- Memory management (mmap, brk)
- Networking (send, recv, connect)

#### Shared Kernel Mapping
- User memory and kernel memory share the same page tables
- Kernel memory is mapped into the upper half of the virtual address space
- User pages are in the lower half
### Flow
User Space:
- CPU is executing in `user mode (ring 3)`
	- `RSP` pointing to user stack
- User program calls a library function (e.g., `read()`)
	- Puts syscall number in `RAX` register.
	- Puts arguments in registers.
- Special `trap` instruction executed (`syscall` / `svc`)

Entering kernel:
- CPU switches from user mode → kernel mode.
- Saves minimal user state to registers (`RIP`, `RFLAGS`)
- Change `RIP` into kernel text, point `RSP` to kernel stack of thread
- Save user registers onto kernel stack → build trapframe `pt_regs` (**expensive**)

Kernel dispatch:
- Use syscall number to find handler to do requested work
- Accessing kernel memory works as each `PTE` has `U/S bit = Supervisor`
- Accessing user memory works (demand paged, kernel handles page fault)
- Sets return value / error code

Leaving kernel:
- Kernel restores saved registers from `pt_regs`
- `sysretq` executed, CPU switches from kernel mode → user mode


Legend

| Abbrev | Full                       |
| ------ | -------------------------- |
| RSP    | Register Stack Pointer     |
| RIP    | Return Instruction Pointer |
| U/S    | User / Supervisor          |
