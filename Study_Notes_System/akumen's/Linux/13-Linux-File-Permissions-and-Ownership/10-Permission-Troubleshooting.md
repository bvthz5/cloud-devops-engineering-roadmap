# 10 - Permission Troubleshooting

"Permission denied" is arguably the most common error in Linux. When faced with this error, resist the urge to simply run `chmod 777`. Follow this systematic troubleshooting process instead.

---

## 🚫 The Error

```bash
$ cat /var/log/app/secure.log
cat: /var/log/app/secure.log: Permission denied
```

---

## 🕵️ Systematic Diagnosis

### Step 1: Who are you?
Verify which user you are currently running as, and what groups you belong to.
```bash
whoami
groups
# Or both combined:
id
```
*Are you the user you thought you were? Are you missing a secondary group?*

### Step 2: Check the Target File's Permissions
Use `ls -l` to see the ownership and mode of the file itself.
```bash
ls -l /var/log/app/secure.log
# Output: -rw-r----- 1 root adm 1024 Aug 10 12:00 secure.log
```
*Apply the evaluation logic:*
1. Are you `root`? No.
2. Are you the owner? No.
3. Are you in the `adm` group? (Check the output of `id`). Let's assume No.
4. You fall into the "Others" category. The permissions are `---`. That explains the denial.

### Step 3: The Hidden Culprit — The Path
If you have read permissions on the file, but still get denied, the problem is higher up the directory tree. **You need Execute (`x`) permissions on every directory in the path.**

Use `namei` to trace the path:
```bash
namei -m /var/log/app/secure.log
```
*Output:*
```text
f: /var/log/app/secure.log
 drwxr-xr-x /
 drwxr-xr-x var
 drwxr-x--- log         <-- If you aren't in the group owning 'log', you stop here!
 drwxr-xr-x app
 -rw-r--r-- secure.log
```
Even if `secure.log` is `644` (world readable), you cannot read it if you cannot traverse through `/var/log`.

---

## 🛠️ Common Scenarios and Fixes

### 1. The "Lockout" (Owner has fewer rights than Others)
```text
----rw-rw- 1 alice devs 100 file.txt
```
If Alice tries to read this, she gets Permission Denied. The kernel checks the owner triad (`---`), applies it, and stops evaluating.
**Fix:** Alice must add permissions for herself: `chmod u+rw file.txt`

### 2. The Missing `+x` on a Directory
If you can `ls` a directory but get errors reading its files, the directory is likely `r--` instead of `r-x`.
**Fix:** Add execute to the directory: `chmod +x /path/to/dir`

### 3. Creating a File in a Directory
If you get "Permission denied" when running `touch /app/data/new.txt`, it has nothing to do with the permissions of `new.txt` (it doesn't exist yet!). It means you lack Write (`w`) permissions on `/app/data/`.
**Fix:** `chmod g+w /app/data/` (assuming you are in the correct group).

### 4. Running a Script Yields "Permission denied"
Even if a script is owned by you and is perfectly written:
```bash
$ ./script.sh
bash: ./script.sh: Permission denied
```
This means the file lacks the Execute (`x`) bit. By default, new text files are not executable.
**Fix:** `chmod +x script.sh`

---

## 🛑 The `chmod 777` Trap

`chmod 777` grants Read, Write, and Execute to Everyone.
**Never do this as a fix.** It is a severe security vulnerability.

If an application requires write access to a directory, figure out what user the application runs as (e.g., `www-data`), and either:
1. `chown` the directory to that user.
2. `chgrp` the directory to that user's group, and `chmod 775`.

Leave `777` strictly for explicitly shared scratch spaces like `/tmp` (and always with the sticky bit: `1777`).

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [09 - Real World DevOps Scenarios](./09-Real-World-DevOps-Scenarios.md) | [README](./README.md) | [11 - Interview QA](./11-Interview-QA.md) |
