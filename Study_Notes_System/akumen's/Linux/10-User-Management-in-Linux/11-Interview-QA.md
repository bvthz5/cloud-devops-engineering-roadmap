# 11 - Interview Questions and Answers

This collection covers technical and scenario-based interview questions on Linux User Management for DevOps, SRE, and System Administration roles.

---

## 🟢 Junior / Associate Level

### Q1: What is the difference between `/etc/passwd` and `/etc/shadow`? Why are passwords stored in `/etc/shadow`?
**Answer:**
* `/etc/passwd` stores user account metadata (Username, UID, Primary GID, Home directory, Default shell). It has world-readable permissions (`644`) so system utilities (`ls -l`, `id`, `whoami`) can convert numerical UIDs to usernames.
* `/etc/shadow` stores encrypted password hashes and password aging parameters. It has restricted permissions (`600` / `640` readable only by root/shadow). Storing encrypted hashes in a restricted file prevents unprivileged users from extracting hashes and cracking them offline using rainbow tables or hash-cracking tools.

---

### Q2: What is the difference between `useradd` and `adduser`?
**Answer:**
* `useradd` is a compiled binary executable found on all Linux distributions. It is non-interactive, low-level, and requires explicit flags (e.g. `-m` for home directory, `-s` for shell). It is preferred for automated shell scripts and Ansible roles.
* `adduser` is a high-level interactive Perl wrapper script native to Debian/Ubuntu. It automatically creates home directories, copies skeleton files from `/etc/skel`, sets up user private groups, and prompts interactively for passwords and GECOS information.

---

### Q3: What happens if you run `usermod -G docker alice` instead of `usermod -aG docker alice`?
**Answer:**
Omitting the `-a` (append) flag replaces the user's entire supplementary group list with **only** the group specified (`docker`). The user will be removed from all other secondary groups they previously belonged to (such as `sudo`, `wheel`, or `devs`). Adding the `-a` flag (`-aG`) appends the new group while preserving existing group memberships.

---

## 🟡 Mid-Level DevOps

### Q4: Explain the structural difference between primary group and supplementary groups in Linux.
**Answer:**
* **Primary Group:** Defined in the 4th field of `/etc/passwd`. Every user has exactly one primary group. By default, any new file or directory created by the user inherits this primary GID as its group owner.
* **Supplementary (Secondary) Groups:** Defined in `/etc/group`. A user can belong to zero or multiple supplementary groups. These groups grant additional access permissions to files, directories, or special devices (e.g. `/var/run/docker.sock`, `/dev/kvm`).

---

### Q5: How do you configure a user in `/etc/sudoers` to run specific administrative commands without entering a password?
**Answer:**
Using `visudo` (or adding a file in `/etc/sudoers.d/`), specify the `NOPASSWD:` tag before the list of target command binaries:
```text
jenkins ALL=(ALL) NOPASSWD: /usr/bin/systemctl restart nginx, /usr/bin/docker
```
This rule allows user `jenkins` to run `sudo systemctl restart nginx` and `sudo docker` without password prompts, while denying execution of unlisted commands as root.

---

## 🔴 Senior / Lead SRE Level

### Q6: If an attacker modifies `/etc/passwd` to change their user UID from `1001` to `0`, what access do they gain?
**Answer:**
In Linux, privilege level is governed exclusively by numerical UID. UID `0` is hardcoded in the Linux kernel as the Superuser (root). If an account's UID in `/etc/passwd` is changed to `0`, the kernel treats that user as root during system call permission checks, granting full unrestricted access to all files, devices, processes, and kernel capabilities, regardless of the username string.

---

### Q7: How does PAM (Pluggable Authentication Modules) interact with `/etc/shadow` during an SSH login attempt?
**Answer:**
When a user attempts SSH login via password:
1. `sshd` delegates authentication to the PAM stack (`/etc/pam.d/sshd`).
2. PAM invokes `pam_unix.so` (or `pam_sss.so` for SSSD/LDAP).
3. `pam_unix.so` reads the user's encrypted hash from `/etc/shadow`.
4. It hashes the user-provided password using the same salt and algorithm (e.g. SHA-512 `$6$` or yescrypt `$y$`).
5. If the computed hash matches `/etc/shadow`, PAM checks account aging parameters (expiration date, maximum age, inactivity period).
6. If authentication and account validity checks pass, PAM returns success, and `sshd` spawns the user's shell.
