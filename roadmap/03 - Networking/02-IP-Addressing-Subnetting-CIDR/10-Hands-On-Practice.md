# 10 — Hands-On Practice Labs: Subnetting & CIDR

---

## Lab 1: Subnet Calculation Drills with Python

```python
import ipaddress

# Create a network object
net = ipaddress.ip_network('192.168.10.0/24')
print(f"Total Hosts: {net.num_addresses}")
print(f"Netmask:     {net.netmask}")

# Subnet into 4 smaller subnets (/26)
subnets = list(net.subnets(new_prefix=26))
for i, sub in enumerate(subnets):
    print(f"Subnet {i+1}: {sub} | Usable Range: {sub[1]} - {sub[-2]}")
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [09 - Interview Q&A](./09-Interview-QA.md) | [README](./README.md) | [11 - Multiple Choice Questions](./11-MCQ.md) |
