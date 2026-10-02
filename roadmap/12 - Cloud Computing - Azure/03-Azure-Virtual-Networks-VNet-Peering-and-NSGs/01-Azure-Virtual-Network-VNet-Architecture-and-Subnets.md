# 01 - Azure Virtual Network (VNet) Architecture

```text
VNet CIDR: 10.1.0.0/16
 ├── Subnet-Web  : 10.1.1.0/24
 ├── Subnet-App  : 10.1.2.0/24
 └── Subnet-DB   : 10.1.3.0/24
```

Azure reserves 5 IP addresses per subnet (.0, .1, .2, .3, and broadcast .255).

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [README](./README.md) | [README](./README.md) | [02 - NSG & ASG](./02-Network-Security-Groups-NSG-and-Application-Security-Groups-ASG.md) |
