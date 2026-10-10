# 01 - Azure App Service Architecture, Runtimes & App Service Plans

> **Focus:** Platform as a Service (PaaS) web hosting, supported runtime stacks, App Service Plans (Free, Basic, Standard, Premium, Isolated), continuous deployment, and zip deployment workflows.

---

## ☁️ 1. Azure App Service PaaS Architecture

**Azure App Service** is an enterprise HTTP-based Platform as a Service (PaaS) solution for hosting web applications, REST APIs, and mobile backends. You manage application code and data; Azure manages the operating system, security patching, runtime frameworks, and physical hardware infrastructure.

```text
┌────────────────────────────────────────────────────────┐
│                   Azure App Service                    │
│                                                        │
│  Supported Languages:                                  │
│  • Node.js, Python, Java, .NET, PHP, Ruby              │
│  • Custom Docker Containers (Linux & Windows)          │
│                                                        │
│  Platform Capabilities:                                │
│  • Automatic OS patching & security updates            │
│  • Built-in SSL/TLS termination & custom domains       │
│  • Deployment slots for zero-downtime swaps            │
│  • Automatic horizontal & vertical scaling             │
└───────────────────────────┬────────────────────────────┘
                            │ Runs On Top Of
                            ▼
┌────────────────────────────────────────────────────────┐
│                   App Service Plan                     │
│  Defines: Compute Region, VM Instance Size, Tier & Cost │
└────────────────────────────────────────────────────────┘
```

---

## 💳 2. App Service Plans (Pricing Tiers) Comparison

| Tier | Dedicated Compute? | Auto-Scaling? | Staging Slots? | VNet Integration? | Best Enterprise Use Case |
| :--- | :---: | :---: | :---: | :---: | :--- |
| **Free / Shared** | ❌ Shared | ❌ No | ❌ No | ❌ No | Testing and prototype evaluation. No SLA. |
| **Basic (B1–B3)** | ✅ Dedicated | ❌ Manual only | ❌ No | ❌ No | Non-production dev/test environments. |
| **Standard (S1–S3)**| ✅ Dedicated | ✅ **Auto-Scale (up to 10 instances)** | ✅ **5 Slots** | ✅ Yes | **Standard production web applications**. |
| **Premium (P1v3–P3v3)**| ✅ Dedicated | ✅ **Auto-Scale (up to 30 instances)** | ✅ **20 Slots**| ✅ Yes | High-traffic production, memory-intensive apps. |
| **Isolated (I1v2–I6v2)**| ✅ Dedicated (ASE) | ✅ Auto-Scale (up to 100 instances) | ✅ Yes | ✅ **Private VNet Injection** | Highly regulated banking, defense, and healthcare. |

---

### 💻 Deploying Python Web Application via Azure CLI

```bash
# 1. Create Linux App Service Plan (Standard Tier S1)
az appservice plan create   --name my-app-plan   --resource-group demo-rg   --sku S1   --is-linux

# 2. Create Python Web App
az webapp create   --resource-group demo-rg   --plan my-app-plan   --name my-python-app-12345   --runtime "PYTHON:3.11"

# 3. Deploy Local Code via Zip Deploy
zip -r app.zip .
az webapp deployment source config-zip   --resource-group demo-rg   --name my-python-app-12345   --src app.zip

# 4. Retrieve Default Public URL
az webapp show   --resource-group demo-rg   --name my-python-app-12345   --query defaultHostName   --output tsv
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Prev Module (07-Azure-Kubernetes-Service-AKS)](../07-Azure-Kubernetes-Service-AKS/12-Quick-Revision.md) | [Index](../../../README.md) | [02 - App Service Deployment Slots and Zero Downtime Swaps →](./02-App-Service-Deployment-Slots-and-Zero-Downtime-Swaps.md) |
