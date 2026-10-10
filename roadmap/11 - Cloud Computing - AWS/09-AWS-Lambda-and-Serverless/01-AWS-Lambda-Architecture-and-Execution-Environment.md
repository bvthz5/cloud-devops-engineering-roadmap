# 01 - AWS Lambda: Serverless Architecture, Concurrency & Cold Starts

> **Focus:** Event-driven serverless computing, Firecracker microVM execution sandboxes, 15-minute maximum timeout, memory and CPU allocation ratios, Cold Start mitigation, and Provisioned Concurrency.

---

## ⚡ 1. AWS Lambda Architecture

**AWS Lambda** executes your code in response to events and automatically manages underlying compute resources:

```text
EVENT SOURCE                     LAMBDA SERVICE                   DESTINATION
┌──────────────┐                 ┌────────────────────┐           ┌──────────────┐
│  Amazon S3   │ ──(Event)─────► │ AWS Lambda Runtime │ ────────► │  DynamoDB    │
│ (Object PUT) │                 │ (Firecracker MicroVM)          │ (Save Record)│
└──────────────┘                 └────────────────────┘           └──────────────┘
```

### ⚙️ Core Technical Limits & Specifications:
- **Maximum Execution Timeout:** **`15 Minutes` (900 seconds)**.
- **Memory Allocation:** Configurable from **`128 MB` to `10,240 MB`** (10 GB) in 1 MB increments.
- **CPU Scaling:** You cannot configure vCPUs directly. **CPU power scales linearly with memory.** At `1,769 MB` RAM, your function gets exactly 1 full dedicated vCPU.
- **Ephemeral Scratch Space:** `/tmp` directory configurable from **`512 MB` up to `10 GB`**.

---

## ❄️ 2. Cold Starts vs. Provisioned Concurrency

- **Cold Start (100–1000 ms):** When a function is invoked after being idle, AWS must download the deployment package, spin up a new Firecracker microVM, and initialize the runtime (Python/Node/Java JVM).
- **Warm Invocation (Single-digit ms):** Subsequent requests reuse the running container environment.
- **Provisioned Concurrency:** Pre-initializes a requested number of execution environments 24/7 so functions respond with double-digit millisecond latency instantly, eliminating cold starts completely!

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Prev Module (08-AWS-ECS-EKS-and-Container-Services)](../08-AWS-ECS-EKS-and-Container-Services/12-Quick-Revision.md) | [Index](../../../README.md) | [02 - Lambda Layers Container Images and Runtimes →](./02-Lambda-Layers-Container-Images-and-Runtimes.md) |
