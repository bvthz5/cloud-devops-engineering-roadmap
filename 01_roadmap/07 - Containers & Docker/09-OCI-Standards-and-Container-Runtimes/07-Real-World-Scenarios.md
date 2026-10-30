# 07 - Real-World Scenarios & Outage Post-Mortems

## Scenario 1: The Dockershim Removal Upgrade Outage

### Context & Incident
A company upgraded their self-managed Kubernetes cluster from v1.23 to v1.24. Following the node upgrade, all logging DaemonSets and monitoring agents failed with:
`dial unix /var/run/docker.sock: connect: no such file or directory`

### Root Cause
The legacy monitoring and logging agents had hardcoded mounts to `/var/run/docker.sock` to inspect container metrics and logs. When the cluster migrated to pure `containerd`, the Docker daemon was removed, leaving the socket nonexistent.

### Solution: Migrate to CRI-Compliant Loggers
Modern logging agents (Fluent Bit, Promtail) must read standard OCI container logs directly from `/var/log/pods/` rather than querying the Docker daemon socket.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 06 - The Dockershim Deprecation Story](./06-The-Dockershim-Deprecation-Story.md) | [Index](../../../README.md) | [08 - Troubleshooting →](./08-Troubleshooting.md) |
