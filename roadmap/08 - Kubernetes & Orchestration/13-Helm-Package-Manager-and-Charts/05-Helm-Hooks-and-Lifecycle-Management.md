# 05 - Helm Hooks and Lifecycle Management

## 1. Intercepting the Release Lifecycle

**Helm Hooks** execute batch jobs at specific points during a release:
- `pre-install` / `pre-upgrade`: Run database schema migrations before new pods start.
- `post-install` / `post-upgrade`: Trigger Slack notifications or cache warmup.
- `post-rollback`: Execute cleanup after an aborted release.

```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: db-migration-hook
  annotations:
    "helm.sh/hook": pre-upgrade,pre-install
    "helm.sh/hook-weight": "5"
    "helm.sh/hook-delete-policy": before-hook-creation,hook-succeeded
spec:
  template:
    spec:
      restartPolicy: Never
      containers:
      - name: migrator
        image: my-app:v2
        command: ["python", "manage.py", "migrate"]
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [04 - Subcharts & Library Charts](./04-Subcharts-Chart-Dependencies-and-Library-Charts.md) | [README](./README.md) | [06 - OCI Registries](./06-OCI-Chart-Registries-and-Enterprise-Distribution.md) |
