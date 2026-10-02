# 12 - Git Internals: Quick Revision Cheat Sheet

## Object Types Summary

| Object | Contains | Command to Inspect |
|---|---|---|
| **Blob** | Raw file data (no filename) | `git cat-file -p <SHA>` |
| **Tree** | List of (mode, type, SHA, filename) | `git ls-tree <SHA>` |
| **Commit** | Tree SHA, Parent SHA, Author, Message | `git cat-file -p <SHA>` |
| **Tag** | Commit SHA, Tagger, Message, GPG Sig | `git cat-file -p <SHA>` |

## Essential Plumbing Commands
- `git hash-object -w <file>`: Compute SHA and write blob.
- `git cat-file -p <SHA>`: Pretty-print object content.
- `git write-tree`: Write current staging index as a tree object.
- `git commit-tree <tree_sha> -m "msg"`: Create commit object.
- `git fsck --full`: Audit repository object integrity.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 11 - MCQ](./11-MCQ.md) | [Index](../../../README.md) | [Next Module (05-GitHub-and-GitLab-Collaboration) →](../05-GitHub-and-GitLab-Collaboration/01-Remotes-and-Tracking-Branches-Architecture.md) |
