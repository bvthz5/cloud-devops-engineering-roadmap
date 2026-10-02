# 09 - Interview Questions & Architectural Scenarios

### Q1: What is the purpose of `containerd-shim` in Docker Engine?
**Answer**: `containerd-shim` sits between `containerd` and the running container process. It holds open standard I/O file descriptors, handles container exit status reporting, and reparents the container process, allowing `dockerd` and `containerd` to be restarted or upgraded without terminating running containers (`live-restore`).

### Q2: What is the difference between `docker stop` and `docker kill`?
**Answer**: `docker stop` sends `SIGTERM` (Signal 15) to PID 1, allowing graceful shutdown within a configurable timeout (default 10s) before sending `SIGKILL`. `docker kill` immediately sends `SIGKILL` (Signal 9) without giving the process any opportunity to clean up resources or finish active requests.

### Q3: Why should `userland-proxy: false` be set in production `daemon.json`?
**Answer**: By default, Docker spawns a user-space `docker-proxy` process for every published port to forward traffic. Disabling it routes packets directly through Linux kernel `iptables` NAT rules, drastically reducing memory overhead and CPU context switches under high connection concurrency.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [08 - Troubleshooting](./08-Troubleshooting.md) | [README](./README.md) | [10 - Hands-On Practice](./10-Hands-On-Practice.md) |
