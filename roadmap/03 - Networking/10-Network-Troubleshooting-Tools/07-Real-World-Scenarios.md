# 07 - Troubleshooting Tools: Real-World Production Scenarios

## Scenario 1: The "Hanging Database Dump" (MTU Blackhole)

### Incident Summary
Nightly backups from a remote database server in AWS to an on-premises disaster recovery server consistently stalled at exactly 12% progress. Small interactive queries and ping worked flawlessly, but bulk `pg_dump` transfers hung indefinitely.

### Diagnostic Steps
1. Executed `ping -M do -s 1472 <dest>`: Discovered packets larger than 1400 bytes were dropped silently by an intermediate Cisco ASA firewall blocking ICMP type 3 code 4 (`Fragmentation Needed`).
2. SRE captured packets during `pg_dump` with `tcpdump`:
   ```bash
   sudo tcpdump -nn -i eth0 host <on_prem_ip>
   ```
   Saw TCP Retransmissions of full 1500-byte packets repeating every 3 seconds until timeout.

### Resolution
Enabled TCP MSS clamping on the corporate VPN router:
```bash
iptables -t mangle -A FORWARD -p tcp --tcp-flags SYN,RST SYN -j TCPMSS --set-mss 1360
```
Subsequent backups succeeded at maximum wire speed.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 06 - Bandwidth and Latency Testing iperf3 and ping](./06-Bandwidth-and-Latency-Testing-iperf3-and-ping.md) | [Index](../../../README.md) | [08 - Troubleshooting →](./08-Troubleshooting.md) |
