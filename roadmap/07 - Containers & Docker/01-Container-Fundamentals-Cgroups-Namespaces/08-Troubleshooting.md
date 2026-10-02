# 08 - Troubleshooting & Diagnostic Runbooks

## 1. Injecting Debug Tools with `nsenter`

When a stripped minimal container (Distroless or Scratch) crashes or lacks basic debugging utilities (`curl`, `ip`, `ps`), use **`nsenter`** from the host to attach to the container's namespaces using the host's debugging tools:

```bash
# 1. Identify container's main PID on the host
CONTAINER_PID=$(docker inspect --format '{{ .State.Pid }}' my_broken_container)

# 2. Enter container's Network namespace using host's ip and tcpdump
sudo nsenter -t $CONTAINER_PID -n ip addr show
sudo nsenter -t $CONTAINER_PID -n tcpdump -i any -nn port 80

# 3. Enter container's Mount, PID, and Network namespaces with bash
sudo nsenter -t $CONTAINER_PID -m -p -n /bin/bash
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [07 - Real-World Scenarios](./07-Real-World-Scenarios.md) | [README](./README.md) | [09 - Interview Questions](./09-Interview-QA.md) |
