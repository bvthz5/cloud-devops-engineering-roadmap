# 01 - AWX vs AAP vs Tower Architecture Overview

AWX is the upstream open-source project for Red Hat Ansible Automation Platform (AAP) (formerly Ansible Tower).

```text
+-------------------------------------------------+
|               AWX / AAP WEB UI & API            |
+------------------------+------------------------+
                         |
                         v
+-------------------------------------------------+
|             EXECUTION ENVIRONMENT (EE)          |
|  Containerized Runtime Image (Podman / K8s)     |
|  - ansible-core + Py modules + CLI tools        |
+-------------------------------------------------+
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Prev Module (09-Ansible-for-Cloud-and-Kubernetes)](../09-Ansible-for-Cloud-and-Kubernetes/12-Quick-Revision.md) | [Index](../../../README.md) | [02 - Automation Execution Environments EE and ansible builder →](./02-Automation-Execution-Environments-EE-and-ansible-builder.md) |
