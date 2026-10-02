# 04 - Distroless, Alpine, and Scratch Base Images

## 1. Base Image Comparison

| Base Image | Size | C Library | Shell Included? | Package Manager? | Best For |
|---|---|---|:---:|:---:|---|
| **Ubuntu / Debian** | ~80 - 120 MB | `glibc` | Yes (`bash`) | Yes (`apt`) | Complex legacy apps, Python C-extensions. |
| **Alpine Linux** | ~7 MB | `musl libc` | Yes (`sh`) | Yes (`apk`) | General microservices. |
| **Google Distroless**| ~20 MB | `glibc` | **No** | **No** | High-security Node.js, Python, Java runtime. |
| **Scratch** | **0 MB** | None | **No** | **No** | Statically linked Go and Rust binaries. |

---

## 2. The Alpine `musl libc` DNS Gotcha

Alpine uses `musl libc` rather than standard GNU `glibc`.
- **Gotcha**: `musl` executes parallel DNS queries (A and AAAA simultaneously) and handles timeouts differently than `glibc`. Under heavy Kubernetes traffic, this can cause intermittent 5-second DNS lookup delays unless `options single-request` is configured in `/etc/resolv.conf`.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 03 - Multi Stage Builds Architecture](./03-Multi-Stage-Builds-Architecture.md) | [Index](../../../README.md) | [05 - Non Root Users and Least Privilege Execution →](./05-Non-Root-Users-and-Least-Privilege-Execution.md) |
