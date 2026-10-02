# 09 - Interview Q&A: Molecule & Lint

### Q: How does Molecule verify role idempotency?
**Answer:** Molecule runs the converge playbook twice. On the second execution, it verifies that 0 tasks report a `changed` state.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [08 - Troubleshooting](./08-Troubleshooting.md) | [README](./README.md) | [10 - Hands-On Practice](./10-Hands-On-Practice.md) |
