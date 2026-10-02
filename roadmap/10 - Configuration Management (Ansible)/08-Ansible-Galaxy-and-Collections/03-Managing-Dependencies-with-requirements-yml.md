# 03 - Managing Dependencies with `requirements.yml`

```yaml
# requirements.yml
collections:
  - name: amazon.aws
    version: ">=6.0.0"
  - name: kubernetes.core
    version: "2.4.0"

roles:
  - name: geerlingguy.nginx
    version: "3.2.0"
```

Install command:
```bash
ansible-galaxy install -r requirements.yml
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [02 - Collections Architecture](./02-Ansible-Collections-Namespace-Collection-Name-Structure.md) | [README](./README.md) | [04 - Custom Collections](./04-Building-Publishing-and-Hosting-Custom-Collections.md) |
