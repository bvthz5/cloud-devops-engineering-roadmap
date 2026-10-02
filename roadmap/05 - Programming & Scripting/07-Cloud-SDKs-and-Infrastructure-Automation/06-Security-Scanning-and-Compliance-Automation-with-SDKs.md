# 06 - Security Scanning and Compliance Automation with SDKs

## 1. Automated Detection and Remediation of Open Security Groups

Exposing port 22 (SSH) or port 3389 (RDP) directly to `0.0.0.0/0` (the entire internet) violates virtually every compliance benchmark (CIS AWS, SOC2, PCI-DSS).

```python
import boto3
import logging

logger = logging.getLogger("SecOps-AutoRemediate")
logger.setLevel(logging.INFO)

DANGEROUS_PORTS = {22: "SSH", 3389: "RDP"}

def audit_and_quarantine_security_groups(region: str, auto_remediate: bool = False):
    ec2 = boto3.client('ec2', region_name=region)
    sgs = ec2.describe_security_groups()['SecurityGroups']

    for sg in sgs:
        sg_id = sg['GroupId']
        sg_name = sg['GroupName']
        
        for rule in sg.get('IpPermissions', []):
            from_port = rule.get('FromPort')
            to_port = rule.get('ToPort')
            
            # Check IPv4 ranges
            for ip_range in rule.get('IpRanges', []):
                cidr = ip_range.get('CidrIp')
                if cidr == '0.0.0.0/0':
                    for d_port, desc in DANGEROUS_PORTS.items():
                        if from_port is not None and to_port is not None and from_port <= d_port <= to_port:
                            logger.error(f"VIOLATION: Security Group {sg_name} ({sg_id}) exposes {desc} (Port {d_port}) to the internet!")
                            
                            if auto_remediate:
                                logger.warning(f"AUTO-REMEDIATION: Revoking dangerous ingress rule on {sg_id}...")
                                ec2.revoke_security_group_ingress(
                                    GroupId=sg_id,
                                    IpPermissions=[{
                                        'IpProtocol': rule.get('IpProtocol'),
                                        'FromPort': from_port,
                                        'ToPort': to_port,
                                        'IpRanges': [{'CidrIp': '0.0.0.0/0'}]
                                    }]
                                )
                                logger.info(f"Ingress revoked successfully for {sg_id}.")

if __name__ == "__main__":
    audit_and_quarantine_security_groups("us-east-1", auto_remediate=False)
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 05 - Automated Cloud Cost Optimization and Janitor Scripts](./05-Automated-Cloud-Cost-Optimization-and-Janitor-Scripts.md) | [Index](../../../README.md) | [07 - Real World Scenarios →](./07-Real-World-Scenarios.md) |
