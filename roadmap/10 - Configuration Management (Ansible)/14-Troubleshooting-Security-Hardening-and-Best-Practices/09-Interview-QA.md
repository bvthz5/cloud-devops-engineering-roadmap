# 09 - Interview Q&A: Debugging & Security

### Q: How does Ansible interactive debugger work?
**Answer:** When a task fails, specifying `debugger: on_failed` drops into an interactive shell allowing you to inspect task variables (`p task_vars`), modify variables (`task_vars['key'] = 'val'`), and retry execution (`redo`).

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [08 - Troubleshooting](./08-Troubleshooting.md) | [README](./README.md) | [10 - Hands-On Practice](./10-Hands-On-Practice.md) |
