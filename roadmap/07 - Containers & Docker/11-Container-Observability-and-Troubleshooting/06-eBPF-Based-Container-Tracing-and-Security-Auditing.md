# 06 - eBPF-Based Container Tracing and Security Auditing

## 1. Zero-Overhead Tracing with eBPF

Extended Berkeley Packet Filter (**eBPF**) allows running sandboxed programs inside the Linux kernel without changing kernel source code or loading kernel modules.

In container environments, tools like **Falco** and **BCC** use eBPF to monitor container syscalls in real time:
- Detect unauthorized interactive shells spawned inside containers (`execve`).
- Detect sensitive file access (`/etc/shadow`).
- Trace DNS queries and outbound TCP sockets per container without injecting sidecars.

```bash
# Trace all file opens made by a specific container using BCC opensnoop
sudo opensnoop-bpfcc -c my_container_name
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [05 - Live Debugging with nsenter](./05-Live-Debugging-with-Ephemeral-Containers-and-nsenter.md) | [README](./README.md) | [07 - Real-World Scenarios](./07-Real-World-Scenarios.md) |
