# 03 - App Service Security: Custom Domains, SSL Certificates & Regional VNet Integration

> **Focus:** Enterprise networking and TLS security for Azure App Service. Deep dive into Custom Domain verification, Free App Service Managed Certificates, TLS/SNI binding, and Regional VNet Integration for secure outbound communication to private databases and on-premises systems.

---

## 🔒 1. What, Why, and History of App Service Security & Networking

### What are Custom Domains and VNet Integration?
- **Custom Domains & SSL:** Binds corporate domain names (e.g., `api.contoso.com`) to Azure App Service instances backed by trusted TLS/SSL encryption certificates.
- **Regional VNet Integration:** Connects your App Service web app to a dedicated subnet within your private Azure Virtual Network (VNet), allowing backend requests to reach private databases, Redis caches, and internal microservices without traversing the public internet.

### Why is Default App Service Networking Insufficient?
1. **Corporate Branding & Trust:** Public users reject default `*.azurewebsites.net` URLs.
2. **Backend Protection:** Database engines (Azure SQL, Cosmos DB, PostgreSQL) must not have public internet firewalls opened to receive queries from web servers.
3. **Hybrid Connectivity:** App Service must be able to query on-premises SAP or Oracle systems across existing VPN or ExpressRoute connections.

### 📜 History & Evolution:
- **2013:** Initial custom domain binding required manual PFX certificate uploads and expensive dedicated IP-based SSL bindings.
- **2019:** Introduction of **SNI (Server Name Indication) SSL** and **Regional VNet Integration** using lightweight network routing without needing an expensive App Service Environment (ASE).
- **2021:** **App Service Managed Certificates (ASMC)** made free, automated, and auto-renewing for all Custom Domains on Basic/Standard/Premium tiers.

---

## 🌐 2. Custom Domain Verification & TLS Binding Architecture

```text
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                              CUSTOM DOMAIN VERIFICATION FLOW                           │
├────────────────────────────────────────────────────────────────────────────────────────┤
│ STEP 1: ADD DNS TXT RECORD AT DOMAIN REGISTRAR                                         │
│   Host: asuid.api.contoso.com  ➔  Value: <Custom-Domain-Verification-ID-from-Azure>    │
│   (Prevents domain hijacking; proves you own the domain)                               │
│                                                                                        │
│ STEP 2: ADD DNS CNAME RECORD                                                           │
│   Host: api.contoso.com        ➔  Value: myapp-prod.azurewebsites.net                  │
│                                                                                        │
│ STEP 3: PROVISION TLS CERTIFICATE & BIND SNI                                           │
│   • Azure requests free DigiCert/Let's Encrypt TLS certificate.                        │
│   • Binds certificate to domain using SNI (Server Name Indication) on port 443.       │
│   • Auto-renewal occurs automatically 45 days before expiration.                       │
└────────────────────────────────────────────────────────────────────────────────────────┘
```

---

## 🏛️ 3. Regional VNet Integration Architecture

Regional VNet Integration modifies **Outbound** traffic routing from the App Service:

```text
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                          REGIONAL VNET INTEGRATION TOPOLOGY                            │
│                                                                                        │
│   PUBLIC INTERNET                                    AZURE VIRTUAL NETWORK (VNET)      │
│   ┌────────────────────┐                             ┌─────────────────────────────┐   │
│   │ Client Web Browser │                             │ Dedicated Subnet:           │   │
│   └─────────┬──────────┘                             │ snet-appservice             │   │
│             │ Inbound HTTPS (443)                    │ (Delegation:                │   │
│             ▼                                        │  Microsoft.Web/serverFarms) │   │
│   ┌────────────────────────────────┐                 │ ┌─────────────────────────┐ │   │
│   │ Azure App Service Frontend     │                 │ │ App Service Outbound NIC│ │   │
│   │ (myapp.azurewebsites.net)      ├─────────────────┼>│ (Private IP: 10.0.1.4)  │ │   │
│   └────────────────────────────────┘                 │ └────────────┬────────────┘ │   │
│                                                      └──────────────┼──────────────┘   │
│                                                                     │                  │
│                                                      ┌──────────────▼──────────────┐   │
│                                                      │ Private Subnet: snet-db     │   │
│                                                      │ ┌─────────────────────────┐ │   │
│                                                      │ │ Azure SQL Private Endpoint│ │
│                                                      │ │ (Private IP: 10.0.2.10) │ │   │
│                                                      │ └─────────────────────────┘ │   │
│                                                      └─────────────────────────────┘   │
└────────────────────────────────────────────────────────────────────────────────────────┘
```

> **Crucial Rule:** Inbound traffic still reaches the App Service via its public endpoint or an Azure Application Gateway. VNet Integration routes **Outbound** traffic through the delegated subnet, granting access to private resources!

---

## 💻 4. Hands-on CLI: Configuring Custom Domain, SSL & VNet Integration

```bash
# 1. Retrieve the Custom Domain Verification ID
VERIFICATION_ID=$(az webapp show -n myapp-prod -g rg-apps --query customDomainVerificationId -o tsv)
echo "Add TXT record 'asuid.api.contoso.com' with value: $VERIFICATION_ID"

# 2. Bind the custom hostname
az webapp config hostname add \
  --webapp-name myapp-prod \
  --resource-group rg-apps \
  --hostname api.contoso.com

# 3. Create and bind a free Azure Managed Certificate
az webapp config ssl create \
  --name myapp-prod \
  --resource-group rg-apps \
  --hostname api.contoso.com

az webapp config ssl bind \
  --name myapp-prod \
  --resource-group rg-apps \
  --certificate-thumbprint $(az webapp config ssl show --name myapp-prod -g rg-apps --query "[0].thumbprint" -o tsv) \
  --ssl-type SNI

# 4. Enable Regional VNet Integration with a delegated subnet
az network vnet subnet create \
  --name snet-appservice-outbound \
  --resource-group rg-network \
  --vnet-name vnet-corp \
  --address-prefixes 10.0.10.0/24 \
  --delegations Microsoft.Web/serverFarms

az webapp vnet-integration add \
  --name myapp-prod \
  --resource-group rg-apps \
  --vnet vnet-corp \
  --subnet snet-appservice-outbound
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 02 - App Service Deployment Slots and Zero Downtime Swaps](./02-App-Service-Deployment-Slots-and-Zero-Downtime-Swaps.md) | [Index](../../../README.md) | [04 - Azure Functions Architecture Triggers and Bindings →](./04-Azure-Functions-Architecture-Triggers-and-Bindings.md) |
