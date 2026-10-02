# 06 - Security Contexts: RunAsUser and Privilege Escalation

## 1. Hardening Pods with SecurityContext

By default, containerized processes run as `root` (UID 0). A hardened Pod enforces the principle of least privilege:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: hardened-pod
spec:
  securityContext:
    runAsNonRoot: true
    runAsUser: 10001
    runAsGroup: 10001
    fsGroup: 10001
    seccompProfile:
      type: RuntimeDefault
  containers:
  - name: app
    image: my-app:v1
    securityContext:
      allowPrivilegeEscalation: false
      readOnlyRootFilesystem: true
      capabilities:
        drop:
        - ALL
    volumeMounts:
    - name: tmp-volume
      mountPath: /tmp
  volumes:
  - name: tmp-volume
    emptyDir: {}
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 05 - Pod Disruption Budgets PDB and Graceful Termination](./05-Pod-Disruption-Budgets-PDB-and-Graceful-Termination.md) | [Index](../../../README.md) | [07 - Real World Scenarios →](./07-Real-World-Scenarios.md) |
