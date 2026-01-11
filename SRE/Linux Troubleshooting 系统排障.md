# Gather signals


# Linux commands

1. From `vmstat`, we can immediately determine the rough constraints before drilling down
   
   在操作系统里，我会先用 `vmstat` 来快速判断乃一个资源被限制，其次再进一步深入排查。
   `vmstat` command 非常好用...同时看到 CPU、内存、I/O、进程的状态，非常核心的工具

| procs            | memory                    | swap        | io            | system                    | cpu                                      |
| ---------------- | ------------------------- | ----------- | ------------- | ------------------------- | ---------------------------------------- |
| r  b             | swpd   free   buff  cache | si   so     | bi    bo      | in cs                     | us sy id wa st                           |
| running, blocked | swap, free, buffer, cache | swap in/out | blocks in/out | interrupt, context switch | user / system CPU, idle, I/O wait, steal |
**Common Signals:**

| Constraint                         | Signals                                                              | Next Step                                                                                                                                                                   |
| ---------------------------------- | -------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| CPU bound (healthy)                | high `us`, low `sy`, `wa = 0`                                        | `top` (determine hot ps)                                                                                                                                                    |
| Excessive context switching        | `r` > CPU cores<br>high `cs` and `sy`                                | `mpstat -P ALL 1` (confirm)<br>`top -H` (determine hot ps)<br>`pidstat -w`<br>high nvcswch/s:<br>threads forced off CPU<br>high cswch/s: generally ok<br>mutex (e.g. await) |
| I/O wait stall (network / storage) | `b` rising<br>high, constant `wa`<br>low `us` (not used)             | `iostat -x 1` (%util > 60)<br>`sar -n DEV 1` (rx/txdrops)                                                                                                                   |
| Memory thrashing (swap)            | constantly high `si` and `so`<br>high `wa`, low `us` (I/O stall)<br> | `free -h` (available)<br>`ps --sort=-rss`<br>(high resident set size)                                                                                                       |




