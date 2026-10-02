# Module 32: SSH Architecture, Key Management, and Tunneling

Secure Shell (SSH) is the standard cryptographic protocol for remote Linux administration, automated configuration management (Ansible), and secure data pipelines. Beyond logging into remote shells, SSH provides powerful port forwarding, bastion proxy jumping, and cryptographic tunneling capabilities that every DevOps engineer must master.

---

## 🎯 Learning Objectives
By the end of this module, you will be able to:
- Understand the SSH protocol handshake, Diffie-Hellman key exchange, and host verification.
- Differentiate modern cryptographic algorithms (**Ed25519** vs RSA) and generate secure keys.
- Harden the OpenSSH daemon (`sshd_config`) to resist automated brute-force attacks.
- Configure `~/.ssh/config` for seamless multi-environment management and **ProxyJump** bastions.
- Create **Local (`-L`)**, **Remote (`-R`)**, and **Dynamic SOCKS5 (`-D`)** SSH tunnels.
- Understand the security risks of `ssh-agent` forwarding and how `ProxyJump` eliminates them.
- Debug SSH connectivity failures with verbose logging (`ssh -vvv`) and fix key permissions.

---

## 📑 Module Index

| # | Topic | Description | Status |
| :-: | :--- | :--- | :-: |
| 01 | [SSH Protocol Architecture & Cryptography](./01-SSH-Protocol-Architecture-and-Cryptography.md) | Diffie-Hellman exchange, session encryption, and `known_hosts` | ✅ Complete |
| 02 | [Modern SSH Key Types: Ed25519 vs RSA](./02-Modern-SSH-Key-Types-Ed25519-vs-RSA.md) | Elliptic curve cryptography, key generation, and passphrase hardening | ✅ Complete |
| 03 | [OpenSSH Server Hardening (sshd_config)](./03-OpenSSH-Server-Hardening-sshd_config.md) | Disabling passwords/root login, key ciphers, and `sshd -T` audit | ✅ Complete |
| 04 | [Client Config & Bastion Jump Hosts](./04-SSH-Client-Configuration-and-Bastion-Jump-Hosts.md) | `~/.ssh/config`, `ProxyJump`, and transparent multi-hop routing | ✅ Complete |
| 05 | [SSH Tunneling & Port Forwarding](./05-SSH-Tunneling-and-Port-Forwarding.md) | Local (`-L`), Remote (`-R`), and Dynamic SOCKS5 (`-D`) proxying | ✅ Complete |
| 06 | [SSH Agent Forwarding & Security](./06-SSH-Agent-Forwarding-and-Security-Risks.md) | `ssh-agent`, Unix socket hijacking risks, and secure alternatives | ✅ Complete |
| 07 | [Real-World Production Scenarios](./07-Real-World-Scenarios.md) | Accessing private VPC RDS database via tunnel; Ansible bastion setup | ✅ Complete |
| 08 | [Troubleshooting Guide & Runbook](./08-Troubleshooting.md) | `ssh -vvv` debugging, bad permissions `0644`, and host key resets | ✅ Complete |
| 09 | [Interview Q&A](./09-Interview-QA.md) | 10 technical SSH interview questions for DevOps, SRE, and Security | ✅ Complete |
| 10 | [Hands-On Practice Labs](./10-Hands-On-Practice.md) | Generate Ed25519 keys, harden server, configure local port forwarding | ✅ Complete |
| 11 | [Multiple Choice Questions (MCQ)](./11-MCQ.md) | Self-assessment test with detailed answers and technical explanations | ✅ Complete |
| 12 | [Quick Revision Cheat Sheet](./12-Quick-Revision.md) | High-density command matrix, config directives, and tunnel flags | ✅ Complete |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [Module 31: Backup & rsync](../31-Linux-Backup-Archiving-and-rsync/README.md) | [Linux Roadmap Index](../README.md) | [01 - SSH Protocol Architecture](./01-SSH-Protocol-Architecture-and-Cryptography.md) |
