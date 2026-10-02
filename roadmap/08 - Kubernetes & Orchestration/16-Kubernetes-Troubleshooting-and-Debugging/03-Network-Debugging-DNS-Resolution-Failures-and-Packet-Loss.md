# 03 - Network Debugging: DNS Resolution Failures and Packet Loss

## 1. Network Debugging Workflow

```text
[ Test 1: Can Pod ping localhost? ] ──► NO ──► Loopback interface broken in pod network namespace!
                  │ YES
                  ▼
[ Test 2: Can Pod resolve internal DNS? ] ──► NO ──► CoreDNS pods down or iptables rule dropping port 53!
                  │ YES
                  ▼
[ Test 3: Can Pod hit Service ClusterIP? ] ──► NO ──► kube-proxy down or Netfilter conntrack table full!
                  │ YES
                  ▼
[ Test 4: Can Pod hit Pods on other nodes? ] ──► NO ──► CNI overlay (VXLAN UDP 8472 / BGP 179) firewall block!
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [02 - Node NotReady Diagnostics](./02-Node-NotReady-Troubleshooting-and-Kubelet-Diagnostics.md) | [README](./README.md) | [04 - Control Plane Diagnostics](./04-Control-Plane-Diagnostics-API-Server-and-etcd-Failures.md) |
