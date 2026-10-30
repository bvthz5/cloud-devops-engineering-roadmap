# 01 - Docker Engine Architecture and Subsystems

## 1. Docker Engine Architecture Overview

Docker Engine consists of three primary parts that work together to build, ship, and run containers:

```text
┌─────────────────────────────────────────────────────────────┐
│                    Docker CLI (docker)                      │
│ The command-line interface that sends commands to REST API. │
└──────────────────────────────┬──────────────────────────────┘
                               │ HTTP / Unix Socket (/var/run/docker.sock)
                               ▼
┌─────────────────────────────────────────────────────────────┐
│                   Docker REST API                           │
│ A set of HTTP endpoints exposing daemon functionality.      │
└──────────────────────────────┬──────────────────────────────┘
                               │ Direct Internal Binding
                               ▼
┌─────────────────────────────────────────────────────────────┐
│                 Docker Daemon (dockerd)                     │
│ The background service managing images, containers,         │
│ networks, and volumes on your host.                         │
└─────────────────────────────────────────────────────────────┘
```

---

## 2. Historical Evolution: LXC to Libcontainer to OCI

1. **2013 (LXC Dependency):** When Docker launched in 2013, it used **Linux Containers (LXC)** to isolate processes via namespaces and cgroups.
2. **2014 (Libcontainer, Version 0.9):** By version 0.9, Docker introduced **Libcontainer**, an in-house Go library interfacing directly with kernel primitives—eliminating the LXC dependency and simplifying container management.
3. **2015 (Open Container Initiative - OCI):** Before 2015, container formats and runtimes were fragmented. Docker, CoreOS, and other industry leaders formed the **Open Container Initiative (OCI)** to standardize:
   - **Runtime Specification:** Defines lifecycle operations (`create`, `start`, `delete`, etc.).
   - **Image Specification:** Specifies how container images are formatted and distributed.
4. **Docker Engine 1.11 Modular Refactoring:** With these standards, Docker Engine 1.11 was refactored into modular components: `dockerd`, `containerd`, `containerd-shim`, and `runc`.

---

## 3. The Subsystem Decomposition: Docker to runc

```text
[ Docker CLI (docker) ]
          │ (Unix Socket: /var/run/docker.sock - HTTP/REST)
          ▼
[ dockerd (Docker Daemon) ]
  - High-level features: Image building, volumes, user authentication, Docker API
          │ (gRPC: /run/containerd/containerd.sock)
          ▼
[ containerd ]
  - OCI container supervisor: Image push/pull, container lifecycle management
          │ (Forks per container)
          ▼
[ containerd-shim ]
  - Decouples running container from daemon! Allows dockerd to restart without killing containers.
          │ (Executes runc)
          ▼
[ runc (OCI Reference Runtime) ]
  - Configures namespaces and cgroups, starts application process, then exits!
          │
          ▼
[ Application Process (PID 1 inside container) ]
```

---

## 4. Why `containerd-shim` Matters

When `runc` finishes setting up kernel namespaces, it exits. The **`containerd-shim`** stays alive as the parent process of the container:
1. **Keeps File Descriptors Open:** Preserves stdin/stdout/stderr pipes even if `dockerd` crashes or restarts.
2. **Exit Code Capturing:** Captures container exit codes and reports them back to `containerd`.
3. **Live Restore Support:** Enables zero-downtime daemon upgrades (`"live-restore": true` in `/etc/docker/daemon.json`).

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Prev Module (01-Container-Fundamentals-Cgroups-Namespaces)](../01-Container-Fundamentals-Cgroups-Namespaces/12-Quick-Revision.md) | [Index](../../../README.md) | [02 - Container Lifecycle and Process States →](./02-Container-Lifecycle-and-Process-States.md) |
