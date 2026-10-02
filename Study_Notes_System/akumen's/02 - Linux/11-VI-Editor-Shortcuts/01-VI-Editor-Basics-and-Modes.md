# 01 - VI Editor Basics and Modes

VI is a modal text editor designed by Bill Joy in 1976 for Unix. Unlike conventional GUI text editors (like Notepad or VS Code) where every keystroke immediately types characters onto the screen, VI operates under the paradigm of **Modal Editing**.

---

## 💡 The Modal Editing Philosophy

In traditional editors, keys on the keyboard are dedicated solely to typing characters, forcing users to rely on mouse movements or modifier keys (`Ctrl`, `Alt`, `Cmd`) for navigation, deletion, copy, and paste.

VI separates these actions into distinct operational **Modes**:
* In **Normal Mode**, keys act as powerful command shortcuts (`d` deletes, `y` copies, `w` jumps words).
* In **Insert Mode**, keys type raw text directly onto the document.

This design enables developers and system administrators to edit files rapidly without taking their hands off the home row (`ASDF HJKL`) of the keyboard.

---

## 🔄 The 4 Essential VI Modes

```text
 ┌──────────────────────────────────────────────────────────────────────────┐
 │                                NORMAL MODE                               │
 │                  (Default mode upon launching vi / vim)                  │
 └──────┬───────────────────────┬───────────────────┬───────────────────────┘
        │                       │                   │
        │ Press 'i','a','o'     │ Press ':'         │ Press 'v','V','Ctrl+V'
        ▼                       ▼                   ▼
 ┌──────────────┐       ┌─────────────────┐       ┌──────────────────┐
 │ INSERT MODE  │       │  COMMAND MODE   │       │   VISUAL MODE    │
 │ [--INSERT--] │       │      [ : ]      │       │  [-- VISUAL --]  │
 └──────┬───────┘       └────────┬────────┘       └────────┬─────────┘
        │                        │                         │
        └────────────────────────┴─────────────────────────┘
                            Press <ESC>
```

### 1. Normal Mode (Command Mode)
* **Default state** when you open any file with `vi filename.txt`.
* Used for moving the cursor, deleting text, copying/pasting, searching, and invoking operators.
* Typing text in Normal Mode executes commands instead of inserting characters.
* **Return to Normal Mode at any time by pressing the `<ESC>` key.**

### 2. Insert Mode
* Used for typing and writing text into the document.
* Displayed as `-- INSERT --` at the bottom left of the terminal.
* Entered from Normal Mode by pressing `i` (insert before cursor), `a` (append after cursor), `o` (open new line below), etc.

### 3. Command-Line / Ex Mode
* Used for file-level operations, saving, quitting, buffer management, search-and-replace, and configuring editor settings.
* Entered from Normal Mode by typing a colon (`:`).
* Examples: `:w` (save), `:q` (quit), `:%s/foo/bar/g` (replace).

### 4. Visual Mode
* Used for selecting blocks of text visually with the cursor before applying actions (delete, copy, indent, comment out).
* Entered from Normal Mode by pressing:
  * `v` - Character-wise Visual Mode
  * `V` - Line-wise Visual Mode
  * `Ctrl + V` - Block-wise (Column) Visual Mode (Essential for multi-line comment blocks in DevOps files!)

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [README (Index)](./README.md) | [README](./README.md) | [02 - Navigation Shortcuts](./02-Navigation-Shortcuts.md) |
