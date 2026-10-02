# 08 - GitOps: Troubleshooting Guide

## 1. Argo CD Application Out-of-Sync Loop

If an application is stuck in `OutOfSync`:
- **Mutable Fields:** Mutating webhooks (e.g. Istio sidecar injection or cert-manager) are injecting fields into pods that do not exist in Git.
- **Fix:** Add `ignoreDifferences` to the Argo CD Application manifest:
```yaml
spec:
  ignoreDifferences:
  - group: apps
    kind: Deployment
    jsonPointers:
    - /spec/template/metadata/annotations/sidecar.istio.io~1status
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [07 - Real-World Scenarios](./07-Real-World-Scenarios.md) | [README](./README.md) | [09 - Interview Q&A](./09-Interview-QA.md) |
