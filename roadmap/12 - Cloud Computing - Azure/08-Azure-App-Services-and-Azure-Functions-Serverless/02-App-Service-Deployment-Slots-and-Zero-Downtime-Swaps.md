# 02 - Azure App Service Deployment Slots & Zero-Downtime Swaps

> **Focus:** Production deployment strategies on Azure App Service. Deep dive into Deployment Slots, Staging-to-Production swap mechanics, HTTP warmup phases, Slot-specific sticky settings, and Canary testing with traffic percentage routing.

---

## 🚀 1. What, Why, and History of Deployment Slots

### What are Deployment Slots?
Deployment Slots are live, independently running web applications hosted within the same App Service Plan as your primary production application. Each slot possesses its own private DNS hostname (e.g., `myapp-staging.azurewebsites.net`), environment variables, and compute allocations.

### Why do Production Teams Use Deployment Slots?
1. **Zero-Downtime Releases:** Deploying code directly into a live production application restarts the runtime worker, causing dropped connections and HTTP 503 errors. Slot swapping eliminates downtime.
2. **Pre-Warming & Smoke Testing:** New code can be fully compiled, caches loaded, and end-to-end integration tests validated against the staging slot before any live customer traffic hits the instances.
3. **Instant Rollback:** If a regression is detected after swapping to production, performing an immediate reverse swap restores the previous working version within seconds without rebuilding or redeploying code.

### 📜 History & Evolution:
- **2013:** Launched under "Windows Azure Websites" (WAWS) allowing a single staging slot on Standard tier.
- **2016:** Introduction of **Auto-Swap** and **Traffic Routing (Testing in Production)** allowing percentage-based traffic splitting (e.g., 90% Production / 10% Staging) for Canary releases.
- **Current Standard:** Multi-slot support (up to 20 slots on Premium plans) with integrated health check probes preventing swap if instances fail readiness verification.

---

## 🔄 2. The Internal Mechanics of a Slot Swap

When you trigger a swap between `staging` and `production`, Azure executes the following automated lifecycle:

```text
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                              SLOT SWAP INTERNAL LIFECYCLE                              │
├────────────────────────────────────────────────────────────────────────────────────────┤
│ PHASE 1: TARGET CONFIGURATION APPLIED                                                  │
│   • Azure applies Production configuration settings to the Staging worker instances.  │
│                                                                                        │
│ PHASE 2: WARMUP & HEALTH CHECK EXECUTION                                               │
│   • Azure sends HTTP GET requests to the root path or specified warmup endpoint        │
│     (configured in web.config or applicationInitialization).                           │
│   • All instances across all scaled workers must return HTTP 200 OK.                   │
│   • If ANY instance fails warmup, the SWAP IS ABORTED; Production is untouched!        │
│                                                                                        │
│ PHASE 3: ROUTING TABLE POINTER UPDATE (Instant VIP Flip)                               │
│   • Azure Load Balancer routing rules update their internal IP routing pointers:       │
│     myapp.azurewebsites.net ─────────> Points to New Code Instances                    │
│     myapp-staging.azurewebsites.net ─> Points to Old Code Instances                    │
│   • ZERO HTTP requests are dropped; established TCP connections complete cleanly.      │
└────────────────────────────────────────────────────────────────────────────────────────┘
```

---

## ⚙️ 3. Slot Settings: Sticky vs. Non-Sticky Configuration

When swapping slots, some settings must follow the code, while others must stay fixed to the slot:

| Configuration Setting | Default Behavior | Recommended Setting | Rationale |
| :--- | :---: | :---: | :--- |
| **Database Connection String** | Swaps with code | **Slot Setting (Sticky)** | Staging must point to Staging DB; Prod must point to Prod DB! |
| **Log Analytics Instrumentation Key** | Swaps with code | **Slot Setting (Sticky)** | Isolates telemetry logs between environments. |
| **Runtime Version / Framework** | Swaps with code | Non-sticky (Swaps) | Tests new .NET / Node runtime before making live. |
| **Custom Domain & SSL Certs** | Fixed to slot | Fixed to slot | `myapp.com` must always point to Production slot. |

---

## 💻 4. Practical CLI Commands: Managing Deployment Slots

```bash
# 1. Create a Staging deployment slot
az webapp deployment slot create \
  --name webapp-corp-prod \
  --resource-group rg-appservice \
  --slot staging

# 2. Configure Database Connection String as a Sticky Slot Setting
az webapp config connection-string set \
  --name webapp-corp-prod \
  --resource-group rg-appservice \
  --slot staging \
  --connection-string-type SQLAzure \
  --settings DB_CONN="Server=tcp:sql-staging.database.windows.net..." \
  --slot-settings DB_CONN

# 3. Route 15% of live production traffic to Staging (Canary Testing)
az webapp traffic-routing set \
  --name webapp-corp-prod \
  --resource-group rg-appservice \
  --distribution staging=15

# 4. Perform the Zero-Downtime Production Slot Swap
az webapp deployment slot swap \
  --name webapp-corp-prod \
  --resource-group rg-appservice \
  --slot staging \
  --target-slot production
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 01 - Azure App Service Architecture and App Service Plans](./01-Azure-App-Service-Architecture-and-App-Service-Plans.md) | [Index](../../../README.md) | [03 - Custom Domains SSL Certificates and VNet Integration →](./03-Custom-Domains-SSL-Certificates-and-VNet-Integration.md) |
