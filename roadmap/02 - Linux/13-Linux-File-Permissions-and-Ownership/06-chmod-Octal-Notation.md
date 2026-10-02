# 06 - chmod: Octal (Numeric) Notation

While symbolic notation is great for tweaking, **octal (numeric) notation** is the professional standard for defining absolute permissions. It allows you to specify the exact permissions for User, Group, and Others simultaneously using a three-digit number.

Octal notation is faster to type and is universally used in configuration management (Ansible, Terraform, Puppet) and documentation.

---

## 🔢 The Numeric Values

Each permission is assigned a numeric value based on binary representation.

| Permission | Value | Binary |
| :--- | :---: | :---: |
| Read (`r`) | **4** | 100 |
| Write (`w`) | **2** | 010 |
| Execute (`x`) | **1** | 001 |
| None (`-`) | **0** | 000 |

To determine the final number for a triad, simply **add the values together**.

### Common Combinations

*   **7** = 4 + 2 + 1 (`rwx`) → Read, Write, Execute
*   **6** = 4 + 2 (`rw-`) → Read, Write
*   **5** = 4 + 1 (`r-x`) → Read, Execute
*   **4** = 4 (`r--`) → Read only
*   **0** = 0 (`---`) → No permissions

---

## 🧩 Building the 3-Digit Code

A standard `chmod` octal command uses three digits:

1.  **First digit:** User (Owner) permissions.
2.  **Second digit:** Group permissions.
3.  **Third digit:** Others permissions.

```bash
chmod 755 script.sh
```

**Breakdown of 755:**
*   `7` (User)  = 4+2+1 = `rwx`
*   `5` (Group) = 4+1 = `r-x`
*   `5` (Others)= 4+1 = `r-x`
*   Resulting string: `-rwxr-xr-x`

---

## 🌟 The Magic Numbers (Must Know!)

You should memorize these common octal permissions. They cover 95% of real-world use cases.

### For Files

| Code | String | Use Case |
| :--- | :--- | :--- |
| **644** | `-rw-r--r--` | **Standard File:** Owner can read/write. Everyone else can only read. (Configs, HTML, documents). |
| **600** | `-rw-------` | **Private File:** Only the owner can read/write. Group and Others have no access. (SSH private keys, password files). |
| **755** | `-rwxr-xr-x` | **Standard Executable:** Owner can read/write/execute. Everyone else can read/execute. (Scripts, binaries). |

### For Directories

| Code | String | Use Case |
| :--- | :--- | :--- |
| **755** | `drwxr-xr-x` | **Standard Directory:** Owner has full control. Everyone else can read and enter. |
| **700** | `drwx------` | **Private Directory:** Only the owner can read, write, or enter. (e.g., `~/.ssh`). |
| **777** | `drwxrwxrwx` | **Wide Open:** Anyone can read, write, and enter. **(Dangerous! Usually a security risk unless combined with the Sticky Bit).** |

---

## 🛠️ Octal vs Symbolic

**When to use Octal?**
When you want to force the file to an exact, known state, regardless of what its previous permissions were.
```bash
# Sets exact state: rw-r--r--
chmod 644 config.yml 
```

**When to use Symbolic?**
When you want to add or remove a specific permission but preserve the others.
```bash
# Adds execute for the owner, but doesn't change group/others
chmod u+x script.py
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [05 - chmod Symbolic Notation](./05-chmod-Symbolic-Notation.md) | [README](./README.md) | [07 - Special Permissions setuid setgid sticky bit](./07-Special-Permissions-setuid-setgid-sticky-bit.md) |
