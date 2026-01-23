### What's in Process Memory (`mm_struct`)

![[Diagram 3.svg]]

#### Stack frame (per function call)

![[Diagram 2.svg]]

| Item                 | Why                                       |
| -------------------- | ----------------------------------------- |
| Local Variables      | Function’s temp data                      |
| Parameters           | Inputs to function                        |
| Saved regs           | Preserve caller state                     |
| Stack Pointer (SP)   | Points to the current top of the stack    |
| Frame Pointer (FP)   | Points to base of current stack frame     |
| Program Counter (PC) | Points to the next instruction to execute |
#### Executing function calls

| On executing function call                                                                                                                                                                                         | On returning from function call                                                                                                                                         |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Caller:<br>- Pass arguments to registers<br>- Saves return PC to stack<br>- Transfer controller to callee<br><br>Callee:<br>- Saves caller’s old FP, SP<br>- Sets its own FP<br>- Moves SP "up" to allocate locals | Callee:<br>- Set return value to register<br>- Restore saved SP, FP<br>- Jump to return PC<br>- Transfer control to caller<br><br>Caller:<br>- Read registers, continue |
