# 08 - Python Automation: Troubleshooting Guide

## 1. Deadlock in `subprocess.Popen` with Pipes

### The Bug
Calling `proc.stdout.read()` when the process produces massive output locks up the script forever.

### The Fix
Use `proc.communicate()`:
```python
proc = subprocess.Popen(cmd, stdout=subprocess.PIPE, stderr=subprocess.PIPE, text=True)
stdout, stderr = proc.communicate(timeout=60) # Safely drains OS pipe buffers!
```

---

## 2. Resolving SSL Certificate Verification Errors
```python
# During corporate proxy inspection (Zscaler / Netskope):
# Point requests to custom corporate root CA bundle:
session.verify = "/etc/ssl/certs/corporate_ca.pem"
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 07 - Real World Scenarios](./07-Real-World-Scenarios.md) | [Index](../../../README.md) | [09 - Interview QA →](./09-Interview-QA.md) |
