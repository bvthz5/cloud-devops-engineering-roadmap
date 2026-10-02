# 01 - Pod Architecture, Pause Container, and Lifecycle

## 1. What Is a Pod?

A **Pod** is the smallest deployable compute unit in Kubernetes. It encapsulates one or more containers that share:
1. **Network Namespace:** Shared IP address, port space, and `localhost` loopback interface.
2. **IPC Namespace:** Shared Inter-Process Communication mechanisms (POSIX shared memory).
3. **Storage Volumes:** Shared local or network storage mounted at declared paths.

```text
+-------------------------------------------------------------------------------+
|                                    POD                                        |
|   IP: 10.244.1.45                                                             |
|                                                                               |
|   +-----------------------------------------------------------------------+   |
|   |                      Pause Container (Infra)                          |   |
|   |  - Holds Network & IPC namespaces open                                |   |
|   |  - Reaps zombie child processes                                       |   |
|   +-----------------------------------------------------------------------+   |
|                                       │                                       |
|            Joined via --net=container / --ipc=container                       |
|                   ┌───────────────────┴───────────────────┐                   |
|                   v                                       v                   |
|   +-------------------------------+       +-------------------------------+   |
|   | Container A (Web Server)      |       | Container B (Log Shipper)     |   |
|   | Listens on localhost:8080     |       | Sends logs to Elasticsearch   |   |
|   +-------------------------------+       +-------------------------------+   |
|                   │                                       │                   |
|                   └───────────────────┬───────────────────┘                   |
|                                       v                                       |
|   +-----------------------------------------------------------------------+   |
|   |                     Shared Volume (/var/log/app)                      |   |
|   +-----------------------------------------------------------------------+   |
+-------------------------------------------------------------------------------+
```

---

## 2. Pod Lifecycle Phases

| Phase | Description |
|---|---|
| **`Pending`** | Pod accepted by API server, but not yet scheduled or downloading container images. |
| **`Running`** | Pod bound to a node, all containers created, and at least one container is active. |
| **`Succeeded`** | All containers in the Pod completed successfully (exit code 0) and will not restart. |
| **`Failed`** | All containers terminated, and at least one container failed (non-zero exit code). |
| **`Unknown`** | Kubelet cannot communicate with the API server to report status. |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [README](./README.md) | [README](./README.md) | [02 - Health Probes](./02-Liveness-Readiness-and-Startup-Probes.md) |
