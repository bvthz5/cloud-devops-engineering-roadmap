# 03 - Checkov Policy-as-Code Scanner

## 1. Installation & Usage

```bash
pip install checkov
checkov -d .
checkov -d . --output json > checkov-results.json
checkov -d . --framework terraform --check CKV_AWS_18   # Specific check
```

## 2. Custom Check

```python
# custom_checks/no_public_s3.py
from checkov.terraform.checks.resource.base_resource_check import BaseResourceCheck
from checkov.common.models.enums import CheckResult, CheckCategories

class S3NoPublicAccess(BaseResourceCheck):
    def __init__(self):
        name = "Ensure S3 bucket is not publicly accessible"
        id = "CKV_CUSTOM_1"
        supported_resources = ["aws_s3_bucket"]
        categories = [CheckCategories.GENERAL_SECURITY]
        super().__init__(name=name, id=id, categories=categories,
                         supported_resources=supported_resources)

    def scan_resource_conf(self, conf):
        acl = conf.get("acl", ["private"])
        if acl[0] in ["public-read", "public-read-write"]:
            return CheckResult.FAILED
        return CheckResult.PASSED

check = S3NoPublicAccess()
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [02 - tfsec](./02-tfsec-Static-Security-Scanner.md) | [README](./README.md) | [04 - Secret Management](./04-Secret-Management-Vault-AWS-SSM-SOPS.md) |
