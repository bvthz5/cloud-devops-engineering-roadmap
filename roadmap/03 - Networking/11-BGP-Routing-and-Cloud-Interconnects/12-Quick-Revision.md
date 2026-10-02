# 12 - BGP Routing: Quick Revision Cheat Sheet

## BGP Path Selection Mnemonic: **W-L-O-A-O-M-E-I**

1. **W**eight (Highest)
2. **L**ocal Preference (Highest)
3. **O**riginate Locally (Local over learned)
4. **A**S-Path (Shortest)
5. **O**rigin Code (IGP < EGP < Incomplete)
6. **M**ED (Lowest)
7. **E**xternal over Internal (eBGP over iBGP)
8. **I**GP cost to next hop (Lowest)

## Neighbor States Cheat Sheet
- `Idle`: No route to neighbor IP.
- `Active`: TCP handshake on port 179 failing (Check firewall!).
- `Established`: Fully operational.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 11 - MCQ](./11-MCQ.md) | [Index](../../../README.md) | [Next Module (12-Overlay-Networks-VXLAN-and-Container-CNI) →](../12-Overlay-Networks-VXLAN-and-Container-CNI/01-Overlay-Networking-Principles-and-Encapsulation.md) |
