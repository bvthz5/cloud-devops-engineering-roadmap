# 02 - Read, Write, Execute: Files vs. Directories

The concepts of Read, Write, and Execute seem straightforward, but they behave very differently depending on whether they are applied to a **File** or a **Directory**.

This is one of the most common sources of confusion for beginners and a frequent topic in DevOps interviews.

---

## 📄 Permissions on Regular Files

For a regular file (like a script, config file, or document), the permissions are intuitive:

| Permission | Symbol | What it allows you to do with the FILE |
| :---: | :---: | :--- |
| **Read** | `r` | You can read the contents of the file (e.g., using `cat`, `less`, `grep`). |
| **Write** | `w` | You can modify the contents of the file (e.g., using `vi`, `echo "text" > file`, `sed -i`). |
| **Execute** | `x` | You can run the file as a program or script (e.g., `./script.sh`). |

> **Important Note on File Deletion:**
> The ability to delete or rename a file is **NOT** controlled by the `w` permission on the file itself! It is controlled by the `w` permission on the **directory** containing the file. You can have a file with read-only permissions (`r--`), but if you have write access to its parent directory, you can delete that file.

---

## 📁 Permissions on Directories

For directories, the permissions have slightly different, structural meanings:

| Permission | Symbol | What it allows you to do with the DIRECTORY |
| :---: | :---: | :--- |
| **Read** | `r` | You can list the contents of the directory (e.g., using `ls`). Requires `x` to be useful. |
| **Write** | `w` | You can create, delete, and rename files *within* the directory (e.g., `touch`, `rm`, `mv`). Requires `x`. |
| **Execute** | `x` | You can enter the directory (e.g., using `cd`) and access known files within it. |

### The Critical Role of Directory Execute (`x`)

The Execute (`x`) permission on a directory is often called the **"Search"** or **"Pass-through"** permission. It is the most important permission for a directory.

*   **If you lack `x` on a directory:** You cannot `cd` into it, you cannot read files inside it (even if the file itself has read permissions!), and you cannot `ls -l` its contents properly (you might see file names if you have `r`, but you'll get "Permission denied" trying to read their attributes).
*   **If you have `x` but lack `r` on a directory:** You cannot run `ls` to see what's inside. However, if you already know the exact name of a file inside, you can access it directly (e.g., `cat /secret_dir/known_file.txt`).
*   **If you have `w` but lack `x`:** The write permission is useless. You cannot create or delete files unless you can also enter the directory.

---

## 🧠 Summary Table: The Difference

| Action | Needs on File | Needs on Parent Directory |
| :--- | :---: | :---: |
| View file contents (`cat`) | `r` | `x` |
| Edit file contents (`vi`) | `r` + `w` | `x` |
| Execute a script (`./script`) | `r` + `x` | `x` |
| List directory contents (`ls`) | N/A | `r` + `x` |
| Create a new file (`touch`) | N/A | `w` + `x` |
| Delete a file (`rm`) | N/A | `w` + `x` (File permissions do not matter) |
| Rename a file (`mv`) | N/A | `w` + `x` (File permissions do not matter) |

---

## 💡 Practical DevOps Examples

### Example 1: Web Server Directory (`/var/www/html`)
A web server (like Nginx or Apache) runs as the user `www-data`. For the web server to serve a file:
1. It needs `x` on `/var`, `x` on `www`, and `x` on `html` to traverse the path.
2. It needs `r` on the actual file `index.html`.

### Example 2: The `/tmp` Directory
The `/tmp` directory is unique. Everyone needs to create files there, so it has `rwx` for Everyone. However, to prevent User A from deleting User B's files, a special permission (the Sticky Bit) is used. (Covered in Chapter 7).
