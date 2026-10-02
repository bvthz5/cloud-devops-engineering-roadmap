# Module 06: SSH and Secure Remote Access

Secure Shell (SSH) is the standard cryptographic protocol for remote administration, automated deployment (Ansible), and network tunneling. Beyond basic terminal access, master SSH key algorithms (Ed25519 vs RSA), bastion jump hosts (`ProxyJump`), port forwarding (`-L`, `-R`, `-D`), and modern zero-trust SSH access.

---

## 🎯 Learning Objectives
By the end of this module, you will be able to:
- Dissect the **SSH-2 handshake, Diffie-Hellman exchange**, and host key verification.
- Generate and manage high-security **Ed25519** and FIDO2 hardware keys.
- Harden the OpenSSH server daemon (`/etc/ssh/sshd_config`).
- Configure `~/.ssh/config` for seamless **ProxyJump** through cloud bastion hosts.
- Create **Local (`-L`)**, **Remote (`-R`)**, and **Dynamic SOCKS5 (`-D`)** tunnels.
- Understand the evolution to **SSH Certificates** and zero-trust access tools (Teleport, Vault).

---

## 📑 Module Index

| # | Topic | Description | Status |
| :-: | :--- | :--- | :-: |
| 01 | [SSH Protocol Architecture & Handshake](./01-SSH-Protocol-Architecture-and-Handshake.md) | Transport layer, authentication phase, and `known_hosts` | ✅ Complete |
| 02 | [Modern SSH Keys: Ed25519 vs RSA](./02-Modern-SSH-Keys-Ed25519-vs-RSA.md) | Elliptic curves, key generation, and passphrase hardening | ✅ Complete |
| 03 | [Hardening OpenSSH Server (sshd_config)](./03-Hardening-OpenSSH-Server-sshd_config.md) | Disabling passwords/root login, key ciphers, and `sshd -t` audit | ✅ Complete |
| 04 | [Client Config & Bastion Jump Hosts](./04-SSH-Client-Configuration-and-Bastions.md) | `~/.ssh/config`, `ProxyJump`, and transparent multi-hop proxying | ✅ Complete |
| 05 | [SSH Tunneling & Port Forwarding](./05-SSH-Tunneling-and-Port-Forwarding-Mastery.md) | Local (`-L`), Remote (`-R`), and Dynamic SOCKS5 (`-D`) proxying | ✅ Complete |
| 06 | [SSH Certificates & Zero-Trust Access](./06-SSH-Certificates-and-Zero-Trust-Access.md) | Ephemeral certificates, HashiCorp Vault SSH, and Teleport | ✅ Complete |
| 07 | [Real-World Production Scenarios](./07-Real-World-Scenarios.md) | Accessing private VPC RDS database via tunnel; Ansible bastion setup | ✅ Complete |
| 08 | [Troubleshooting Guide & Runbook](./08-Troubleshooting.md) | `ssh -vvv` debugging, bad permissions `0644`, and host key resets | ✅ Complete |
| 09 | [Interview Q&A](./09-Interview-QA.md) | 10 technical SSH interview questions for DevOps, SRE, and Security | ✅ Complete |
| 10 | [Hands-On Practice Labs](./10-Hands-On-Practice.md) | Generate Ed25519 keys, harden server, configure local port forwarding | ✅ Complete |
| 11 | [Multiple Choice Questions (MCQ)](./11-MCQ.md) | Self-assessment test with detailed answers and technical explanations | ✅ Complete |
| 12 | [Quick Revision Cheat Sheet](./12-Quick-Revision.md) | High-density command matrix, config directives, and tunnel flags | ✅ Complete |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [Module 05: TCP, UDP & Sockets](../05-TCP-UDP-and-Sockets/README.md) | [Networking Master Index](../README.md) | [01 - SSH Protocol Architecture](./01-SSH-Protocol-Architecture-and-Handshake.md) |
