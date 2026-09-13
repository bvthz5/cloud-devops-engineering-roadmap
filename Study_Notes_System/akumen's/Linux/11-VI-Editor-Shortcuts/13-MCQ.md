# 13 - Multiple-Choice Practice Quiz

Test your understanding of VI / Vim modes, navigation, editing, search-and-replace, and file operations.

---

### Q1. Which key combination returns you to Normal Mode from ANY other mode in VI?
A) `Ctrl+C`  
B) `Ctrl+Z`  
C) `<ESC>`  
D) `q`  

<details>
<summary><b>View Answer & Explanation</b></summary>
<b>Correct Answer: C</b><br>
<b>Explanation:</b> Pressing <code>&lt;ESC&gt;</code> returns the editor to Normal Mode from Insert, Visual, or Command-Line Mode. Pressing it twice guarantees you are in Normal Mode even if you were in a sub-mode.
</details>

---

### Q2. You need to add text BELOW the current line and immediately enter Insert Mode. Which key do you press?
A) `i`  
B) `a`  
C) `o`  
D) `O`  

<details>
<summary><b>View Answer & Explanation</b></summary>
<b>Correct Answer: C</b><br>
<b>Explanation:</b> <code>o</code> (lowercase) opens a new blank line BELOW the current line and switches to Insert Mode. <code>O</code> (uppercase) opens a new line ABOVE the current line.
</details>

---

### Q3. What does `5dd` do in Normal Mode?
A) Deletes 5 words forward from the cursor  
B) Duplicates the current line 5 times  
C) Deletes 5 lines starting from the current line  
D) Jumps forward 5 lines  

<details>
<summary><b>View Answer & Explanation</b></summary>
<b>Correct Answer: C</b><br>
<b>Explanation:</b> In VI, a numeric prefix multiplies the action. <code>dd</code> deletes one line, so <code>5dd</code> deletes 5 consecutive lines starting from the cursor's current position.
</details>

---

### Q4. Which command replaces ALL occurrences of `http` with `https` in the ENTIRE file?
A) `:s/http/https/`  
B) `:%s/http/https/`  
C) `:%s/http/https/g`  
D) `:g/http/https/g`  

<details>
<summary><b>View Answer & Explanation</b></summary>
<b>Correct Answer: C</b><br>
<b>Explanation:</b> <code>:%s/http/https/g</code> — The <code>%</code> range applies to all lines, and the <code>g</code> flag replaces ALL occurrences on each line. Without <code>g</code>, only the first occurrence per line is replaced.
</details>

---

### Q5. You opened `/etc/hosts` without `sudo`, made edits, and now `:w` fails with "Permission denied". What is the correct rescue command?
A) `:q!` and then re-open with `sudo vi`  
B) `:sudo w`  
C) `:w !sudo tee %`  
D) `:wq!`  

<details>
<summary><b>View Answer & Explanation</b></summary>
<b>Correct Answer: C</b><br>
<b>Explanation:</b> <code>:w !sudo tee %</code> pipes the current buffer through <code>sudo tee</code> to write the file with root privileges. Option A loses all unsaved edits. <code>:wq!</code> fails the same way since you still lack write permission.
</details>

---

### Q6. What VI command opens `server.conf` in a vertical split window alongside the current file?
A) `:sp server.conf`  
B) `:vsp server.conf`  
C) `:split -v server.conf`  
D) `Ctrl+W v server.conf`  

<details>
<summary><b>View Answer & Explanation</b></summary>
<b>Correct Answer: B</b><br>
<b>Explanation:</b> <code>:vsp filename</code> (vertical split) divides the window into left and right panes. <code>:sp filename</code> creates a horizontal (top/bottom) split.
</details>

---

### Q7. What is the purpose of a `.swp` swap file in Vim?
A) It stores encrypted file permissions  
B) It is a compressed backup of the previous file version  
C) It stores unsaved buffer changes for crash recovery  
D) It is used as a temporary pipe for sudo tee operations  

<details>
<summary><b>View Answer & Explanation</b></summary>
<b>Correct Answer: C</b><br>
<b>Explanation:</b> Vim writes buffer changes to a swap file incrementally while editing. If Vim crashes or the SSH session is disconnected, the swap file allows recovery of unsaved changes by selecting <code>R</code> (Recover) when Vim detects the orphaned swap file.
</details>

---

### Q8. Which shortcut pastes the last yanked/deleted text ABOVE the current line?
A) `p`  
B) `P`  
C) `gp`  
D) `"+p`  

<details>
<summary><b>View Answer & Explanation</b></summary>
<b>Correct Answer: B</b><br>
<b>Explanation:</b> <code>P</code> (uppercase) pastes before/above the cursor or above the current line. <code>p</code> (lowercase) pastes after/below.
</details>
