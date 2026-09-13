# 08 - Changing Ownership: chown and chgrp

While `chmod` changes *what* you can do with a file, `chown` (change owner) and `chgrp` (change group) change *who* owns the file.

Because changing ownership can bypass security boundaries and quotas, **only the root user (or a user with sudo privileges) can change the ownership of a file.** Even if you own a file, you cannot give it away to another user.

---

## 👤 `chown` - Change Owner

The primary tool for changing the User Owner of a file or directory.

### Basic Syntax

```bash
chown [NewUser] file
```

### Examples

Change the owner of `index.html` to the user `nginx`:
```bash
sudo chown nginx index.html
```

---

## 👥 `chgrp` - Change Group

Used specifically to change the Group Owner. (However, `chown` is often used to do this instead, as seen below).

### Basic Syntax

```bash
chgrp [NewGroup] file
```

### Examples

Change the group of a shared folder to `developers`:
```bash
sudo chgrp developers /shared/data
```

---

## 🤝 The Power of `chown` (User and Group Simultaneously)

In practice, DevOps engineers rarely use `chgrp` because `chown` can change *both* the User and the Group in a single command.

You do this by separating the User and Group with a colon (`:`).

### Syntax

```bash
chown [NewUser]:[NewGroup] file
```

### Examples

Change owner to `alice` and group to `devs`:
```bash
sudo chown alice:devs config.yml
```

Change ONLY the group using `chown` (notice the leading colon):
```bash
sudo chown :devs config.yml
# This is identical in function to `chgrp devs config.yml`
```

Change owner to `bob` and automatically set the group to Bob's primary login group:
```bash
sudo chown bob: config.yml
```

---

## 🔄 Recursive Ownership Changes (`-R`)

Just like `chmod`, you can apply ownership changes recursively to a directory and all its contents using the `-R` flag.

```bash
sudo chown -R www-data:www-data /var/www/html
```
*This is one of the most frequently typed commands when setting up a web server to ensure the application can read and write to its document root.*

---

## ⚠️ Warning: Recursive chown on `/` or Hidden Files

Be extremely careful with `chown -R`.

If you accidentally run:
```bash
# DISASTER COMMAND
sudo chown -R alice:alice /
```
You will irreversibly break the entire Linux operating system, as critical system binaries, `/etc` configurations, and device files will suddenly belong to `alice` instead of `root`, causing the system to fail immediately.

Also, be careful when using wildcards:
```bash
chown -R alice:alice .*
```
In some shells, `.*` expands to include `..` (the parent directory), which can recursively change ownership of parent directories unintentionally. It's safer to specify the directory explicitly: `chown -R alice:alice /path/to/folder`.
