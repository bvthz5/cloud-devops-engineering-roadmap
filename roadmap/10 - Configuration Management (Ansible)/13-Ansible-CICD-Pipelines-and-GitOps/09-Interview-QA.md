# 09 - Interview Q&A: Ansible CI/CD

### Q: How do you implement dry-run validation in an automated Pull Request pipeline?
**Answer:** By running `ansible-playbook --check --diff` in CI and capturing the diff output to post as a PR review comment.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [08 - Troubleshooting](./08-Troubleshooting.md) | [README](./README.md) | [10 - Hands-On Practice](./10-Hands-On-Practice.md) |
