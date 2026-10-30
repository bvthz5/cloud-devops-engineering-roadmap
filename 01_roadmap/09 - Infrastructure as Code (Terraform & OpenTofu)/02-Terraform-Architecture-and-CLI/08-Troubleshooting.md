# 08 - Troubleshooting & Diagnostic Runbooks

## Common `terraform init` Failures

```text
[ Error: Failed to install provider ]
    │
    ▼
Can you reach registry.terraform.io?
├── NO  ──► Check network proxy / firewall rules
│           Configure provider mirror for air-gapped environments
└── YES ──► Is the provider source address correct?
            ├── NO  ──► Fix source in required_providers block
            └── YES ──► Version constraint too restrictive?
                        Run: terraform init -upgrade
```

## Common Errors

| Error | Root Cause | Fix |
|---|---|---|
| `Error acquiring the state lock` | Crashed apply left lock | `terraform force-unlock <LOCK_ID>` |
| `Backend configuration changed` | Backend block modified | `terraform init -reconfigure` or `-migrate-state` |
| `Plugin reinitialization required` | `.terraform/` deleted or corrupted | `terraform init` |
| `Inconsistent dependency lock file` | Lock file out of sync | `terraform init -upgrade` |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 07 - Real World Scenarios](./07-Real-World-Scenarios.md) | [Index](../../../README.md) | [09 - Interview QA →](./09-Interview-QA.md) |
