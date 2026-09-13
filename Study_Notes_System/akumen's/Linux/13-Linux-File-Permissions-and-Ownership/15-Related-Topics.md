# 15 - Related Topics

Once you have mastered the standard Linux permission model (`rwx`, `ugo`, `chmod`, `chown`), you will encounter situations where this model is not granular enough. The following advanced topics extend or override standard permissions.

---

## 🛡️ Access Control Lists (ACLs)

**What it is:** A system that allows you to grant specific permissions to specific users or groups, beyond the standard User/Group/Others triad.
**Why it matters:** Standard permissions only allow one owner and one group. If you need Alice to have read access, Bob to have write access, and the `devs` group to have execute access, standard `rwx` fails. ACLs solve this.
*   **Commands:** `getfacl` (view ACLs), `setfacl` (modify ACLs).
*   **Indicator:** An `+` sign at the end of the `ls -l` permissions string (e.g., `-rw-rw-r--+`).

---

## 🎭 `umask` (User File-Creation Mask)

**What it is:** A setting that determines the default permissions assigned to newly created files and directories.
**Why it matters:** By default, Linux wants to give `666` to files and `777` to directories. The `umask` subtracts from these defaults. A standard `umask` of `022` results in directories being `755` (`777 - 022`) and files being `644` (`666 - 022`).
*   **Commands:** `umask` (view current), `umask 027` (set new mask).

---

## 🔒 Mandatory Access Control (SELinux & AppArmor)

**What it is:** Security modules integrated into the Linux kernel that enforce security policies far beyond standard permissions.
**Why it matters:** Standard permissions are Discretionary Access Control (DAC)—the owner decides who gets access. SELinux (common on RHEL/CentOS) and AppArmor (common on Ubuntu) use Mandatory Access Control (MAC). Even if a file is `chmod 777`, SELinux can block a web server from reading it if the file lacks the correct SELinux "context" tag (e.g., `httpd_sys_content_t`).
*   **Commands:** `getenforce`, `sestatus`, `chcon`, `restorecon`, `ls -Z`.

---

## 🔑 `sudo` and `visudo`

**What it is:** The mechanism allowing authorized users to execute commands as the superuser (`root`) or another user.
**Why it matters:** Directly logging in as `root` is a security risk. `sudo` provides granular control over who can run what commands, and logs all actions. It bypasses standard file permissions.
*   **Commands:** `sudo`, `sudo -i`, `visudo` (safely edits `/etc/sudoers`).

---

## 🧩 Linux Capabilities

**What it is:** A system that breaks down the monolithic power of `root` into smaller, distinct privileges.
**Why it matters:** Instead of giving an executable SUID `root` (which gives it total control over the system), you can grant it a specific capability. For example, `ping` needs to open raw network sockets. Instead of making it SUID `root`, you can give it the `CAP_NET_RAW` capability.
*   **Commands:** `getcap`, `setcap`.

---

## 📦 Extended Attributes (xattr)

**What it is:** Metadata associated with a file, stored as name/value pairs, distinct from standard permissions.
**Why it matters:** Used for various advanced file system features, including SELinux contexts, capabilities, or custom application metadata. A file might be made immutable (cannot be deleted or modified even by root) using extended attributes.
*   **Commands:** `chattr` (`chattr +i file` makes it immutable), `lsattr`.

---

## 🐳 Container/Mount Permissions

**What it is:** Handling permissions across boundaries (e.g., Host OS to Docker Container, or mounting an NFS share).
**Why it matters:** A frequent DevOps headache is mapping User IDs (UIDs). If a process inside a Docker container runs as UID 1000, and it mounts a host volume owned by UID 1001, it will get "Permission denied," regardless of the usernames involved.
