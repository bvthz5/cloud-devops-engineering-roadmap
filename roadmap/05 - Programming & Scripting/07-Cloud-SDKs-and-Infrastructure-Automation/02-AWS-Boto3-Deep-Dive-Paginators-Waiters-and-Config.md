# 02 - AWS Boto3 Deep Dive: Paginators, Waiters, and Config

## 1. Paginators: Handling Truncated Result Sets

Most cloud APIs truncate list responses (e.g., returning a maximum of 1,000 items per call) and provide a `NextToken` or `ContinuationToken`. Manually writing `while` loops for pagination is error-prone. Boto3 **Paginators** abstract token management cleanly.

```python
import boto3
from botocore.exceptions import ClientError

def get_all_s3_objects(bucket_name: str, prefix: str = ""):
    s3_client = boto3.client('s3')
    paginator = s3_client.get_paginator('list_objects_v2')
    
    page_iterator = paginator.paginate(
        Bucket=bucket_name,
        Prefix=prefix,
        PaginationConfig={
            'PageSize': 1000  # Number of items per API call
        }
    )

    total_size = 0
    object_count = 0

    for page in page_iterator:
        for item in page.get('Contents', []):
            total_size += item['Size']
            object_count += 1

    return object_count, total_size
```

---

## 2. Waiters: Polling for Asynchronous Resource State Transitions

Creating or modifying cloud resources (e.g., provisioning an RDS database, launching an EC2 instance, or restoring a snapshot) is asynchronous. **Waiters** evaluate resource state at defined intervals until the desired state is reached or a timeout occurs.

```python
import boto3

ec2 = boto3.client('ec2', region_name='us-east-1')

# Stop an instance
instance_id = "i-0a1b2c3d4e5f67890"
print(f"Stopping instance {instance_id}...")
ec2.stop_instances(InstanceIds=[instance_id])

# Initialize Waiter
waiter = ec2.get_waiter('instance_stopped')
print("Waiting for instance to fully transition to 'stopped'...")

# Polls every 15 seconds up to 40 attempts (600s total)
waiter.wait(
    InstanceIds=[instance_id],
    WaiterConfig={
        'Delay': 15,
        'MaxAttempts': 40
    }
)

print(f"Instance {instance_id} is successfully stopped.")
```

---

## 3. Cross-Account STS AssumeRole with External ID

```python
import boto3

def get_cross_account_client(target_account_id: str, role_name: str, external_id: str, service: str):
    sts_client = boto3.client('sts')
    role_arn = f"arn:aws:iam::{target_account_id}:role/{role_name}"
    
    assumed_role = sts_client.assume_role(
        RoleArn=role_arn,
        RoleSessionName="CloudJanitorAuditSession",
        ExternalId=external_id,
        DurationSeconds=3600
    )
    
    credentials = assumed_role['Credentials']
    
    return boto3.client(
        service,
        aws_access_key_id=credentials['AccessKeyId'],
        aws_secret_access_key=credentials['SecretAccessKey'],
        aws_session_token=credentials['SessionToken']
    )
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 01 - Cloud SDK Architecture Client vs Resource Models](./01-Cloud-SDK-Architecture-Client-vs-Resource-Models.md) | [Index](../../../README.md) | [03 - Azure SDK for Python and Identity Libraries →](./03-Azure-SDK-for-Python-and-Identity-Libraries.md) |
