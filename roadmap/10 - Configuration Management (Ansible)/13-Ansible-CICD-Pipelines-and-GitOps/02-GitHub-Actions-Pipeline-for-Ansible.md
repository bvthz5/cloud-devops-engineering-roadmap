# 02 - GitHub Actions Pipeline for Ansible

```yaml
name: Ansible CI
on: [push, pull_request]

jobs:
  lint:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Set up Python
        uses: actions/setup-python@v4
        with:
          python-version: '3.10'
      - name: Install dependencies
        run: pip install ansible-core ansible-lint
      - name: Run ansible-lint
        run: ansible-lint site.yml
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [01 - CI/CD Pipeline Architecture](./01-Ansible-in-CI-CD-Pipeline-Architecture.md) | [README](./README.md) | [03 - GitLab CI Pipeline](./03-GitLab-CI-Pipeline-for-Ansible.md) |
