# 03 - Plumbing vs. Porcelain Commands Deep Dive

## 1. Architectural Distinction

- **Porcelain Commands:** User-friendly, high-level commands designed for human everyday use (`git add`, `git commit`, `git checkout`, `git status`).
- **Plumbing Commands:** Low-level, UNIX-style tools designed for scripts, automation, and internal Git engine execution.

---

## 2. Core Plumbing Commands Reference

### 1. `git hash-object`
Computes the SHA hash of a file and optionally writes it to `.git/objects`:
```bash
# Write file content directly to Git database as a blob:
echo "Hello DevOps World" | git hash-object -w --stdin
# Returns 40-character SHA hash: 557db03de997c86a4a028e1ebd3a1ceb225be238
```

### 2. `git cat-file`
Inspects objects in the `.git/objects/` store:
```bash
# View object type (blob, tree, commit, tag):
git cat-file -t 557db03

# View object size in bytes:
git cat-file -s 557db03

# View object contents (pretty-print):
git cat-file -p 557db03
```

### 3. `git ls-tree`
Inspects the directory contents of a Tree object:
```bash
git ls-tree HEAD
# Output:
# 100644 blob 557db03...    README.md
# 040000 tree 8f2a1b...    src
```

### 4. `git update-ref`
Safely updates a branch reference pointer:
```bash
git update-ref refs/heads/main 1a2b3c4d...
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [02 - 4 Core Objects](./02-The-Four-Core-Git-Objects-Blobs-Trees-Commits-Tags.md) | [README](./README.md) | [04 - Packfiles & GC](./04-Packfiles-Delta-Compression-and-Garbage-Collection.md) |
