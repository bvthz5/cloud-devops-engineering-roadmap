# Hands-On Practice - Multi-Cloud Observability & Centralized Telemetry

> **Module**: Multi-Cloud Observability & Centralized Telemetry

---

## 🛠 Lab: Automated Cloud Waste Cleanup with Cloud Custodian

### Objective
In this lab, you will install Cloud Custodian, write a YAML policy to detect unattached AWS EBS volumes or Azure Managed Disks, and execute a dry-run audit report.

---

## 📋 Task 1: Install Cloud Custodian
```bash
# Install Cloud Custodian via Python pip
pip install c7n

# Verify installation
custodian version
```

---

## 📋 Task 2: Create Waste Detection Policy (`policy.yml`)
```yaml
policies:
  - name: ebs-unattached-waste-cleanup
    resource: aws.ebs
    comment: |
      Identify unattached EBS volumes older than 7 days
    filters:
      - status: available
      - type: value
        key: create_time
        op: greater-than
        value_type: age
        value: 7
    actions:
      - type: mark-for-op
        op: delete
        days: 7
```

---

## 📋 Task 3: Run Policy in Dry-Run Audit Mode
```bash
# Execute dry-run to generate output report
custodian run --dryrun -s ./output-logs policy.yml

# Inspect identified unattached volumes
cat ./output-logs/ebs-unattached-waste-cleanup/resources.json | jq .
```
