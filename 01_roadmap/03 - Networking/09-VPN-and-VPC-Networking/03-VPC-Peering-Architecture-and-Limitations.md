# 03 - VPC Peering Architecture and Limitations

## 1. How VPC Peering Works
VPC Peering connects two VPCs using AWS/cloud internal networking infrastructure. Traffic is routed over private fiber paths without going over the public internet.

---

## 2. Non-Transitive Routing Constraint

```
[ VPC A (10.1.0.0/16) ] ◄── Peering 1 ──► [ VPC B (10.2.0.0/16) ] ◄── Peering 2 ──► [ VPC C (10.3.0.0/16) ]

Traffic between VPC A and VPC B: ALLOWED
Traffic between VPC B and VPC C: ALLOWED
Traffic between VPC A and VPC C: BLOCKED! (VPC B cannot act as a transit router)
```

To connect VPC A and VPC C using peering, you **must create an explicit third peering connection** directly between VPC A and VPC C.

### Scalability Limit: $N(N-1)/2$ Full Mesh
Connecting 50 VPCs via peering requires $50 \times 49 / 2 = 1,225$ individual peering connections and thousands of route table entries. This is why enterprises adopt **Transit Gateways**.

---

## 3. Overlapping CIDRs Hazard
VPC Peering strictly forbids connecting VPCs that share identical or overlapping CIDR blocks (e.g. attempting to peer two VPCs that both use `10.0.0.0/16`). Solving this requires complex Private NAT gateways or network migration.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 02 - Cloud VPC Architecture and Subnet Topology](./02-Cloud-VPC-Architecture-and-Subnet-Topology.md) | [Index](../../../README.md) | [04 - Transit Gateways and Cloud Interconnects →](./04-Transit-Gateways-and-Cloud-Interconnects.md) |
