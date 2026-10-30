# 03 - Seccomp and AppArmor LSM Profiles: What, About & Working

> **Concept:** System call filtering via Seccomp and Mandatory Access Control (MAC) via AppArmor and SELinux to harden the container execution sandbox.

---

## 🔒 1. What is Seccomp (Secure Computing Mode)?

**Seccomp** is a Linux kernel security facility that restricts which system calls (`syscalls`) a process can make. The Linux kernel supports over 350+ system calls, but standard microservice containers only need 40 to 50 of them. Seccomp acts as a firewall between your application and the Linux kernel.

```text
APPLICATION PROCESS
        │
        ├──► read()    ──► [ SECCOMP FILTER ] ──► ALLOWED  ──► Linux Kernel executes read()
        ├──► write()   ──► [ SECCOMP FILTER ] ──► ALLOWED  ──► Linux Kernel executes write()
        └──► reboot()  ──► [ SECCOMP FILTER ] ──► BLOCKED! ──► Returns -EPERM (Operation Not Permitted)
```

### Docker's Default Seccomp Profile:
Docker automatically applies a default seccomp profile that **blocks ~44 dangerous system calls**, including:
- `reboot`: Prevents containers from restarting the physical host.
- `sys_chroot`: Restricts directory jail manipulation.
- `bpf`: Blocks untrusted eBPF bytecode loading.
- `kexec_load`: Blocks loading new kernel images.

---

## 🛡️ 2. What is AppArmor?

**AppArmor** (Application Armor) is a Linux Security Module (LSM) that enforces **Mandatory Access Control (MAC)** policies on a per-program basis. While Unix permissions check *who* owns a file, AppArmor specifies *what actions* a specific program is allowed to perform, regardless of whether it runs as root!

### Docker's Default AppArmor Profile (`docker-default`):
Enforced automatically on Debian and Ubuntu hosts:
- Denies direct writes to `/proc/sys/` kernel parameters.
- Denies raw mounting of filesystems inside containers.
- Denies direct read/write access to host hardware devices in `/dev/`.

---

## ⚡ 3. Working: Applying Custom Seccomp & AppArmor Profiles

```bash
# 1. Run container with custom strict Seccomp JSON profile
docker run --security-opt seccomp=/etc/docker/strict-seccomp.json -d nginx:alpine

# 2. Run container with custom AppArmor profile
docker run --security-opt apparmor=my-custom-profile -d nginx:alpine

# 3. Disabling Seccomp (DANGEROUS: For debugging only!)
docker run --security-opt seccomp=unconfined alpine
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 02 - Linux Capabilities and Least Privilege](./02-Linux-Capabilities-and-Least-Privilege.md) | [Index](../../../README.md) | [04 - Rootless Docker Architecture →](./04-Rootless-Docker-Architecture.md) |
