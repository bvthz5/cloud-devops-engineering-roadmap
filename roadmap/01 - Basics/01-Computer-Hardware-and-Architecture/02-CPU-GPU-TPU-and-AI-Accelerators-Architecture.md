# 02 — CPU, GPU, TPU, NPU & AI Accelerators Architecture

---

## 1. Executive Summary & Why Accelerators Matter in DevOps

In traditional IT, servers had one type of processor: the **CPU (Central Processing Unit)**. It ran the operating system, handled user requests, executed application logic, and managed disk I/O.

In modern cloud-native, AI, and distributed systems, computing demands have exploded. Workloads like Large Language Models (LLMs), Computer Vision, Real-Time Video Transcoding, and Line-Rate 100Gbps Packet Filtering require trillions of mathematical operations per second. Running these tasks purely on CPUs is prohibitively slow and expensive.

This led to the **Hardware Accelerator Revolution**: pairing general-purpose CPUs with specialized silicon designed for specific computational patterns:
- **CPU (Central Processing Unit):** Low-latency, sequential logic, general-purpose master.
- **GPU (Graphics Processing Unit):** High-throughput, massively parallel SIMD matrix & vector math.
- **TPU / NPU (Tensor / Neural Processing Unit):** Hardwired 2D systolic arrays optimized purely for tensor matrix multiplications ($A \times B + C$).
- **DPU (Data Processing Unit):** Infrastructure offload engine (SmartNIC) handling software-defined networking (SDN), NVMe-over-Fabrics storage, and TLS encryption without touching host CPU cores.

---

## 2. Visual Architectural Comparison

```text
┌─────────────────────────────────────────────────────────────────────────────────────────────────┐
│                                PROCESSOR ARCHITECTURE COMPARISON                                │
├───────────────────────────────┬─────────────────────────────────┬───────────────────────────────┤
│ CPU (Latency-Optimized)       │ GPU (Throughput-Optimized)      │ TPU / NPU (Matrix-Optimized)  │
├───────────────────────────────┼─────────────────────────────────┼───────────────────────────────┤
│ • 4 to 128 Powerful Cores     │ • 2,000 to 18,000 Tiny Cores    │ • Dedicated 2D Systolic Array │
│ • Massive L2/L3 Caches (MBs)  │ • Small Caches per SM           │ • Processing Elements (PEs)   │
│ • Complex Branch Prediction   │ • High-Bandwidth VRAM (HBM3)    │ • Zero-overhead Tensor MACs   │
│ • Out-of-Order Execution      │ • SIMT / SIMD Execution Model   │ • Bfloat16 / INT8 Precision   │
│ • Sequential Branchy Logic    │ • Massively Parallel Throughput │ • AI Training & Inference     │
└───────────────────────────────┴─────────────────────────────────┴───────────────────────────────┘
```

### Die Layout Diagram: CPU vs. GPU vs. TPU

```text
            CPU CORE LAYOUT                           GPU STREAMING MULTIPROCESSOR (SM)
┌─────────────────────────────────────────┐       ┌───────────────────────────────────────────────┐
│ ┌───────────────┐     ┌───────────────┐ │       │ ┌────┐┌────┐┌────┐┌────┐┌────┐┌────┐┌────┐┌────┐│
│ │ Core 0        │     │ Core 1        │ │       │ │ALU ││ALU ││ALU ││ALU ││ALU ││ALU ││ALU ││ALU ││
│ │ ┌─────┐┌────┐ │     │ ┌─────┐┌────┐ │ │       │ ├────┼────┼────┼────┼────┼────┼────┼────┤│
│ │ │ CU  ││ALU │ │     │ │ CU  ││ALU │ │ │       │ │ALU ││ALU ││ALU ││ALU ││ALU ││ALU ││ALU ││ALU ││
│ │ └─────┘└────┘ │     │ └─────┘└────┘ │ │       │ ├────┼────┼────┼────┼────┼────┼────┼────┤│
│ │ Regs & L1/L2  │     │ Regs & L1/L2  │ │       │ │ Tensor Cores (Matrix Multiply FP16/INT8)    ││
│ └───────────────┘     └───────────────┘ │       │ ├─────────────────────────────────────────────┤│
│ ┌─────────────────────────────────────┐ │       │ │ Warp Scheduler & Register File (Huge)       ││
│ │ Huge Shared L3 Cache (LLC, 32-256MB)│ │       │ └─────────────────────────────────────────────┘│
│ └─────────────────────────────────────┘ │       │ Hundreds of SMs per GPU Package (VRAM 24-141GB)│
│ Control Logic & Branch Predictor: 60%   │       └───────────────────────────────────────────────┘
│ Computation ALUs: 20% | Cache: 20%      │       Control Logic: 5% | ALUs & Math Engines: 85%
└─────────────────────────────────────────┘

                     TPU / NPU SYSTOLIC ARRAY ARCHITECTURE
┌─────────────────────────────────────────────────────────────────────────────────────────────────┐
│ Input Weights (Stationary in PEs)                                                               │
│       │                                                                                         │
│       ▼                                                                                         │
│ Feature Data Stream ──► [ PE (0,0) ] ──► [ PE (0,1) ] ──► [ PE (0,2) ] ──► Matrix Output        │
│                                │                 │                 │                            │
│                                ▼                 ▼                 ▼                            │
│                         [ PE (1,0) ] ──► [ PE (1,1) ] ──► [ PE (1,2) ] ──► Matrix Output        │
│                                │                 │                 │                            │
│                                ▼                 ▼                 ▼                            │
│                         [ PE (2,0) ] ──► [ PE (2,1) ] ──► [ PE (2,2) ] ──► Matrix Output        │
│                                                                                                 │
│ Values flow like a heartbeat (systole) from cell to cell without writing back to registers/RAM! │
└─────────────────────────────────────────────────────────────────────────────────────────────────┘
```

---

## 3. Real-World Kitchen & Factory Analogies (Kids-Mind Style)

| Component | Real-World Kitchen / Industrial Analogy | How it Becomes Crystal Clear |
| :--- | :--- | :--- |
| **CPU** | **The Executive Master Chef** | Can prepare any complex dish from memory, taste-test, improvise recipes, manage the kitchen staff, and handle complex orders one at a time with near-zero delay. |
| **GPU** | **1,000 Kitchen Apprentices with Chopping Knives** | None of them can run the kitchen alone. But if you give them 10,000 onions and tell them: *"Everyone chop one onion simultaneously,"* they finish the entire pile in 2 seconds. |
| **TPU / NPU** | **An Automated Dumpling Factory Machine** | Can only do one thing: stamp dough and crimp dumplings. It cannot make soup or bake bread. But it churns out 500,000 perfect dumplings an hour using 90% less electricity. |
| **DPU** | **The Security Guard & Package Delivery Clerk** | Handles door security, signs for deliveries, and inspects packages outside so the Master Chef never wastes a second walking to the kitchen door. |

---

## 4. Deep-Dive: How Each Processor Works Internally

### 4.1 CPU (Central Processing Unit)
- **Design Philosophy:** Low-latency sequential instruction execution.
- **Pipeline:** 5-stage classic pipeline (Fetch ➔ Decode ➔ Execute ➔ Memory Access ➔ Writeback).
- **Core Strengths:**
  - Aggressive branch prediction (guessing `if/else` outcomes with 95%+ accuracy before code executes).
  - Out-of-order execution (reordering instructions to prevent CPU stalls).
  - High single-thread clock speeds (3.0 GHz to 5.5 GHz).
- **DevOps Context:** Runs the Linux OS kernel, Nginx web servers, Node.js/Python APIs, Git workflows, database queries, and system daemons.

### 4.2 GPU (Graphics Processing Unit)
- **Design Philosophy:** High-throughput data-parallel execution using the **SIMT (Single Instruction, Multiple Threads)** model.
- **How it Works:** 
  - Instructions are executed in groups called **Warps** (typically 32 threads in NVIDIA architectures).
  - All 32 threads in a warp execute the exact same instruction simultaneously on 32 different data elements.
  - When threads branch (`if (x > 0) else`), warp divergence occurs and speed drops.
- **Memory Architecture:** High-Bandwidth Memory (HBM3, HBM3e) offering memory bandwidths exceeding **3.0 TB/s** (compared to ~300 GB/s on DDR5 CPU buses).
- **DevOps Context:**
  - AI model fine-tuning and inference (Llama-3, Mistral, Whisper).
  - Vector search indexing.
  - Managed via NVIDIA Container Toolkit (`nvidia-docker` / `nvidia-ctk`), Kubernetes GPU Operator, and Triton Inference Server.

### 4.3 TPU / NPU (Tensor / Neural Processing Unit)
- **Design Philosophy:** Application-Specific Integrated Circuit (ASIC) designed exclusively for deep learning tensor operations (matrix multiplication and addition).
- **The Systolic Array Innovation:**
  - In a standard CPU/GPU, every addition or multiplication requires reading operands from registers/cache, calculating in the ALU, and writing back to registers. This consumes huge electrical energy and memory bus bandwidth.
  - In a **Systolic Array**, data flows through a grid of Processing Elements (PEs) rhythmically like blood pumped by a heart. 
  - Each PE multiplies an input by its stored weight and passes the accumulated sum directly to the adjacent PE on the next clock tick.
  - Result: Thousands of Multiply-Accumulate (MAC) operations happen per cycle without reading or writing to intermediate memory.
- **DevOps Context:**
  - Google Cloud TPU pods (v4, v5e, v5p).
  - High cost-efficiency for large-scale training and low-latency inference on Google Cloud.
  - Configured using Google Cloud TPU APIs, XLA (Accelerated Linear Algebra) compilers, and Vertex AI.

### 4.4 DPU (Data Processing Unit / SmartNIC)
- **Design Philosophy:** System-on-Chip (SoC) combining high-performance network interfaces with embedded ARM cores and hardware crypto/storage engines.
- **What it Offloads:**
  1. **Software-Defined Networking:** Open vSwitch (OVS), Geneve/VXLAN encapsulation, and firewall packet inspection at wire speed (100–400 Gbps).
  2. **Storage Virtualization:** NVMe-over-Fabrics (NVMe-oF) so remote cloud block storage looks like local bare-metal drives.
  3. **Security:** Zero-trust isolation, wire-rate TLS/IPsec encryption, and monitoring telemetry.
- **DevOps Context:**
  - AWS Nitro System (the secret behind AWS EC2 multi-tenant performance and security isolation).
  - NVIDIA BlueField DPUs in private enterprise Kubernetes clouds running Cilium/Calico eBPF networking.

---

## 5. Comprehensive Comparison Matrix

| Specification | CPU (Central Processor) | GPU (Graphics Processor) | TPU / NPU (Tensor Processor) | DPU (Data Processor) |
| :--- | :--- | :--- | :--- | :--- |
| **Core Count** | 4 – 128 big cores | 2,000 – 18,000 tiny cores | 2D Matrix Array (128x128 / 256x256 PEs) | 8 – 32 ARM cores + ASIC accelerators |
| **Execution Paradigm** | MIMD (Multiple Instruction, Multiple Data) | SIMT (Single Instruction, Multiple Threads) | Systolic Array Dataflow | Specialized packet/crypto pipelines |
| **Latency vs Throughput** | Ultra-low latency | Ultra-high throughput | High throughput matrix math | Line-rate networking & storage |
| **Memory Bandwidth** | 50 – 300 GB/s (DDR4 / DDR5) | 1,000 – 3,350 GB/s (GDDR6 / HBM3e) | 800 – 4,800 GB/s (HBM2 / HBM3) | 50 – 100 GB/s + 400 Gbps PCIe/Ethernet |
| **Power Consumption (TDP)**| 65W – 400W | 250W – 700W (e.g. H100) | 200W – 450W | 50W – 150W |
| **Programming Model** | C, C++, Go, Rust, Python, Java | CUDA, OpenCL, ROCm, Triton | XLA, PyTorch, TensorFlow | DPDK, SPDK, DOCA, eBPF |
| **Dominant Vendors** | Intel, AMD, ARM, Apple | NVIDIA, AMD (ROCm), Intel | Google (TPU), AWS (Trainium/Inferentia), Apple (Neural Engine) | NVIDIA (BlueField), AWS (Nitro), Intel |

---

## 6. Cloud Offerings & Sizing Guide for DevOps Engineers

### 6.1 AWS Cloud Compute Instances
- **General & Compute CPU:** `c6i.large` (Intel Xeon), `c7g.large` (AWS Graviton3 ARM), `m6i.xlarge`.
- **GPU Instances:**
  - `g5.xlarge` (NVIDIA A10G 24GB VRAM) — Ideal for single-model inference, video rendering, vLLM.
  - `p4d.24xlarge` (8x NVIDIA A100 40GB) — Multi-node deep learning training.
  - `p5.48xlarge` (8x NVIDIA H100 80GB SXM5) — Hyperscale LLM foundation training.
- **Custom AI Accelerators:**
  - `inf2.xlarge` (AWS Inferentia2) — 50% better price-performance for LLM inference than GPUs.
  - `trn1.32xlarge` (AWS Trainium) — High-throughput distributed model training.

### 6.2 Google Cloud Platform (GCP)
- **CPU Instances:** `c2-standard-4`, `c3-highcpu-8`, `t2a-standard` (ARM).
- **GPU Instances:** `a2-highgpu-1g` (NVIDIA A100), `a3-highgpu-8g` (NVIDIA H100).
- **Cloud TPUs:**
  - **TPU v4:** 275 TFLOPS per chip, 32 GB HBM2.
  - **TPU v5e:** Optimized for cost-effective inference and training ($1.20/hour).
  - **TPU v5p:** 4x compute power for training trillion-parameter models.

### 6.3 Microsoft Azure
- **CPU Instances:** `Standard_D4s_v5`, `Standard_F8s_v2`.
- **GPU Instances:** `Standard_NC6s_v3` (V100), `Standard_ND96amsr_A100_v4` (A100), `Standard_NDv5` (H100).

---

## 7. How DevOps Engineers Manage GPUs & Accelerators in Production

### 7.1 Linux Host Layer: Drivers & Toolkits
To use a GPU on a Linux server, three software layers must be configured:

```text
┌────────────────────────────────────────────────────────┐
│ 4. Container Application (PyTorch, TensorFlow, vLLM)   │
├────────────────────────────────────────────────────────┤
│ 3. NVIDIA Container Toolkit (nvidia-ctk hook)          │
├────────────────────────────────────────────────────────┤
│ 2. CUDA User-Space Libraries (libcuda.so, cudart)      │
├────────────────────────────────────────────────────────┤
│ 1. NVIDIA Kernel Module Driver (nvidia.ko)             │
├────────────────────────────────────────────────────────┤
│ Physical Hardware: NVIDIA PCIe / SXM GPU               │
└────────────────────────────────────────────────────────┘
```

1. **Verify GPU hardware presence via PCIe:**
   ```bash
   lspci | grep -i -E "vga|3d|nvidia"
   ```
2. **Inspect GPU health, temperature, and VRAM with `nvidia-smi`:**
   ```bash
   nvidia-smi
   ```
   *Sample Output:*
   ```text
   +-----------------------------------------------------------------------------------------+
   | NVIDIA-SMI 535.129.03             Driver Version: 535.129.03     CUDA Version: 12.2     |
   |-----------------------------------------+------------------------+----------------------+
   | GPU  Name                 Persistence-M | Bus-Id          Disp.A | Volatile Uncorr. ECC |
   | Fan  Temp   Perf          Pwr:Usage/Cap |           Memory-Usage | GPU-Util  Compute M. |
   |=========================================+========================+======================|
   |   0  NVIDIA A10G                    On  |   00000000:00:1E.0 Off |                    0 |
   | N/A   38C    P0              58W / 300W |    1420MiB / 23028MiB  |      8%      Default |
   +-----------------------------------------+------------------------+----------------------+
   ```

### 7.2 Kubernetes GPU Scheduling
In Kubernetes, GPUs are managed using the **NVIDIA GPU Operator** and exposed as extended resources:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: llm-inference-service
  namespace: ai-workloads
spec:
  replicas: 1
  selector:
    matchLabels:
      app: vllm
  template:
    metadata:
      labels:
        app: vllm
    spec:
      containers:
      - name: vllm-server
        image: vllm/vllm-openai:latest
        args: ["--model", "mistralai/Mistral-7B-Instruct-v0.2"]
        resources:
          limits:
            nvidia.com/gpu: 1        # Request 1 dedicated GPU
            memory: "16Gi"
            cpu: "4"
          requests:
            nvidia.com/gpu: 1
            memory: "16Gi"
            cpu: "4"
        ports:
        - containerPort: 8000
```

### 7.3 Multi-Instance GPU (MIG) Slicing
In production, allocating an entire $30,000 H100 80GB GPU to a tiny microservice wastes massive amounts of money. DevOps engineers configure **MIG (Multi-Instance GPU)** to slice 1 physical GPU into up to **7 isolated hardware partitions**:
- Each slice gets dedicated hardware compute cores, memory controllers, and isolated memory.
- Slices do not experience "noisy neighbor" interference.
- Exposed in Kubernetes as `nvidia.com/mig-1g.10gb: 1`.

---

## 8. Hands-On Troubleshooting Runbook for AI & GPU Infrastructure

### Problem 1: `CUDA out of memory (OOM)`
- **Symptom:** AI application crashes immediately during model load with: `RuntimeError: CUDA out of memory. Tried to allocate 4.20 GiB`.
- **Cause:** Model weights plus KV-cache exceed physical GPU VRAM.
- **Triage & Fix:**
  1. Check VRAM usage with `nvidia-smi`.
  2. Enable 4-bit or 8-bit quantization (`AWQ`, `bitsandbytes`, `GPTQ`) to cut model VRAM by 50%–75%.
  3. Reduce batch size or limit context length (`max_model_len` in vLLM).
  4. Span model across multiple GPUs using Tensor Parallelism (`--tensor-parallel-size 2`).

### Problem 2: `CUDA driver version is insufficient for CUDA runtime version`
- **Symptom:** Container crashes with driver mismatch error.
- **Cause:** The host kernel driver is older than the CUDA toolkit compiled inside the Docker image.
- **Fix:** Update host NVIDIA driver (`sudo apt install nvidia-driver-535`) or use forward-compatibility packages. The host driver must be $\ge$ the CUDA runtime version required by the container.

### Problem 3: Pod Stuck in `Pending` with `0/5 nodes available: Insufficient nvidia.com/gpu`
- **Symptom:** Kubernetes cannot schedule the GPU pod.
- **Cause:** No worker node has an available unallocated GPU, or NVIDIA Device Plugin DaemonSet is not healthy.
- **Triage:**
  ```bash
  kubectl describe pod <pod-name>
  kubectl get daemonset -n gpu-operator
  ```

---

## 9. Quick Revision Quiz

1. **Which processor is best suited for executing the Linux kernel, web requests, and sequential branchy code?**
   - A) TPU
   - B) GPU
   - C) CPU *(Correct)*
   - D) DPU

2. **Why are GPUs faster than CPUs for rendering and training neural networks?**
   - A) They have higher clock speed (GHz)
   - B) They have thousands of smaller cores that perform SIMD math in parallel *(Correct)*
   - C) They don't use RAM
   - D) They have larger L3 caches

3. **What hardware mechanism allows TPUs to perform matrix multiplications with near-zero memory writebacks?**
   - A) Out-of-order execution
   - B) Branch prediction
   - C) 2D Systolic Array Processing Elements *(Correct)*
   - D) Hyper-threading

4. **What is the primary role of a DPU (Data Processing Unit) in a modern cloud server?**
   - A) Train Large Language Models
   - B) Display 4K graphics
   - C) Offload networking, NVMe-oF storage, and encryption from the host CPU *(Correct)*
   - D) Replace motherboard RAM

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 01 - Computer Architecture and CPU Fundamentals](./01-Computer-Architecture-and-CPU-Fundamentals.md) | [Index](../../../README.md) | [03 - ISA, x86-64, ARM64, RISC-V & Microarchitecture →](./03-ISA-x86-ARM-RISCV-and-Microarchitecture.md) |
