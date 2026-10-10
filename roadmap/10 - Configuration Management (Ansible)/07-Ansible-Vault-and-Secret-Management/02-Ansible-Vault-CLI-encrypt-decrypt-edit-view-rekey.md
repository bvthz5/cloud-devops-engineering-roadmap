# 02 - Ansible Vault CLI Operations: `encrypt`, `decrypt`, `edit`, `view`, & `rekey`

The `ansible-vault` command-line utility provides the operational toolset to manage the lifecycle of encrypted secrets. It allows operators to encrypt plaintext files, edit encrypted files in memory, rotate encryption keys, and encrypt individual variable strings without ever exposing plaintext secrets to the local filesystem or git staging area.

---

## 🛠️ 1. Core Lifecycle Commands Breakdown

Ansible Vault provides six primary operations to manage encrypted secret files:

```text
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                          ANSIBLE VAULT CLI OPERATION LIFECYCLE                         │
│                                                                                        │
│   1. CREATE (New Vault) ──► Opens $EDITOR in RAM ──► Saves as AES-256 encrypted file  │
│                                                                                        │
│   2. ENCRYPT (Existing) ──► Reads Plaintext YAML ──► Overwrites file with Ciphertext  │
│                                                                                        │
│   3. EDIT (Modify)      ──► Decrypts into RAM Temp ──► Opens $EDITOR ──► Re-encrypts  │
│                             (Plaintext never touches persistent disk storage)          │
│                                                                                        │
│   4. VIEW (Read-Only)   ──► Decrypts to stdout without modifying the file on disk      │
│                                                                                        │
│   5. REKEY (Rotate Key) ──► Re-encrypts payload under a new passphrase in-place       │
│                                                                                        │
│   6. DECRYPT (Revert)   ──► Permanently converts encrypted file back to plaintext      │
└────────────────────────────────────────────────────────────────────────────────────────┘
```

### Detailed Command Grammar & Recipes

```bash
# 1. Create a new encrypted file from scratch
# Prompts for vault password twice, then opens your default $EDITOR
ansible-vault create group_vars/production/vault.yml

# 2. Encrypt an existing plaintext file in-place
ansible-vault encrypt group_vars/production/secrets.yml

# 3. Edit an encrypted file safely in memory
# Decrypts payload into protected RAM buffer; re-encrypts upon editor exit
ansible-vault edit group_vars/production/vault.yml

# 4. View encrypted contents directly in stdout without modifying disk
ansible-vault view group_vars/production/vault.yml

# 5. Rekey: Rotate the encryption password without plaintext exposure
# Prompts for the current password, then prompts for the new replacement password
ansible-vault rekey group_vars/production/vault.yml

# 6. Decrypt an encrypted file back into permanent plaintext
# CAUTION: Never commit decrypted files to Git!
ansible-vault decrypt group_vars/production/vault.yml
```

---

## 🔑 2. Password Provisioning Strategies

Manually typing vault passwords with `--ask-vault-pass` is acceptable for local testing, but enterprise automation and CI/CD pipelines require non-interactive password provisioning.

### Method 1: Local Password File (`--vault-password-file`)
```bash
# Point to a local secret key file containing the passphrase
ansible-playbook -i hosts.ini site.yml --vault-password-file ~/.ansible/vault_pass.txt
```

> **Security Rule:** Ensure the password file has strict POSIX permissions (`chmod 600 ~/.ansible/vault_pass.txt`) and is listed in `.gitignore`.

### Method 2: Environment Variable (`ANSIBLE_VAULT_PASSWORD_FILE`)
```bash
export ANSIBLE_VAULT_PASSWORD_FILE=~/.ansible/vault_pass.txt
ansible-playbook -i hosts.ini site.yml
```

### Method 3: Dynamic Executable Password Script
If the path specified by `--vault-password-file` is marked as executable (`chmod +x`), Ansible **executes the script** and reads standard output as the passphrase. This enables integration with external secret management tools like AWS Secrets Manager, HashiCorp Vault, or 1Password:

```bash
#!/bin/bash
# /opt/scripts/get_vault_key.sh
# Fetch master secret dynamically from AWS Secrets Manager
aws secretsmanager get-secret-value --secret-id "prod/ansible-vault-key" --query SecretString --output text
```

In `ansible.cfg`:
```ini
[defaults]
vault_password_file = /opt/scripts/get_vault_key.sh
```

---

## 🏷️ 3. Multi-Environment Secret Management with Vault IDs (`--vault-id`)

In multi-tier enterprise deployments, developers should have access to development and staging passwords, but never the production master key. Single password vaults cannot support this segregation. Modern Ansible solves this through **Vault IDs** using the format `label@source`.

### Encrypting with a Specific Vault ID
```bash
# Encrypt development secrets using the 'dev' label
ansible-vault encrypt --vault-id dev@prompt group_vars/development/vault.yml

# Encrypt production secrets using the 'prod' label
ansible-vault encrypt --vault-id prod@/etc/ansible/keys/prod.key group_vars/production/vault.yml
```

### Running Playbooks with Multiple Vault IDs
When executing a multi-tier playbook, you can pass multiple vault IDs simultaneously. Ansible automatically matches each encrypted file with its corresponding vault ID label:

```bash
ansible-playbook -i inventory/ \
  --vault-id dev@~/.vault_dev \
  --vault-id prod@~/.vault_prod \
  site.yml
```

---

## 🔏 4. Inline Variable Encryption (`encrypt_string`)

Encrypting entire YAML files creates a major operational problem in Git: developers cannot see which variable keys exist, and every update creates a massive, unreadable binary git diff. 

Ansible solves this with `encrypt_string`, allowing you to keep YAML keys visible in plaintext while encrypting only the sensitive value itself:

```bash
# Encrypt an individual secret string for use in YAML
ansible-vault encrypt_string 'MySuperSecretP@ssword!' --name 'db_password'
```

### Output:
```yaml
db_password: !vault |
          $ANSIBLE_VAULT;1.1;AES256
          62653265333534343734323635323230623265353535323933393962633036663363383030313837
          6137653738376566613864383134376366623638363765380a653835613333333333306232326262
          39613133613636383437303861616164623136306338363830613864376531393665323963353862
```

This inline `!vault |` block can be committed directly into `group_vars/all/vars.yml`. Other non-sensitive variables in the same file remain readable and diff-friendly in Git!

---

## 🛡️ 5. Pre-Commit Hook: Preventing Plaintext Secret Leaks

To prevent engineers from accidentally committing unencrypted secrets to Git, configure a Git pre-commit hook (`.git/hooks/pre-commit`):

```bash
#!/bin/bash
# Pre-commit hook to verify vault files are encrypted
FILES=$(git diff --cached --name-only | grep 'vault.ya\?ml$')

for FILE in $FILES; do
    if ! head -n 1 "$FILE" | grep -q '^\$ANSIBLE_VAULT'; then
        echo "❌ ERROR: File $FILE is not encrypted with Ansible Vault!"
        echo "Run: ansible-vault encrypt $FILE before committing."
        exit 1
    fi
done
exit 0
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 01 - Ansible Vault Architecture and AES256 Encryption](./01-Ansible-Vault-Architecture-and-AES256-Encryption.md) | [Index](../../../README.md) | [03 - Vault Passwords Vault ID and Multiple Vault Keys →](./03-Vault-Passwords-Vault-ID-and-Multiple-Vault-Keys.md) |
