# 06 - Azure Web Application Firewall (WAF) & Azure DDoS Protection

> **Focus:** Edge security and Layer 7 protection in Microsoft Azure. Deep dive into Azure Web Application Firewall (WAF) on Application Gateway & Azure Front Door, OWASP ModSecurity Core Rule Sets (CRS), Detection vs. Prevention modes, and Azure DDoS Network Protection with Cost Guarantees.

---

## 🌐 1. What, Why, and History of Perimeter Security in Azure

### What are WAF and DDoS Protection?
- **Azure Web Application Firewall (WAF):** A Layer 7 application security inspection engine that inspects incoming HTTP/HTTPS traffic to block web exploits, SQL Injection, Cross-Site Scripting (XSS), and malicious bots before they reach backend application code.
- **Azure DDoS Protection:** A dedicated volumetric and protocol attack mitigation service protecting Layer 3 (IP) and Layer 4 (TCP/UDP) network infrastructure against multi-gigabit flooding attacks.

### Why are Traditional Firewalls and NSGs Insufficient?
1. **The Blindness of Layer 4 Security:** Network Security Groups (NSGs) and standard packet-filtering firewalls only inspect source IP, destination IP, and destination port (e.g., port 443). Because port 443 must remain open to the public for web users, an attacker can transmit a devastating SQL Injection attack (`' OR '1'='1`) straight through an NSG without detection!
2. **Volumetric Bandwidth Saturation:** A 500 Gbps DDoS attack saturates datacenter bandwidth and crashes load balancers long before application code executes.
3. **Financial Protection (Cost Spikes):** If a DDoS attack hits an auto-scaling App Service or VMSS, the cloud infrastructure scales to maximum capacity, resulting in tens of thousands of dollars in shock bills. Azure DDoS Protection provides a **Cost Protection Guarantee** that credits billing spikes caused by documented DDoS attacks!

---

## ⚖️ 2. Architectural Comparison: Azure WAF on Application Gateway vs. Azure Front Door

```text
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                              WAF DEPLOYMENT ARCHITECTURE                               │
├────────────────────────────────────────────────────────────────────────────────────────┤
│ OPTION A: GLOBAL EDGE WAF (AZURE FRONT DOOR)                                           │
│   • Location: Deployed at 200+ Microsoft Global Edge Points of Presence (PoPs).        │
│   • Advantage: Blocks malicious HTTP requests AT THE CLOSEST EDGE TO THE ATTACKER,     │
│     preventing malicious traffic from ever entering the Azure private backbone!        │
│                                                                                        │
│ OPTION B: REGIONAL WAF (AZURE APPLICATION GATEWAY)                                     │
│   • Location: Deployed inside customer private Virtual Network subnet.                 │
│   • Advantage: Inspects internal intra-VNet and hybrid on-premises HTTP traffic.       │
└────────────────────────────────────────────────────────────────────────────────────────┘
```

| Feature | WAF on Azure Front Door | WAF on Azure Application Gateway |
| :--- | :--- | :--- |
| **Architectural Scope** | **Global Edge (Anycast BGP)** | **Regional (Inside customer VNet)** |
| **Inspection Location** | 200+ Edge PoPs worldwide | Dedicated Regional Gateway Subnet |
| **Best Used For** | Public-facing global web apps & APIs | Regional internal or hybrid apps |
| **Rule Sets** | Microsoft Default Rule Set (DRS 2.1) | OWASP Core Rule Set (CRS 3.2 / 3.3) |
| **Bot Protection** | Native Microsoft Threat Intelligence Bot Manager | Bot Manager ruleset add-on |

---

## 🛡️ 3. WAF Operating Modes & OWASP Top 10 Rules

Azure WAF evaluates every incoming HTTP request through two operational modes:
1. **Detection Mode:** Monitors and inspects traffic; logs all detected attacks into the `AzureDiagnostics` Log Analytics table without blocking requests. **Essential for staging validation to baseline false positives!**
2. **Prevention Mode:** Actively drops malicious HTTP requests, immediately returning an **HTTP 403 Forbidden** response with a unique Tracking ID.

### 🛑 Supported OWASP CRS Threat Protections:
- SQL Injection (SQLi)
- Cross-Site Scripting (XSS)
- Remote File Inclusion (RFI) & Local File Inclusion (LFI)
- Command Injection & Remote Code Execution (RCE)
- HTTP Protocol Violations & Request Smuggling
- Malicious Web Crawlers and Scanners

---

## ⚡ 4. Azure DDoS Protection Tiers: Network Protection vs. IP Protection

```text
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                              AZURE DDOS PROTECTION TIERS                               │
├────────────────────────────────────────────────────────────────────────────────────────┤
│ 1. DDOS IP PROTECTION (Per-Resource Flexibility):                                      │
│    • Billed per individual Public IP address (~$199 / month per public IP).            │
│    • Ideal for small organizations with 1 or 2 critical public endpoints.              │
│                                                                                        │
│ 2. DDOS NETWORK PROTECTION (Enterprise VNet Scale):                                    │
│    • Billed per organization profile (~$2,944 / month covers up to 100 public IPs).    │
│    • Includes Adaptive Tuning: AI continuously models your specific traffic patterns.  │
│    • Includes DDoS Rapid Response (DRR) 24/7 access to Microsoft CIRT security team.   │
│    • Includes COST PROTECTION GUARANTEE: Credits resource scaling costs during attacks!│
└────────────────────────────────────────────────────────────────────────────────────────┘
```

---

## 💻 5. Practical CLI: Creating a WAF Policy in Prevention Mode

```bash
# 1. Create a WAF Policy with OWASP Core Rule Set 3.2 in Prevention Mode
az network application-gateway waf-policy create \
  --name "waf-policy-prod" \
  --resource-group "rg-security-edge" \
  --location eastus \
  --type ApplicationGateway

# 2. Configure Prevention Mode and Enable Request Body Inspection
az network application-gateway waf-policy policy-setting update \
  --name "waf-policy-prod" \
  --resource-group "rg-security-edge" \
  --mode Prevention \
  --state Enabled \
  --max-request-body-size-in-kb 128 \
  --file-upload-limit-in-mb 100

# 3. Add Managed OWASP Rule Set 3.2
az network application-gateway waf-policy managed-rule rule-set add \
  --policy-name "waf-policy-prod" \
  --resource-group "rg-security-edge" \
  --type OWASP \
  --version 3.2
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 05 - Azure Sentinel Cloud Native SIEM and SOAR](./05-Azure-Sentinel-Cloud-Native-SIEM-and-SOAR.md) | [Index](../../../README.md) | [07 - Real World Scenarios →](./07-Real-World-Scenarios.md) |
