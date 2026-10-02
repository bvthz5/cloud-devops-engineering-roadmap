# 01 - Container Threat Modeling and Escape Vectors

## 1. The Shared Kernel Attack Surface

Containers do not virtualize the kernel; all containers share the single underlying Linux host kernel. An unpatched kernel vulnerability (e.g., Dirty COW, eBPF bugs) allows an attacker inside an unprivileged container to escalate privileges and take over the physical host:

```text
[ Container Process ] ──► (Executes malicious syscall: clone / bpf) ──► [ Vulnerable Host Kernel ]
                                                                                   │
[ Full Host Compromise & Adjacent Container Data Access ] ◄────────────────────────┘
```

---

## 2. Common Container Escape Vectors
1. **Privileged Mode (`--privileged`)**: Bypasses all namespaces and mounts `/dev` host devices.
2. **Mounted Docker Socket (`/var/run/docker.sock`)**: Grants root access to host daemon.
3. **Sensitive Host Mounts (`-v /:/host` or `-v /etc:/etc`)**: Modifying host cron jobs or `/etc/shadow`.
4. **Dangerous Capabilities (`CAP_SYS_ADMIN`, `CAP_SYS_PTRACE`)**: Allows attaching to host processes.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [README](./README.md) | [README](./README.md) | [02 - Linux Capabilities & Least Privilege](./02-Linux-Capabilities-and-Least-Privilege.md) |
