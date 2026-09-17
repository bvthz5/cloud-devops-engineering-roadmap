# 05 - Yank, Copy, and Paste

In VI terminology, copying text is called **Yanking** (`y`), and pasting text is called **Pasting** (`p`).

---

## 📋 Yank (Copy) Commands

Yank commands copy text into registers without deleting it from the line:

| Command | Action |
| :---: | :--- |
| `yy` *or* `Y` | **Yank (copy) entire current line** |
| `5yy` | **Yank 5 lines** starting from current line |
| `yw` | Yank from cursor to start of **next word** |
| `y$` | Yank from cursor to **end of line** |
| `y0` | Yank from cursor to **start of line** |
| `yi"` | Yank text **inside double quotes** |
| `yi{` | Yank text **inside curly braces** |

---

## 📌 Paste Commands

Pasting inserts text from the unnamed default register (or specified register) into the document:

| Command | Action |
| :---: | :--- |
| `p` (lower) | Paste register contents **AFTER** current cursor position / **BELOW** current line |
| `P` (Upper) | Paste register contents **BEFORE** current cursor position / **ABOVE** current line |

---

## 📂 Registers (Multi-Clipboard Storage)

VI provides multiple named storage registers (`a-z`) allowing you to hold separate snippets of text simultaneously.

### Using Named Registers
To save or paste from a specific register, prefix the command with `"` followed by the register letter:

* `"ayyg` - Yank current line into register **`a`**.
* `"byw` - Yank current word into register **`b`**.
* `"ap` - Paste contents of register **`a`** below current line.
* `"bp` - Paste contents of register **`b`** below current line.

### System Clipboard Integration (`"+`)
In environments where Vim is compiled with X11/clipboard support (`+clipboard` in `vim --version`):
* `"+yy` - Yank current line into the **OS System Clipboard** (Ctrl+C equivalent).
* `"+p` - Paste from the **OS System Clipboard** into Vim.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [04 - Delete Undo Redo Operations](./04-Delete-Undo-Redo-Operations.md) | [README](./README.md) | [06 - Search and Replace](./06-Search-and-Replace.md) |
