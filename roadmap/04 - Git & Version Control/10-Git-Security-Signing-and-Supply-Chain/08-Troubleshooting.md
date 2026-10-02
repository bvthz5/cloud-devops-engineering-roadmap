# 08 - Git Security: Troubleshooting Guide

## 1. GPG Signing Fails: "Secret Key Not Available"

```bash
# Error: gpg: signing failed: secret key not available
# Fix: Export GPG_TTY in your ~/.bashrc or ~/.zshrc
export GPG_TTY=$(tty)

# Test GPG agent
echo "test" | gpg --clearsign
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [07 - Real-World Scenarios](./07-Real-World-Scenarios.md) | [README](./README.md) | [09 - Interview Q&A](./09-Interview-QA.md) |
