### What the TLB actually is

The **TLB (Translation Lookaside Buffer)** is a per-CPU cache of:
`(Virtual Page Number, ASID) → Physical Frame + permissions`

Key properties:
- It **bypasses page tables**
- The CPU **trusts it blindly**

If the TLB is wrong, the CPU will:
- Allow writes that should fault
- Map to wrong physical memory
 ![[Diagram.svg]]

**Refcount** is a **kernel-maintained counter** attached to a **physical memory page** that records **how many page-table entries (across all processes) currently map to that same physical page**.
- Stored in struct page[] allocated on boot

