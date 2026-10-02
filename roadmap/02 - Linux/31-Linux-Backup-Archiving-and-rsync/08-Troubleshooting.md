# 08 — Backup Troubleshooting Guide & Runbook

---

## 1. Safe rsync Testing Protocol

Before running any script containing `--delete`, always execute with dry-run and itemize changes:
```bash
rsync -avzPn --delete /src/ /dest/
```
Verify the itemized output before removing the `-n` flag!

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [07 - Real-World Scenarios](./07-Real-World-Scenarios.md) | [README](./README.md) | [09 - Interview Q&A](./09-Interview-QA.md) |
