# 11 - Interview Questions and Answers

Common VI / Vim interview questions for DevOps Engineer, SRE, and Linux System Administrator roles.

---

## 🟢 Junior / Associate Level

### Q1: What is a "mode" in VI? Why does VI use modes?
**Answer:**
VI is a **modal text editor**, meaning the keyboard operates in distinct operational states called modes. The primary modes are:
* **Normal Mode:** Default state. Keyboard keys execute commands (navigation, deletion, copy).
* **Insert Mode:** Text is typed directly into the document.
* **Command-Line Mode:** Entered with `:` for file operations, save, quit, search/replace.
* **Visual Mode:** Select text visually before applying operations.

The modal design stems from VI being built for keyboards without dedicated function keys or arrow keys (early terminal hardware). By separating movement from typing, VI users can edit files far faster than in non-modal editors without ever needing a mouse.

---

### Q2: How do you save and exit VI?
**Answer:**

| Goal | Command |
| :--- | :--- |
| Save and quit | `:wq` or `ZZ` in Normal Mode |
| Quit without saving | `:q!` or `ZQ` in Normal Mode |
| Save only (stay open) | `:w` |
| Force save read-only | `:w !sudo tee %` |

---

### Q3: You accidentally opened `/etc/nginx/nginx.conf` without `sudo` in VI. You have made important changes. How do you save the file without losing your edits?
**Answer:**
```text
:w !sudo tee %
```
This command pipes the current Vim buffer through `sudo tee`, which writes the content to disk with root privileges. This works without closing Vim, preserving all edits.

---

## 🟡 Mid-Level DevOps

### Q4: What is the difference between `dd` and `"_dd` in VI?
**Answer:**
* `dd` — Deletes the current line **and stores it in the default unnamed register** (`"`). If you then press `p`, the deleted line will be pasted, which is often undesirable after a series of delete operations.
* `"_dd` — Deletes the current line into the **blackhole register** (`_`), which discards the content completely. Subsequent `p` commands paste from the previous unnamed register, not the deleted content.

This distinction matters when you want to delete unwanted lines without contaminating the default clipboard.

---

### Q5: How do you replace all occurrences of the string `staging` with `production` in a file, with interactive confirmation before each replacement?
**Answer:**
```text
:%s/staging/production/gc
```
* `%` — Applies across the entire file.
* `s/staging/production/` — Substitutes the pattern.
* `g` — Global (all occurrences per line, not just first).
* `c` — Confirm mode (prompts `y/n/a/q/l` before each replacement).

---

## 🔴 Senior / Lead SRE Level

### Q6: How do you comment out lines 5 through 20 in a shell script using VI's Visual Block Mode?
**Answer:**
1. Navigate to line 5: `:5`
2. Press `Ctrl+V` to enter **Visual Block Mode**.
3. Press `20G` or `15j` to extend selection to line 20.
4. Press `I` (capital I = Insert at start of block column).
5. Type `# ` (the comment prefix).
6. Press `<ESC>`. All 16 lines have `# ` prepended simultaneously.

---

### Q7: Explain the purpose of a VI swap file (`.swp`). What is the procedure when Vim warns about an existing swap file?
**Answer:**
Vim automatically creates a swap file (e.g., `.nginx.conf.swp`) when you open a file for editing. The swap file stores unsaved changes incrementally, acting as a crash recovery mechanism.

When Vim warns about an existing swap file, it indicates a previous session crashed or was killed without clean exit. Options:
* `R` — **Recover** (restores unsaved changes from swap file into buffer).
* `D` — **Delete** (discards swap file, opens original file state — loses previous unsaved edits).
* `O` — **Open Read-Only** (inspect file without risk).

After a successful recovery with `R`:
1. Review recovered content, save with `:wq`.
2. Delete the orphaned swap file: `rm .filename.swp`.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [10 - Troubleshooting](./10-Troubleshooting.md) | [README](./README.md) | [12 - Hands On Lab](./12-Hands-On-Lab.md) |
