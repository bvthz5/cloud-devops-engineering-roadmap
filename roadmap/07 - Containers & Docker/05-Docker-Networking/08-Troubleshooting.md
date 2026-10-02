# 08 - Troubleshooting & Diagnostic Runbooks

## 1. Inspecting Container IP & Gateway

```bash
# View network details of a running container
docker inspect --format '{{json .NetworkSettings.Networks}}' my_container | jq .

# Trace container network traffic from host using tcpdump
# 1. Find container's veth interface index
VETH_INDEX=$(docker exec my_container cat /sys/class/net/eth0/iflink)
ip link | grep "^$VETH_INDEX:"
# 2. Sniff traffic on that veth interface
sudo tcpdump -i vethxxxxxx -nn
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 07 - Real World Scenarios](./07-Real-World-Scenarios.md) | [Index](../../../README.md) | [09 - Interview QA →](./09-Interview-QA.md) |
