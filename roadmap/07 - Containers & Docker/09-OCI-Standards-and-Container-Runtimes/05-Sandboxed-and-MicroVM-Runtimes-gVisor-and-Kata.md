# 05 - Sandboxed and MicroVM Runtimes: gVisor and Kata

## 1. Strong Isolation for Untrusted Code

In multi-tenant cloud environments (e.g. executing untrusted user-submitted code in SaaS platforms), standard containers do not provide strong enough security boundaries because they share the host Linux kernel.

```text
Standard Container (runc):
[ Untrusted App ] ──► [ Shared Host Linux Kernel ] (Kernel exploit = Host Takeover!)

gVisor (runsc - Application Kernel in User Space):
[ Untrusted App ] ──► [ Sentry (Go Kernel Interceptor) ] ──► (Filters 300+ Syscalls) ──► [ Host Kernel ]

Kata Containers (Hardware MicroVM):
[ Untrusted App ] ──► [ Dedicated Guest Kernel ] ──► [ QEMU / Cloud-Hypervisor (VT-x) ] ──► [ Host Kernel ]
```

---

## 2. Choosing Between gVisor and Kata
- **gVisor (`runsc`)**: Fast startup (<100ms), low memory overhead, ideal for web services and untrusted CI jobs.
- **Kata Containers**: True hardware virtualization (runs real guest Linux kernel in lightweight QEMU/Firecracker microVM). Highest security isolation, supports custom kernel modules, higher memory overhead.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 04 - Low Level Runtimes runc vs crun vs youki](./04-Low-Level-Runtimes-runc-vs-crun-vs-youki.md) | [Index](../../../README.md) | [06 - The Dockershim Deprecation Story →](./06-The-Dockershim-Deprecation-Story.md) |
