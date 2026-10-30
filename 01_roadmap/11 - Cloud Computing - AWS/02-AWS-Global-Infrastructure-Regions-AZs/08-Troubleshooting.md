# 08 - Troubleshooting Cross-Region Latency & Global Infrastructure Issues

> **Focus:** Diagnosing cross-region network degradation, Availability Zone outages, MTU packet drops across Direct Connect, and split-brain write conflicts.

---

## 🩺 Top 4 Global Infrastructure Issues & Resolutions

### 1. High Cross-Region Latency Spikes
- **Symptom:** API calls between `us-east-1` and `eu-west-1` jump from 75 ms to 250 ms.
- **Cause:** Traffic routing over public internet instead of AWS global backbone.
- **Fix:** Establish a **VPC Peering** or **AWS Transit Gateway Inter-Region Peering** connection so all traffic stays strictly on the private AWS fiber backbone.

### 2. Single Availability Zone Service Degradation
- **Symptom:** 33% of web requests timeout with HTTP 504.
- **Cause:** Underlying physical hardware issue in `us-east-1a`.
- **Fix:** Use AWS CLI to shift traffic away from the degraded AZ instantly:
  ```bash
  # Remove degraded AZ subnet from Application Load Balancer
  aws elbv2 set-subnets \
    --load-balancer-arn <alb-arn> \
    --subnets subnet-az-1b subnet-az-1c
  ```

### 3. MTU Mismatch Across AWS Direct Connect (Black Hole Drops)
- **Symptom:** Small ping packets succeed, but large file transfers hang indefinitely.
- **Cause:** Jumbo frames (MTU 9001) enabled on EC2, but on-premises router or Direct Connect link only supports standard Ethernet (MTU 1500).
- **Fix:** Standardize MTU to 1500 or verify end-to-end Jumbo Frame support:
  ```bash
  sudo ip link set dev eth0 mtu 1500
  ```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 07 - Real World Scenarios](./07-Real-World-Scenarios.md) | [Index](../../../README.md) | [09 - Interview QA →](./09-Interview-QA.md) |
