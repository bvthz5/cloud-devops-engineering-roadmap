# 02 - Ansible Vault CLI Operations

```bash
# Encrypt existing YAML file
ansible-vault encrypt group_vars/production/secrets.yml

# Edit encrypted file in place
ansible-vault edit group_vars/production/secrets.yml

# View encrypted file without decrypting on disk
ansible-vault view group_vars/production/secrets.yml

# Change vault password
ansible-vault rekey group_vars/production/secrets.yml
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [01 - Vault Architecture](./01-Ansible-Vault-Architecture-and-AES256-Encryption.md) | [README](./README.md) | [03 - Vault IDs & Multi-Keys](./03-Vault-Passwords-Vault-ID-and-Multiple-Vault-Keys.md) |
