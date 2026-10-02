# 06 - Production Ansible Anti-Patterns & Clean Code

## Anti-Patterns to Avoid
1. Using `shell` module when a native module exists (e.g. `shell: apt-get install` instead of `ansible.builtin.apt`).
2. Hardcoding passwords in plain text YAML files instead of using Vault.
3. Suppressing errors blindly with `ignore_errors: true`.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 05 - Ansible Idempotency Audit and Compliance Reporting](./05-Ansible-Idempotency-Audit-and-Compliance-Reporting.md) | [Index](../../../README.md) | [07 - Real World Scenarios →](./07-Real-World-Scenarios.md) |
