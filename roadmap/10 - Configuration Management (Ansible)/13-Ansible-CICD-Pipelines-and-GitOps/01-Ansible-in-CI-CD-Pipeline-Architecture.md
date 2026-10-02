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
| [← Prev Module (12-Ansible-Performance-Optimization-and-Strategy)](../12-Ansible-Performance-Optimization-and-Strategy/12-Quick-Revision.md) | [Index](../../../README.md) | [02 - GitHub Actions Pipeline for Ansible →](./02-GitHub-Actions-Pipeline-for-Ansible.md) |
