# 09 — Compilers & Runtimes Interview Q&A

10 technical interview questions for DevOps, SRE, and Infrastructure roles.

---

### Q1: What is the difference between the Stack and the Heap in process memory?
**Answer:**
- **Stack:** Used for automatic storage of function call frames, local variables, and return addresses. Allocation and deallocation are instantaneous because the CPU simply moves the stack pointer register (`RSP`). Stack memory is thread-private and strictly bounded in size (usually 8MB limit).
- **Heap:** Used for dynamic memory allocations requested at runtime (`malloc()`, `new`). Heap memory must be manually freed or managed by a runtime Garbage Collector. It is shared across all threads in a process and can grow up to the physical memory/swap limits, but is prone to memory leaks and fragmentation.

---

### Q2: Why does a binary compiled on Ubuntu often fail to run inside an Alpine Linux container?
**Answer:**
Ubuntu uses **glibc**, whereas Alpine Linux uses **musl libc**. If the binary was dynamically linked, its ELF header contains a hardcoded path to the glibc dynamic linker (`/lib64/ld-linux-x86-64.so.2`). Since Alpine only provides the musl dynamic linker (`/lib/ld-musl-x86_64.so.1`), the Linux kernel cannot locate the required interpreter, producing the deceptive `no such file or directory` error.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 08 - Troubleshooting](./08-Troubleshooting.md) | [Index](../../../README.md) | [10 - Hands On Practice →](./10-Hands-On-Practice.md) |
