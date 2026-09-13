# 14 - Quick Revision Cheat Sheet

High-density 5-minute summary for fast review before interviews, exams, or on-call tasks.

---

## 🔑 The Fundamentals

| Concept | Explanation |
| :--- | :--- |
| **Classes (Who)** | **U**ser (Owner), **G**roup, **O**thers (World), **A**ll |
| **Rights (What)** | **R**ead (r/4), **W**rite (w/2), e**X**ecute (x/1) |
| **Logic** | Linux checks sequentially: Root? -> Owner? -> Group? -> Others? (Stops at first match). |

---

## 📁 Files vs. Directories

| Permission | On a File... | On a Directory... |
| :---: | :--- | :--- |
| **Read (`r`)** | View contents (`cat`) | List contents (`ls`) — *requires x* |
| **Write (`w`)** | Edit contents (`vi`) | Create/Delete files inside — *requires x* |
| **Execute (`x`)**| Run as a script/program | Enter (`cd`) / Pass-through traversal |

> **Critical Note:** You delete a file using the **Write** permission of the **Directory** it lives in, not the permissions of the file itself!

---

## 🧮 Octal Permissions (The Magic Numbers)

`r=4`, `w=2`, `x=1`. Add them up.

| Number | String | Meaning | Common Usage |
| :---: | :---: | :--- | :--- |
| **755** | `rwxr-xr-x` | Owner has full, Others read/enter. | Standard Directories & Executable Scripts |
| **644** | `rw-r--r--` | Owner can edit, Others read-only. | Standard Regular Files (Configs, HTML) |
| **700** | `rwx------` | Only Owner has any access. | Private Directories (`~/.ssh`) |
| **600** | `rw-------` | Only Owner can read/write. | Private Keys (`id_rsa`) |
| **777** | `rwxrwxrwx` | Wide open. Anyone can do anything. | **DANGEROUS.** Avoid unless using Sticky Bit. |

---

## ✂️ chmod One-Liners (Symbolic)

```text
chmod u+x file.sh       → Make executable for owner
chmod a+r data.txt      → Make readable by everyone
chmod g-w shared/       → Remove write access from group
chmod o= secret.key     → Strip all permissions from others
chmod -R a+X /dir       → Recursively add enter/execute to directories only
```

---

## 👤 chown One-Liners (Ownership)

```text
sudo chown alice file.txt        → Change User to alice
sudo chown alice:devs file.txt   → Change User to alice, Group to devs
sudo chown :devs file.txt        → Change Group only (same as chgrp)
sudo chown -R nginx:nginx /var/www → Recursively change user and group
```

---

## 🌟 Special Permissions (Prepended 4th Digit)

| Special | Octal | Applies To | What it does | Example Use Case |
| :--- | :---: | :---: | :--- | :--- |
| **SUID** | `4xxx` | Executables | Runs with privileges of file owner. | `/usr/bin/passwd` (runs as root) |
| **SGID** | `2xxx` | Directories | New files inherit directory's group. | Collaborative team folders |
| **Sticky**| `1xxx` | Directories | Only file owner can delete their file. | `/tmp` directory (`1777`) |

---

## 🕵️ Troubleshooting "Permission Denied"

1.  **Who am I?** Run `id` to check your User and Groups.
2.  **File Perms:** Run `ls -l <file>` or `stat <file>`. Do your User/Group classes grant access?
3.  **Directory Path:** Run `namei -m /full/path/to/file`. You need `x` on every single directory in the path.

---

## 🎯 30-Second Interview Answer

> *"Linux permissions govern access through three classes: User, Group, and Others, each possessing independent Read, Write, and Execute rights. These rights function differently on files versus directories—where directory execute acts as a traversal pass. Permissions are manipulated using `chmod` (via symbolic or octal notation like 755/644), while ownership is managed via `chown`. For complex shared or secure environments, special permissions like SGID for collaborative folders and the Sticky Bit for shared scratch spaces like `/tmp` are essential."*
