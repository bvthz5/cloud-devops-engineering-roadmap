# 10 - Service Accounts and /sbin/nologin

Not all user accounts are for humans. Many exist solely to run background services (daemons) like Nginx, PostgreSQL, or Docker.

---

## 🤖 What is a Service Account?

A service account is a user specifically created to own and run a particular application or daemon. It should:

1.  **Never be used for interactive login.**
2.  **Own only the files and processes belonging to that service.**
3.  **Have a UID in the system range (1–999).**

---

## 🚫 `/sbin/nologin` and `/bin/false`

To prevent interactive login, the login shell in `/etc/passwd` (Field 7) is set to a non-interactive program.

*   **`/sbin/nologin`**: When someone attempts to log in as this user (e.g., via SSH), the system prints a polite message ("This account is currently not available") and immediately disconnects.
*   **`/bin/false`**: Does the same thing but silently (returns a "false/failure" exit code and disconnects).

Both achieve the same security goal. `/sbin/nologin` is preferred because it gives a descriptive message to the person attempting to log in.

---

## 🛠️ Creating a Service Account

```bash
sudo useradd \
  --system \
  --no-create-home \
  --shell /sbin/nologin \
  --comment "My Application Service Account" \
  myapp
```

| Flag | Purpose |
| :--- | :--- |
| `--system` | Assigns a UID in the system range (1–999), and does not create a home directory by default. |
| `--no-create-home` | Explicitly ensures no home directory is created. |
| `--shell /sbin/nologin` | Prevents interactive login. |

---

## 🔍 UID 0 Security Auditing

Any user with UID `0` has full, unrestricted root access to the entire system. There should only ever be **one** UID 0 account: `root`.

An attacker who gains access might create a hidden backdoor account with UID 0:

```text
# Malicious entry in /etc/passwd:
hacker:x:0:0::/root:/bin/bash
```

### The Audit Command
Regularly scan for unauthorized UID 0 accounts:

```bash
awk -F: '$3 == 0 {print $1}' /etc/passwd
```
*If this command returns anything other than `root`, you have a critical security breach.*

---

## 🏗️ DevOps Practice: Application Isolation

Each application or microservice on a server should run under its own dedicated service account.

*   Nginx runs as `www-data` or `nginx`.
*   PostgreSQL runs as `postgres`.
*   Docker daemon runs as `root` (but containers should run as non-root).
*   Your custom application runs as `myapp`.

This ensures that if one application is compromised (e.g., a vulnerability in Nginx), the attacker's access is limited to the files and processes owned by `www-data`. They cannot read PostgreSQL's data directory or your application's configuration files.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [09 - Password Management and Aging](./09-Password-Management-and-Aging.md) | [README](./README.md) | [11 - Privilege Escalation Concepts](./11-Privilege-Escalation-Concepts.md) |
