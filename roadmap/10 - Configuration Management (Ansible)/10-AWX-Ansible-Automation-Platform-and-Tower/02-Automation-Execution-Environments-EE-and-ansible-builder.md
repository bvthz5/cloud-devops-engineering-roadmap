# 02 - Automation Execution Environments (EE) & `ansible-builder`

Execution Environments replace legacy virtualenvs with container images built using `ansible-builder`.

```yaml
# execution-environment.yml
version: 3
images:
  base_image:
    name: registry.redhat.io/ansible-automation-platform-2.4/ee-supported-rhel9:latest

dependencies:
  galaxy: requirements.yml
  python: requirements.txt
  system: bindep.txt
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [01 - AWX vs AAP Architecture](./01-AWX-vs-AAP-vs-Tower-Architecture-Overview.md) | [README](./README.md) | [03 - Job Templates & Workflows](./03-Job-Templates-Workflow-Job-Templates-and-Inventories.md) |
