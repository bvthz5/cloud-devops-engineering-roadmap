# 05 - Finalizers and Safe Resource Teardown

## 1. How Finalizers Prevent Premature Deletion

When a user executes `kubectl delete database prod-db`, Kubernetes will **not delete the object immediately** if `metadata.finalizers` is populated!
Instead:
1. `metadata.deletionTimestamp` is set.
2. The custom controller executes external cleanup (e.g. deleting AWS RDS instances, backing up data, removing DNS records).
3. The controller removes its string from `metadata.finalizers`.
4. Kubernetes permanently removes the object from `etcd`.

```yaml
metadata:
  finalizers:
  - databases.example.com/cleanup-cloud-storage
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [04 - Reconciliation Loops](./04-Operator-Reconciliation-Loops-and-Event-Handling.md) | [README](./README.md) | [06 - OLM & OperatorHub](./06-Operator-Lifecycle-Manager-OLM-and-OperatorHub.md) |
