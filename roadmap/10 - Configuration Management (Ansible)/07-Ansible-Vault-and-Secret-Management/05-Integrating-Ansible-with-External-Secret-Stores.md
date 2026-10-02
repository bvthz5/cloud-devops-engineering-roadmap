# 05 - Integrating Ansible with External Secret Stores

Using HashiCorp Vault lookup plugin:
```yaml
- name: Retrieve DB password from HashiCorp Vault
  set_fact:
    db_pass: "{{ lookup('community.hashi_vault.hashi_vault', 'secret=secret/data/db:password') }}"
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [04 - Inline Encrypted Variables](./04-Encrypting-Individual-Variables-vs-Whole-Files.md) | [README](./README.md) | [06 - Vault Keys in CI/CD](./06-Securing-Vault-Keys-in-CI-CD-Pipelines.md) |
