# 02 - Cost-Effectiveness & Licensing Models

One of the primary strategic reasons enterprises choose Linux over Windows for infrastructure is the stark difference in **Total Cost of Ownership (TCO)** driven by licensing models.

---

## 💰 Windows Server Licensing Model

Windows Server uses a proprietary per-core licensing model combined with Client Access Licenses (CALs):

1. **Per-Core Licensing:** Windows Server Datacenter & Standard licenses require purchasing licenses for every physical or virtual CPU core (minimum 16 cores per server).
2. **Client Access Licenses (CALs):** Additional fees for every user or device connecting to the Windows Server services.
3. **Cloud Surcharge:** Running Windows Server on AWS EC2 or Azure incurs an hourly OS license surcharge added to raw compute rates.

---

## 🐧 Linux Licensing Model (GNU General Public License - GPL)

Linux kernel and standard core utilities are distributed under the **GNU General Public License (GPL)**:

1. **Free to Use & Modify:** Zero cost for operating system licenses regardless of the number of CPU cores, servers, or user connections.
2. **Optional Enterprise Support:** Organizations can choose community distros (Ubuntu Server, Debian, Rocky Linux, AlmaLinux) for $0, or opt for paid enterprise SLAs (Red Hat Enterprise Linux - RHEL, Canonical Ubuntu Advantage) only when needed.

---

## 📊 Cost Impact Breakdown in Cloud Infrastructure

```text
AWS EC2 Cost Comparison (Example: t3.large - 2 vCPU, 8 GB RAM):
  - Linux (Ubuntu / Debian / Amazon Linux): ~$0.0832 / hour  (~ $60 / month)
  - Windows Server:                        ~$0.1760 / hour  (~ $126 / month)
  
Cost Difference: Windows is ~110% more expensive per instance per month.
At a scale of 100 instances, Linux saves ~$66,000 per year on OS license fees alone.
```
---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [01 - Why Linux Is Preferred](./01-Why-Linux-Is-Preferred.md) | [README](./README.md) | [03 - Performance Efficiency and Resource Usage](./03-Performance-Efficiency-and-Resource-Usage.md) |
