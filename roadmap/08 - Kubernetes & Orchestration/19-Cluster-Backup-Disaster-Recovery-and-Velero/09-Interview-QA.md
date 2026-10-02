# 09 - Interview Questions & Architectural Scenarios

### Q1: Why should you never restore an etcd snapshot onto an actively running multi-member etcd cluster?
**Answer:**
If you restore an etcd snapshot on one member while other members are running, the restored member will have an older transaction ID and differing cluster token. The active cluster will reject it, or Raft consensus will fail with split-brain. You must stop all etcd members and API servers across all control plane nodes, restore the snapshot on all members using the same initial cluster token, and then restart the cluster together.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 08 - Troubleshooting](./08-Troubleshooting.md) | [Index](../../../README.md) | [10 - Hands On Practice →](./10-Hands-On-Practice.md) |
