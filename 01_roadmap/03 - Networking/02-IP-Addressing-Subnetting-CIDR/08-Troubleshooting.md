# 08 — Subnetting Troubleshooting Guide & Tools

---

## 1. CLI Tools for Subnet Verification

```bash
# 1. Install ipcalc on Debian/Ubuntu
sudo apt-get install -y ipcalc

# 2. Calculate network address, broadcast, and host range
ipcalc 10.0.1.50/26

# Output:
# Address:   10.0.1.50            00001010.00000000.00000001.00 110010
# Netmask:   255.255.255.192 = 26 11111111.11111111.11111111.11 000000
# Wildcard:  0.0.0.63             00000000.00000000.00000000.00 111111
# =>
# Network:   10.0.1.0/26
# HostMin:   10.0.1.1
# HostMax:   10.0.1.62
# Broadcast: 10.0.1.63
# Hosts/Net: 62                   (Private Internet)
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 07 - Real World Scenarios](./07-Real-World-Scenarios.md) | [Index](../../../README.md) | [09 - Interview QA →](./09-Interview-QA.md) |
