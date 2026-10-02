# 03 - Seccomp and AppArmor LSM Profiles

## 1. Seccomp (Secure Computing Mode)

The Linux kernel exposes ~450 system calls (syscalls). Most standard web applications only require ~50 syscalls. Unnecessary syscalls (like `reboot`, `kexec_load`, `ptrace`) increase the kernel attack surface.

Docker applies a default **Seccomp Profile** that blocks ~44 dangerous system calls out of the box:

```bash
# Explicitly apply a custom strict Seccomp JSON filter
docker run --security-opt seccomp=/etc/docker/strict-profile.json myapp
```

---

## 2. AppArmor Mandatory Access Control

AppArmor restricts what files and capabilities a program can access. Docker automatically applies the default `docker-default` profile:

```bash
# Verify container is running under AppArmor protection
docker inspect --format '{{.AppArmorProfile}}' myapp
# Output: docker-default
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 02 - Linux Capabilities and Least Privilege](./02-Linux-Capabilities-and-Least-Privilege.md) | [Index](../../../README.md) | [04 - Rootless Docker Architecture →](./04-Rootless-Docker-Architecture.md) |
