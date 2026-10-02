# 03 - Vault Passwords, Vault IDs & Multi-Keys

```bash
# Using labelled Vault IDs
ansible-playbook -i hosts site.yml --vault-id dev@.vault_dev --vault-id prod@.vault_prod
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [02 - Vault CLI Operations](./02-Ansible-Vault-CLI-encrypt-decrypt-edit-view-rekey.md) | [README](./README.md) | [04 - Inline Encrypted Variables](./04-Encrypting-Individual-Variables-vs-Whole-Files.md) |
