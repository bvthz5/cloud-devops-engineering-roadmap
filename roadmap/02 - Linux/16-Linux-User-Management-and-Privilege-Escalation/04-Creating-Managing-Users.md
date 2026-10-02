# 04 - Creating and Managing Users

Linux provides a set of commands for the full lifecycle of user accounts: creation, modification, and deletion.

---

## ➕ Creating Users (`useradd`)

`useradd` creates a new user account. On most distributions, it adds entries to `/etc/passwd`, `/etc/shadow`, and `/etc/group`.

### Basic Usage
```bash
sudo useradd alice
```
*This creates the user but does NOT set a password, create a home directory (on some distros), or configure a login shell beyond defaults.*

### The Production-Ready Command
In practice, you always want to specify a home directory and a shell:

```bash
sudo useradd -m -s /bin/bash -c "Alice Smith" alice
```

| Flag | Purpose |
| :--- | :--- |
| `-m` | Create the home directory (`/home/alice`). |
| `-s /bin/bash` | Set the login shell. |
| `-c "Alice Smith"` | Set the GECOS/comment field (full name). |
| `-G sudo,docker` | Add to supplementary groups (comma-separated, no spaces). |
| `-u 1050` | Specify a custom UID. |
| `-e 2027-01-01` | Set an account expiration date. |

### Setting the Password
`useradd` does not prompt for a password. You must set it separately:
```bash
sudo passwd alice
```

---

## ✏️ Modifying Users (`usermod`)

`usermod` changes the properties of an existing user.

```bash
# Change Alice's login shell to zsh
sudo usermod -s /bin/zsh alice

# Add Alice to the 'docker' group without removing her from existing groups
sudo usermod -aG docker alice

# Lock Alice's account (prepends '!' to her password hash in /etc/shadow)
sudo usermod -L alice

# Unlock Alice's account
sudo usermod -U alice
```

> **Critical Flag: `-a` (Append):** When adding supplementary groups, you **must** use `-aG` (append to groups). If you use `-G` without `-a`, `usermod` **replaces** all existing supplementary groups with the new list, potentially revoking `sudo` access.

---

## ➖ Deleting Users (`userdel`)

`userdel` removes a user account.

```bash
# Remove the user, but keep their home directory and mail spool
sudo userdel alice

# Remove the user AND delete their home directory and mail spool
sudo userdel -r alice
```

---

## 👥 Managing Groups

```bash
# Create a new group
sudo groupadd devteam

# Add an existing user to the group
sudo usermod -aG devteam alice

# Delete a group
sudo groupdel devteam

# View members of a group
getent group devteam
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [03 - etc shadow Deep Dive](./03-etc-shadow-Deep-Dive.md) | [README](./README.md) | [05 - sudo Fundamentals](./05-sudo-Fundamentals.md) |
