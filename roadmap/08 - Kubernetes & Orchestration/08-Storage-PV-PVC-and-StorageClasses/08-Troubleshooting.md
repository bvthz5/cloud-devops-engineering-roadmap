# 08 - Troubleshooting & Diagnostic Runbooks

## Diagnostic Decision Tree: PVC in "Pending" State

```text
[ Symptom: PersistentVolumeClaim remains in "Pending" state ]
                         │
                         ▼
        Run: kubectl describe pvc <pvc-name>
        Look at "Events" section:
        ├── "no VolumeListener or StorageClass found" ──► StorageClass name is misspelled or missing!
        ├── "waiting for first consumer to be created"──► NORMAL for WaitForFirstConsumer!
        │                                                PVC will bind once a Pod references it.
        └── "Failed to provision volume: Unauthorized"──► CSI Controller lacks cloud IAM permissions
                                                         (e.g., ec2:CreateVolume denied).
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [07 - Real-World Scenarios](./07-Real-World-Scenarios.md) | [README](./README.md) | [09 - Interview QA](./09-Interview-QA.md) |
