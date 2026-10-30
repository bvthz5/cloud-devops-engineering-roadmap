# 06 - Bandwidth and Latency Testing: iperf3 and ping

## 1. Network Performance Benchmarking with `iperf3`

`iperf3` generates synthetic TCP and UDP workloads to measure bandwidth, jitter, and packet loss between two endpoints.

### Step 1: Start Server on Target Machine
```bash
iperf3 -s -p 5201
```

### Step 2: Run TCP Throughput Benchmark from Client
```bash
# Run 10-second test across 4 parallel streams
iperf3 -c 10.0.1.50 -p 5201 -P 4
```

### Step 3: Run UDP Jitter and Loss Benchmark
```bash
# Transmit 100 Mbps UDP stream
iperf3 -c 10.0.1.50 -u -b 100M
```

---

## 2. Path MTU Discovery (PMTUD) with `ping`

Determine the exact maximum transmission unit (MTU) supported along a network path without fragmentation:

```bash
# On Linux: -M do sets the 'Don't Fragment' (DF) bit. -s specifies payload size.
# Total IP Packet = Payload (1472) + ICMP Header (8) + IP Header (20) = 1500 bytes
ping -c 2 -M do -s 1472 8.8.8.8

# If MTU is restricted (e.g. VPN or VXLAN tunnel):
ping -c 2 -M do -s 1472 10.100.0.1
# Output: ping: local error: message too long, mtu=1420
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 05 - HTTP and API Diagnostics curl and dig](./05-HTTP-and-API-Diagnostics-curl-and-dig.md) | [Index](../../../README.md) | [07 - Real World Scenarios →](./07-Real-World-Scenarios.md) |
