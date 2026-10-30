# 03 - Configuring Git LFS and .gitattributes

## 1. Tracking File Extensions

```bash
# Initialize LFS once on your system
git lfs install

# Track specific large binary file types
git lfs track "*.onnx"
git lfs track "*.psd"
git lfs track "*.mp4"
```

---

## 2. The `.gitattributes` File

Tracking updates `.gitattributes`. **This file MUST be committed to Git:**

```text
*.onnx filter=lfs diff=lfs merge=lfs -text
*.psd filter=lfs diff=lfs merge=lfs -text
*.mp4 filter=lfs diff=lfs merge=lfs -text
```

```bash
git add .gitattributes
git commit -m "chore: track AI model and video files with Git LFS"
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 02 - Git LFS Architecture and Pointer Files](./02-Git-LFS-Architecture-and-Pointer-Files.md) | [Index](../../../README.md) | [04 - File Locking and Binary Conflict Prevention →](./04-File-Locking-and-Binary-Conflict-Prevention.md) |
