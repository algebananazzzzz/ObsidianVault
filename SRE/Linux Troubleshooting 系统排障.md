# Gather signals
- 

# Linux commands

1. From `vmstat`, we can immediately determine the rough constraints before drilling down

| procs            | memory                    | swap        | io            | system                    | cpu                                      |
| ---------------- | ------------------------- | ----------- | ------------- | ------------------------- | ---------------------------------------- |
| r  b             | swpd   free   buff  cache | si   so     | bi    bo      | in cs                     | us sy id wa st                           |
| running, blocked | swap, free, buffer, cache | swap in/out | blocks in/out | interrupt, context switch | user / system CPU, idle, I/O wait, steal |
**Common Signals:**

| Constraint                         | Signals                                                              | Next Step                                                                                                                                                                                                                                      |
| ---------------------------------- | -------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| CPU bound (healthy)                | high `us`, low `sy`, `wa = 0`                                        | `top` (determine hot ps)                                                                                                                                                                                                                       |
| Excessive context switching        | `r` > CPU cores<br>high `cs` and `sy`                                | `mpstat -P ALL 1` (confirm)<br>`top -H` (determine hot ps)<br>`pidstat -w -p <pid>`<br>**high nvcswch/s (non-voluntary):**<br>threads forced off CPU due to too many runnable threads<br>**high cswch/s:** generally ok, caused by app (mutex) |
| I/O wait stall (network / storage) | `b` rising<br>high, constant `wa`<br>low `us` (not used)             | **storage:** `iostat -x 1` (%util > 60)<br>**network:** `sar -n DEV 1` (rx/txdrops)                                                                                                                                                            |
| Memory thrashing (swap)            | constantly high `si` and `so`<br>high `wa`, low `us` (I/O stall)<br> | `free -h` (available)<br>`ps --sort=-rss`<br>(high resident set size)                                                                                                                                                                          |


| Command                                    | Use                                                                                                      | Experience                            |
| ------------------------------------------ | -------------------------------------------------------------------------------------------------------- | ------------------------------------- |
| ip link (L1)                               | Show network interface UP/DOWN<br>Debug "no network" after reboot <br>(restart with ip link set)         | Debug tailscale traffic on server     |
| ip neigh<br>(flush) (L2)                   | Kernel ARP table<br>Check REACHABLE                                                                      | Same network, server unreachable      |
| ip addr<br>(add/del)                       | Check server IP address and subnet                                                                       | Multiple IP across network interfaces |
| ip route<br>(add/del)                      | Check default routes and which interface is used                                                         | Route to VPC router in AWS            |
| ss -t / -u (L4)                            | Active TCP / UDP sockets<br>Verify services are listening with `nc -zv`<br>Use `nmap` to scan open ports |                                       |
| /etc/resolv.conf<br>nslookup<br>dig +trace |                                                                                                          |                                       |
