# 06 - Constructed Inventory & Multiple Inventory Sources

You can pass multiple inventory directories or files to Ansible:
```bash
ansible-playbook -i inventory/static_hosts.ini -i inventory/aws_ec2.yml site.yml
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [05 - AWS EC2 Dynamic Inventory](./05-AWS-EC2-Dynamic-Inventory-Configuration.md) | [README](./README.md) | [07 - Real-World Scenarios](./07-Real-World-Scenarios.md) |
