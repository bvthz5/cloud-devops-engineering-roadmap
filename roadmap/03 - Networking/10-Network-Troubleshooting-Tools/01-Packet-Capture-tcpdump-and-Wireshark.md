# 01 - Packet Capture: tcpdump and Wireshark

## 1. tcpdump Essentials

`tcpdump` is the premier command-line packet analyzer powered by `libpcap`.

### Golden Rule Flags
```bash
# -n: Do not resolve hostnames (prevents hanging DNS lookups during outages)
# -nn: Do not resolve hostnames OR port names (shows raw port numbers)
# -v / -vvv: Increase verbosity (shows TTL, IP ID, TCP options)
# -i <interface>: Listen on specific NIC (or 'any')
# -s0: Capture full packet snaplen without truncation
# -w <file.pcap>: Write raw binary packets to file for Wireshark inspection
sudo tcpdump -nnvvv -i eth0 -s0 -w /tmp/capture.pcap
```

---

## 2. Berkeley Packet Filters (BPF) Syntax

BPF allows in-kernel packet filtering before packets are copied to user space:

```bash
# Capture traffic on port 443 only
sudo tcpdump -nn -i eth0 port 443

# Capture traffic between host and specific remote IP
sudo tcpdump -nn -i eth0 host 10.0.1.25

# Capture only initial TCP SYN packets (Connection attempts)
sudo tcpdump -nn -i eth0 "tcp[tcpflags] & (tcp-syn) != 0 and tcp[tcpflags] & (tcp-ack) == 0"

# Capture TCP RST packets (Connection aborts/resets)
sudo tcpdump -nn -i eth0 "tcp[tcpflags] & (tcp-rst) != 0"
```

---

## 3. Wireshark and Tshark CLI Analysis

```bash
# Inspect top talking IP addresses in a pcap using tshark
tshark -r /tmp/capture.pcap -q -z conv,ip

# Filter HTTP requests returning status >= 400
tshark -r /tmp/capture.pcap -Y "http.response.status_code >= 400"
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [README](./README.md) | [README](./README.md) | [02 - Path Diagnostics](./02-Path-Diagnostics-traceroute-mtr-and-iproute2.md) |
