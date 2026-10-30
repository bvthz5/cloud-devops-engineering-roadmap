# 04 — Process Memory Layout: Heap, Stack, and Segments

When the Linux kernel loads an executable into memory, it assigns a private **Virtual Address Space** structured into distinct segments.

---

## 1. Process Virtual Address Space

```text
High Memory (0xFFFFFFFFFFFFFFFF)
   +---------------------------------------+
   | Kernel Space (Accessible via Syscalls)|
   +---------------------------------------+
   | Environment Variables & Command Args  |
   +---------------------------------------+
   | Stack (Grows Downward  | )           |
   |   - Function local variables          |
   |   - Return addresses & stack frames   |
   |   - Fast allocation / auto-deallocated|
   +---------------------------------------+
   |                  ↓                    |
   |                (Free Memory Space)    |
   |                  ↑                    |
   +---------------------------------------+
   | Heap (Grows Upward    | )             |
   |   - Dynamic memory (malloc, new)      |
   |   - Managed by runtime garbage coll.  |
   |   - Prone to memory leaks             |
   +---------------------------------------+
   | BSS Segment                           |
   |   - Uninitialized static/global vars  |
   +---------------------------------------+
   | Data Segment                          |
   |   - Initialized static/global vars    |
   +---------------------------------------+
   | Text / Code Segment (Read-Only)       |
   |   - Binary machine instructions       |
   +---------------------------------------+
Low Memory (0x0000000000000000)
```

---

## 2. Stack vs Heap Comparison

| Characteristic | Stack | Heap |
| :--- | :--- | :--- |
| **Allocation Speed**| Instantaneous (increments stack pointer register)| Slower (searches free-list or calls `brk`/`mmap`) |
| **Deallocation** | Automatic when function returns | Manual (`free()`) or via Garbage Collector |
| **Size Limit** | Small (typically 8MB per thread, set by `ulimit -s`)| Large (limited only by available system RAM/Swap) |
| **Common Errors** | **Stack Overflow** (infinite recursion) | **Memory Leak**, Out of Memory (OOM), fragmentation |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 03 - C Standard Libraries glibc vs musl](./03-C-Standard-Libraries-glibc-vs-musl.md) | [Index](../../../README.md) | [05 - Execution Models Compiled Interpreted and JIT →](./05-Execution-Models-Compiled-Interpreted-and-JIT.md) |
