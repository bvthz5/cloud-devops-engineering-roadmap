# 02 - Linux Capabilities and Least Privilege: What, About & Working

> **Concept:** Deconstructing monolithic root privileges into granular permission bits to enforce the Principle of Least Privilege in production containers.

---

## 🛡️ 1. What Are Linux Capabilities?

In traditional Unix/Linux systems, access control was strictly binary:
- **Unprivileged Process (UID > 0):** Standard users cannot perform system administration tasks.
- **Privileged Process (`root` / UID 0):** Possesses absolute, unrestricted power over the entire kernel, hardware, filesystems, and networking.

**Linux Capabilities** (introduced in Linux 2.2) break the all-powerful `root` authority into **~40 discrete, independent permission flags**. Instead of granting a process full root privileges just to open a privileged network port, the kernel grants only the specific capability (`CAP_NET_BIND_SERVICE`) required.

```text
TRADITIONAL MONOLITHIC ROOT (All-or-Nothing Danger):
[ Process runs as UID 0 ] ──► Can reboot system, format drives, sniff traffic, modify memory! 💥

LINUX CAPABILITIES (Granular Least Privilege):
                        ┌──► CAP_NET_BIND_SERVICE (Bind to port 80/443)
                        ├──► CAP_CHOWN (Change file ownership)
[ Root Privileges ] ────┼──► CAP_SYS_ADMIN (Mount disks, configure cgroups)
                        ├──► CAP_SYS_PTRACE (Debug & trace other processes)
                        └──► CAP_NET_ADMIN (Modify firewall & network cards)
```

---

## 📜 2. About: Why Capabilities Matter in Container Security

By default, Docker does **NOT** grant all capabilities to containers. Out of ~40 Linux capabilities, Docker drops over 25 dangerous ones and retains only a default whitelist of **14 safe capabilities**:

### The Default Retained Capabilities:
| Capability | What It Allows | Why It Is Safe / Necessary |
| :--- | :--- | :--- |
| **`CAP_CHOWN`** | Change file ownership (`chown`). | Package managers (`apt`, `apk`) setting up software files. |
| **`CAP_DAC_OVERRIDE`** | Bypass file read/write/execute permission checks. | Root process editing configuration files in container. |
| **`CAP_FOWNER`** | Bypass permission checks on operations requiring file ownership. | Modifying file attributes and permissions. |
| **`CAP_FSETID`** | Set SUID and SGID bits on executable files. | Necessary for utilities like `passwd` and `sudo`. |
| **`CAP_KILL`** | Send signals (`SIGKILL`, `SIGTERM`) to other processes. | Process supervisors restarting worker processes. |
| **`CAP_SETGID`** | Change process group ID. | Switching from root to unprivileged app groups. |
| **`CAP_SETUID`** | Change process user ID. | Switching from root to unprivileged app user (`su`, `USER`). |
| **`CAP_SETPCAP`** | Modify process capabilities. | Transferring permitted capabilities to child processes. |
| **`CAP_NET_BIND_SERVICE`** | **Bind to privileged network ports below 1024.** | Running web servers on port 80, 443, or DNS on 53. |
| **`CAP_NET_RAW`** | Construct raw network packets. | Allows running `ping` (ICMP echo requests). |
| **`CAP_SYS_CHROOT`** | Use the `chroot()` system call. | Sandboxing internal directory trees. |
| **`CAP_MKNOD`** | Create special device files (`mknod`). | Creating pipes and character devices in `/dev`. |
| **`CAP_AUDIT_WRITE`** | Write records to kernel audit log. | PAM authentication and audit trail logging. |
| **`CAP_SETFCAP`** | Set file capabilities on disk. | Hardening binaries with extended attributes. |

---

## 🚨 3. Dangerous Capabilities That Docker Drops by Default

The following capabilities are **explicitly blocked** by default because they easily allow **Container Escape** to the host:
- **`CAP_SYS_ADMIN` (The "New Root"):** Allows mounting filesystems, creating cgroups, loading eBPF programs, and escaping namespaces. Never grant this!
- **`CAP_SYS_PTRACE`:** Allows debugging and memory injection into other processes via `ptrace()`.
- **`CAP_SYS_MODULE`:** Allows loading and unloading kernel modules directly into the host Linux kernel!
- **`CAP_SYS_RAWIO`:** Allows raw access to physical memory (`/dev/mem`) and hardware ports.

---

## ⚡ 4. Working: Production Hardening via `--cap-drop` and `--cap-add`

The gold standard for zero-trust container security is the **Drop-All Whitelist Strategy**:
1. Drop **EVERY SINGLE** capability (`--cap-drop=ALL`).
2. Add back **ONLY** the precise capability your application requires!

### 4.1 Docker CLI Example:
```bash
# Run hardened Nginx: drops all capabilities, whitelists ONLY port binding
docker run -d   --name hardened_web   --cap-drop=ALL   --cap-add=NET_BIND_SERVICE   -p 80:80   nginx:alpine
```

### 4.2 Docker Compose Example:
```yaml
version: "3.8"
services:
  web:
    image: nginx:alpine
    ports:
      - "80:80"
    cap_drop:
      - ALL
    cap_add:
      - NET_BIND_SERVICE
    restart: always
```

### 4.3 Kubernetes SecurityContext Example:
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: secure-web
spec:
  containers:
    - name: nginx
      image: nginx:alpine
      securityContext:
        capabilities:
          drop:
            - ALL
          add:
            - NET_BIND_SERVICE
```

---

## 🔍 5. Inspecting Capabilities Inside a Running Container

DevOps engineers can verify active process capabilities using the `libcap` utility tools:

```bash
# 1. Inspect running container with capsh
docker run --rm --cap-drop=ALL --cap-add=NET_BIND_SERVICE alpine:latest   sh -c "apk add --no-cache libcap && capsh --print"
```

### Example Output:
```text
Current: = cap_net_bind_service+eip
Bounding set =cap_net_bind_service
Securebits: 00/0x0/1'b0
```
Notice that the bounding set contains **ONLY** `cap_net_bind_service`! Even if an attacker compromises the process, they cannot execute `chown`, cannot create raw sockets, and cannot escalate privileges.

---

## ❓ 6. Quick Revision & Interview Questions

1. **What are Linux capabilities?**
   - *Answer:* Distinct, granular privilege units that divide the traditional monolithic power of root into ~40 independent flags (e.g. `CAP_NET_BIND_SERVICE`, `CAP_CHOWN`).
2. **What does `--cap-drop=ALL` do?**
   - *Answer:* Strips all default Linux capabilities from the container, minimizing the attack surface and enforcing the Principle of Least Privilege.
3. **Why is `CAP_SYS_ADMIN` dangerous in a container?**
   - *Answer:* It provides near-total kernel authority, including mounting filesystems, manipulating namespaces, and accessing host kernel data structures, making container escapes trivial.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 01 - Container Threat Modeling and Escape Vectors](./01-Container-Threat-Modeling-and-Escape-Vectors.md) | [Index](../../../README.md) | [03 - Seccomp and AppArmor LSM Profiles →](./03-Seccomp-and-AppArmor-LSM-Profiles.md) |
