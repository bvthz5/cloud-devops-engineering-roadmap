# 03 - OrderedReady vs Parallel Pod Management Policies

## 1. OrderedReady (Default)

- **Scale Up:** Pods are created sequentially from $0$ to $N-1$. Pod $1$ is not created until Pod $0$ is in `Running` and `Ready` state.
- **Scale Down:** Pods are terminated in reverse order from $N-1$ down to $0$. Pod $1$ is completely terminated before Pod $0$ receives SIGTERM.

---

## 2. Parallel Pod Management

For stateless workloads that still require stable network identities or batch processing workloads where startup ordering is irrelevant:

```yaml
spec:
  podManagementPolicy: Parallel     # Launch all replicas concurrently!
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [02 - VolumeClaimTemplates](./02-VolumeClaimTemplates-and-Per-Replica-Storage.md) | [README](./README.md) | [04 - Clustered DB Patterns](./04-Clustered-Database-Deployment-Patterns-MySQL-Postgres.md) |
