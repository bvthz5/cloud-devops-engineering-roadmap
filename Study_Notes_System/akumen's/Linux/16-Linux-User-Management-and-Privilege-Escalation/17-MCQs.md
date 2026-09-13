# 17 - MCQs

15 multiple-choice questions. Attempt each before revealing the answer.

---

**Q1. Which file stores the actual password hashes on a modern Linux system?**

- A. `/etc/passwd`
- B. `/etc/shadow`
- C. `/etc/group`
- D. `/etc/sudoers`

<details>
<summary>Answer</summary>

**B — `/etc/shadow`**

`/etc/passwd` contains a placeholder `x` in the password field. The actual hashes are in `/etc/shadow`, which is readable only by root.

</details>

---

**Q2. What is the UID range for regular (human) users on most Linux distributions?**

- A. 0–99
- B. 100–999
- C. 1000+
- D. 65534+

<details>
<summary>Answer</summary>

**C — 1000+**

UIDs 0 is root, 1–999 are system/service accounts, and 1000+ are regular users.

</details>

---

**Q3. Why should you use `visudo` instead of directly editing `/etc/sudoers`?**

- A. `visudo` encrypts the file.
- B. `visudo` validates syntax and prevents concurrent edits.
- C. `visudo` automatically backs up the file to the cloud.
- D. `visudo` is faster than `vi`.

<details>
<summary>Answer</summary>

**B — `visudo` validates syntax and prevents concurrent edits.**

A syntax error in `/etc/sudoers` will break `sudo` for everyone, potentially locking administrators out of root.

</details>

---

**Q4. What does `usermod -aG docker alice` do?**

- A. Creates a user `alice` in the `docker` group.
- B. Appends `alice` to the `docker` group without removing her from other groups.
- C. Removes `alice` from all groups except `docker`.
- D. Changes Alice's primary group to `docker`.

<details>
<summary>Answer</summary>

**B — Appends `alice` to the `docker` group without removing her from other groups.**

The `-a` (append) flag is critical. Without it, `-G docker` would replace all of Alice's existing supplementary groups.

</details>

---

**Q5. What does the `$6$` prefix in a password hash in `/etc/shadow` indicate?**

- A. The password was set 6 days ago.
- B. The password uses the MD5 algorithm.
- C. The password uses the SHA-512 algorithm.
- D. The password will expire in 6 months.

<details>
<summary>Answer</summary>

**C — The password uses the SHA-512 algorithm.**

`$1$` = MD5, `$5$` = SHA-256, `$6$` = SHA-512, `$y$` = yescrypt.

</details>

---

**Q6. How do you force a user to change their password on the next login?**

- A. `passwd -f alice`
- B. `chage -d 0 alice`
- C. `usermod --expire alice`
- D. `sudo passwd --force alice`

<details>
<summary>Answer</summary>

**B — `chage -d 0 alice`**

This sets the "last password change" date to Day 0 (the Unix Epoch), making the system believe the password is infinitely old.

</details>

---

**Q7. What does `/sbin/nologin` do when set as a user's login shell?**

- A. Logs the user in but disables network access.
- B. Prevents the user from interactive login, printing a message and disconnecting.
- C. Automatically logs the user out after 5 minutes.
- D. Forces the user to use `sudo` for every command.

<details>
<summary>Answer</summary>

**B — Prevents the user from interactive login, printing a message and disconnecting.**

It is the standard way to create service accounts that should never be used for human login.

</details>

---

**Q8. In the sudoers syntax `alice ALL=(root) /usr/bin/apt update`, what does `(root)` mean?**

- A. Alice must be in the root group.
- B. Alice can only run this command AS the root user.
- C. Only root can edit this rule.
- D. The rule only applies to the root host.

<details>
<summary>Answer</summary>

**B — Alice can only run this command AS the root user.**

`(root)` specifies the "run as" user. `(ALL:ALL)` would mean any user and any group.

</details>

---

**Q9. How do you find all accounts with UID 0 (root-level access)?**

- A. `grep root /etc/passwd`
- B. `cat /etc/shadow | head -1`
- C. `awk -F: '$3 == 0 {print $1}' /etc/passwd`
- D. `find / -user root`

<details>
<summary>Answer</summary>

**C — `awk -F: '$3 == 0 {print $1}' /etc/passwd`**

This checks Field 3 (UID) for the value 0 and prints the username. Only `root` should appear.

</details>

---

**Q10. What is the difference between `passwd -l` and setting the shell to `/sbin/nologin`?**

- A. They are identical.
- B. `passwd -l` disables password login; `/sbin/nologin` disables ALL interactive login (including SSH keys).
- C. `passwd -l` deletes the user; `/sbin/nologin` keeps them.
- D. `/sbin/nologin` only works on RHEL systems.

<details>
<summary>Answer</summary>

**B — `passwd -l` disables password login; `/sbin/nologin` disables ALL interactive login (including SSH keys).**

For complete lockout, use both.

</details>

---

**Q11. Files in `/etc/sudoers.d/` that contain a dot (`.`) in their name are:**

- A. Processed first, before the main sudoers file.
- B. Silently ignored by sudo.
- C. Treated as backup files and archived.
- D. Only loaded on Debian-based systems.

<details>
<summary>Answer</summary>

**B — Silently ignored by sudo.**

This is a safety mechanism to prevent editor backup files (like `alice.bak`) from being loaded as active sudo rules.

</details>

---

**Q12. An attacker gains a shell as a normal user. Which of these is a privilege escalation vector?**

- A. The user has `sudo vi` access, allowing a shell escape from within `vi`.
- B. The user's password expires in 7 days.
- C. The user's home directory has `755` permissions.
- D. The `/tmp` directory has the Sticky Bit set.

<details>
<summary>Answer</summary>

**A — The user has `sudo vi` access, allowing a shell escape from within `vi`.**

Running `sudo vi` and then typing `:!bash` gives an instant root shell. Never grant sudo access to editors or interpreters.

</details>

---

**Q13. What does the command `sudo chage -E 2027-06-30 contractor` do?**

- A. Changes the contractor's password to expire June 30, 2027.
- B. Sets the contractor's account to be completely disabled after June 30, 2027.
- C. Creates a new user named `contractor` expiring on that date.
- D. Locks the contractor's account until June 30, 2027.

<details>
<summary>Answer</summary>

**B — Sets the contractor's account to be completely disabled after June 30, 2027.**

`-E` sets the absolute account expiration date, independent of the password.

</details>

---

**Q14. What is the purpose of the `#includedir /etc/sudoers.d` line in `/etc/sudoers`?**

- A. It is a comment and is ignored.
- B. It tells sudo to load all valid files from `/etc/sudoers.d/` as additional configuration.
- C. It creates the `/etc/sudoers.d/` directory.
- D. It includes only files that start with `#`.

<details>
<summary>Answer</summary>

**B — It tells sudo to load all valid files from `/etc/sudoers.d/` as additional configuration.**

Despite the `#`, it is NOT a comment — `#includedir` is a special directive.

</details>

---

**Q15. Which command shows you exactly what sudo privileges the current user has?**

- A. `sudo -i`
- B. `sudo -l`
- C. `sudo -v`
- D. `sudo --list-all`

<details>
<summary>Answer</summary>

**B — `sudo -l`**

This lists all commands the current user is allowed (or denied) to run via sudo on this host.

</details>
