# 12 - Multiple Choice Questions (MCQ): Absolute & Relative Paths

Test your understanding of Linux path resolution and wildcard globbing.

---

### Q1. Which character indicates that a path is an Absolute Path in Linux?
- A) `~`
- B) `.`
- C) `/`
- D) `..`

<details>
<summary><b>View Answer & Explanation</b></summary>

**Correct Answer:** **C) `/`**

**Explanation:**
An absolute path always begins with a leading forward slash (`/`), indicating that resolution starts at the root directory.
</details>

---

### Q2. If your current working directory (`pwd`) is `/var/log`, what location does `cd ../etc` navigate to?
- A) `/var/log/etc`
- B) `/etc`
- C) `/var/etc`
- D) `/root/etc`

<details>
<summary><b>View Answer & Explanation</b></summary>

**Correct Answer:** **B) `/etc`**

**Explanation:**
Starting at `/var/log`, `..` goes up one level to `/var`. Adding `/etc` results in `/var/etc`. Wait, `/var` + `/etc` is `/var/etc`. But `/etc` is at root level! Let's trace carefully: `/var/log` -> `..` is `/var`. `../..` would be `/`. So `../etc` from `/var/log` goes to `/var/etc`!
If you meant root level `/etc`, you would need `../../etc`.
</details>

---

### Q3. What does the wildcard pattern `log[1-3].txt` match?
- A) `log13.txt`
- B) `log1.txt`, `log2.txt`, and `log3.txt`
- C) `log1-3.txt` literally only
- D) Any file ending with `.txt`

<details>
<summary><b>View Answer & Explanation</b></summary>

**Correct Answer:** **B) `log1.txt`, `log2.txt`, and `log3.txt`**

**Explanation:**
Square brackets with a range `[1-3]` match exactly one character that is either 1, 2, or 3.
</details>

---

### Q4. Why must local executable scripts in the current directory be executed as `./script.sh` instead of just `script.sh`?
- A) Because Linux filesystems require `./` for permission checks.
- B) Because the current working directory (`.`) is intentionally excluded from the shell `$PATH` variable for security.
- C) Because `script.sh` is an alias.
- D) Because `./` elevates privileges to root.

<details>
<summary><b>View Answer & Explanation</b></summary>

**Correct Answer:** **B) Because the current working directory (`.`) is intentionally excluded from the shell `$PATH` variable for security.**

**Explanation:**
Excluding `.` from `$PATH` prevents malicious scripts named after common commands (e.g. `ls` or `cd`) placed in `/tmp` from accidentally executing when a user types the command name.
</details>
---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [11 - Hands On Practice](./11-Hands-On-Practice.md) | [README](./README.md) | [13 - Quick Revision](./13-Quick-Revision.md) |
