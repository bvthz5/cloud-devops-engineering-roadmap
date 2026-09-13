# 03 - Insert and Editing Modes

Transitioning from Normal Mode to Insert Mode allows you to type text into your document. Different keys enter Insert Mode at specific cursor positions relative to current text.

---

## ✏️ Transitioning to Insert Mode

All entry keys below switch VI from Normal Mode into **Insert Mode**. Press `<ESC>` to return to Normal Mode.

| Key | Action & Cursor Placement |
| :---: | :--- |
| `i` | **Insert** before current cursor position |
| `I` | **Insert** at the first non-whitespace character of current line |
| `a` | **Append** after current cursor position |
| `A` | **Append** at the end of current line |
| `o` | **Open** a new blank line **below** current line and enter Insert Mode |
| `O` | **Open** a new blank line **above** current line and enter Insert Mode |
| `s` | **Substitute** character under cursor (deletes single character and enters Insert Mode) |
| `S` | **Substitute** entire current line (clears current line and enters Insert Mode) |

---

## 🔄 Quick Character Replacement (Without Entering Full Insert Mode)

| Key | Action |
| :---: | :--- |
| `r` | **Replace** single character under cursor with next character typed, then immediately return to Normal Mode |
| `R` | Enter **Replace Mode** (overwrites characters as you type until `<ESC>` is pressed) |

---

## ✂️ Change Operators (`c` + Motion)

Change commands delete target text and **immediately drop you into Insert Mode** to type replacements.

| Command | Action |
| :---: | :--- |
| `cw` | Change from cursor position to end of current **word** |
| `c$` *or* `C` | Change from cursor position to **end of line** |
| `c0` | Change from cursor position to **start of line** |
| `cc` | Change **entire current line** (clears line and enters Insert Mode) |
| `ci"` | Change **Inside Quotes** (deletes text between `"` and drops into Insert Mode) |
| `ci(` *or* `ci))` | Change **Inside Parentheses** (deletes text between `(` and `)`) |
| `ci{` *or* `ci}` | Change **Inside Curly Braces** (essential for code/JSON editing!) |
