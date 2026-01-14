
| Aspect               | Virtual Machines (VMs)                                                          | Containers (Docker)                                                   |
| -------------------- | ------------------------------------------------------------------------------- | --------------------------------------------------------------------- |
| Virtualization level | Hypervisor virtualizes kernel<br>VMs dont share kernel<br>Ring 0 (VMX non-Root) | OS-level (kernel shared)<br>Container shares kernel<br>Ring 3 in host |
| Startup time         | Slow (seconds–minutes)<br>- setup virtual pages                                 | Fast (ms–seconds)<br>- cgroups easy to setup                          |
| OS flexibility       | Any OS per VM, match arch                                                       | Must match host kernel                                                |
| Typical tools        | VMware, KVM, Hyper-V                                                            | Docker, containerd                                                    |

