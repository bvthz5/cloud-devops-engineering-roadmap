# 10 - Hands-On Practice Labs

## Lab 01: Run tfsec on Your Project

```bash
brew install tfsec   # or download binary
cd /path/to/terraform/project
tfsec .
# Review findings and fix HIGH/CRITICAL issues
```

## Lab 02: Run Checkov

```bash
pip install checkov
checkov -d . --framework terraform
# Review compliance results
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 09 - Interview QA](./09-Interview-QA.md) | [Index](../../../README.md) | [11 - MCQ →](./11-MCQ.md) |
