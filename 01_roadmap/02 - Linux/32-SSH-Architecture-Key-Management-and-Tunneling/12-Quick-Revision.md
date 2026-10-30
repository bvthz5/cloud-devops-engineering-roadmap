# 12 — Quick Revision Cheat Sheet: SSH Architecture

---

## 1. Command Reference

```bash
# Key Generation
ssh-keygen -t ed25519 -a 100 -C "email"

# Tunneling Flags
ssh -N -L 8080:10.0.1.5:80 user@bastion    # Local port forwarding
ssh -N -R 9000:localhost:3000 user@server  # Remote port forwarding
ssh -N -D 1080 user@proxy                  # SOCKS5 proxy

# Bastion Jump
ssh -J user@bastion user@internal-host
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 11 - MCQ](./11-MCQ.md) | [Index](../../../README.md) | [Next Module (33-Linux-Boot-Troubleshooting-and-Rescue-Mode) →](../33-Linux-Boot-Troubleshooting-and-Rescue-Mode/01-Linux-Boot-Process-Deep-Dive-UEFI-GRUB2-Initrd.md) |
