# 05 - System Libraries & `glibc`

System Libraries sit between user utilities/applications and the raw kernel system call interface. They provide pre-compiled, reusable functions that abstract low-level OS operations.

---

## 📚 The GNU C Library (`glibc`)

The most important system library in GNU/Linux systems is **`glibc`** (GNU C Library, `libc.so.6`).

### Role of `glibc`:
1. **System Call Wrapper:** Wrapping raw assembly kernel `syscall` invocations into standard C function calls like `printf()`, `malloc()`, `open()`, and `read()`.
2. **POSIX Standard Implementation:** Ensuring applications follow POSIX standards across different hardware architectures.
3. **Utility Functions:** String manipulation (`strcpy`), math (`math.h`), networking (`getaddrinfo`), threading (`pthread`).

---

## 🔗 Static vs. Dynamic Linking

When software binaries are compiled in Linux, they resolve library functions in one of two ways:

```text
Static Linking:
  [ Application Source Code ] + [ Static Library libx.a ] ──(gcc)──> Self-contained Large Binary

Dynamic Linking:
  [ Application Source Code ] ──(gcc)──> Small Binary file ──(Runtime ld.so)──> Shared Library libc.so.6
```

| Property | Static Linking (`.a` files) | Dynamic Linking (`.so` Shared Objects) |
| :--- | :--- | :--- |
| **Runtime Dependencies** | None. All library code is embedded inside the binary executable file. | Requires `.so` shared libraries to exist on host system. |
| **Binary Size** | Large file size. | Compact file size. |
| **RAM Utilization** | High. Multiple running instances duplicate library memory. | Low. RAM shares a single read-only `.so` memory page across process instances. |
| **Security Updates** | Recompiling every executable is required when a library bug is patched. | Updating the `.so` file patches all dependent applications instantly. |

---

## 🛠️ Inspecting Shared Library Dependencies (`ldd`)

```bash
# Check shared library dependencies of the /bin/ls executable
ldd /bin/ls

# Output example:
#   linux-vdso.so.1 (0x00007ffe...)
#   libselinux.so.1 => /lib/x86_64-linux-gnu/libselinux.so.1
#   libc.so.6 => /lib/x86_64-linux-gnu/libc.so.6 (0x00007f...)
```

---

## ⬅️ Navigation
- Previous: [04 - Device Drivers](file:///c:/Users/binil/OneDrive/Desktop/cloud-devops-engineering-roadmap/Study_Notes_System/akumen's/Linux/07-Core-Components-of-a-Linux-Machine/04-Device-Drivers.md)
- Next: [06 - System Utilities & Binaries](file:///c:/Users/binil/OneDrive/Desktop/cloud-devops-engineering-roadmap/Study_Notes_System/akumen's/Linux/07-Core-Components-of-a-Linux-Machine/06-System-Utilities-and-Binaries.md)
