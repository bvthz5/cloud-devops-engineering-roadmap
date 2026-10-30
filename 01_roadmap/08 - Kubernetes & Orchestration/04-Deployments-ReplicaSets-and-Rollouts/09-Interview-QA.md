# 09 - Interview Questions & Architectural Scenarios

### Q1: What does `progressDeadlineSeconds` do in a Deployment?
**Answer:**
`progressDeadlineSeconds` (default: 600 seconds / 10 minutes) defines how long the Deployment controller waits for a rollout to make observable progress before reporting a failed condition (`Type=Progressing, Status=False, Reason=ProgressDeadlineExceeded`). This is crucial for CI/CD pipelines to fail fast when a bad image or configuration causes a deployment to hang.

---

### Q2: What happens to old ReplicaSets when a Deployment completes a rollout?
**Answer:**
The old ReplicaSet is **not deleted**; its `spec.replicas` is scaled to `0`. It remains in `etcd` so that engineers can perform an instant rollback (`kubectl rollout undo`). The total number of preserved old ReplicaSets is governed by `spec.revisionHistoryLimit`.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 08 - Troubleshooting](./08-Troubleshooting.md) | [Index](../../../README.md) | [10 - Hands On Practice →](./10-Hands-On-Practice.md) |
