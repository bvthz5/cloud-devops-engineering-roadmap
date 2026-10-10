# 01 - Ansible Vault Architecture & AES256 Cryptographic Internals

Infrastructure as Code and GitOps require that all server configuration files, playbooks, and roles be committed to version control. However, real-world infrastructure configurations unavoidably require sensitive data: database root credentials, TLS private keys, OAuth client secrets, AWS access keys, and third-party API tokens. 

Storing plaintext secrets in Git creates severe vulnerability to exfiltration and compliance violations. **Ansible Vault** is the built-in cryptographic security layer that allows operators to encrypt entire files or specific variable strings using military-grade cryptography, enabling teams to safely commit encrypted secrets directly into public or private Git repositories.

---

## 🔐 1. Cryptographic Architecture: AES-256 & PBKDF2

Ansible Vault implements the **Encrypt-then-MAC** (Message Authentication Code) security paradigm to provide both confidentiality and cryptographic integrity verification.

```text
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                        ANSIBLE VAULT CRYPTOGRAPHIC PIPELINE                            │
│                                                                                        │
│   Operator Password (Passphrase or Key File)                                           │
│               │                                                                        │
│               ▼                                                                        │
│   1. Cryptographic Salt Generation (32-byte cryptographically secure random salt)     │
│               │                                                                        │
│               ▼                                                                        │
│   2. PBKDF2-HMAC-SHA256 Key Derivation (10,000+ iterations)                            │
│       ├── Key 1: 256-bit AES Encryption Key                                            │
│       └── Key 2: 256-bit HMAC Integrity Key                                            │
│               │                                                                        │
│               ├─────────────────────────────────────────┐                              │
│               ▼                                         ▼                              │
│   3. AES-256-CBC Encryption                 4. HMAC-SHA256 Generation                  │
│      Plaintext YAML/Data                       Computes HMAC over Ciphertext           │
│      + PKCS#7 Padding                          Guarantees zero tampering               │
│               │                                         │                              │
│               └───────────────────┬─────────────────────┘                              │
│                                   ▼                                                    │
│   5. Hexadecimal Serialization & Header Assembly                                       │
│      $ANSIBLE_VAULT;1.1;AES256 \n <salt_hex> \n <hmac_hex> \n <ciphertext_hex>        │
└────────────────────────────────────────────────────────────────────────────────────────┘
```

### Cryptographic Primitives Breakdown

1. **Symmetric Cipher (AES-256-CBC):**
   Ansible Vault utilizes the Advanced Encryption Standard with a 256-bit key in Cipher Block Chaining (CBC) mode. Plaintext is divided into 16-byte blocks, and PKCS#7 padding is applied to ensure exact block alignment.
2. **Key Derivation Function (PBKDF2):**
   User passphrases are not used directly as the AES key. Instead, PBKDF2 (Password-Based Key Derivation Function 2) runs the passphrase through thousands of HMAC-SHA256 hashing iterations combined with a unique 32-byte random salt. This protects against brute-force attacks and rainbow table precomputation.
3. **Integrity & Authenticity (HMAC-SHA256):**
   Before decrypting any payload, Ansible computes an HMAC across the ciphertext and verifies it against the embedded MAC header. If a single byte of the encrypted file has been modified or corrupted, decryption aborts immediately with a MAC mismatch error, preventing chosen-ciphertext attacks.

---

## 🔬 2. Anatomy of an Encrypted Vault File

When you inspect an Ansible Vault file on disk, it appears as an ASCII text file with a standardized header followed by a hexadecimal stream:

```text
$ANSIBLE_VAULT;1.1;AES256
38356133383038663836336338383838333532663935646138616164326533316131343734663866
3732363737383238623533363633393430343734633230340a333938633834663731306331623933
65623838363236313162623136343534346132646635393437393465366432373030376264313439
```

### Header Specifications

| Header Field | Value | Technical Meaning |
| :--- | :--- | :--- |
| **Magic Identifier** | `$ANSIBLE_VAULT` | Identifies the file as an Ansible Vault payload to CLI and parser tools. |
| **Format Version** | `1.1` or `1.2` | `1.1` is the standard format; `1.2` introduces support for Vault IDs (`--vault-id label@source`). |
| **Cipher Identifier** | `AES256` | Specifies the underlying cryptographic algorithm and key length. |

The hex-encoded body contains three concatenated components separated by newlines:
- **Salt (32 bytes / 64 hex characters):** Unique per-file salt used in PBKDF2 key generation.
- **HMAC (32 bytes / 64 hex characters):** SHA256 checksum verifying payload authenticity.
- **Ciphertext:** The AES-256 encrypted bytes of the original file.

---

## 🛡️ 3. Threat Modeling: What Vault Protects vs. Attack Surfaces

Understanding the security boundaries of Ansible Vault is essential for compliance audits (SOC2, ISO 27001, PCI-DSS):

### ✅ What Ansible Vault Protects
- **Git Repository Exposure:** If a public or private GitHub repository is cloned or leaked, attackers cannot read secrets without the master passphrase.
- **Data at Rest on Workstations:** Developers with cloned repositories cannot inspect production secrets unless granted access to the production vault key.
- **Accidental Plaintext Commits:** Automated pre-commit hooks can enforce that secret files must start with the `$ANSIBLE_VAULT` header.

### ❌ What Ansible Vault DOES NOT Protect
- **Secrets in Host Memory (RAM):** When Ansible executes a playbook, it decrypts vaulted variables in memory on the control node. Memory dumps of the control node process can expose plaintext secrets.
- **Task Output Logging:** If a task logs the evaluated variable to the terminal or CI/CD logs, the plaintext secret is exposed unless protected by `no_log: true`.
- **Target Node Disk Artifacts:** If SSH pipelining is disabled (`pipelining = False`), Ansible writes temporary Python modules containing the evaluated parameters to `/tmp/.ansible/` on the remote target node.

---

## 🔒 4. Production Hardening: The `no_log: true` Directive

Whenever an Ansible task handles a vaulted secret, password, or private key, you must add `no_log: true` to the task definition. This prevents Ansible from printing the secret to stdout, saving it in AWX job logs, or recording it in system logging facilities:

```yaml
---
- name: Deploy Database Credentials Securely
  hosts: db_servers
  tasks:
    - name: Configure PostgreSQL root user password
      community.postgresql.postgresql_user:
        name: dbadmin
        password: "{{ vault_postgres_admin_password }}"
        state: present
      no_log: true   # CRITICAL: Suppresses printing the password in execution logs!
```

Without `no_log: true`, running `ansible-playbook -v` or `-vvv` will dump the parameter dictionary—including the unencrypted password—directly to the terminal.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Prev Module (06-Ansible-Roles-and-Directory-Structure)](../06-Ansible-Roles-and-Directory-Structure/12-Quick-Revision.md) | [Index](../../../README.md) | [02 - Ansible Vault CLI encrypt decrypt edit view rekey →](./02-Ansible-Vault-CLI-encrypt-decrypt-edit-view-rekey.md) |
