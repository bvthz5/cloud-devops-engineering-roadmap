# 10 - Hands-On Practice Lab: Querying Regions & Testing Edge Latency

> **Lab Objective:** Query regional infrastructure details and immutable AZ IDs via AWS CLI, and benchmark network latency to regional endpoints.

---

### 🧪 Task 1: Query All Enabled Regions & AZ IDs via AWS CLI
```bash
# 1. List all available AWS Regions
aws ec2 describe-regions \
  --all-regions \
  --query "Regions[].{RegionName:RegionName, OptIn:OptInStatus}" \
  --output table

# 2. List AZ Names and immutable AZ IDs in us-east-1
aws ec2 describe-availability-zones \
  --region us-east-1 \
  --query "AvailabilityZones[].{ZoneName:ZoneName, ZoneId:ZoneId, State:State}" \
  --output table
```

### 🧪 Task 2: Benchmark Network Latency to AWS Global Edges
```bash
# Compare latency: CloudFront Anycast vs direct regional origin
curl -w "DNS: %{time_namelookup}s | Connect: %{time_connect}s | TTFB: %{time_starttransfer}s | Total: %{time_total}s\n" \
  -o /dev/null -s https://aws.amazon.com
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 09 - Interview QA](./09-Interview-QA.md) | [Index](../../../README.md) | [11 - MCQ →](./11-MCQ.md) |
