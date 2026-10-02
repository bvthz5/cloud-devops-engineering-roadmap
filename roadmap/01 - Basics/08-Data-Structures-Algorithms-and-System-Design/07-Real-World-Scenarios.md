# 07 — Real-World System Design Production Scenarios

---

## Scenario 1: The $O(N^2)$ Terraform Inventory Sync Outage

### Incident Summary
A custom Python script synchronizes 15,000 AWS EC2 instances with an internal CMDB.
Originally, when there were 200 servers, the script ran in 3 seconds.
At 15,000 servers, the script runs for 4 hours, pegging CPU at 100% and timing out.

### Root Cause Analysis
Code snippet in the script:
```python
# Anti-pattern: For every AWS instance, search a Python List of CMDB records
for aws_vm in aws_instances:          # N = 15,000
    for cmdb_vm in cmdb_records:      # N = 15,000
        if aws_vm.id == cmdb_vm.id:
            sync(aws_vm, cmdb_vm)
```
Searching a Python List is $O(N)$.
Total comparisons: $15,000 	imes 15,000 = \mathbf{225,000,000}$ iterations ($O(N^2)$)!

### Production Solution
Convert the CMDB list into a **Hash Map (Dictionary)** before the loop ($O(N)$ preparation):
```python
cmdb_dict = {vm.id: vm for vm in cmdb_records}  # O(N) once

for aws_vm in aws_instances:                    # O(N)
    if aws_vm.id in cmdb_dict:                  # O(1) hash lookup!
        sync(aws_vm, cmdb_dict[aws_vm.id])
```
Total complexity dropped from $O(N^2)$ to $O(N)$.
**Execution time dropped from 4 hours to 1.2 seconds!**

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [06 - Rate Limiting Algorithms](./06-Rate-Limiting-Algorithms-in-API-Gateways.md) | [README](./README.md) | [08 - Troubleshooting](./08-Troubleshooting.md) |
