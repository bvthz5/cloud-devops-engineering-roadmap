# 05 - sudo Fundamentals

`sudo` (Superuser Do) allows an authorized user to execute a command as another user — typically `root` — without logging in as that user. It is the cornerstone of safe privilege escalation.

---

## 🔑 Why Not Just Log in as root?

1.  **Accountability:** `sudo` logs every command executed with elevated privileges (to `/var/log/auth.log` or `journalctl`). If four admins share the `root` password, you cannot tell who ran `rm -rf /`.
2.  **Principle of Least Privilege:** Users operate with their own (limited) permissions for 99% of their work. They only escalate to root for specific, auditable commands.
3.  **Reduced Attack Surface:** If an attacker compromises a normal user account, they still don't have root. They must find a *second* exploit to escalate.

---

## 🛠️ Core `sudo` Commands

### 1. Execute a Command as root (The Default)
```bash
sudo apt update
```
*You are prompted for **your own** password (not the root password). Once authenticated, `sudo` caches your credentials for a short period (usually 15 minutes).*

### 2. Check Your Privileges (`sudo -l`)
This is the first thing you should run on any new server. It shows exactly what commands your user is allowed (or denied) to run via `sudo`.
```bash
sudo -l
```
*Output example:*
```text
User alice may run the following commands on server01:
    (ALL : ALL) ALL
```
*(This means alice can run any command as any user on this host).*

### 3. Open a Root Shell (`sudo -i`)
Opens a full login shell as root. This loads root's profile and environment variables, as if you had logged in as root directly.
```bash
sudo -i
```
*(Your prompt changes to `root@server#`. Type `exit` to return to your normal user).*

### 4. Open a Root Shell Without Profile (`sudo -s`)
Opens a root shell but **keeps your current environment variables** and does not run root's login profile scripts. Slightly less "clean" than `-i`, but faster and keeps your aliases.
```bash
sudo -s
```

### 5. Execute as a Different User (`sudo -u`)
You can run a command as any user, not just root.
```bash
sudo -u postgres psql
```
*This opens the PostgreSQL command-line as the `postgres` user — the standard way to administer PostgreSQL.*

---

## 📊 `sudo -i` vs `sudo -s` vs `sudo su`

| Command | Shell Type | Environment | Use Case |
| :--- | :--- | :--- | :--- |
| `sudo -i` | Login shell | Root's environment (clean). | When you need a full root session. |
| `sudo -s` | Non-login shell | Keeps your current environment. | Quick root shell for a few commands. |
| `sudo su` | Login shell (via `su`) | Root's environment. | Legacy habit; `sudo -i` is preferred. |
| `sudo su -` | Login shell (via `su -`) | Root's environment (clean). | Identical to `sudo -i` in practice. |

> **Best Practice:** Use `sudo -i` for a clean root session. Use `sudo <command>` for individual commands. Avoid `sudo su` as it is redundant.
