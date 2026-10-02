# 01 - Users, Groups, UID, and GID

Linux is a multi-user operating system. The kernel does not understand usernames like "alice" — it only understands numbers. Every security decision is made based on numeric identifiers.

---

## 👤 What is a User?

A user is an identity that owns processes and files. There are three categories:

| Category | UID Range | Purpose | Example |
| :--- | :---: | :--- | :--- |
| **Root (Superuser)** | `0` | Has unrestricted access to everything. Bypasses all permission checks. | `root` |
| **System / Service** | `1–999` | Non-human accounts created for running daemons and services. They should never be used for interactive login. | `www-data`, `nginx`, `mysql` |
| **Regular Users** | `1000+` | Human users who log in interactively. | `alice`, `bob`, `deploy` |

---

## 🆔 UID (User ID)

The **UID** is the unique numeric identifier assigned to every user. The kernel uses UIDs — not usernames — for all access control decisions.

*   **UID 0** is always `root`. If any account has UID 0, it has full root power, regardless of its name.
*   UIDs are assigned sequentially when new users are created.

```bash
# View your own UID
id
# Output: uid=1001(alice) gid=1001(alice) groups=1001(alice),27(sudo)
```

---

## 👥 What is a Group?

A group is a collection of users. Groups simplify permission management — instead of granting access to each user individually, you grant access to a group.

### Primary Group vs. Secondary (Supplementary) Groups

*   **Primary Group:** Every user has exactly one primary group. When a user creates a new file, the file's group owner is set to the user's primary group. By default, Linux creates a private group with the same name as the user (e.g., user `alice` gets primary group `alice`).
*   **Secondary Groups:** A user can belong to additional groups (up to a system limit, typically 65,536). These grant additional permissions. For example, adding `alice` to the `sudo` group gives her `sudo` privileges on Debian/Ubuntu.

```bash
# View all groups for a user
groups alice
# Output: alice : alice sudo docker
```

---

## 🔢 GID (Group ID)

Just like UIDs for users, **GIDs** are the numeric identifiers for groups.

---

## 📁 The Identity Files

Linux stores user and group information in three plain-text files:

| File | Purpose |
| :--- | :--- |
| **`/etc/passwd`** | Maps usernames to UIDs, GIDs, home directories, and login shells. Readable by everyone. |
| **`/etc/shadow`** | Stores password hashes and password aging policies. Readable only by `root`. |
| **`/etc/group`** | Maps group names to GIDs and lists group members. |

These files are covered in depth in the following chapters.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [README (Index)](./README.md) | [README](./README.md) | [02 - etc passwd Deep Dive](./02-etc-passwd-Deep-Dive.md) |
