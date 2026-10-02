# 09 - Interview Questions & Architectural Scenarios

### Q1: What is the primary architectural difference between Docker and Podman?
**Answer**: Docker relies on a centralized, privileged background daemon (`dockerd`) running as root to manage all containers. Podman is daemonless; it uses a traditional Unix fork/exec model where container processes are direct child processes of the user, managed via `conmon` and systemd, with rootless execution enabled by default.

### Q2: How does Skopeo optimize CI/CD pipeline bandwidth?
**Answer**: Skopeo can inspect manifests, copy images between remote registries, and sign images without ever pulling or unpacking the layer tarballs onto the local CI runner disk, drastically speeding up promotion workflows.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [08 - Troubleshooting](./08-Troubleshooting.md) | [README](./README.md) | [10 - Hands-On Practice](./10-Hands-On-Practice.md) |
