# 04 - Resource Constraints and Limits

## 1. Enforcing Production Hardware Boundaries

Unconstrained containers will consume all available host memory and CPU, leading to noisy-neighbor resource starvation.

```bash
docker run -d \
  --name worker \
  --cpus="1.5" \                 # Limit to 1.5 CPU cores
  --cpu-shares=1024 \           # Relative CPU weight during CPU contention
  --memory="1g" \               # Hard memory limit (OOMKilled if exceeded)
  --memory-reservation="512m" \ # Soft memory limit (triggers warning / reclaim)
  --memory-swap="1g" \          # Total memory + swap (setting equal to --memory disables swap)
  --pids-limit=100 \            # Prevent fork-bomb attacks
  my_worker_image
```

---

## 2. Memory Swappiness

To prevent performance degradation from disk swapping, disable swap completely for latency-sensitive applications:
```bash
# Setting --memory and --memory-swap to the same value completely disables swap!
docker run -m 512m --memory-swap 512m redis:alpine
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [03 - Essential Docker CLI Commands](./03-Essential-Docker-CLI-Commands.md) | [README](./README.md) | [05 - Daemon Configuration & Tuning](./05-Daemon-Configuration-and-Production-Tuning.md) |
