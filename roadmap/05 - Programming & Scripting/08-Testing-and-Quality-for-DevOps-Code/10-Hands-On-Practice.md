# 10 - Hands-On Practice Labs

## Lab 1: Comprehensive pytest + Moto Suite for Cloud Watchdog

### Objective
Write an automated unit test suite using `pytest` and `moto` to verify that an AWS EC2 instance reboot watchdog handles both running and stopped instances correctly.

### Implementation
```python
import boto3
import pytest
from moto import mock_aws

def reboot_stale_instances(tag_key: str, tag_val: str):
    ec2 = boto3.client('ec2', region_name='us-east-1')
    response = ec2.describe_instances(
        Filters=[
            {'Name': f'tag:{tag_key}', 'Values': [tag_val]},
            {'Name': 'instance-state-name', 'Values': ['running']}
        ]
    )
    
    rebooted = []
    for r in response.get('Reservations', []):
        for inst in r.get('Instances', []):
            i_id = inst['InstanceId']
            ec2.reboot_instances(InstanceIds=[i_id])
            rebooted.append(i_id)
    return rebooted

@mock_aws
def test_reboot_stale_instances_only_targets_running_tagged():
    ec2 = boto3.client('ec2', region_name='us-east-1')
    
    # 1. Launch a tagged running instance
    inst1 = ec2.run_instances(
        ImageId='ami-12345678', MinCount=1, MaxCount=1,
        TagSpecifications=[{'ResourceType': 'instance', 'Tags': [{'Key': 'Env', 'Value': 'Stage'}]}]
    )['Instances'][0]['InstanceId']

    # 2. Launch an untagged instance
    inst2 = ec2.run_instances(ImageId='ami-12345678', MinCount=1, MaxCount=1)['Instances'][0]['InstanceId']

    # 3. Test execution
    rebooted = reboot_stale_instances(tag_key='Env', tag_val='Stage')

    assert rebooted == [inst1]
    assert inst2 not in rebooted
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [09 - Interview Questions](./09-Interview-QA.md) | [README](./README.md) | [11 - Multiple-Choice Assessment](./11-MCQ.md) |
