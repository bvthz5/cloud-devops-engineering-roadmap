# 14 - Quick Revision Cheat Sheet

High-density 5-minute summary for fast review before interviews, exams, or on-call tasks.

---

## 🔄 Mode Summary

| Press | Enters Mode | Return with |
| :---: | :--- | :---: |
| `i` / `a` / `o` / `O` | **Insert Mode** | `<ESC>` |
| `:` | **Command-Line Mode** | `<ESC>` / `<Enter>` |
| `v` / `V` / `Ctrl+V` | **Visual Mode** | `<ESC>` |

> **Golden Rule:** When in doubt, press `<ESC>` to get back to Normal Mode!

---

## 🧭 Navigation One-Liners

```text
gg          → First line of file
G           → Last line of file
:42         → Jump to line 42
Ctrl+f/b    → Page down / Page up
w / b       → Jump forward / backward by word
0 / ^ / $   → Start of line / First char / End of line
```

---

## ✂️ Editing One-Liners

```text
i           → Insert before cursor
o           → New line below (Insert Mode)
dd          → Delete current line
5dd         → Delete 5 lines
dw          → Delete word
D / d$      → Delete to end of line
u           → Undo
Ctrl+R      → Redo
.           → Repeat last edit
```

---

## 📋 Copy / Paste One-Liners

```text
yy          → Yank (copy) current line
5yy         → Yank 5 lines
yw          → Yank current word
p           → Paste BELOW / AFTER cursor
P           → Paste ABOVE / BEFORE cursor
"ayw        → Yank word into register 'a'
"ap         → Paste from register 'a'
"+yy        → Yank to OS system clipboard
"+p         → Paste from OS system clipboard
```

---

## 🔍 Search & Replace One-Liners

```text
/pattern    → Search forward
?pattern    → Search backward
n / N       → Next / Previous match
:noh        → Clear search highlights
:%s/old/new/g   → Replace all in file
:%s/old/new/gc  → Replace all with confirmation
:5,20s/old/new/g → Replace in lines 5-20
```

---

## 💾 Save & Quit One-Liners

```text
:w          → Save (write)
:q          → Quit
:wq or ZZ   → Save and quit
:q! or ZQ   → Quit without saving
:w !sudo tee %  → Save file that was opened without sudo
```

---

## 🪟 Splits One-Liners

```text
:sp file    → Horizontal split
:vsp file   → Vertical split
Ctrl+W h/j/k/l → Move between panes
Ctrl+W =    → Equalize pane sizes
:only       → Close all other panes
```

---

## 💡 Must-Know Rescue Commands

| Situation | Solution |
| :--- | :--- |
| Can't exit Vim | `:q!` (force quit) or `ZQ` |
| Opened without sudo | `:w !sudo tee %` |
| Swap file warning on open | Press `R` to recover |
| Search highlights stuck | `:noh` |
| Accidentally in wrong mode | `<ESC>` (twice if unsure) |
