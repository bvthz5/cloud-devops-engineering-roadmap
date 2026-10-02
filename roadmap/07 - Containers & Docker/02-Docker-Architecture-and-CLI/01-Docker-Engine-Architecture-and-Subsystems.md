# 01 - Docker Engine Architecture and Subsystems

## 1. The Subsystem Decomposition: Docker to runc

Historically, Docker was a monolithic binary. Modern Docker Engine is decoupled into standardized, modular layers aligned with OCI specifications:

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

## 2. Why `containerd-shim` Matters

When `runc` finishes setting up kernel namespaces, it exits. The **`containerd-shim`** stays alive as the parent process of the container:
1. Keeps stdin/stdout/stderr file descriptors open even if `dockerd` crashes or restarts.
2. Captures container exit codes and reports them back to `containerd`.
3. Enables **`live-restore`**: You can upgrade or restart the `docker` daemon with zero downtime to active production containers!

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [README](./README.md) | [README](./README.md) | [02 - Container Lifecycle & States](./02-Container-Lifecycle-and-Process-States.md) |
