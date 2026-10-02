# 12 - Quick-Revision & Enterprise Cheat Sheet

## Disaster Recovery Cheat Sheet

- **etcd Snapshot:** `etcdctl snapshot save <path>`.
- **etcd Status:** `etcdctl snapshot status <path> -w table`.
- **Velero Backup:** `velero backup create <name> --include-namespaces=<ns>`.
- **Velero Restore:** `velero restore create --from-backup <name>`.
- **Golden Rule:** Test restores periodically in an isolated sandbox cluster.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [11 - MCQ](./11-MCQ.md) | [README](./README.md) | [Master Index](../../00-Master-Index.md) |
