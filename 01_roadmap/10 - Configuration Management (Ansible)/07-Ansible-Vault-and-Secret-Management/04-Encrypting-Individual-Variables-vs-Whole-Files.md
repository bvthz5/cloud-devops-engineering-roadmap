# 04 - Encrypting Individual Variables (`encrypt_string`)

```bash
ansible-vault encrypt_string 'SuperSecretPassword123' --name 'db_password'
```

Output embedded directly into YAML:
```yaml
db_password: !vault |
          $ANSIBLE_VAULT;1.1;AES256
          66386438316335...
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 03 - Vault Passwords Vault ID and Multiple Vault Keys](./03-Vault-Passwords-Vault-ID-and-Multiple-Vault-Keys.md) | [Index](../../../README.md) | [05 - Integrating Ansible with External Secret Stores →](./05-Integrating-Ansible-with-External-Secret-Stores.md) |
