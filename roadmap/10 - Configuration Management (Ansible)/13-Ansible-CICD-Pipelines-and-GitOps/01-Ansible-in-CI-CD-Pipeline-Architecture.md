# 01 - Ansible in CI/CD Pipeline Architecture

```text
 [Developer PR] ===> [GitHub Actions: ansible-lint & Molecule]
                            │ (Merge to main)
                            ▼
                    [AWX Webhook Trigger]
                            │
                            ▼
                [Automated Playbook Execution]
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [README](./README.md) | [README](./README.md) | [02 - GitHub Actions Pipeline](./02-GitHub-Actions-Pipeline-for-Ansible.md) |
