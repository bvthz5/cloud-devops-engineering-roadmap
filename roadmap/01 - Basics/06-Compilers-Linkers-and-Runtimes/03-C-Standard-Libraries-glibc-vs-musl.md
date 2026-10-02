# 03 — C Standard Libraries: glibc vs musl libc

Every Linux program relies on a standard C library implementation (`libc`) to make kernel system calls (`open`, `read`, `fork`, `socket`). The two dominant implementations are **glibc** and **musl**.

---

## 1. glibc vs musl Architecture

- **glibc (GNU C Library):**
  - Used by: Ubuntu, Debian, RHEL, CentOS, Rocky Linux, Fedora, Arch.
  - Characteristics: Feature-complete, high performance, large footprint (~2.5MB), complex codebase.
- **musl libc:**
  - Used by: **Alpine Linux** (the popular base for lightweight Docker containers).
  - Characteristics: Extremely small footprint (~600KB), clean, standards-compliant, static-linking friendly.

---

## 2. The Classic Alpine Docker Gotcha: `sh: ./app: not found`

You compile a Go or C++ application on Ubuntu and copy it into an Alpine Linux container:
```dockerfile
FROM alpine:3.19
COPY ./myapp /usr/local/bin/myapp
CMD ["/usr/local/bin/myapp"]
```
When running the container, it crashes with:
```text
standard_init_linux.go:228: exec user process caused: no such file or directory
# Or:
sh: /usr/local/bin/myapp: not found
```

### Why does this happen?
The binary was dynamically linked against the **glibc dynamic linker** (`/lib64/ld-linux-x86-64.so.2`).
Alpine Linux uses musl, which provides `/lib/ld-musl-x86_64.so.1`.
Because the path specified in the binary's ELF header does not exist on Alpine, the Linux kernel returns `ENOENT` (No such file or directory)!

### How to Fix:
1. **Option A (Best for Go):** Build a 100% static binary without CGO:
   ```bash
   CGO_ENABLED=0 GOOS=linux go build -a -installsuffix cgo -o myapp .
   ```
2. **Option B:** Build on an Alpine build image with musl:
   ```dockerfile
   FROM golang:1.22-alpine AS builder ...
   ```
3. **Option C:** Use a distroless or Debian slim base image (`FROM gcr.io/distroless/base-debian12`).

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [02 - Static vs Dynamic Linking](./02-Static-vs-Dynamic-Linking-and-Shared-Libraries.md) | [README](./README.md) | [04 - Process Memory Layout](./04-Process-Memory-Layout-Heap-Stack-and-Segments.md) |
