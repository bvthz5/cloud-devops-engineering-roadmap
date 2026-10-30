# 01 - Merge Conflict Anatomy and diff3 Visualization

## 1. What Causes a Merge Conflict?

A conflict occurs when two branches modify the **exact same lines of a file** differently since their common merge base, or when one branch deletes a file that the other modified.

```text
<<<<<<< HEAD (Current Branch)
DB_PORT=5432
=======
DB_PORT=5433
>>>>>>> feature/new-port (Incoming Branch)
```

---

## 2. Enabling `diff3` Style (Show Common Ancestor)

By default, Git only shows your version and the incoming version. You have no idea what the file originally looked like!
Enabling **`diff3`** reveals the **Original Common Ancestor (Base)**:

```bash
git config --global merge.conflictstyle diff3
```

Now conflict markers display three distinct sections:
```text
<<<<<<< HEAD
DB_PORT=5432
||||||| merged common ancestors (What it looked like originally)
DB_PORT=3306
=======
DB_PORT=5433
>>>>>>> feature/new-port
```
*Clarity:* You can immediately deduce that the original port was `3306`, HEAD changed it to `5432`, and the incoming branch changed it to `5433`.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Prev Module (06-Git-Hooks-and-Automation)](../06-Git-Hooks-and-Automation/12-Quick-Revision.md) | [Index](../../../README.md) | [02 - git rerere Reuse Recorded Resolution →](./02-git-rerere-Reuse-Recorded-Resolution.md) |
