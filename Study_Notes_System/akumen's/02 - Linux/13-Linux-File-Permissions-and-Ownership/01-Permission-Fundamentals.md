# 01 - Permission Fundamentals

In Linux, everything is considered a file (even directories and hardware devices). Because Linux is designed to support multiple users simultaneously, it must have a rigorous system to determine who can access these files and what they can do with them.

---

## 🛡️ The Security Model

The Linux permissions model is based on three fundamental concepts:

1.  **Ownership Classes (Who?):** Who is trying to access the file?
    *   **User (u):** The owner of the file.
    *   **Group (g):** Other users who belong to the file's group.
    *   **Others (o):** Everyone else on the system (the world).
2.  **Permissions (What?):** What action are they trying to perform?
    *   **Read (r):** Viewing the contents.
    *   **Write (w):** Modifying the contents.
    *   **Execute (x):** Running the file as a program.
3.  **File Type:** Is the object a regular file, a directory, a symbolic link, etc.?

---

## 🔍 Decoding the 10-Character String

When you run `ls -l`, the first column displays a 10-character string representing the file type and permissions.

```text
-rwxr-xr--
```

This string is broken down into four distinct parts:

```text
  -     rwx     r-x     r--
  │      │       │       │
  │      │       │       └──▶ Permissions for Others (Read only)
  │      │       └──────────▶ Permissions for Group (Read, Execute)
  │      └──────────────────▶ Permissions for User/Owner (Read, Write, Execute)
  └─────────────────────────▶ File Type (- = file, d = directory, l = link)
```

### The File Type Character (1st Character)

| Character | Meaning |
| :---: | :--- |
| `-` | Regular File (text file, executable, zip, etc.) |
| `d` | Directory |
| `l` | Symbolic Link (shortcut) |
| `c` | Character Device (e.g., a terminal like `/dev/tty`) |
| `b` | Block Device (e.g., a hard drive like `/dev/sda`) |
| `s` | Socket (for process communication) |
| `p` | Named Pipe (FIFO) |

### The Permission Triads (Characters 2-10)

The remaining 9 characters are divided into three groups of three (triads). Each position in the triad has a specific meaning:

1.  **First position:** Always `r` (read) or `-` (denied).
2.  **Second position:** Always `w` (write) or `-` (denied).
3.  **Third position:** Always `x` (execute) or `-` (denied).

```text
User Triad (u)  |  Group Triad (g)  |  Others Triad (o)
    r w x       |      r - x        |      r - -
```

In the example above (`-rwxr-xr--`):
*   It is a regular file (`-`).
*   The **User** (owner) can read, write, and execute (`rwx`).
*   The **Group** can read and execute, but not write (`r-x`).
*   **Others** can only read (`r--`).

---

## 🧠 Why Triads? The Binary Connection

The `rwx` system maps directly to 3-bit binary numbers, which is the foundation of the **Octal Notation** we will learn in later chapters.

*   `r` = 4 (binary 100)
*   `w` = 2 (binary 010)
*   `x` = 1 (binary 001)
*   `-` = 0 (binary 000)

If a permission is present, the bit is 1. If it's absent (`-`), the bit is 0.

```text
r w x  --> 1 1 1  --> 4 + 2 + 1 = 7
r - x  --> 1 0 1  --> 4 + 0 + 1 = 5
r - -  --> 1 0 0  --> 4 + 0 + 0 = 4
```
So, `-rwxr-xr--` translates to the octal permission `754`.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [README (Index)](./README.md) | [README](./README.md) | [02 - Read Write Execute Files vs Directories](./02-Read-Write-Execute-Files-vs-Directories.md) |
