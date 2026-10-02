# 05 - AWS EC2 Dynamic Inventory Configuration

## Sample `aws_ec2.yml`
```yaml
plugin: amazon.aws.aws_ec2
regions:
  - us-east-1
  - us-west-2
filters:
  instance-state-name: running
keyed_groups:
  - key: tags.Environment
    prefix: env
  - key: tags.Role
    prefix: role
hostnames:
  - dns-name
  - private-ip-address
compose:
  ansible_host: private_ip_address
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [04 - Dynamic Inventory Plugins](./04-Dynamic-Inventory-Plugins-vs-Legacy-Scripts.md) | [README](./README.md) | [06 - Constructed Inventory](./06-Constructed-Inventory-and-Multiple-Inventory-Sources.md) |
