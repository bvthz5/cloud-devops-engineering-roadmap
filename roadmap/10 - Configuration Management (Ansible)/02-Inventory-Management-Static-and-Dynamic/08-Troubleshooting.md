# 08 - Troubleshooting Inventory Issues

| Issue | Cause | Fix |
|---|---|---|
| `aws_ec2 plugin not found` | Collection `amazon.aws` missing. | `ansible-galaxy collection install amazon.aws` |
| `No hosts matched` | Target group name typo or `--limit` filter active. | Verify with `ansible-inventory --graph`. |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [07 - Real-World Scenarios](./07-Real-World-Scenarios.md) | [README](./README.md) | [09 - Interview Q&A](./09-Interview-QA.md) |
