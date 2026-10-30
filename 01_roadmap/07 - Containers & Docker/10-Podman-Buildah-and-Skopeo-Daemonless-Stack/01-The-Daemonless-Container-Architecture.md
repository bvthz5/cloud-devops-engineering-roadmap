# 01 - The Daemonless Container Architecture

## 1. Docker Daemon vs Podman Fork/Exec Model

```text
Docker Architecture (Centralized Daemon):
[ Docker CLI ] ──► (Unix Socket) ──► [ dockerd (Root Daemon) ] ──► [ containerd ] ──► [ runc ]
* Critical Flaw: Single point of failure! If dockerd crashes, all container management dies.

Podman Architecture (Traditional Unix Fork/Exec):
[ User / CI Script ] ──► [ podman CLI ] ──► [ conmon ] ──► [ crun / runc ] ──► [ Container Process ]
* Zero background daemon!
* Container process is a direct child of the invoking user/shell.
* Seamlessly monitored by standard Linux tools (systemd, ps, auditd).
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Prev Module (09-OCI-Standards-and-Container-Runtimes)](../09-OCI-Standards-and-Container-Runtimes/12-Quick-Revision.md) | [Index](../../../README.md) | [02 - Rootless Podman and User Namespaces →](./02-Rootless-Podman-and-User-Namespaces.md) |
