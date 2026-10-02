# 05 - Cross-Cluster Migration and Cluster Rebuilding

## 1. Migrating Across Clusters via Velero

```bash
# In Cluster A (Old / On-Prem):
velero backup create migration-backup --include-namespaces=ecommerce

# In Cluster B (New / Cloud EKS):
# Point Velero to the same S3 bucket:
velero restore create --from-backup migration-backup
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [04 - Scheduled Automated Backups](./04-Scheduled-Automated-Backups-and-Object-Storage-Targets.md) | [README](./README.md) | [06 - DR Testing & Validation](./06-DR-Testing-Validation-and-RTO-Benchmarking.md) |
