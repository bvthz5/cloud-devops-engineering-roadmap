# 04 - Resource Requests, Limits, and Quality of Service (QoS)

## 1. Requests vs Limits

- **`requests`**: Guaranteed resource allocation used by `kube-scheduler` for node placement.
- **`limits`**: Maximum resource boundary enforced at runtime by the Linux kernel.
  - **CPU:** Enforced via CFS Quota (`cfs_quota_us`). Exceeding CPU limit results in **CPU Throttling**, not killing!
  - **Memory:** Enforced via cgroups memory limit. Exceeding memory limit results in immediate **OOMKilled (Exit Code 137)**!

---

## 2. Quality of Service (QoS) Classes

Kubernetes classifies every Pod into one of three QoS classes, which determines its eviction priority when the node experiences resource pressure:

```text
EVICTION PRIORITY (Lowest to Highest Survival):
1. BestEffort   ──► Killed First!
2. Burstable    ──► Killed Second!
3. Guaranteed   ──► Killed Last (Only when system is collapsing)!
```

| QoS Class | Criteria |
|---|---|
| **Guaranteed** | Every container has CPU and Memory requests and limits explicitly set, and `requests == limits`. |
| **Burstable** | At least one container has CPU or Memory requests specified, but `requests != limits`. |
| **BestEffort** | No requests or limits are set on any container in the Pod. |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 03 - Init Containers Sidecars and Ephemeral Containers](./03-Init-Containers-Sidecars-and-Ephemeral-Containers.md) | [Index](../../../README.md) | [05 - Pod Disruption Budgets PDB and Graceful Termination →](./05-Pod-Disruption-Budgets-PDB-and-Graceful-Termination.md) |
