# Hands-On Practice - FinOps Principles & Cost Allocation Tagging

> **Module**: FinOps Principles & Cost Allocation Tagging

---

## 🛠 Lab: Implementing Shift-Left Cost Analysis with Infracost & Terraform

### Objective
In this lab, you will install Infracost, evaluate cost impacts of Terraform infrastructure changes locally, and integrate cost breakdown reports into your workflow.

---

## 📋 Task 1: Install Infracost CLI
```bash
# Install Infracost CLI on Linux/macOS
curl -fsSL https://raw.githubusercontent.com/infracost/infracost/master/scripts/install.sh | sh

# Verify installation
infracost --version
```

---

## 📋 Task 2: Generate Cost Breakdown for Terraform Project
```bash
# Navigate to Terraform codebase
cd terraform-infrastructure/

# Run cost breakdown
infracost breakdown --path .
```

---

## 📋 Task 3: Compare Cost Difference on Branch Changes
```bash
# Save baseline cost of main branch
infracost breakdown --path . --out-file infracost-base.json

# Switch feature branch and run diff
infracost diff --path . --compare-to-ip --out-file infracost-base.json
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 09 - Interview QA](./09-Interview-QA.md) | [Index](../../../README.md) | [11 - MCQ →](./11-MCQ.md) |
