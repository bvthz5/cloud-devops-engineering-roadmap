# 10 - Hands-On Practice Labs

## Lab 1: Multi-Region Orphaned Snapshot Cleaner with Boto3

### Objective
Write a Python script that iterates over all enabled AWS regions, detects snapshots older than 90 days that do not have an active AMI association, and outputs a CSV report.

### Code Blueprint
```python
import boto3
from datetime import datetime, timezone, timedelta

def audit_orphaned_snapshots(days_threshold=90):
    ec2_global = boto3.client('ec2', region_name='us-east-1')
    regions = [r['RegionName'] for r in ec2_global.describe_regions()['Regions']]
    cutoff = datetime.now(timezone.utc) - timedelta(days=days_threshold)
    
    sts = boto3.client('sts')
    account_id = sts.get_caller_identity()['Account']

    for region in regions:
        print(f"Auditing snapshots in {region}...")
        ec2 = boto3.client('ec2', region_name=region)
        
        # Only query self-owned snapshots
        snapshots = ec2.describe_snapshots(OwnerIds=[account_id])['Snapshots']
        for snap in snapshots:
            if snap['StartTime'] < cutoff:
                print(f"Candidate for deletion: {snap['SnapshotId']} in {region} (Created: {snap['StartTime']})")

if __name__ == '__main__':
    audit_orphaned_snapshots()
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [09 - Interview Questions](./09-Interview-QA.md) | [README](./README.md) | [11 - Multiple-Choice Assessment](./11-MCQ.md) |
