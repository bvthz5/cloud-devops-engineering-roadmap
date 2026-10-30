# 06 - Container Security Boundaries and Kernel Surface

## 1. Virtual Machines vs Containers: Attack Surface

```text
Virtual Machine:
[ Application ] ──► [ Guest OS Kernel ] ──► [ Virtual Hardware ] ──► [ Hypervisor ] ──► [ Host Kernel ]
* Full hardware virtualization. Breaching VM requires hypervisor escape (Extremely rare).

Container:
[ Application ] ──────────────────────────────────────────────────► [ Host Linux Kernel ]
* Shared Kernel! Any kernel vulnerability (e.g. Dirty COW, eBPF bugs) allows host takeover!
```

---

## 2. The Danger of `--privileged`

Running a container with `docker run --privileged`:
1. Disables all Seccomp filter blocks (allows all 450+ Linux syscalls).
2. Disables AppArmor and SELinux enforcement.
3. Grants all Linux capabilities (`CAP_SYS_ADMIN`, `CAP_NET_ADMIN`, `CAP_SYS_RAWIO`).
4. Mounts the host's `/dev` filesystem into the container!

```bash
# How an attacker takes over the host from a privileged container in 10 seconds:
fdisk -l                       # Lists host hard drives
mkdir /host
mount /dev/sda1 /host          # Mounts host root disk directly into container!
chroot /host                   # You are now root on the host machine!
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 05 - Building a Container from Scratch in Bash](./05-Building-a-Container-from-Scratch-in-Bash.md) | [Index](../../../README.md) | [07 - Real World Scenarios →](./07-Real-World-Scenarios.md) |
