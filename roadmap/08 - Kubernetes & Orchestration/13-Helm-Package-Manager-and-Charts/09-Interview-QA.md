# 09 - Interview Questions & Architectural Scenarios

### Q1: What does the `--atomic` flag do during `helm install` or `helm upgrade`?
**Answer:**
The `--atomic` flag ensures all-or-nothing transactional deployments. If any resource fails to become healthy or the command times out, Helm automatically triggers `helm rollback` to the previous release state and deletes any newly created resources.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 08 - Troubleshooting](./08-Troubleshooting.md) | [Index](../../../README.md) | [10 - Hands On Practice →](./10-Hands-On-Practice.md) |
