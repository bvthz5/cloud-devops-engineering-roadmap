# 10 - Hands-On Practice Labs

## Lab 1: Generating and Running an OCI Bundle with `runc`

### Objective
Manually unpack an image and execute a container using raw `runc` without any daemon running.

### Implementation
```bash
# 1. Export rootfs from alpine image
mkdir -p /tmp/runc-demo/rootfs
cd /tmp/runc-demo
docker export $(docker create alpine) | tar -C rootfs -xvf -

# 2. Generate default OCI runtime configuration (config.json)
runc spec

# 3. Launch container with runc
sudo runc run my-runc-container
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 09 - Interview QA](./09-Interview-QA.md) | [Index](../../../README.md) | [11 - MCQ →](./11-MCQ.md) |
