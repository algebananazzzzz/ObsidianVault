### Fork
Duplicates the address-space structure. Useful when child is cooperating with parent / relies upon parent data

**Workflow:**
Userspace: `fork()` syscall
Kernel:
`do_fork()` → allocate child `task_struct`, `pid`, set `ppid`
→ copy `thread_struct`, `files_struct` etc.
→ copy `mm_struct (VMAs)`, `page tables`
→ mark child and parent's `PTEs` as `read-only`, `COW`
→ flush parent `TLB` as `perms` changed
→ place child on `runqueue`, return `pid` (parent) / `0` (child)

On write (parent / child):
write to `RO` page → `page fault` →  CPU trap to kernel mode
→ kernel sees: `COW` and `refcount > 1`
→ allocate new `physical page`, copy data from old page
→ update `PTEs` to new pages, `read-write`, `refcount - 1`
→ flush process `TLB`

Next write (parent / child):
write to `RO` page → `page fault` →  CPU trap to kernel mode
→ kernel sees: `COW` and `refcount = 1` → need not copy
→ update `PTEs` to `read-write`
→ flush process `TLB`

### Execute
Destroys the address space and builds a new one

**Workflow:**
Userspace: `exec()` syscall
Kernel:
`execve()` → retain child `task_struct`, `pid`
→ allocate new `mm_struct (VMAs)`, detach old `mm_struct`
→ unmap old `VMAs`, walk and `refcount - 1` for `PTEs`
→ free old `pages` to allocator, install new `mm_struct`
→ `page tables` created on demand, populate `user stack`

### Exit
Destroys the process’s execution context and releases all resources

**Workflow:**
Userspace: `exit(status)`
Kernel:
`exit()` → detach and destroy `mm_struct`
→ close all open `file descriptors`, destroy `FD table`
→ if `refcount = 0` flush buffers, release inode / socket

Parent exited / no parent process:
Process has parent / is reparented to `PID 1` (init)
→ init always calls `wait()`

Zombie (parent process):
after resources are freed → process enters `ZOMBIE`
→ kernel sends `SIGCHILD` to parent
→ parent may ignore / call `wait()`
→ if `wait()`, kernel copies exit status to parent, release `task_struct`
→ frees `PID`, remove process from `PCB`