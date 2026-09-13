# 11 - Interview Questions and Answers

Permissions and ownership are guaranteed topics in any Linux System Administration, DevOps, or SRE interview. 

---

## 🟢 Fundamentals

**Q1. What are the three basic file permissions in Linux?**
> Read (`r`), Write (`w`), and Execute (`x`).

---

**Q2. What are the three ownership classes?**
> User (the owner), Group (the group owner), and Others (everyone else).

---

**Q3. How do you view the permissions of a file?**
> By using `ls -l` for a formatted list, or `stat <filename>` for detailed inode metadata including the octal representation.

---

**Q4. What does the command `chmod 755 script.sh` do?**
> It sets the permissions to `-rwxr-xr-x`. The owner gets read, write, and execute (7). The group and others get read and execute (5).

---

## 🔵 Applied Concepts

**Q5. What is the difference between Read/Write/Execute on a file vs. a directory?**
> - **File:** `r` allows viewing contents, `w` allows modifying contents, `x` allows running it as a program.
> - **Directory:** `r` allows listing contents (`ls`), `w` allows creating/deleting files inside it (requires `x`), `x` allows entering the directory (`cd`) and traversing it.

---

**Q6. I have a file with permissions `r--r--r--`. Can I delete it?**
> The permissions on the file itself do not determine if you can delete it. To delete a file, you need Write (`w`) and Execute (`x`) permissions on the **parent directory** containing the file.

---

**Q7. What is the octal equivalent of `rw-r--r--`?**
> `644`. (User: 4+2=6, Group: 4, Others: 4).

---

**Q8. A file is owned by `alice`, group `devs`. Permissions are `---rwxrwx`. If Alice tries to read the file, what happens?**
> She is denied access. Linux evaluates permissions sequentially. It checks if she is the owner, sees the owner permissions are `---`, applies them, and stops checking. It does not matter that the group has `rwx`.

---

## 🟠 Advanced & Security

**Q9. What is the Sticky Bit? Give a common example.**
> The sticky bit is a special permission applied to directories. It ensures that only the owner of a file (or the root user) can delete or rename that file within the directory, even if the directory is world-writable. The most common example is the `/tmp` directory (`chmod 1777 /tmp`).

---

**Q10. What is SUID (Set User ID)?**
> SUID is a special permission applied to executables. When run, the program executes with the privileges of the file's owner, rather than the user running it. For example, `/usr/bin/passwd` has SUID `root` so normal users can change their passwords in `/etc/shadow`.

---

**Q11. Why is SUID considered a security risk?**
> If an executable has SUID `root` and contains a vulnerability (like a buffer overflow or shell escape), an attacker can exploit it to execute arbitrary commands as the `root` user, leading to privilege escalation.

---

**Q12. What does `chmod -R a+X /var/www` do? Note the capital X.**
> It recursively adds the execute permission (`+X`) to all **directories** within `/var/www`, allowing traversal. It will *not* make regular text files executable (unless they already had an execute bit set for some other user). This is much safer than `+x`.

---

**Q13. You get "Permission denied" trying to `cat /var/log/app/error.log`. `ls -l` shows you have read access to the file. What is the problem?**
> You are missing Execute (`x`) permissions on one of the parent directories (`/var`, `/var/log`, or `/var/log/app`). The `namei -m /var/log/app/error.log` command can pinpoint which directory is blocking you.

---

## 💡 30-Second Interview Answer

> *"Linux permissions govern access through three classes: User, Group, and Others, each possessing independent Read, Write, and Execute rights. These rights function differently on files versus directories—where directory execute acts as a traversal pass. Permissions are manipulated using `chmod` (via symbolic or octal 755/644 notation), while ownership is managed via `chown`. For complex shared or secure environments, special permissions like SGID for collaborative folders and the Sticky Bit for `/tmp` are essential."*
