# 10 - Troubleshooting VI Editor Issues

This guide covers the most common failure scenarios and confusion points encountered when using VI and Vim in real server and DevOps environments.

---

## 🔍 Issue 1: Swap File Recovery Warning (`.swp`)

### Symptom
Opening a file in Vim produces an error like:
```text
E325: ATTENTION
Found a swap file by the name ".nginx.conf.swp"
          owned by: ubuntu   dated: Sat Sep 13 18:30:12 2026
         file name: /etc/nginx/nginx.conf
          modified: YES
[O]pen Read-Only, (E)dit anyway, (R)ecover, (D)elete it, (Q)uit, (A)bort:
```

### Root Cause
A `.swp` (swap file) is automatically created by Vim when you open a file. If the editor crashes (SSH disconnect, power loss, `kill -9`), the swap file persists on disk as a recovery aid.

### Resolution
1. **Choose `R` (Recover)** — Vim restores your unsaved edits from the swap file.
2. After recovering, **immediately save** with `:wq`.
3. **Delete the swap file manually** after recovery to prevent future warnings:
   ```bash
   rm /etc/nginx/.nginx.conf.swp
   ```

---

## 🔍 Issue 2: "Permission denied" — File Opened Without `sudo`

### Symptom
After editing a system file like `/etc/hosts`, attempting `:w` produces:
```text
E212: Can't open file for writing: Permission denied
```

### Root Cause
File was opened without elevated privileges and the current user lacks write access.

### Resolution (No Need to Quit!)
```text
:w !sudo tee %
```
This pipes the buffer through `sudo tee` to write the file as root. Vim will then ask if you want to reload the file.

---

## 🔍 Issue 3: Stuck in Insert / Unknown Mode

### Symptom
Typing commands like `dd`, `yy`, or `:wq` results in literal characters appearing on screen instead of executing operations.

### Root Cause
You are currently in **Insert Mode** or **Visual Mode**, not Normal Mode.

### Resolution
**Always press `<ESC>` first** (sometimes twice) to return to Normal Mode before issuing commands. You can check your current mode at the bottom left of the terminal:
* No label = Normal Mode
* `-- INSERT --` = Insert Mode
* `-- VISUAL --` = Visual Mode

---

## 🔍 Issue 4: Search Pattern Highlights Not Clearing After Search

### Symptom
After searching with `/pattern`, all matches remain visually highlighted in yellow/blue, cluttering readability.

### Resolution
```text
:noh
```
Clears the current search highlight without disabling search highlighting globally.

---

## 🔍 Issue 5: Arrow Keys Produce `A`, `B`, `C`, `D` in Insert Mode

### Symptom
When pressing arrow keys in Insert Mode, the terminal inserts literal characters `A`, `B`, `C`, `D` instead of moving the cursor.

### Root Cause
This is caused by running a minimal POSIX-only `vi` (not Vim) in a terminal with an incompatible `TERM` environment variable setting. Bare-bones `vi` on systems like Alpine Linux Docker images may lack arrow key support.

### Resolution
1. Press `<ESC>` first to leave Insert Mode.
2. Use `h`, `j`, `k`, `l` for movement in Normal Mode.
3. On modern systems, install `vim` instead of `vi`:
   ```bash
   # Debian/Ubuntu
   sudo apt install vim -y
   # RHEL/Rocky
   sudo dnf install vim -y
   ```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [09 - Practical DevOps Workflows](./09-Practical-DevOps-Workflows.md) | [README](./README.md) | [11 - Interview QA](./11-Interview-QA.md) |
