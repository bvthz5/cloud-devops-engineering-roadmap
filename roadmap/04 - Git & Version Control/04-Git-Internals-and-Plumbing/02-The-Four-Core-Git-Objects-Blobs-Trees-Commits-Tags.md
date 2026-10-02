# 02 - The Four Core Git Objects: Blobs, Trees, Commits, Tags

## 1. How Git Stores Content

Git is fundamentally a **Content-Addressable Key-Value Store**. 
- **Key:** A 40-character hexadecimal SHA-1 hash (or 64-character SHA-256 hash).
- **Value:** Compressed raw data prefixed with a header: `"<type> <size>\0<content>"`.

```
                    +---------------------------+
                    |       Commit Object       |
                    | tree: 4a2b1c...           |
                    | parent: 8f9e0d...         |
                    | author: Alice <alice@co>  |
                    | committer: Alice          |
                    | message: feat: add api    |
                    +---------------------------+
                                  │
                                  ▼
                    +---------------------------+
                    |        Tree Object        | (Directory: root /)
                    | 100644 blob 3e8f... main.py|
                    | 040000 tree 9b1c... src/   |
                    +---------------------------+
                                  │
                                  ▼
                    +---------------------------+
                    |        Blob Object        | (File content: main.py)
                    | "import os\nprint('OK')"   |
                    +---------------------------+
```

---

## 2. The 4 Object Types Explained

| Object Type | Represents | Internal Content |
|---|---|---|
| **Blob** (Binary Large Object) | File contents only | Pure byte stream of the file. **Does NOT store filename or permissions!** |
| **Tree** | A directory structure | A list of entries containing: file mode (e.g. `100644`), object type, SHA hash, and **filename**. |
| **Commit** | A permanent historical snapshot | Pointer to the top-level root Tree, pointer(s) to parent Commit(s), author info, committer info, and commit message. |
| **Annotated Tag** | An immutable release reference | Pointer to a commit, tagger identity, timestamp, GPG signature, and tag message. |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [01 - Inside .git](./01-Inside-the-dot-git-Directory-Anatomy.md) | [README](./README.md) | [03 - Plumbing vs Porcelain](./03-Plumbing-vs-Porcelain-Commands-Deep-Dive.md) |
