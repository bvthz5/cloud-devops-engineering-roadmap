# 03 - GitLab CI Pipeline for Ansible

```yaml
stages:
  - lint
  - test
  - deploy

ansible_lint:
  stage: lint
  image: python:3.10
  script:
    - pip install ansible-lint
    - ansible-lint site.yml
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 02 - GitHub Actions Pipeline for Ansible](./02-GitHub-Actions-Pipeline-for-Ansible.md) | [Index](../../../README.md) | [04 - GitOps Workflow with Ansible and AWX Webhooks →](./04-GitOps-Workflow-with-Ansible-and-AWX-Webhooks.md) |
