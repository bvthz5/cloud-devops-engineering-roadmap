# AI for DevOps, AIOps & LLMOps Scenarios: Production Incidents & Triage Scenarios

> **Interview Focus:** Real-world incident response, root-cause analysis (RCA), diagnostic toolchains, and articulate candidate verbal pitches.

---

## 📌 Scenario 1: vLLM GPU Out-Of-Memory (OOM) During High Concurrency LLM Serving

### 🚨 The Production Scenario
A production LLM inference service running vLLM with Llama-3-70B on 4x NVIDIA A100 GPUs crashes with `torch.cuda.OutOfMemoryError: CUDA out of memory` during a traffic spike.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
vLLM utilizes PagedAttention to manage KV-cache memory dynamically. The `gpu_memory_utilization` was set too high (0.95), leaving insufficient GPU memory for activation spikes during large batch sequence decoding, or incoming prompts exceeded the `max_model_len` context window.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Inspect GPU memory allocation and KV cache usage using `nvidia-smi` and vLLM Prometheus metrics.
- Tune vLLM parameters: adjust `gpu_memory_utilization` to a safer threshold (e.g., 0.88 or 0.90) to leave headroom for dynamic activations.
- Configure `max_num_seqs` to limit the number of concurrent sequences processed in a single batch iteration.
- Deploy an intelligent queue / proxy (e.g., LiteLLM or Triton) in front of vLLM to reject or queue requests when KV cache reaches 90% saturation.
- Scale vLLM pods horizontally across additional GPU nodes.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Monitor GPU VRAM and compute utilization in real-time
nvidia-smi --query-gpu=memory.total,memory.used,memory.free,utilization.gpu --format=csv -l 1

# Query vLLM KV-cache memory usage factor via Prometheus
curl -s http://vllm-service:8000/metrics | grep vllm:gpu_cache_usage_factor

# Launch vLLM with safe GPU memory headroom and concurrency caps
python3 -m vllm.entrypoints.openai.api_server --model meta-llama/Meta-Llama-3-70B-Instruct --tensor-parallel-size 4 --gpu-memory-utilization 0.88 --max-num-seqs 128

# Inspect vLLM runtime logs
kubectl logs -l app=vllm-inference -n ai-prod --tail=50

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "vLLM GPU OOMs happen when KV-cache allocation leaves zero headroom for dynamic model activations during peak batching. I set gpu_memory_utilization to 0.88 to reserve VRAM for activations and cap max_num_seqs. Furthermore, we monitor vllm:gpu_cache_usage_factor in Prometheus and autoscale pods or reject traffic at the edge before the GPU VRAM exhausts."

---

## 📌 Scenario 2: Kubernetes GPU Scheduling Starvation & Fragmented GPU Allocation

### 🚨 The Production Scenario
A multi-node Kubernetes cluster with 32 NVIDIA GPUs fails to schedule new ML training and inference pods. Pods remain in `Pending` with `0/8 nodes available: Insufficient nvidia.com/gpu` despite cluster metrics showing 40% overall GPU utilization.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
GPU fragmentation: single-GPU jobs were scheduled on multi-GPU nodes without bin-packing or topology-aware scheduling, leaving multi-GPU jobs requiring 4 or 8 co-located GPUs unable to find a single node with enough contiguous free GPUs.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Implement Kubernetes Volcano or Kueue scheduler with gang scheduling and bin-packing algorithms.
- Dedicate distinct node pools: single-GPU workloads run on single-GPU nodes; distributed 8-GPU workloads run on dedicated 8-GPU HGX nodes with NVLink.
- Enable NVIDIA Multi-Instance GPU (MIG) on A100/H100 instances to slice physical GPUs into smaller hardware-isolated instances for lightweight workloads.
- Enforce resource quotas and priority classes for AI workloads.
- Configure descheduler to evict and re-pack fragmented workloads during off-peak windows.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Audit available allocatable GPUs per node
kubectl get nodes -o custom-columns=NAME:.metadata.name,GPUS:.status.allocatable.'nvidia\.com/gpu'

# List configured Multi-Instance GPU (MIG) instances
nvidia-smi mig -lgi

# Create hardware-isolated MIG GPU slices
nvidia-smi mig -cgi 19,19,19 -C

# Inspect scheduler placement rejection reasons
kubectl describe pod -l app=training-job | grep FailedScheduling

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "GPU scheduling fragmentation occurs when single-GPU pods scatter across multi-GPU nodes, blocking distributed workloads that require 8 GPUs on the same NVLink fabric. I resolve this by segregating node pools by topology, deploying Volcano for gang-scheduling and bin-packing, and enabling NVIDIA MIG on A100/H100 nodes to slice unused compute into isolated virtual GPUs."

---

## 📌 Scenario 3: LLM Time-To-First-Token (TTFT) Latency Degrades by 400% Under Prompt Length Spikes

### 🚨 The Production Scenario
An enterprise RAG (Retrieval-Augmented Generation) application experiences TTFT jumping from 600ms to 4,200ms. Users experience significant typing lag before answers begin streaming.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
RAG retriever injected massive context documents (30,000 tokens) into every prompt. The prefill phase of LLM inference is compute-bound and quadratic with prompt length. Shared inference engines handling both prefill (prompt processing) and decode (token generation) choked.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Implement Chunked Prefill (supported in vLLM and TensorRT-LLM) to interleave prefill computations across multiple batches without blocking active decodes.
- Adopt Disaggregated Serving / Splitwise architecture: separate dedicated prefill nodes from dedicated decode nodes.
- Implement Prompt Caching (Prefix Caching): cache KV states of common system prompts and documentation in memory.
- Tune RAG retrieval: re-rank and truncate retrieved chunks using a cross-encoder to limit context to top 5 relevant snippets.
- Monitor TTFT vs Inter-Token Latency (ITL) metrics separately.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Query TTFT latency distribution metric
curl -s http://vllm:8000/metrics | grep vllm:time_to_first_token_seconds

# Enable Prefix Caching and Chunked Prefill in vLLM
python3 -m vllm.entrypoints.openai.api_server --enable-prefix-caching --enable-chunked-prefill

# Benchmark exact TTFT via curl
curl -X POST http://vllm:8000/v1/chat/completions -H 'Content-Type: application/json' -d '{"model": "llama3", "messages": [{"role":"user", "content":"test"}], "stream": true}' -w 'TTFT: %{time_starttransfer}\n'

# Verify NVLink transfer status between GPU sockets
nvidia-smi nvlink -s

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "TTFT latency spikes occur because prompt prefill is computationally intensive and blocks ongoing token decoding. I fix this with three steps: first, enable Prefix Caching in vLLM so shared system prompts reuse existing KV memory; second, enable Chunked Prefill to slice long prompt ingest into smaller chunks; third, deploy re-rankers in RAG to inject only the highest-relevance context."

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Database Reliability Engineering (DBRE) Scenarios: Rapid-Fire Drills & Master Cheat Sheet](../17-Database-Reliability-Engineering-DBRE-Scenarios/04-Rapid-Fire-Interview-Drills-and-Cheat-Sheet.md) | [Index](../../../README.md) | [AI for DevOps, AIOps & LLMOps Scenarios: Architecture & Scaling Scenarios →](02-Architecture-and-Scaling-Scenarios.md) |

