# 13 - MCQs

15 multiple-choice questions to test your understanding. Attempt each before revealing the answer.

---

**Q1. In the permission string `-rwxr-xr--`, what does the first character (`-`) represent?**

- A. It's a directory
- B. It's a symbolic link
- C. It's a regular file
- D. It's an executable

<details>
<summary>Answer</summary>

**C — It's a regular file**

A `-` at the start denotes a regular file, `d` denotes a directory, and `l` denotes a symlink.

</details>

---

**Q2. Which command correctly grants Execute permission to the User Owner only?**

- A. `chmod u+x file`
- B. `chmod o+x file`
- C. `chmod a+x file`
- D. `chmod x file`

<details>
<summary>Answer</summary>

**A — `chmod u+x file`**

`u` targets the User (owner), `+x` adds the execute permission.

</details>

---

**Q3. What is the octal equivalent of the permission string `-rw-r--r--`?**

- A. 755
- B. 644
- C. 600
- D. 777

<details>
<summary>Answer</summary>

**B — 644**

`rw-` is 4+2=6. `r--` is 4. `r--` is 4. Result: 644.

</details>

---

**Q4. A user is trying to delete `test.txt` but gets "Permission denied". The permissions on `test.txt` are `rwx------`. What is the most likely cause?**

- A. The file is a system file.
- B. The user lacks write (`w`) permission on the file itself.
- C. The user lacks write (`w`) permission on the parent directory.
- D. The user is not in the correct group.

<details>
<summary>Answer</summary>

**C — The user lacks write (`w`) permission on the parent directory.**

Deleting a file modifies the directory containing it, not the file itself. Therefore, directory write access is required to delete files inside it.

</details>

---

**Q5. What does the Sticky Bit do when applied to a directory?**

- A. It prevents anyone from entering the directory.
- B. It forces all new files to inherit the directory's group ownership.
- C. It allows only the file's owner to delete or rename files within the directory.
- D. It makes the directory invisible to the `ls` command.

<details>
<summary>Answer</summary>

**C — It allows only the file's owner to delete or rename files within the directory.**

This is crucial for shared directories like `/tmp` to prevent users from deleting each other's files.

</details>

---

**Q6. Which command changes both the owner (to `alice`) and the group (to `devs`) of a file?**

- A. `chmod alice:devs file`
- B. `chown alice.devs file`
- C. `chown alice:devs file`
- D. `chgrp alice:devs file`

<details>
<summary>Answer</summary>

**C — `chown alice:devs file`**

The `chown` command uses a colon (or historically a dot) to separate the User and Group.

</details>

---

**Q7. What does the command `chmod 700 ~/.ssh` achieve?**

- A. Grants everyone full access to the `.ssh` folder.
- B. Grants the owner read, write, and enter access; denies everyone else.
- C. Denies the owner access, but allows the group.
- D. Applies the Sticky Bit to the `.ssh` folder.

<details>
<summary>Answer</summary>

**B — Grants the owner read, write, and enter access; denies everyone else.**

`7` = `rwx` for User. `0` = `---` for Group. `0` = `---` for Others. This is the mandatory permission for `.ssh` directories.

</details>

---

**Q8. If you want to recursively change the ownership of `/var/www/html` to `www-data`, what is the correct command?**

- A. `chown -r www-data:www-data /var/www/html`
- B. `chown -R www-data:www-data /var/www/html`
- C. `chgrp -R www-data /var/www/html`
- D. `chmod -R www-data:www-data /var/www/html`

<details>
<summary>Answer</summary>

**B — `chown -R www-data:www-data /var/www/html`**

Recursive flags for ownership and permissions are uppercase `-R`.

</details>

---

**Q9. Which permission allows a user to `cd` into a directory?**

- A. Read (`r`)
- B. Write (`w`)
- C. Execute (`x`)
- D. SGID (`s`)

<details>
<summary>Answer</summary>

**C — Execute (`x`)**

The execute bit on a directory functions as a "traversal" or "pass-through" permission.

</details>

---

**Q10. What does the `stat` command do?**

- A. Shows system CPU and memory statistics.
- B. Displays detailed file metadata, including octal permissions and inode info.
- C. Changes the status of a file from read-only to read-write.
- D. Traces the pathname permissions for a file.

<details>
<summary>Answer</summary>

**B — Displays detailed file metadata, including octal permissions and inode info.**

`stat` provides a much deeper look than `ls -l`, clearly showing access, modify, and change times.

</details>

---

**Q11. You run `ls -l` and see `drwxrwsr-x`. What does the `s` indicate?**

- A. The SUID bit is set.
- B. The SGID bit is set.
- C. The Sticky bit is set.
- D. The directory is a symbolic link.

<details>
<summary>Answer</summary>

**B — The SGID bit is set.**

Because the `s` is in the *Group* triad's execute position, it represents Set Group ID (SGID).

</details>

---

**Q12. Why should you avoid `chmod -R 777 /var/www`?**

- A. It consumes too much disk space.
- B. It prevents the web server from reading the files.
- C. It creates a severe security risk by allowing anyone to modify or execute files.
- D. It deletes hidden files.

<details>
<summary>Answer</summary>

**C — It creates a severe security risk by allowing anyone to modify or execute files.**

`777` grants world-write and world-execute permissions, which should almost never be applied recursively to a web directory.

</details>

---

**Q13. How does Linux evaluate permissions when a user accesses a file?**

- A. It combines User, Group, and Others permissions together.
- B. It checks Others first, then Group, then User.
- C. It checks sequentially (Root -> User -> Group -> Others) and stops at the first match.
- D. It only checks the Group permissions.

<details>
<summary>Answer</summary>

**C — It checks sequentially (Root -> User -> Group -> Others) and stops at the first match.**

This sequential checking means an Owner can lock themselves out even if the Group has access.

</details>

---

**Q14. What does the command `namei -m /path/to/file` do?**

- A. Changes the name of the file.
- B. Lists the permissions of every directory component in the given path.
- C. Displays the memory usage of the file.
- D. Finds all files with a similar name.

<details>
<summary>Answer</summary>

**B — Lists the permissions of every directory component in the given path.**

It is invaluable for troubleshooting "Permission denied" errors caused by missing `x` bits on parent directories.

</details>

---

**Q15. Which character represents a symbolic link in `ls -l` output?**

- A. `-`
- B. `d`
- C. `c`
- D. `l`

<details>
<summary>Answer</summary>

**D — `l`**

The first character of the 10-character string is `l` for a symlink, `d` for directory, and `-` for a regular file.

</details>

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [12 - Hands On Terminal Practice](./12-Hands-On-Terminal-Practice.md) | [README](./README.md) | [14 - Quick Revision](./14-Quick-Revision.md) |
