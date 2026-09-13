# 07 - /etc/sudoers.d/ — Modular Rules

Editing the monolithic `/etc/sudoers` file for every user or automation script is risky and hard to manage. The modern approach is to use **drop-in files** in the `/etc/sudoers.d/` directory.

---

## 📂 How It Works

At the bottom of the default `/etc/sudoers` file, you will find a line like:

```text
#includedir /etc/sudoers.d
```

*(Note: The `#` is NOT a comment here. It is part of the `#includedir` directive. This is a common source of confusion.)*

This directive tells `sudo` to read all files inside `/etc/sudoers.d/` and treat them as extensions of the main sudoers configuration.

---

## ✅ Benefits of Using `/etc/sudoers.d/`

1.  **Modularity:** Each user, group, or application gets its own clean, isolated file.
2.  **Automation-Friendly:** Configuration management tools (Ansible, Puppet, Chef) can drop a file into `/etc/sudoers.d/` without modifying the main `sudoers` file, reducing merge conflicts.
3.  **Easy Revocation:** To remove a user's sudo access, just delete their file. No need to edit a large, complex sudoers file.
4.  **Reduced Risk:** A syntax error in a drop-in file only breaks that specific file's rules (depending on the sudo version), not the entire sudo system.

---

## 🛠️ Creating a Drop-in File

**Always use `visudo` with the `-f` flag** to create and validate drop-in files:

```bash
sudo visudo -f /etc/sudoers.d/alice
```

### Example Content: Grant Alice Full Sudo
```text
alice   ALL=(ALL:ALL)   ALL
```

### Example Content: Allow the 'deploy' User to Restart Services Without a Password
```text
deploy   ALL=(root)   NOPASSWD: /usr/bin/systemctl restart nginx, /usr/bin/systemctl restart myapp
```

Save and exit. `visudo` will validate the syntax before writing the file.

---

## 📛 File Naming Rules

*   **No dots (`.`) or tildes (`~`) in the filename.** Files containing `.` or `~` are silently ignored by `sudo`. This is a safety mechanism to prevent editor backup files (like `alice.bak` or `alice~`) from being loaded.
*   Use descriptive names: `90-deploy-user`, `alice`, `ci-runner`.
*   **Permissions:** The file must be owned by `root:root` and have permissions `0440` (`-r--r-----`). `visudo -f` handles this automatically.

---

## 🔍 Verifying Configuration

After creating a drop-in file, verify it works:

```bash
# As alice:
sudo -l
```
This should list the permissions defined in her drop-in file.
