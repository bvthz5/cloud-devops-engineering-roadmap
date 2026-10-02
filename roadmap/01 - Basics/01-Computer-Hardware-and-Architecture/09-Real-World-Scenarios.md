# 09 — Real-World DevOps & Cloud Infrastructure Scenarios

Hardware and architecture understanding is not just theoretical computer science—it directly determines infrastructure costs, application latency, reliability, and security in production cloud systems. Below are real-world enterprise scenarios where hardware knowledge is critical for DevOps/SRE engineers.

---

## Scenario 1: Graviton (ARM64) Migration for AWS Cost Optimization

### Problem Statement
An e-commerce company runs 250 microservices on AWS using `c5.2xlarge` (x86_64 Intel Xeon) instances in an EKS cluster. Compute costs account for 65% of the monthly cloud invoice ($42,000/month). Executive management orders an immediate 20% infrastructure cost reduction without reducing CPU or memory allocations.

### Architecture Analysis
- AWS offers Graviton3 (`c7g.2xlarge`) instances based on ARM64 Neoverse cores.
- `c7g.2xlarge` offers up to 25% better price-performance compared to `c5.2xlarge` and costs 20% less per instance-hour.
- **Hardware Barrier:** Docker images compiled for `x86_64` (AMD64) cannot run natively on ARM64 silicon due to Instruction Set Architecture (ISA) incompatibility. Running x86 binaries on ARM via emulation (QEMU/Rosetta) incurs a 40–300% CPU overhead, destroying throughput.

```text
+-------------------------------------------------------------+
|                ISA Incompatibility Barrier                  |
|                                                             |
|  x86_64 Docker Image   ==[ Emulation / QEMU ]==> Slow (3x)  |
|       (CISC)                      ARM64 Graviton3           |
|                                        (RISC)               |
|                                                             |
|  Multi-Arch Image Manifest:                                 |
|  ├─ arm64 slice  ──────────────────────────────> Native 100%|
|  └─ amd64 slice  ──────────────────────────────> Native 100%|
+-------------------------------------------------------------+
```

### Engineering Solution
1. **CI/CD Pipeline Upgrade (Multi-Architecture Builds):**
   Update GitHub Actions / GitLab CI with Docker `buildx` using native ARM64 runners or cross-compilation:
   ```yaml
   - name: Set up Docker Buildx
     uses: docker/setup-buildx-action@v3

   - name: Build and push Multi-Arch Image
     uses: docker/build-push-action@v5
     with:
       platforms: linux/amd64,linux/arm64
       tags: ghcr.io/org/payments-api:${{ github.sha }}
       push: true
   ```
2. **Kubernetes Multi-Arch Node Pools:**
   Deploy mixed node groups using AWS Karpenter or EKS Managed Node Groups with `kubernetes.io/arch: arm64` tolerations:
   ```yaml
   nodeSelector:
     kubernetes.io/arch: arm64
   ```
3. **Outcome:**
   - Compute bill dropped by 21.8% ($9,150/month savings).
   - Graviton3 DDR5 memory bandwidth reduced API P99 latency from 142ms to 108ms.

---

## Scenario 2: NUMA Node Misconfiguration Causing Database Latency Spikes

### Problem Statement
A high-throughput PostgreSQL primary database deployed on a bare-metal dual-socket server (2x AMD EPYC 7763, 128 cores total, 512 GB RAM) experiences periodic 350ms query latency spikes during peak transaction volume. CPU utilization is only 45%, and disk I/O wait is under 2%.

### Architecture Analysis
Modern multi-socket servers use Non-Uniform Memory Access (NUMA).
- Socket 0 has direct physical pins to Bank A of RAM (256 GB).
- Socket 1 has direct physical pins to Bank B of RAM (256 GB).
- Inter-socket communication traverses the AMD Infinity Fabric or Intel UPI bus.

```text
[ Socket 0: 64 Cores ] <====== 20-35ns ======> [ Local RAM Bank A: 256 GB ]
          ||
  Infinity Fabric (UPI Bus) -> Extra 60-100ns Penalty + Contention
          ||
[ Socket 1: 64 Cores ] <====== 20-35ns ======> [ Local RAM Bank B: 256 GB ]
```

When Linux process scheduler moved a PostgreSQL worker thread from Core 10 (Socket 0) to Core 74 (Socket 1), but the thread's memory remained allocated in Bank A, every memory access became a remote NUMA hop, adding 60–100ns latency penalty and saturating the inter-socket interconnect.

### Diagnostic Commands
```bash
# Check NUMA topology
numactl --hardware

# Monitor remote memory access ratio
numastat -c postgres
```

### Engineering Solution
1. **Configure NUMA Node Interleaving or Binding:**
   Bind each PostgreSQL instance to a specific NUMA socket and its local memory pool:
   ```bash
   numactl --cpunodebind=0 --membind=0 /usr/lib/postgresql/16/bin/postgres -D /data/pg
   ```
2. **Tune Linux Kernel Memory Policy:**
   ```bash
   # Set zone reclaim mode to avoid remote allocation churn
   sysctl -w vm.zone_reclaim_mode=0
   ```
3. **Outcome:**
   Query P99 latency dropped from 350ms to 12ms flat with zero jitter.

---

## Scenario 3: Cloud "CPU Steal Time" Degrading Production Microservices

### Problem Statement
A payments processing API deployed on AWS EC2 `t3.xlarge` instances experiences random connection timeouts (HTTP 504) during marketing flash sales. CloudWatch alarms show CPU utilization at 100%, but internal application metrics indicate low throughput.

### Architecture Analysis
- `t3.xlarge` instances are **burstable performance instances**.
- They use shared physical CPU cores on the hypervisor host.
- When the instance exhausts its accumulated CPU Credits, the hypervisor throttles the guest vCPU.
- In Linux, this hypervisor-imposed CPU deprivation is reported as **CPU Steal Time (`%st`)**.

```text
+-------------------------------------------------------------+
|                  Type 1 Hypervisor (Nitro/KVM)              |
|                                                             |
| Physical Core 0  ──> VM-A (Allocated 100% compute)          |
|                  ──> VM-B (Throttled: Waiting for time slice|
|                            ==> %st reported in VM-B)        |
+-------------------------------------------------------------+
```

### Verification
Run `top` or `mpstat -P ALL 1`:
```text
%Cpu(s): 12.0 us,  4.0 sy,  0.0 ni,  0.0 id,  0.0 wa,  0.0 hi,  0.0 si, 84.0 st
```
Here, **84.0% st** indicates the hypervisor is withholding CPU cycles from the VM because credit balance is zero.

### Engineering Solution
1. **Immediate Mitigation:** Enable `T2/T3 Unlimited` mode via AWS CLI:
   ```bash
   aws ec2 modify-instance-credit-specification \
     --instance-id i-0abcdef1234567890 \
     --cpu-credits-specification CpuCredits=unlimited
   ```
2. **Permanent Architecture Fix:**
   Migrate latency-sensitive production workloads from burstable (`t3`) instances to dedicated compute instances (`c6i.xlarge` or `c7g.xlarge`) with non-shared, non-throttled vCPUs.
3. **Monitoring & Alerting:**
   Implement Prometheus alert on `node_cpu_seconds_total{mode="steal"}` > 5% sustained for 3 minutes.

---

## Scenario 4: Storage IOPS & Queue Depth Bottleneck on NVMe / EBS

### Problem Statement
A high-write Elasticsearch cluster deployed on AWS `gp3` EBS volumes experiences indexing queue drops and `es_rejected_execution_exception` errors. Disk capacity is at 40%, but write latency jumps to 85ms.

### Architecture Analysis
EBS `gp3` volumes default to 3,000 baseline IOPS and 125 MB/s throughput regardless of storage size.
- If the application generates 8,000 write operations per second, operations exceed the IOPS limit.
- The Linux kernel I/O scheduler queues pending I/O requests.
- As the queue depth swells, average wait time (`await`) escalates exponentially.

```text
[ Elasticsearch ] ──(8000 IOPS)──> [ Linux Block Layer Queue ] ──(Throttled: 3000 IOPS)──> [ EBS gp3 ]
                                     Depth: 128 requests
                                     Latency: 85ms
```

### Diagnostics with `iostat`
```bash
iostat -xz 1
```
Output:
```text
Device    r/s     w/s     rkB/s     wkB/s  aqu-sz  await  %util
nvme1n1  0.00 3000.00      0.00 125000.00   45.20  82.10 100.00
```
- `w/s` = 3000 (hitting hard gp3 baseline limit)
- `aqu-sz` (average queue size) = 45.2
- `%util` = 100%

### Engineering Solution
1. **Modify EBS gp3 Configuration Dynamically (No Downtime):**
   ```bash
   aws ec2 modify-volume \
     --volume-id vol-0123456789abcdef0 \
     --iops 12000 \
     --throughput 500
   ```
2. **Alternative for Extreme IOPS:**
   Migrate to AWS `i3en` or `i4i` instances featuring local NVMe SSDs directly connected via PCIe bus (delivering up to 2,500,000 IOPS with sub-millisecond latency). Use local NVMe for Elasticsearch data and snapshot indices to S3.

---

## Scenario Summary Matrix for SREs

| Hardware Constraint | Detection Command | Cloud Impact | Remediation Strategy |
| :--- | :--- | :--- | :--- |
| **ISA Incompatibility** | `uname -m`, `file /path/bin` | Container crash (`exec format error`) | Build multi-arch manifests (`docker buildx`) |
| **NUMA Locality** | `numastat`, `numactl --hardware` | P99 latency spikes (60-100ns memory hops) | Bind DB process to single socket via `numactl` |
| **CPU Steal (%st)** | `mpstat 1`, `top` | Latency drops, timeout errors | Switch from burstable (`t3`) to dedicated (`c6i/c7g`) |
| **EBS IOPS Ceiling** | `iostat -xz 1` (`%util=100`) | Bulk indexing rejection, DB locks | Provision higher gp3 IOPS or switch to NVMe |
| **TLB Miss Thrashing** | `perf stat -e dTLB-load-misses` | CPU cycles wasted in page table walks | Enable Transparent Huge Pages (THP) / HugeTLB |
