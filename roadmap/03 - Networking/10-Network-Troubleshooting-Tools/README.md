# Module 10: Network Troubleshooting Tools and CLI Diagnostics

Welcome to **Module 10: Network Troubleshooting Tools and CLI Diagnostics**. When mission-critical production outages occur, SREs and Cloud DevOps engineers rely on precise diagnostic tools to isolate failures across the network stack.

---

## 🎯 Learning Objectives

By the end of this module, you will be able to:
1. Capture, filter, and inspect raw network packets using **`tcpdump`** and **Wireshark/Tshark**.
2. Isolate routing drops, packet loss, and high latency along internet paths using **`mtr`** and **`traceroute`**.
3. Inspect kernel socket queues, connection states, and port listeners using modern **`ss`** (replacing legacy `netstat`).
4. Perform port scanning, banner grabbing, and TCP/UDP socket piping using **`netcat` (nc)**, **`socat`**, and **`nmap`**.
5. Benchmark microservice API latencies and inspect TLS handshakes using **`curl`** telemetry flags and **`dig`**.
6. Measure true network throughput, jitter, and MTU bottlenecks using **`iperf3`** and **`ping`**.

---

## 📚 Module Index

| # | Topic | Description |
|---|---|---|
| 01 | [Packet Capture: tcpdump & Wireshark](./01-Packet-Capture-tcpdump-and-Wireshark.md) | BPF filters, packet capture flags, pcap dissection, tshark |
| 02 | [Path Diagnostics: traceroute, mtr & iproute2](./02-Path-Diagnostics-traceroute-mtr-and-iproute2.md) | ICMP vs UDP vs TCP probes, interpreting MTR loss, `ip route get` |
| 03 | [Socket Inspection: ss & netstat](./03-Socket-and-Connection-Inspection-ss-and-netstat.md) | `ss` flags, Send-Q / Recv-Q buffer diagnostics, TCP state filtering |
| 04 | [Port Testing: nc, socat & nmap](./04-Port-Scanning-and-Testing-nc-socat-and-nmap.md) | Quick connectivity checks, port scanning, traffic redirection proxies |
| 05 | [HTTP & DNS Diagnostics: curl & dig](./05-HTTP-and-API-Diagnostics-curl-and-dig.md) | `curl -w` microsecond timing breakdowns, SNI overrides, DNS trace |
| 06 | [Bandwidth & MTU Testing: iperf3 & ping](./06-Bandwidth-and-Latency-Testing-iperf3-and-ping.md) | Throughput benchmarking, UDP jitter, Path MTU Discovery (PMTUD) |
| 07 | [Real-World Production Scenarios](./07-Real-World-Scenarios.md) | Intermittent Kubernetes packet drops, MTU blackhole during DB dump |
| 08 | [Troubleshooting Guide](./08-Troubleshooting.md) | SRE 7-step rapid network triage runbook |
| 09 | [Interview Questions & Answers](./09-Interview-QA.md) | 10 high-frequency production SRE/DevOps interview scenarios |
| 10 | [Hands-On Lab Practice](./10-Hands-On-Practice.md) | Live packet dissection of a broken connection using tcpdump and netcat |
| 11 | [Self-Assessment MCQs](./11-MCQ.md) | 10 scenario-based multiple-choice questions with deep explanations |
| 12 | [Quick Revision Cheat Sheet](./12-Quick-Revision.md) | Diagnostic one-liners, CLI flags reference, and triage command matrix |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [09 - VPN & VPC](../09-VPN-and-VPC-Networking/README.md) | [Networking Index](../README.md) | [01 - Packet Capture](./01-Packet-Capture-tcpdump-and-Wireshark.md) |
