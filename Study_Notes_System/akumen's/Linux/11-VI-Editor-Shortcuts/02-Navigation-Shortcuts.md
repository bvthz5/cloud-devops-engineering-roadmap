# 02 - Navigation Shortcuts

Cursor movement in VI is executed in **Normal Mode** without taking hands away from the keyboard home row.

---

## 🕹️ Basic Movement Keys (Home Row)

While arrow keys work in modern Vim, standard touch-typing movement relies on `h`, `j`, `k`, `l`:

```text
             ▲ (k: Up)
             │
 (h: Left) ◄ ┼ ► (l: Right)
             │
             ▼ (j: Down)
```

| Key | Action |
| :---: | :--- |
| `h` | Move cursor **Left** 1 character |
| `j` | Move cursor **Down** 1 line |
| `k` | Move cursor **Up** 1 line |
| `l` | Move cursor **Right** 1 character |

> **Pro Tip:** Prefix movement keys with a number multiplier!
> E.g., `10j` moves down 10 lines; `5w` jumps forward 5 words.

---

## 🔤 Word & Paragraph Movement

| Key | Action |
| :---: | :--- |
| `w` | Jump forward to start of next **word** (punctuation stops count) |
| `W` | Jump forward to start of next **WORD** (delimited strictly by whitespace) |
| `b` | Jump backward to start of previous **word** |
| `B` | Jump backward to start of previous **WORD** |
| `e` | Jump forward to end of current/next **word** |
| `E` | Jump forward to end of current/next **WORD** |
| `{` | Jump backward to start of current/previous **paragraph** (blank line) |
| `}` | Jump forward to start of next **paragraph** |

---

## 🎯 Line Navigation

| Key | Action |
| :---: | :--- |
| `0` (Zero) | Jump to the **absolute start of current line** (column 0) |
| `^` | Jump to the **first non-whitespace character** of current line |
| `$` | Jump to the **end of current line** |
| `g_` | Jump to the **last non-whitespace character** of current line |

---

## 📜 Page & Document Navigation

| Key | Action |
| :---: | :--- |
| `gg` | Jump to the **very first line** of the file |
| `G` | Jump to the **very last line** of the file |
| `:42` *or* `42G` | Jump directly to **line 42** |
| `Ctrl + f` | Scroll **Forward** one full screen page |
| `Ctrl + b` | Scroll **Backward** one full screen page |
| `Ctrl + d` | Scroll **Down** half a screen page |
| `Ctrl + u` | Scroll **Up** half a screen page |
| `H` | Move cursor to **High** (top line of visible screen) |
| `M` | Move cursor to **Middle** (middle line of visible screen) |
| `L` | Move cursor to **Low** (bottom line of visible screen) |

---

## 🔢 Displaying Line Numbers

In Command-Line Mode:
* `:set nu` or `:set number` - Enable line numbers.
* `:set nonu` or `:set nonumber` - Disable line numbers.
* `:set rnu` or `:set relativenumber` - Enable relative line numbers (shows distance from current line).

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [01 - VI Editor Basics and Modes](./01-VI-Editor-Basics-and-Modes.md) | [README](./README.md) | [03 - Insert and Editing Modes](./03-Insert-and-Editing-Modes.md) |
