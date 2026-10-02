# 08 - Troubleshooting & Diagnostic Runbooks

## 1. Resolving User Namespace Permissions with `podman unshare`

When running rootless containers, local files on the host often appear owned by nobody (`65534`):

```bash
# Execute a shell inside the user namespace to inspect file ownership
podman unshare ls -la ~/.local/share/containers/storage/volumes/
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [07 - Real-World Scenarios](./07-Real-World-Scenarios.md) | [README](./README.md) | [09 - Interview Questions](./09-Interview-QA.md) |
