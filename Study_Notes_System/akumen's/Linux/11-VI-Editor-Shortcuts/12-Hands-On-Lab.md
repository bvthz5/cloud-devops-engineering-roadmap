# 12 - Hands-On Terminal Lab

Practical terminal labs to drill VI mode transitions, navigation, editing, search-and-replace, and split window workflows.

---

## 🎯 Lab Objectives
1. Gain muscle memory for mode switching and `<ESC>` key discipline.
2. Navigate large files without using arrow keys.
3. Master `dd`, `yy`, `p`, and `.` (dot-repeat) operators.
4. Execute real-world substitutions using `:%s/`.
5. Practice the `sudo tee` rescue save technique.

---

## 🧪 Setup — Create a Practice File

```bash
cat << 'EOF' > ~/vi-practice.txt
Line 1: The Linux kernel manages resources.
Line 2: NGINX serves web traffic on port 80 and 443.
Line 3: staging-db.internal is the database host.
Line 4: staging-db.internal connects on port 5432.
Line 5: The sudo group grants administrative access on Debian.
Line 6: The wheel group grants administrative access on RHEL.
Line 7: systemd manages all services on modern Linux.
Line 8: Docker containers use non-root users for security.
Line 9: SSH uses authorized_keys for public key authentication.
Line 10: Vim is available on every Linux distribution.
EOF
```

---

## Task 1: Mode Navigation Drill
1. Open the file: `vi ~/vi-practice.txt`
2. Verify you are in **Normal Mode** (no label at bottom).
3. Press `i` to enter Insert Mode — notice `-- INSERT --` at bottom.
4. Press `<ESC>` to return to Normal Mode.
5. Press `:` — notice the colon prompt at bottom.
6. Press `<ESC>` to return to Normal Mode.

---

## Task 2: Navigation Without Arrow Keys
1. Use `j` to move down 5 lines, then `k` to move back up 3 lines.
2. Press `G` — jump to the last line.
3. Press `gg` — jump back to the first line.
4. Press `5G` or `:5` — jump to line 5.
5. Press `$` — jump to end of line. Press `0` — jump to start of line.
6. Press `w` repeatedly to move word-by-word.

---

## Task 3: Delete, Yank, and Paste
1. Go to line 1 with `gg`.
2. Type `yy` to yank line 1.
3. Move to line 10 with `G`.
4. Press `p` — paste line 1 below line 10.
5. Go to line 6 with `:6`.
6. Type `dd` to delete line 6.
7. Press `u` to **undo** the deletion.
8. Press `Ctrl+R` to **redo** it.

---

## Task 4: Search and Replace
1. Press `/staging` and hit `<Enter>` — search forward.
2. Press `n` to jump to the next match.
3. Enter Command Mode:
   ```text
   :%s/staging-db\.internal/production-db.internal/gc
   ```
4. Confirm each replacement with `y`.
5. Clear highlight: `:noh`

---

## Task 5: Rescue Save (Read-Only File Simulation)
1. Create a protected file:
   ```bash
   sudo bash -c 'echo "protected config" > /tmp/test-protected.conf && chmod 600 /tmp/test-protected.conf'
   ```
2. Open it without sudo: `vi /tmp/test-protected.conf`
3. Attempt to add a line in Insert Mode, then press `<ESC>`.
4. Try `:w` — observe "Permission denied" error.
5. Use the rescue save:
   ```text
   :w !sudo tee %
   ```
6. Confirm the save worked: `:q!` and then `sudo cat /tmp/test-protected.conf`

---

## Task 6: Vertical Split Comparison
1. Open two files side by side:
   ```bash
   vim -O ~/vi-practice.txt /etc/hosts
   ```
2. Navigate between panes with `Ctrl+W l` and `Ctrl+W h`.
3. Equalize pane sizes with `Ctrl+W =`.
4. Close one pane with `:q`.
