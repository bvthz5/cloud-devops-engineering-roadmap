# 04 - Delete, Undo, and Redo Operations

Deletion in VI performs a double action: it removes text from the document **and automatically stores it into the default clipboard register** (acting like a "Cut" operation).

---

## 🗑️ Deletion Commands (Normal Mode)

Deletion operates using the `d` key combined with motion targets:

| Command | Action |
| :---: | :--- |
| `x` | Delete single character under cursor |
| `X` | Delete single character before cursor (Back-space action) |
| `dd` | Delete **entire current line** |
| `5dd` | Delete **5 lines** starting from current line |
| `dw` | Delete from cursor to start of **next word** |
| `dW` | Delete from cursor to start of next **WORD** (ignores punctuation) |
| `d$` *or* `D` | Delete from cursor position to **end of line** |
| `d0` | Delete from cursor position to **start of line** |
| `dgg` | Delete all lines from current line to **start of document** |
| `dG` | Delete all lines from current line to **end of document** |
| `di"` | Delete text **inside double quotes** |
| `di(` | Delete text **inside parentheses** |

---

## ↩️ Undo and Redo

VI keeps an internal operational history buffer allowing multi-level undo and redo:

| Key | Action |
| :---: | :--- |
| `u` | **Undo** last change |
| `Ctrl + r` | **Redo** last undone change |
| `U` | Undo **all recent changes on the current line** |

---

## 🔁 The Magic Repeat Operator (`.`)

The dot (`.`) command repeats the **exact last text-editing modification** executed in Normal Mode.

### Practical Workflow Example
1. You find a line needing deletion and type `dd`.
2. Move cursor to another line using `j`.
3. Press `.` to immediately delete that line as well!
4. You change a word using `cw` to `production`, then press `<ESC>`.
5. Move to another word and press `.` to automatically change it to `production`!
