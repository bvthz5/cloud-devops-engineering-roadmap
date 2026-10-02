# 03 - Python Unit and Integration Testing: pytest and Moto

## 1. Production Testing with `pytest`

`pytest` is the de facto Python testing framework, supporting expressive assertions, reusable fixtures with dependency injection, and parametric testing.

```python
import pytest

def calculate_cidr_subnets(base_cidr: str, prefix_len: int):
    import ipaddress
    net = ipaddress.ip_network(base_cidr)
    return [str(sn) for sn in net.subnets(new_prefix=prefix_len)]

# Parameterized Test Matrix
@pytest.mark.parametrize("base,prefix,expected_count", [
    ("10.0.0.0/16", 18, 4),
    ("10.0.0.0/16", 24, 256),
    ("192.168.1.0/24", 26, 4),
])
def test_calculate_cidr_subnets(base, prefix, expected_count):
    subnets = calculate_cidr_subnets(base, prefix)
    assert len(subnets) == expected_count
```

---

## 2. Mocking Cloud Infrastructure with `moto`

Running tests against live cloud providers is slow, incurs infrastructure costs, and introduces non-deterministic test failures (network jitter, IAM eventual consistency). **`moto`** intercepts `botocore` HTTP calls and emulates over 70 AWS services entirely in memory.

```python
import boto3
import pytest
from moto import mock_aws

def purge_empty_s3_buckets():
    s3 = boto3.client('s3', region_name='us-east-1')
    buckets = s3.list_buckets().get('Buckets', [])
    purged = []
    
    for b in buckets:
        name = b['Name']
        objects = s3.list_objects_v2(Bucket=name).get('KeyCount', 0)
        if objects == 0:
            s3.delete_bucket(Bucket=name)
            purged.append(name)
    return purged

@mock_aws
def test_purge_empty_s3_buckets():
    s3 = boto3.client('s3', region_name='us-east-1')
    
    # 1. Setup mock environment
    s3.create_bucket(Bucket='empty-bucket-1')
    s3.create_bucket(Bucket='empty-bucket-2')
    s3.create_bucket(Bucket='populated-bucket')
    s3.put_object(Bucket='populated-bucket', Key='data.json', Body=b'{}')

    # 2. Execute target function
    purged = purge_empty_s3_buckets()

    # 3. Assertions
    assert 'empty-bucket-1' in purged
    assert 'empty-bucket-2' in purged
    assert 'populated-bucket' not in purged
    
    remaining = [b['Name'] for b in s3.list_buckets()['Buckets']]
    assert remaining == ['populated-bucket']
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 02 - Testing Bash Scripts with Bats and ShellCheck](./02-Testing-Bash-Scripts-with-Bats-and-ShellCheck.md) | [Index](../../../README.md) | [04 - Golang Testing and Testcontainers →](./04-Golang-Testing-and-Testcontainers.md) |
