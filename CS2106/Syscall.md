### What it is
- A controlled way for a user program to request services from the OS kernel
- Only safe path from user mode to privileged kernel mode
### Purpose
- Kernel is shared by all processes
- Kernel controls memory, CPU, files, devices

**If kernel not protected:**
- A program could overwrite kernel data
- A program could read other processes’ memory
- A program could control hardware directly
- A program could crash the whole system
### When it is used
- File and device I/O (read, write, open, close)
- Process/thread control (fork, exec, exit, wait)
- Memory management (mmap, brk)
- Networking (send, recv, connect)
### Flow
1. User Space
	- User program calls a library function (e.g., `read()`, `getpid()`).    
	- Library wrapper:
	    - Puts syscall number in a register.
	    - Puts arguments in registers/stack.
2. Entering kernel
	- Executes special instruction (`syscall`, `trap`, `svc`).
    - CPU switches from user mode → kernel mode.
    - Saves minimal user state (return PC, CPU flags).
    - Jumps to kernel entry point.
3. Kernel dispatch
	   - Kernel saves full register state to kernel stack. **(expensive)**
    - Uses syscall number to find handler.
4. Syscall handler:
    - Does the requested work.
    - Sets return value or error code.
5. Leaving kernel
	- Kernel restores saved registers. **(expensive)**
	- Executes “return from trap”.
    - CPU switches from kernel mode → user mode.
6. User Space
	   - User program continues with return value.