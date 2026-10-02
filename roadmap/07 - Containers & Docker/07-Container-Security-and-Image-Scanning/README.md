# 07 - Container Security and Image Scanning

Containers share the host operating system kernel, making container security fundamentally different from virtual machine security. Securing container environments requires a multi-layered defense-in-depth approach: dropping Linux capabilities, enforcing Seccomp system call filters, running rootless daemons, performing automated vulnerability scanning (CVEs) in CI/CD, and enforcing read-only root filesystems.

---

## Module Index

| # | Topic | Description |
|---|---|---|
| 01 | [Container Threat Modeling & Escape Vectors](./01-Container-Threat-Modeling-and-Escape-Vectors.md) | Shared kernel attack surface, escape vectors, `/proc` and `/sys` leakage, kernel exploits. |
| 02 | [Linux Capabilities & Least Privilege](./02-Linux-Capabilities-and-Least-Privilege.md) | Dissecting root privileges, `--cap-drop=ALL`, `--cap-add`, fine-grained privileges. |
| 03 | [Seccomp & AppArmor LSM Profiles](./03-Seccomp-and-AppArmor-LSM-Profiles.md) | System call filtering via Seccomp default profile, custom JSON profiles, AppArmor. |
| 04 | [Rootless Docker Architecture](./04-Rootless-Docker-Architecture.md) | Running dockerd without root, user namespaces (`subuid`/`subgid`), RootlessKit, limits. |
| 05 | [Static Image Vulnerability Scanning](./05-Static-Image-Vulnerability-Scanning.md) | Trivy, Grype, scanning OS packages vs language lockfiles, CI/CD quality gates. |
| 06 | [CIS Docker Benchmark & Runtime Auditing](./06-CIS-Docker-Benchmark-and-Runtime-Auditing.md) | Docker Bench for Security, runtime threat detection with Falco, immutable `--read-only` rootfs. |
| 07 | [Real-World Scenarios](./07-Real-World-Scenarios.md) | Enterprise post-mortems: Exposed Docker daemon (port 2375) botnet, runc escape (CVE-2019-5736). |
| 08 | [Troubleshooting & Diagnostic Runbooks](./08-Troubleshooting.md) | Debugging Seccomp syscall denials with `auditd`, fixing read-only rootfs permission issues. |
| 09 | [Interview Questions & Architectural Scenarios](./09-Interview-QA.md) | 10 Senior DevSecOps/SRE interview scenarios on container security, capabilities, and escapes. |
| 10 | [Hands-On Practice Labs](./10-Hands-On-Practice.md) | Lab 1: Hardening container with `--cap-drop=ALL` and `--read-only`; Lab 2: Trivy scan; Lab 3: Seccomp. |
| 11 | [Multiple-Choice Assessment (MCQ)](./11-MCQ.md) | 10 technical scenario MCQs with deep architectural rationales. |
| 12 | [Quick-Revision & Enterprise Cheat Sheet](./12-Quick-Revision.md) | High-density reference for security run flags, capability lists, and scanning commands. |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [06 - Docker Compose](../06-Docker-Compose-Multi-Container-Apps/README.md) | [README](./README.md) | [01 - Container Threat Modeling](./01-Container-Threat-Modeling-and-Escape-Vectors.md) |
