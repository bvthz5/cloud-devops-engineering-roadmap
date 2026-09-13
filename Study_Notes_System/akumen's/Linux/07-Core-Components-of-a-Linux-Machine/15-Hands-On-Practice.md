# 15 - Hands-On Practice & Terminal Labs: Core Components

Practical terminal labs for inspecting system libraries, shell built-ins, and hardware drivers.

---

## 🧪 Lab 1: Differentiating Shell Built-ins vs External Utilities

1. Open your terminal and run `type` on various commands:
   ```bash
   type cd
   type echo
   type ls
   type grep
   type pwd
   ```
2. Verify which commands report `shell builtin` vs `aliased to` vs `/usr/bin/file`.
3. Locate the physical disk path of external binaries:
   ```bash
   which ls grep cat python3
   ```

---

## 🧪 Lab 2: Inspecting Shared Library Dependencies

1. Use `ldd` to inspect the shared libraries required by `cat`:
   ```bash
   ldd /usr/bin/cat
   ```
2. Check the dynamic linker path (`ld-linux-x86-64.so.2`) and `libc.so.6`.
3. Try running `ldd` on a statically linked binary (if available, e.g., Docker CLI or Go binary):
   ```bash
   ldd /usr/bin/docker
   ```

---

## 🧪 Lab 3: System Call Tracing with `strace`

1. Trace system calls executed by `echo "Hello Linux"`:
   ```bash
   strace echo "Hello Linux"
   ```
2. Identify system calls `write()`, `mmap()`, and `execve()`.
3. Filter `strace` to display file open calls only:
   ```bash
   strace -e trace=openat cat /etc/issue
   ```
---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [14 - Interview QA](./14-Interview-QA.md) | [README](./README.md) | [16 - MCQ](./16-MCQ.md) |
