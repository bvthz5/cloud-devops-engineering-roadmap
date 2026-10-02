# 06 - Group Management

Group management enables administrators to apply permissions efficiently to multiple users simultaneously across shared directories, files, and system resources.

---

## 👥 Group Commands Overview

Linux provides dedicated utilities for creating, modifying, and managing group structures:
* `groupadd` - Create a new group
* `groupmod` - Modify group attributes
* `groupdel` - Delete a group
* `gpasswd` - Administer group passwords and group membership
* `groups` / `id` - Display user group memberships

---

## 🛠️ 1. Creating Groups (`groupadd`)

```bash
groupadd [options] groupname
```

### Options & Examples

* **Create standard group:**
  ```bash
  sudo groupadd devops
  ```
* **Create group with specific numerical GID:**
  ```bash
  sudo groupadd -g 1500 devops
  ```
* **Create system group (GID < 1000):**
  ```bash
  sudo groupadd -r sysadmins
  ```

---

## ✏️ 2. Modifying Groups (`groupmod`)

```bash
groupmod [options] groupname
```

* **Rename a group:**
  ```bash
  sudo groupmod -n cloudops devops
  ```
* **Change group GID:**
  ```bash
  sudo groupmod -g 1600 cloudops
  ```
  *(Note: Files previously owned by GID 1500 will retain numerical 1500 and will require `chgrp` or `find / -gid 1500` updates).*

---

## ❌ 3. Deleting Groups (`groupdel`)

```bash
sudo groupdel cloudops
```
* **Restriction:** A group cannot be removed if it is currently defined as the **primary group** of any existing user account in `/etc/passwd`. Change the user's primary group first using `usermod -g`.

---

## 🔑 4. Managing Group Membership (`gpasswd`)

`gpasswd` is a versatile administration tool for managing `/etc/group` and `/etc/gshadow`.

### Key `gpasswd` Options

| Command | Action |
| :--- | :--- |
| `sudo gpasswd -a alice devops` | **Add** user `alice` to group `devops`. |
| `sudo gpasswd -d alice devops` | **Delete / Remove** user `alice` from group `devops`. |
| `sudo gpasswd -M alice,bob devops` | Set **Members list** explicitly (overwrites existing members!). |
| `sudo gpasswd -A alice devops` | Assign user `alice` as **Group Administrator** in `/etc/gshadow`. |

---

## 🔄 Switching Active Primary Group (`newgrp`)

When a user is added to a new supplementary group, the change does **not** take effect immediately in the current running shell session because process token credentials are immutable after login.

To activate new group permissions without logging out and back in:
```bash
newgrp devops
```
This spawns a sub-shell where `devops` becomes the user's active primary group for new file creations.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [05 - User Modification and Deletion](./05-User-Modification-and-Deletion.md) | [README](./README.md) | [07 - Sudo Privilege Escalation and Sudoers](./07-Sudo-Privilege-Escalation-and-Sudoers.md) |
