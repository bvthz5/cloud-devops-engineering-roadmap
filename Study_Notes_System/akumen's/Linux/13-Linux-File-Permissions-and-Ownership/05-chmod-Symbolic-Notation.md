# 05 - chmod: Symbolic Notation

The `chmod` (change mode) command is used to modify file and directory permissions. 

**Symbolic notation** uses letters and operators to change permissions. It is highly intuitive and is best used when you want to tweak existing permissions (e.g., "just add execute for the user") without changing or knowing the other permissions.

---

## 🔤 The Symbolic Syntax

```bash
chmod [Target][Operator][Permission] file
```

### 1. Targets (Who?)

| Letter | Meaning |
| :---: | :--- |
| `u` | User (Owner) |
| `g` | Group |
| `o` | Others (World) |
| `a` | All (`u`, `g`, and `o`) |

*(If no target is specified, `a` is assumed, subject to the `umask`).*

### 2. Operators (How?)

| Symbol | Meaning |
| :---: | :--- |
| `+` | Add the permission |
| `-` | Remove the permission |
| `=` | Set exact permission (removes existing, sets only what is specified) |

### 3. Permissions (What?)

| Letter | Meaning |
| :---: | :--- |
| `r` | Read |
| `w` | Write |
| `x` | Execute |

---

## 📝 Common Examples

### Adding Permissions

Make a script executable for the owner:
```bash
chmod u+x deploy.sh
```

Give the group write access to a directory:
```bash
chmod g+w /shared/data
```

Make a file readable by everyone:
```bash
chmod a+r document.txt
# Or simply:
chmod +r document.txt
```

### Removing Permissions

Remove write access for Others (a common security fix):
```bash
chmod o-w config.yaml
```

Remove execute permission for everyone:
```bash
chmod a-x malware.bin
```

### Setting Exact Permissions

The `=` operator wipes out existing permissions for the target and sets exactly what you provide.

Set the group to read-only (removes write or execute if they existed):
```bash
chmod g=r confidential.pdf
```

Strip all permissions from Others:
```bash
chmod o= secret.key
# Leaves Others with ---
```

---

## 🔗 Combining Multiple Changes

You can perform multiple changes in a single command by separating them with commas (no spaces!).

Add execute for User, remove write from Group and Others:
```bash
chmod u+x,go-w script.py
```

Set User to read/write, Group to read, Others to none:
```bash
chmod u=rw,g=r,o= private.key
```

---

## 🔄 Recursive Changes (`-R`)

To apply changes to a directory and everything inside it, use the uppercase `-R` flag.

```bash
chmod -R g+w /var/www/html
# Adds group write access to the directory and all files/folders inside it.
```

### The Capital `X` Trick

When making recursive changes, adding `+x` is dangerous because it makes all plain text files executable.

Using uppercase `+X` tells `chmod` to add execute permissions **only** to directories, or to files that already have execute permission set for someone else.

```bash
chmod -R a+X /var/www/html
# Ensures all subdirectories can be entered, without making .html files executable.
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [04 - Inspecting Permissions ls namei stat](./04-Inspecting-Permissions-ls-namei-stat.md) | [README](./README.md) | [06 - chmod Octal Notation](./06-chmod-Octal-Notation.md) |
