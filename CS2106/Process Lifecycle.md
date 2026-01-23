### Fork
Duplicates the address-space structure

**Workflow:**
Userspace: `fork()` syscall
Kernel:
`do_fork()` → allocate child `task_struct`, `pid`
→ copy `thread_struct`, `files_struct` etc.
→ copy `mm_struct (VMAs)`, `page tables`
→ mark child and parent's `PTEs` as `read-only`, `COW`
→ flush parent `TLB` as `perms` changed
→ place child on `runqueue`

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