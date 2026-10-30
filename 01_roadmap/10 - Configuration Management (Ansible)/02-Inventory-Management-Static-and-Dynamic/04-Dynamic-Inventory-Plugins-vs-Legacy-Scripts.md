# 04 - Dynamic Inventory Plugins vs Legacy Scripts

## Why Dynamic Inventory?
In cloud environments (AWS EC2, Azure VMs, GCP Compute), servers auto-scale and IP addresses change frequently. Static inventory files quickly become stale.

## Ansible Inventory Plugins Architecture
Inventory plugins (file ending in `aws_ec2.yml`, `azure_rm.yml`) replace legacy executable python scripts (`ec2.py`).

```bash
# Query active dynamic inventory via CLI
ansible-inventory -i aws_ec2.yml --graph
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 03 - Inventory Variables host_vars and group_vars](./03-Inventory-Variables-host_vars-and-group_vars.md) | [Index](../../../README.md) | [05 - AWS EC2 Dynamic Inventory Configuration →](./05-AWS-EC2-Dynamic-Inventory-Configuration.md) |
