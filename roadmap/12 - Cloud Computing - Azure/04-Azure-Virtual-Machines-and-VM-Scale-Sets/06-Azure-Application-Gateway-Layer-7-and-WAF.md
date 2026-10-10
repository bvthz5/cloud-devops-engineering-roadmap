# 06 - Azure Application Gateway: Layer 7 Routing, SSL Offloading & WAF v2

> **Focus:** Layer 7 application load balancing, URL path-based routing, SSL/TLS termination offload, Web Application Firewall (WAF v2 OWASP 3.2), cookie-based affinity, and dedicated subnet requirements.

---

## 🌐 1. Application Gateway (Layer 7) Architecture

**Azure Application Gateway** is a web traffic load balancer that enables intelligent routing decisions based on HTTP/HTTPS attributes (host headers, URL paths, cookies).

```text
┌────────────────────────────────────────────────────────┐
│             Azure Application Gateway (L7)             │
│  Public IP: 20.60.1.50                                 │
│  Listener: HTTPS Port 443 (SSL/TLS Offload)            │
│  WAF v2: OWASP CRS 3.2 (Prevention Mode)               │
│                                                        │
│  URL Path Routing Rules:                               │
│    • /api/*     ────────► API Backend Pool             │
│    • /images/*  ────────► Media Storage / Pool         │
│    • /* (Default)───────► Web Frontend Pool            │
└───────────────────────────┬────────────────────────────┘
                            │
       ┌────────────────────┼────────────────────┐
       ▼                    ▼                    ▼
┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│  Web Pool    │     │  Media Pool  │     │   API Pool   │
│  (VM-1/VM-2) │     │ (Storage/VM) │     │  (App Svc)   │
└──────────────┘     └──────────────┘     └──────────────┘
```

---

## 🛡️ 2. Core Features & Capabilities

1. **URL Path-Based Routing:** Route `/api/*` to microservices and `/static/*` to caching pools using a single public IP.
2. **SSL/TLS Termination:** Decrypt incoming SSL requests at the gateway. Backend VM servers run unencrypted HTTP inside the private VNet, saving CPU overhead.
3. **Web Application Firewall (WAF v2):** Protects web apps from common vulnerabilities including SQL Injection (SQLi), Cross-Site Scripting (XSS), and HTTP Protocol violations matching OWASP Core Rule Set (CRS 3.2).
4. **Cookie-Based Session Affinity:** Injects an `ARRAffinity` cookie into client HTTP responses to ensure a user's subsequent requests are pinned to the same backend instance.
5. **Dedicated Subnet Requirement:** Application Gateway **MUST** reside inside its own dedicated subnet. No VMs, storage accounts, or other resources may share this subnet.

---

### 💻 Production Azure CLI Commands

```bash
# 1. Create Dedicated Subnet for App Gateway
az network vnet subnet create   --resource-group rg-networking   --vnet-name vnet-prod   --name subnet-appgw   --address-prefix 10.0.3.0/27

# 2. Create Public IP for App Gateway
az network public-ip create   --resource-group rg-networking   --name pip-appgw   --sku Standard   --allocation-method Static

# 3. Create Application Gateway (Standard_v2)
az network application-gateway create   --resource-group rg-networking   --name appgw-prod   --location eastus   --sku Standard_v2   --capacity 2   --vnet-name vnet-prod   --subnet subnet-appgw   --public-ip-address pip-appgw   --frontend-port 80   --http-settings-port 80   --http-settings-protocol Http   --routing-rule-type Basic   --priority 1

# 4. Enable WAF in Prevention Mode (WAF_v2 SKU)
az network application-gateway waf-config set   --resource-group rg-networking   --gateway-name appgw-prod   --enabled true   --firewall-mode Prevention   --rule-set-type OWASP   --rule-set-version 3.2
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 05 - Azure Load Balancer Standard vs Basic](./05-Azure-Load-Balancer-Standard-vs-Basic.md) | [Index](../../../README.md) | [07 - Real World Scenarios →](./07-Real-World-Scenarios.md) |
