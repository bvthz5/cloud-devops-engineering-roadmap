# 03 - Ownership and Groups

Every file and directory in Linux has two primary owners: a **User Owner** and a **Group Owner**. This dual-ownership system is the foundation for determining which set of permissions (from the `rwx` triads) applies to any given person trying to access the file.

---

## 👤 1. The User (Owner)

*   **What is it?** The single user account that owns the file.
*   **Default Behavior:** When you create a new file (e.g., using `touch` or saving in an editor), you automatically become the User Owner of that file.
*   **Which permissions apply?** If the person accessing the file is the User Owner, Linux looks *only* at the first triad (the **User permissions**). It ignores the Group and Others permissions entirely.
*   **Superpower:** Only the User Owner (and the root user) can change the permissions of a file using `chmod`.

---

## 👥 2. The Group

*   **What is it?** A collection of users defined on the system (usually in `/etc/group`).
*   **Default Behavior:** When you create a file, the Group Owner is usually set to your primary group (often a group with the same name as your username).
*   **Which permissions apply?** If the person accessing the file is *not* the User Owner, but they *are* a member of the Group Owner, Linux looks *only* at the second triad (the **Group permissions**).
*   **Why use groups?** Groups allow you to grant permissions to a specific team without making the file public. For example, you can create a `developers` group, assign it as the Group Owner of `/var/www/html`, and give the group `rwx` access.

---

## 🌍 3. Others (The World)

*   **What is it?** Everyone else. Any user account on the system that is neither the User Owner nor a member of the Group Owner.
*   **Which permissions apply?** If the person accessing the file is neither the owner nor in the group, Linux applies the third triad (the **Others permissions**).
*   **Security Risk:** Giving write (`w`) or execute (`x`) permissions to Others is often a security risk. Usually, Others should only have read (`r`) access, or no access (`-`).

---

## 👑 The Root User Exception

The `root` user (UID 0) is the superuser. **The root user bypasses all permission checks.**

*   Even if a file is owned by `alice`, and the permissions are `---------` (no access for anyone), `root` can still read, write, and delete that file.
*   The only exception is execution: `root` cannot execute a file unless at least *one* of the `x` bits is set (by User, Group, or Others), as a safety mechanism to prevent executing non-executable text files.

---

## 🛠️ How Permissions are Evaluated (The Logic Flow)

When a user requests access to a file, the Linux kernel evaluates permissions in a strict sequence. It stops at the first match.

1.  **Is the user `root`?**
    *   Yes → Grant access (bypass checks).
    *   No → Proceed to step 2.
2.  **Is the user the User Owner?**
    *   Yes → Apply the User Permissions. (Stop checking).
    *   No → Proceed to step 3.
3.  **Is the user in the Group Owner?**
    *   Yes → Apply the Group Permissions. (Stop checking).
    *   No → Proceed to step 4.
4.  **Apply Others Permissions.**

### The "Lockout" Scenario

Because evaluation stops at the first match, you can create interesting (and sometimes frustrating) scenarios.

Imagine a file owned by `alice`, group `devs`.
Permissions: `---rwxrwx`

*   **User (alice):** Has `---`. She cannot read or write her own file!
*   **Group (devs):** Has `rwx`. Bob (in the `devs` group) can read, write, and execute the file.
*   **Others:** Has `rwx`.

When Alice tries to read the file, the kernel checks step 2: "Is Alice the owner?" Yes. It applies the User Permissions (`---`) and denies access. It does not matter that the Group and Others have access, because the kernel stopped evaluating. Alice would have to use `chmod u+r` on the file to regain access.
