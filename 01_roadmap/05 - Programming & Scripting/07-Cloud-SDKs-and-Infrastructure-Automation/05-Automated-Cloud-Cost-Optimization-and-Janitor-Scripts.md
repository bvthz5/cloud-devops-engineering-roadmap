# 05 - Automated Cloud Cost Optimization and Janitor Scripts

## 1. Enterprise Cloud Waste Metrics

Cloud bills commonly accumulate thousands of dollars in "zombie infrastructure":
1. **Unattached EBS Volumes**: Disks detached after EC2 instances were terminated without `-DeleteOnTermination`.
2. **Orphaned Snapshots**: Old automated snapshots past retention policies whose parent volumes no longer exist.
3. **Idle Elastic IP Addresses**: Reserved static IPs not attached to running instances (AWS charges hourly for unattached IPs).
4. **Orphaned Disk Volumes in Azure/GCP**: OS and data disks left behind when VMs are deleted.

---

## 2. Production AWS Janitor Script: Unattached EBS Volumes

```python
import boto3
from datetime import datetime, timezone
import logging

logging.basicConfig(level=logging.INFO, format="%(asctime)s [%(levelname)s] %(message)s")
logger = logging.getLogger("AWS-Janitor")

def clean_unattached_ebs_volumes(region: str, dry_run: bool = True):
    ec2 = boto3.client('ec2', region_name=region)
    paginator = ec2.get_paginator('describe_volumes')
    
    # Filter only volumes in 'available' state (unattached)
    page_iterator = paginator.paginate(
        Filters=[{'Name': 'status', 'Values': ['available']}]
    )

    total_wasted_gb = 0
    candidate_volumes = []

    for page in page_iterator:
        for vol in page.get('Volumes', []):
            vol_id = vol['VolumeId']
            size_gb = vol['Size']
            created_at = vol['CreateTime']
            
            # Check for exemption tags
            tags = {t['Key']: t['Value'] for t in vol.get('Tags', [])}
            if tags.get('JanitorExempt', '').lower() == 'true':
                logger.info(f"Skipping exempt volume {vol_id}")
                continue

            total_wasted_gb += size_gb
            candidate_volumes.append((vol_id, size_gb))

    logger.info(f"Found {len(candidate_volumes)} unattached volumes totaling {total_wasted_gb} GB in {region}.")

    for vol_id, size_gb in candidate_volumes:
        if dry_run:
            logger.info(f"[DRY-RUN] Would delete unattached volume: {vol_id} ({size_gb} GB)")
        else:
            try:
                logger.warning(f"DELETING unattached volume: {vol_id}...")
                ec2.delete_volume(VolumeId=vol_id)
                logger.info(f"Successfully deleted {vol_id}.")
            except Exception as e:
                logger.error(f"Failed to delete {vol_id}: {str(e)}")

if __name__ == "__main__":
    clean_unattached_ebs_volumes(region="us-east-1", dry_run=True)
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 04 - Google Cloud Client Libraries and Service Accounts](./04-Google-Cloud-Client-Libraries-and-Service-Accounts.md) | [Index](../../../README.md) | [06 - Security Scanning and Compliance Automation with SDKs →](./06-Security-Scanning-and-Compliance-Automation-with-SDKs.md) |
