# 8. File Permissions and Umask

## Critical System File Permissions
Ensure core system configuration and credential files have strict permission masks:

| File / Directory | Recommended Permissions | Command |
| --- | --- | --- |
| `/etc/passwd` | `644` (`rw-r--r--`) | `sudo chmod 644 /etc/passwd` |
| `/etc/shadow` | `600` (`rw-------`) | `sudo chmod 600 /etc/shadow` |
| `/etc/group` | `644` (`rw-r--r--`) | `sudo chmod 644 /etc/group` |
| `/etc/gshadow` | `600` (`rw-------`) | `sudo chmod 600 /etc/gshadow` |
| `/etc/sudoers` | `440` (`r--r-----`) | `sudo chmod 440 /etc/sudoers` |
| `/etc/ssh/sshd_config` | `600` (`rw-------`) | `sudo chmod 600 /etc/ssh/sshd_config` |

## Umask Setting
`umask` determines default permissions assigned to newly created files and directories.

- **Default Standard:** `022` (Files: `644`, Directories: `755`)
- **Hardened Standard:** `027` (Files: `640`, Directories: `750`) or `077` (Private)

Set hardened default umask in `/etc/profile` or `/etc/bash.bashrc`:
```bash
umask 027
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [07 - Fail2Ban Intrusion Prevention](./07-Fail2Ban-Intrusion-Prevention.md) | [README](./README.md) | [09 - SUID SGID and World Writable Files](./09-SUID-SGID-and-World-Writable-Files.md) |
