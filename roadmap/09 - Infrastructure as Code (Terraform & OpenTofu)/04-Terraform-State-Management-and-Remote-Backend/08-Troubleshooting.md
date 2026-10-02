# 08 - Troubleshooting

## State Error Diagnostic Tree

```text
[ Symptom: terraform plan shows unexpected changes ]
    │
    ▼
Is there drift (manual changes)?
├── YES ──► terraform plan shows ~ (update) for manually changed resources
│           Fix: Decide to import changes or revert with apply
└── NO  ──► Is state file corrupted?
            ├── YES ──► Restore from S3 versioning
            │           Or rebuild with terraform import
            └── NO  ──► Are there orphaned resources in state?
                        ├── YES ──► terraform state rm <resource>
                        └── NO  ──► Check provider version for breaking changes
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [07 - Real-World Scenarios](./07-Real-World-Scenarios.md) | [README](./README.md) | [09 - Interview QA](./09-Interview-QA.md) |
