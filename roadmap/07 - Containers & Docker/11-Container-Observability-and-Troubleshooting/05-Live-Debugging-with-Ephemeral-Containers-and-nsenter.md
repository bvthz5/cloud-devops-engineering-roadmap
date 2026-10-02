# 05 - Live Debugging with Ephemeral Containers and nsenter

## 1. Debugging Stripped Containers

Security-hardened containers (Distroless or Scratch) have no shell, no package manager, and no curl. When an incident occurs, SREs attach to the container's namespaces from the host using **`nsenter`**:

```bash
# 1. Obtain container PID on host
PID=$(docker inspect --format '{{.State.Pid}}' my_distroless_app)

# 2. Attach host's tcpdump to container's network namespace
sudo nsenter -t $PID -n tcpdump -i any -c 20

# 3. Enter container with host's bash shell
sudo nsenter -t $PID -m -u -i -n -p /bin/bash
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 04 - Container Health Monitoring with cAdvisor and Prometheus](./04-Container-Health-Monitoring-with-cAdvisor-and-Prometheus.md) | [Index](../../../README.md) | [06 - eBPF Based Container Tracing and Security Auditing →](./06-eBPF-Based-Container-Tracing-and-Security-Auditing.md) |
