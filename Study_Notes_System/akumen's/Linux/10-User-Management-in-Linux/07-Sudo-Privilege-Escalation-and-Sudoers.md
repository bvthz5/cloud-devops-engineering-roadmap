# 07 - Sudo, Privilege Escalation, and Sudoers

Privilege escalation allows non-root users to execute administrative tasks safely without revealing or sharing the root user password.

---

## ⚡ `su` vs. `sudo` Comparison

| Feature | `su` (Switch User) | `sudo` (SuperUser Do) |
| :--- | :--- | :--- |
| **Authentication Requirement** | Requires **Target user's password** (e.g., Root password) | Requires **Invoking user's own password** |
| **Granularity** | All-or-nothing full administrative shell access | Fine-grained per-command authorization policy |
| **Audit Logging** | Low visibility into commands executed inside `su` shell | Detailed audit logging to `/var/log/auth.log` or `/var/log/secure` |
| **Security Risk** | Requires sharing root password among admins | Root password remains secret; access can be revoked instantly |

### Interactive Shell Switches
* `su` - Switches to root user environment while retaining current user environment variables.
* `su -` or `su -l` - Switches to root user with a **clean login shell** (loads root `.bash_profile`, `PATH`, and working directory `/root`).
* `sudo -i` - Simulates initial root login shell via sudo rules.
* `sudo -s` - Runs shell specified by `$SHELL` environment variable with root privileges.

---

## 📄 `/etc/sudoers` and `visudo`

The `/etc/sudoers` configuration file defines authorization rules controlling which users and groups can execute specific commands as root (or other target accounts).

> ⚠️ **CRITICAL RULE:** **NEVER edit `/etc/sudoers` directly with `nano` or `vi`!**
> Always use **`visudo`**. `visudo` locks the file against concurrent edits and performs strict syntax checks before saving. A syntax error in `/etc/sudoers` can completely lock all administrators out of root access!

```bash
sudo visudo
```

---

## 📝 Syntax Structure of Sudoers Rules

A standard sudoers rule follows this format:

```text
user_or_%group   host_list=(target_users:target_groups) [options] command_list
```

### Examples Breakdown

1. **Full Administrative Access (Password Required):**
   ```text
   alice   ALL=(ALL:ALL) ALL
   ```
   * `alice`: The user account.
   * `ALL`: Applies on ALL hostname servers.
   * `(ALL:ALL)`: Can run commands as ALL target users and ALL target groups.
   * `ALL`: Can run ALL executable commands.

2. **Group Access Rule:**
   ```text
   %sysadmins  ALL=(ALL) ALL
   ```
   * `%`: Percent sign indicates a group name (`sysadmins`).

3. **`NOPASSWD` Rule (Passwordless Execution):**
   Used for automation service accounts, CI/CD runners, and monitoring agents:
   ```text
   jenkins ALL=(ALL) NOPASSWD: /usr/bin/systemctl restart nginx, /usr/bin/docker
   ```
   * User `jenkins` can run `systemctl restart nginx` and `docker` without entering a password.

4. **Restricting Commands (Least Privilege):**
   ```text
   bob ALL=(ALL) /usr/bin/systemctl restart httpd, /usr/bin/systemctl status httpd
   ```
   * User `bob` can ONLY restart or check status of `httpd`. He cannot run `systemctl stop httpd` or edit configs.

---

## 🐧 Distribution Differences: Debian/Ubuntu vs. RHEL/CentOS

Different Linux distributions manage default administrative sudo groups differently out of the box:

```text
               ┌──────────────────────────────────────────────┐
               │         Default Administrative Groups        │
               └──────────────────────┬───────────────────────┘
                                      │
              ┌───────────────────────┴───────────────────────┐
              │                                               │
              ▼                                               ▼
  ┌──────────────────────┐                       ┌─────────────────────────┐
  │    Debian / Ubuntu   │                       │ RHEL / Rocky / Fedora   │
  │     Group: sudo      │                       │     Group: wheel        │
  ├──────────────────────┤                       ├─────────────────────────┤
  │ Sudoers Rule:        │                       │ Sudoers Rule:           │
  │ %sudo ALL=(ALL:ALL)  │                       │ %wheel ALL=(ALL) ALL    │
  │ ALL                  │                       │                         │
  └──────────────────────┘                       └─────────────────────────┘
```

* **Debian / Ubuntu:** Default admin group is **`sudo`**. Adding a user to the `sudo` group (`usermod -aG sudo username`) grants full sudo privileges.
* **RHEL / Rocky / CentOS / Fedora:** Default admin group is **`wheel`**. Adding a user to `wheel` (`usermod -aG wheel username`) grants administrative rights.

---

## 📁 Sudoers Drop-In Directory (`/etc/sudoers.d/`)

Rather than modifying `/etc/sudoers` directly, modern DevOps environments deploy modular configuration files into `/etc/sudoers.d/`:

```bash
# Example: Adding /etc/sudoers.d/devops file via visudo -f
sudo visudo -f /etc/sudoers.d/devops
```

**File Contents (`/etc/sudoers.d/devops`):**
```text
# Grant devops team passwordless NGINX restart permissions
%devops ALL=(ALL) NOPASSWD: /usr/bin/systemctl restart nginx
```

### File Requirements for `/etc/sudoers.d/`:
1. File permissions must be set to `0440` (`chmod 0440 /etc/sudoers.d/devops`).
2. Owned by `root:root`.
3. File names **must NOT contain dots (`.`) or end in `~`**. (e.g., `50-devops` is valid; `devops.conf` will be ignored by sudo!).
