# 06 - Home Directories: `/home` vs `/root`

A critical distinction in Linux administration is understanding the difference between the **Filesystem Root (`/`)**, standard user home directories (**`/home/username`**), and the administrative superuser home directory (**`/root`**).

---

## 🆚 Comparison Table

| Attribute | Filesystem Root (`/`) | User Home (`/home/username`) | Root User Home (`/root`) |
| :--- | :--- | :--- | :--- |
| **Role** | The top-level root directory of the entire OS hierarchy. | Home storage location for regular system users. | Dedicated home directory for the administrative `root` account. |
| **Example Path** | `/` | `/home/alice`, `/home/bob`, `/home/ubuntu` | `/root` |
| **Tilde Shortcut** | N/A | `~` (when logged in as `alice`) | `~` (when logged in as `root`) |
| **Standard Permissions** | `755` (`rwxr-xr-x`) | `750` (`rwxr-x---`) or `700` (`rwx------`) | `700` (`rwx------`) |
| **Partition Placement** | Mounted on primary root volume. | Frequently mounted on a separate partition/disk. | Kept on root volume (`/`) for single-user emergency boot. |

---

## 🔍 Why `/root` is NOT inside `/home`

A common question from beginners is: *Why is the root user's home directory located at `/root` instead of `/home/root`?*

### 1. Emergency Single-User Boot Maintenance
During system maintenance or repair boot scenarios (e.g., repairing corrupted filesystems), non-essential disk volumes—including `/home` or `/usr`—might fail to mount or remain unmounted.

If the root user's home directory were located at `/home/root`, the root account would lack a valid home directory, configuration files (`.bashrc`), or SSH keys during recovery.

### 2. Security Isolation
Keeping `/root` outside `/home` isolates administrative credentials, private SSH keys, and command histories from standard users who have accounts under `/home`.

---

## 🛠️ Navigating Home Directories

```bash
# Print current user's home directory
echo $HOME

# Change to current user's home directory (both equivalent)
cd ~
cd

# Change to specific user's home directory
cd ~alice

# Check home directory ownership and permissions
ls -ld /home/* /root
```
---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [05 - var](./05-var.md) | [README](./README.md) | [07 - opt and srv](./07-opt-and-srv.md) |
