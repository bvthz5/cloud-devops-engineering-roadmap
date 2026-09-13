# Linux User Management & Privilege Escalation

## Purpose
Understand the Linux identity model, control privilege escalation through `sudo`, and secure user accounts for production environments.

```text
The Identity Core
/etc/passwd  →  WHO exists
/etc/shadow  →  HOW they authenticate
/etc/sudoers →  WHAT they can escalate to
```

## Source Foundation

The supplied material covers `sudo` fundamentals and options (`sudo -l`, `sudo -i`, `sudo -s`, `sudo -u`), the `/etc/sudoers` file and its syntax, `visudo` for safe editing with syntax validation, modular rules under `/etc/sudoers.d/`, `NOPASSWD` directives, and the principle of least privilege.

It also covers `/etc/passwd` (all 7 fields) and `/etc/shadow` (all 9 fields), including password hash formats (`$1$`, `$5$`, `$6$`, `$y$`), account locking/unlocking, `chage` for password aging, UID 0 security auditing, and service accounts with `/sbin/nologin`.

The source specifically recommends using `visudo` rather than directly editing `/etc/sudoers`, and using modular rules under `/etc/sudoers.d/`.

The original source concepts have been expanded into a structured study module with security auditing checklists, privilege escalation attack vectors, real-world DevOps scenarios, and hands-on practice labs.

## Rule of thumb
*   Need to grant root access safely? → `sudo` + `visudo` + `/etc/sudoers.d/`
*   Need to create an automation account? → `useradd --system --shell /sbin/nologin`
*   Need to audit security? → Check UID 0, SUID binaries, stale accounts
*   Need to lock out a user immediately? → `passwd -l` + `chage -E 0` + `usermod -s /sbin/nologin`

## Learning path
*   Linux users, groups, UID/GID
*   `/etc/passwd` — all 7 fields
*   `/etc/shadow` — all 9 fields
*   Creating and managing users
*   `sudo` fundamentals and options
*   `/etc/sudoers` and `visudo`
*   `/etc/sudoers.d/` modular rules
*   `NOPASSWD` and least-privilege design
*   Password management and aging (`chage`)
*   Service accounts and `/sbin/nologin`
*   Privilege escalation concepts
*   Security auditing
*   Real-world DevOps scenarios
*   Troubleshooting
*   Interview Q&A
*   Hands-on terminal practice
*   MCQs
*   Quick revision
*   Related topics: PAM, SSH, ACLs, SELinux, LDAP, Cloud IAM
