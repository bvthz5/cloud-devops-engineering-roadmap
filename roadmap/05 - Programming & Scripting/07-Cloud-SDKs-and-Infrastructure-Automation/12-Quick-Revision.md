# 12 - Quick-Revision & Enterprise Cheat Sheet

```python
# AWS Boto3 Config with Adaptive Retries
from botocore.config import Config
import boto3

config = Config(retries={'max_attempts': 5, 'mode': 'adaptive'}, max_pool_connections=25)
s3 = boto3.client('s3', config=config)

# S3 Pagination
paginator = s3.get_paginator('list_objects_v2')
for page in paginator.paginate(Bucket='my-bucket'):
    for obj in page.get('Contents', []):
        print(obj['Key'])

# Azure DefaultAzureCredential
from azure.identity import DefaultAzureCredential
from azure.mgmt.compute import ComputeManagementClient

cred = DefaultAzureCredential()
compute = ComputeManagementClient(cred, "sub-id")

# GCP Application Default Credentials
from google.cloud import storage
client = storage.Client()  # Uses ADC automatically
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [11 - Multiple-Choice Assessment](./11-MCQ.md) | [README](./README.md) | [08 - Testing & Quality for DevOps Code](../08-Testing-and-Quality-for-DevOps-Code/README.md) |
