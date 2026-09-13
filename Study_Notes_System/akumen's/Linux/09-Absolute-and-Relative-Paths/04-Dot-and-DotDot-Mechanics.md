# 04 - Special Path Shortcuts: `.`, `..`, `~`, and `-`

Linux filesystems incorporate special directory symbols and shell expansion shortcuts to streamline path navigation and resolution.

---

## 🔍 Special Path Symbols Reference

| Symbol | Name | Meaning & Resolution |
| :---: | :--- | :--- |
| **`.`** | Single Dot | Represents the **Current Working Directory**. |
| **`..`** | Double Dot | Represents the **Parent Directory** (one level up). |
| **`~`** | Tilde | Shortcut expanding to the current user's **Home Directory** (`$HOME`). |
| **`~username`** | Tilde User | Shortcut expanding to a specific user's Home Directory (`/home/username`). |
| **`-`** | Dash | Shortcut for `cd` to return to the **Previous Working Directory** (`$OLDPWD`). |

---

## 🛠️ Deep Dive into Symbols

### 1. The Single Dot (`.`)
- Every Linux directory contains an internal entry `.` pointing to itself.
- **Executing Local Scripts:** For security reasons, the current directory (`.`) is **NOT** included in the shell `$PATH`. To run an executable script in your working directory, you must prefix it with `./`:
  ```bash
  ./deploy.sh
  ```
- **Copying to Current Directory:**
  ```bash
  cp /etc/nginx/nginx.conf .
  ```

### 2. The Double Dot (`..`)
- Every Linux directory contains an internal entry `..` pointing to its parent directory.
- **Navigating Multiple Levels Up:**
  ```bash
  cd ../..     # Go up 2 directory levels
  cd ../../../ # Go up 3 directory levels
  ```

---

## ⬅️ Navigation
- Previous: [03 - Relative Paths](file:///c:/Users/binil/OneDrive/Desktop/cloud-devops-engineering-roadmap/Study_Notes_System/akumen's/Linux/09-Absolute-and-Relative-Paths/03-Relative-Paths.md)
- Next: [05 - Absolute vs Relative Comparison Matrix](file:///c:/Users/binil/OneDrive/Desktop/cloud-devops-engineering-roadmap/Study_Notes_System/akumen's/Linux/09-Absolute-and-Relative-Paths/05-Absolute-vs-Relative-Comparison.md)
