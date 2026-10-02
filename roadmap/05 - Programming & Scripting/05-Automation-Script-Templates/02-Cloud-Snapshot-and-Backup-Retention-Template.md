# 02 - Cloud Snapshot and Backup Retention Template

```python
#!/usr/bin/env python3
# automated_ebs_backup.py
import boto3
from datetime import datetime, timezone, timedelta

RETENTION_DAYS = 14
ec2 = boto3.client("ec2")

def create_snapshots():
    # Tag volumes with 'Backup=true'
    volumes = ec2.describe_volumes(
        Filters=[{"Name": "tag:Backup", "Values": ["true"]}]
    )["Volumes"]

    for vol in volumes:
        vol_id = vol["VolumeId"]
        desc = f"Automated backup of {vol_id} - {datetime.now(timezone.utc).isoformat()}"
        print(f"Creating snapshot for volume {vol_id}...")
        ec2.create_snapshot(
            VolumeId=vol_id,
            Description=desc,
            TagSpecifications=[{
                "ResourceType": "snapshot",
                "Tags": [{"Key": "CreatedBy", "Value": "AutomatedBackup"}, {"Key": "DeleteAfterDays", "Value": str(RETENTION_DAYS)}]
            }]
        )

def prune_old_snapshots():
    cutoff = datetime.now(timezone.utc) - timedelta(days=RETENTION_DAYS)
    snapshots = ec2.describe_snapshots(OwnerIds=["self"])["Snapshots"]

    for snap in snapshots:
        if snap.get("StartTime") < cutoff:
            tags = {t["Key"]: t["Value"] for t in snap.get("Tags", [])}
            if tags.get("CreatedBy") == "AutomatedBackup":
                print(f"Deleting expired snapshot: {snap['SnapshotId']}")
                ec2.delete_snapshot(SnapshotId=snap["SnapshotId"])

if __name__ == "__main__":
    create_snapshots()
    prune_old_snapshots()
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 01 - Disk Space Watchdog and Log Cleaner Template](./01-Disk-Space-Watchdog-and-Log-Cleaner-Template.md) | [Index](../../../README.md) | [03 - Kubernetes Pod Health Auto Remediator Template →](./03-Kubernetes-Pod-Health-Auto-Remediator-Template.md) |
