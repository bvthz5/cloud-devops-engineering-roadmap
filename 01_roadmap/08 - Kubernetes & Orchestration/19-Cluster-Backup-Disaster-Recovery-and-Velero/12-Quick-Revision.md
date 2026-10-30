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
| [← 11 - MCQ](./11-MCQ.md) | [Index](../../../README.md) | [Next Section (09 - Infrastructure as Code (Terraform & OpenTofu)) →](../../09%20-%20Infrastructure%20as%20Code%20(Terraform%20%26%20OpenTofu)/01-IaC-Concepts-and-Evolution/01-What-Is-IaC-Declarative-vs-Imperative.md) |
