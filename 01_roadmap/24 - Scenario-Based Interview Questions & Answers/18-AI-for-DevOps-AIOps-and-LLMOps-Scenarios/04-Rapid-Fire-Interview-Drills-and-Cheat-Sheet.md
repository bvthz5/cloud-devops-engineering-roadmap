# AI for DevOps, AIOps & LLMOps Scenarios: Rapid-Fire Drills & Master Cheat Sheet

> **Interview Focus:** Real-world incident response, root-cause analysis (RCA), diagnostic toolchains, and articulate candidate verbal pitches.

---

## 📌 Scenario 10: LLM Cost Runaway - Token Consumption Explodes by 1,000% Overnight

### 🚨 The Production Scenario
The monthly cloud bill reveals that OpenAI / Claude API costs surged from $500/day to $12,000/day. An unoptimized agent loop ran in an infinite recursion call over the weekend.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
An autonomous AI agent script had a bug where tool execution failures prompted the LLM to retry the same failed action in an unconstrained loop without maximum recursion depth caps, budget limiters, or token throttling.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Set hard spend limits and budget alerts in the API provider dashboard (OpenAI / Anthropic / Bedrock).
- Implement a centralized LLM Gateway (e.g., Portkey, LiteLLM, or Cloudflare AI Gateway) enforcing per-user, per-service rate limits and daily budget caps.
- Enforce hard recursion limits (`max_iterations = 5`) in LangChain / AutoGen / CrewAI agent frameworks.
- Implement semantic caching (e.g., GPTCache) to serve identical prompt answers from Redis without hitting the LLM API.
- Alert on abnormal token consumption velocity in real-time.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Launch LiteLLM gateway with rate limits and spend caps
litellm --port 8000 --config litellm_config.yaml

# Audit real-time API spend per user and service
curl -s http://litellm:8000/spend/user | jq .

# Verify agent execution code enforces max iteration guardrails
grep -i 'recursion_limit' agent_runner.py

# Verify cached response retrieval from semantic cache
redis-cli get gptcache:prompt_hash_123

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "Unconstrained agent loops are a massive financial liability. I enforce two layers of protection: First, application code must have hard max_iterations: 5 caps. Second, all enterprise LLM calls must route through LiteLLM Gateway, which enforces hard monthly spend caps, semantic caching with Redis to eliminate redundant queries, and real-time Slack alerts if token velocity spikes."

---

## ⚡ Rapid-Fire Diagnostic Cheat Sheet

| Symptom / Scenario | First Command to Run | Root Cause Hypothesis |
| :--- | :--- | :--- |
| **Scenario 1: vLLM GPU Out-Of-Memory (OOM) During High Concurrency LLM Serving** | `nvidia-smi --query-gpu=memory.total,memory.used,memory.free,utilization.gpu --format=csv -l 1` | vLLM utilizes PagedAttention to manage KV-cache memory dynamically. The `gpu_mem... |
| **Scenario 2: Kubernetes GPU Scheduling Starvation & Fragmented GPU Allocation** | `kubectl get nodes -o custom-columns=NAME:.metadata.name,GPUS:.status.allocatable.'nvidia\.com/gpu'` | GPU fragmentation: single-GPU jobs were scheduled on multi-GPU nodes without bin... |
| **Scenario 3: LLM Time-To-First-Token (TTFT) Latency Degrades by 400% Under Prompt Length Spikes** | `curl -s http://vllm:8000/metrics \| grep vllm:time_to_first_token_seconds` | RAG retriever injected massive context documents (30,000 tokens) into every prom... |
| **Scenario 4: ML Data Drift & Model Performance Degradation in Production Fraud Detection** | `python3 -m evidently metric-eval --reference train.csv --current inference.csv --metrics DataDriftTable` | Data drift and concept drift: consumer purchasing patterns shifted (e.g., holida... |
| **Scenario 5: Triton Inference Server Worker Thread Deadlock Under Model Concurrency** | `curl -v http://triton:8000/v2/health/ready` | A custom Python backend model utilized a non-thread-safe C library without relea... |
| **Scenario 6: LLM Hallucination & Prompt Injection Vulnerability in Customer AI Agent** | `python3 -m guardrails_ai check --prompt 'Ignore instructions...'` | Lack of input sanitization, absence of LLM guardrails (NeMo Guardrails / Guardra... |
| **Scenario 7: Vector Database (Milvus/Qdrant/Pinecone) Slow Indexing & Vector Search Latency** | `curl -s http://qdrant:6333/collections/knowledge_base \| jq .result.status` | HNSW (Hierarchical Navigable Small World) index construction was running in real... |
| **Scenario 8: AIOps Incident Noise - Predictive Anomaly Detection Alert Storm** | `python3 -m prophet train --input metrics_history.csv --output model.pkl` | The machine learning anomaly model used an overly narrow confidence interval (3-... |
| **Scenario 9: Model Serving Cold-Start Latency in Serverless GPU Clouds (RunPod/Modal)** | `juicefs mount -d redis://cache:6379/1 /mnt/models` | Downloading a 28 GB model weights file (SafeTensors) from remote object storage ... |
| **Scenario 10: LLM Cost Runaway - Token Consumption Explodes by 1,000% Overnight** | `litellm --port 8000 --config litellm_config.yaml` | An autonomous AI agent script had a bug where tool execution failures prompted t... |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← AI for DevOps, AIOps & LLMOps Scenarios: Security & Disaster Recovery Scenarios](03-Security-and-Disaster-Recovery-Scenarios.md) | [Index](../../../README.md) | [Multi-Cloud, Hybrid Cloud & FinOps Scenarios: Production Incidents & Triage Scenarios →](../19-Multi-Cloud-Hybrid-Cloud-and-FinOps-Scenarios/01-Production-Incidents-and-Triage-Scenarios.md) |

