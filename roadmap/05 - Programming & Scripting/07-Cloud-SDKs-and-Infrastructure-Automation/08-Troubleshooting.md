# 08 - Troubleshooting & Diagnostic Runbooks

## 1. Inspecting Raw HTTP Wire Traces in Boto3

When debugging mysterious authentication errors or payload mismatches, enable low-level wire logging:

```python
import logging
import boto3

# Enable DEBUG logging on botocore wire protocol
logging.basicConfig(level=logging.DEBUG)
logging.getLogger('botocore').setLevel(logging.DEBUG)
logging.getLogger('urllib3').setLevel(logging.DEBUG)

s3 = boto3.client('s3')
s3.list_buckets()
# Outputs full HTTP headers, CanonicalRequest string, and response JSON!
```

---

## 2. Mocking Cloud SDKs with `moto` in Unit Tests

Never run automated test suites against live AWS accounts. Use `moto` to intercept network calls and emulate AWS services locally in-memory.

```python
import boto3
import pytest
from moto import mock_aws

@mock_aws
def test_s3_bucket_creation_and_listing():
    s3 = boto3.client('s3', region_name='us-east-1')
    
    # Create mock bucket
    s3.create_bucket(Bucket='test-audit-bucket')
    
    response = s3.list_buckets()
    bucket_names = [b['Name'] for b in response['Buckets']]
    
    assert 'test-audit-bucket' in bucket_names
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [07 - Real-World Scenarios](./07-Real-World-Scenarios.md) | [README](./README.md) | [09 - Interview Questions](./09-Interview-QA.md) |
