# 09 - Interview Questions & Architectural Scenarios

### Q1: How do updates to ConfigMaps propagate to containers mounted via Volumes vs Environment Variables?
**Answer:**
- **Volumes:** Kubelet periodically syncs mounted ConfigMap volumes using atomic symlink swapping (`..data` directory). The updated content appears in the container within the kubelet sync period (default ~60 seconds) without restarting the pod.
- **Environment Variables:** Environment variables are initialized only when the container process is created. Updating the ConfigMap will **never** update the environment variable of an already running process. The Pod must be restarted (e.g., via `kubectl rollout restart`).

---

### Q2: How do you prevent secret keys from appearing in system shell logs?
**Answer:**
Mount secrets as **files via volumes** rather than environment variables. Environment variables are easily exposed via `/proc/$PID/environ`, crash dumps, application APM agents, or child process inspection.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [08 - Troubleshooting](./08-Troubleshooting.md) | [README](./README.md) | [10 - Hands-On Practice](./10-Hands-On-Practice.md) |
