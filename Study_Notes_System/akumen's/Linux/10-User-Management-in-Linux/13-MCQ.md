# 13 - Multiple-Choice Practice Quiz

Test your understanding of Linux user management, configuration files, password aging, group administration, and privilege escalation.

---

### Q1. Which file contains the user's primary GID and default login shell?
A) `/etc/shadow`  
B) `/etc/group`  
C) `/etc/passwd`  
D) `/etc/gshadow`  

<details>
<summary><b>View Answer & Explanation</b></summary>
<b>Correct Answer: C</b><br>
<b>Explanation:</b> `/etc/passwd` contains 7 colon-separated fields per user: Username, Password flag ('x'), UID, Primary GID (4th field), GECOS comment, Home directory path, and Default login shell path (7th field).
</details>

---

### Q2. What is the standard UID range reserved for regular (normal human) users on modern Linux distributions?
A) `0`  
B) `1 - 999`  
C) `1000 - 60000`  
D) `65534`  

<details>
<summary><b>View Answer & Explanation</b></summary>
<b>Correct Answer: C</b><br>
<b>Explanation:</b> Standard human interactive users start at UID `1000` (up to `60000` by default in `/etc/login.defs`). UID `0` is Root, `1-999` are system accounts, and `65534` is `nobody`.
</details>

---

### Q3. Which command forces a user to change their password on their very next system login?
A) `sudo passwd -l username`  
B) `sudo chage -d 0 username`  
C) `sudo usermod -L username`  
D) `sudo userdel -r username`  

<details>
<summary><b>View Answer & Explanation</b></summary>
<b>Correct Answer: B</b><br>
<b>Explanation:</b> Setting last password change date to `0` using `chage -d 0` marks the password as expired immediately, prompting the user to create a new password upon login.
</details>

---

### Q4. What happens when you run `usermod -G docker alice` without the `-a` flag?
A) It returns a syntax error.  
B) It adds user `alice` to `docker` while preserving existing secondary groups.  
C) It replaces `alice`'s secondary group list with ONLY `docker`, removing her from all other secondary groups.  
D) It converts `docker` into `alice`'s primary group.  

<details>
<summary><b>View Answer & Explanation</b></summary>
<b>Correct Answer: C</b><br>
<b>Explanation:</b> Without `-a` (append), `-G` overwrites the list of secondary groups. To safely append a group, always use `usermod -aG group user`.
</details>

---

### Q5. Why should administrators ALWAYS edit `/etc/sudoers` using `visudo` instead of standard text editors?
A) Standard editors cannot save files in `/etc/`.  
B) `visudo` encrypts `/etc/sudoers`.  
C) `visudo` performs lock checks and strict syntax validation before saving, preventing lockouts caused by syntax errors.  
D) `visudo` automatically generates SSH keys.  

<details>
<summary><b>View Answer & Explanation</b></summary>
<b>Correct Answer: C</b><br>
<b>Explanation:</b> A syntax error in `/etc/sudoers` breaks `sudo` system-wide. `visudo` validates the file syntax prior to writing changes to disk, preventing lockouts.
</details>

---

### Q6. Which file permissions are required for drop-in files inside `/etc/sudoers.d/`?
A) `0777`  
B) `0644`  
C) `0440`  
D) `0700`  

<details>
<summary><b>View Answer & Explanation</b></summary>
<b>Correct Answer: C</b><br>
<b>Explanation:</b> Sudo requires `/etc/sudoers` and drop-in files in `/etc/sudoers.d/` to be read-only by root (`0440` or `0400`). Sudo ignores files with insecure permissions or files containing dots in their names.
</details>

---

### Q7. What administrative group grants sudo privileges by default on RHEL / Rocky Linux?
A) `sudo`  
B) `admin`  
C) `wheel`  
D) `root`  

<details>
<summary><b>View Answer & Explanation</b></summary>
<b>Correct Answer: C</b><br>
<b>Explanation:</b> RHEL / CentOS / Rocky / Fedora use the BSD legacy group name `wheel` for default sudo privilege escalation. Debian / Ubuntu use the `sudo` group.
</details>

---

### Q8. Which command deletes a user account AND removes their home directory and mail spool?
A) `userdel username`  
B) `userdel -r username`  
C) `userdel -f username`  
D) `groupdel username`  

<details>
<summary><b>View Answer & Explanation</b></summary>
<b>Correct Answer: B</b><br>
<b>Explanation:</b> The `-r` (`--remove`) flag tells `userdel` to recursively purge the user's home directory and mail spool.
</details>
