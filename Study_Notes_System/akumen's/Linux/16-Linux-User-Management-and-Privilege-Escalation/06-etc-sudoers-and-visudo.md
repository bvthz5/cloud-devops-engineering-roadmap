# 06 - /etc/sudoers and visudo

The `/etc/sudoers` file is the master configuration that defines **who** can run **what** commands as **which** user on **which** host.

---

## ⚠️ NEVER Edit Directly!

> **Rule #1 from the source material:** Never edit `/etc/sudoers` with `vi`, `nano`, or any regular text editor. **Always use `visudo`.**

If you introduce a syntax error (even a missing comma) into `/etc/sudoers` with a regular editor, `sudo` will break completely. You will be locked out of root access, potentially requiring a reboot into single-user/recovery mode to fix.

### Why `visudo`?

`visudo` is a specialized editor that:
1.  **Locks the file:** Prevents two administrators from editing simultaneously.
2.  **Validates syntax:** When you save and exit, `visudo` parses the file. If it finds an error, it warns you and offers to re-edit, discard the changes, or save anyway (don't save).
3.  **Opens your default editor:** It uses the `EDITOR` environment variable (usually `vi` or `nano`).

```bash
sudo visudo
```

---

## 📐 Sudoers Syntax

The core rule format:

```text
WHO    WHERE = (AS_WHOM)    WHAT
```

Breaking it down:

```text
alice   ALL = (ALL:ALL)    ALL
```

| Field | Value | Meaning |
| :--- | :--- | :--- |
| **WHO** | `alice` | The user (or `%groupname` for a group). |
| **WHERE** | `ALL` | The hostname(s) this rule applies to. `ALL` means any host. Useful in centralized sudoers configs shared across many servers. |
| **AS_WHOM** | `(ALL:ALL)` | The user and group the command can be run as. `(ALL:ALL)` means any user and any group. `(root)` means only as root. |
| **WHAT** | `ALL` | The command(s) allowed. `ALL` means any command. Can be a specific path like `/usr/bin/systemctl restart nginx`. |

---

## 📝 Practical Examples

### Grant Full Root Access to a User
```text
alice   ALL=(ALL:ALL)   ALL
```

### Grant Full Root Access to a Group
```text
%devops   ALL=(ALL:ALL)   ALL
```
*(The `%` prefix indicates a group name).*

### Allow a User to Only Restart Nginx
```text
bob   ALL=(root)   /usr/bin/systemctl restart nginx
```
*Bob can run `sudo systemctl restart nginx` but nothing else.*

### Allow a User to Run Commands Without a Password
```text
deploy   ALL=(ALL)   NOPASSWD: ALL
```
*(Covered in detail in Chapter 8).*

---

## 🔒 The Default Lines (Debian/Ubuntu)

A typical default `/etc/sudoers` on Ubuntu includes:

```text
# Members of the admin group may gain root privileges
%admin ALL=(ALL) ALL

# Allow members of group sudo to execute any command
%sudo   ALL=(ALL:ALL) ALL
```

This is why adding a user to the `sudo` group (`usermod -aG sudo alice`) grants them full `sudo` access.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [05 - sudo Fundamentals](./05-sudo-Fundamentals.md) | [README](./README.md) | [07 - sudoers d Modular Rules](./07-sudoers-d-Modular-Rules.md) |
