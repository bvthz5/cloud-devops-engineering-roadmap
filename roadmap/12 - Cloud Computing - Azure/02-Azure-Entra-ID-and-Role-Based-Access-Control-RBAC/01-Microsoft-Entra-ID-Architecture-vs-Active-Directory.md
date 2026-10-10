# 01 - Microsoft Entra ID Architecture vs. On-Premises Active Directory

Microsoft Entra ID (formerly known as Azure Active Directory / Azure AD) is Microsoft's multi-tenant, cloud-based identity and access management (IAM) service. It serves as the universal authentication and authorization backbone across Microsoft Azure, Microsoft 365, and thousands of third-party SaaS applications (such as ServiceNow, GitHub, Salesforce, and Workday).

Understanding the fundamental architecture of Entra ID—and how it fundamentally departs from legacy on-premises Windows Server Active Directory (AD DS)—is critical for every cloud and DevOps engineer.

---

## 🏛️ 1. Entra ID vs. Traditional On-Premises Active Directory (AD DS)

Many engineers mistakenly assume that Entra ID is simply "Active Directory hosted on an Azure VM". In reality, they are completely different systems built on different protocols and architectural models:

```text
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                        ON-PREMISES AD DS vs. MICROSOFT ENTRA ID                        │
│                                                                                        │
│   ON-PREMISES ACTIVE DIRECTORY (AD DS)          MICROSOFT ENTRA ID (CLOUD IAM)         │
│   ┌─────────────────────────────────────┐       ┌────────────────────────────────────┐ │
│   │ Domain Controller Servers           │       │ Multi-Tenant Cloud Identity Service│ │
│   │ Network: Intranet / VPN Required    │       │ Network: Universal Internet (HTTPS)│ │
│   │ Hierarchical: OUs, Trees, Forests   │       │ Flat Structure: Users, Groups, Apps│ │
│   │ Policies: Group Policy Objects(GPOs)│       │ Policies: Conditional Access/Intune│ │
│   └──────────────────┬──────────────────┘       └─────────────────┬──────────────────┘ │
│                      │                                            │                    │
│   PROTOCOLS:         ▼                          PROTOCOLS:        ▼                    │
│   • Kerberos (Port 88)                          • OAuth 2.0 (Authorization)            │
│   • NTLM (Legacy authentication)                • OpenID Connect (OIDC Authentication) │
│   • LDAP / LDAPS (Port 389/636)                 • SAML 2.0 (Web Single Sign-On)        │
│   • RPC / SMB / DNS                             • Microsoft Graph REST API (Port 443)  │
└────────────────────────────────────────────────────────────────────────────────────────┘
```

### Detailed Architectural Differences

| Dimension | On-Premises Active Directory (AD DS) | Microsoft Entra ID (Cloud IAM) |
| :--- | :--- | :--- |
| **Primary Purpose** | Manages on-premises network resources, local workstations, and internal files. | Manages cloud identities, web applications, APIs, and cloud infrastructure access. |
| **Communication Protocols** | Heavy binary protocols: **Kerberos, LDAP, NTLM, RPC**. | Modern lightweight web protocols: **OAuth 2.0, OpenID Connect, SAML 2.0, REST**. |
| **Internet Accessibility** | Requires corporate network LAN or private VPN connection to domain controllers. | Accessible securely from anywhere over public HTTPS (Port 443) with Zero-Trust controls. |
| **Organizational Layout** | Hierarchical: Forests, Domains, Trees, and Organizational Units (OUs). | Flat structure: Directory Tenants containing Users, Groups, and App Registrations. |
| **Device Configuration** | Enforced via Windows Group Policy Objects (**GPOs**). | Managed via Mobile Device Management (**MDM**) and Microsoft Intune. |
| **Trust Model** | Perimeter-based security (trust anyone inside the corporate intranet). | **Zero-Trust**: Verify explicitly, use least-privilege access, assume breach. |

---

## 🔑 2. Core Entra ID Concepts & Identity Objects

```text
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                              ENTRA ID IDENTITY OBJECTS                                 │
│                                                                                        │
│   TENANT (contoso.onmicrosoft.com)                                                     │
│   │                                                                                    │
│   ├── USERS (Human engineers, administrators, external B2B guests)                     │
│   │                                                                                    │
│   ├── GROUPS (Security Groups - Best practice: assign RBAC to groups, not users!)     │
│   │                                                                                    │
│   ├── SERVICE PRINCIPALS (Application identities for external tools, CI/CD runners)   │
│   │                                                                                    │
│   ├── MANAGED IDENTITIES (Azure-managed identities - ZERO credentials in code!)        │
│   │                                                                                    │
│   └── APP REGISTRATIONS (Digital passport registering custom apps into the tenant)     │
└────────────────────────────────────────────────────────────────────────────────────────┘
```

1. **Tenant:** A dedicated, logically isolated instance of Microsoft Entra ID representing an organization (e.g., `contoso.onmicrosoft.com`).
2. **User:** An identity with credentials (username, password, MFA tokens) representing human employees or external business partners (B2B Guests).
3. **Group:** A collection of users. **Enterprise Golden Rule:** *Always assign Azure RBAC permissions to Groups, never to individual users.*
4. **Service Principal:** An identity created for an application or automated script (like a robot account or CI/CD runner) to authenticate and access Azure resources.
5. **Managed Identity:** A specialized service principal whose lifecycle and cryptographic certificates are automatically managed by Azure, eliminating hardcoded passwords.
6. **App Registration:** The process of registering your application software in Entra ID so it can issue and accept OAuth 2.0 tokens.

---

## 🔄 3. Authentication & Token Flow

When a user or application accesses an Azure resource (e.g., querying an Azure SQL database or deploying a VM), Entra ID acts as the central Identity Provider (IdP):

```text
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                        ENTRA ID AUTHENTICATION LIFECYCLE                               │
│                                                                                        │
│   1. User / Application requests access ──────────────────────────┐                    │
│                                                                   │                    │
│   2. Authenticates to Entra ID (Username, Password, MFA, Risk)    │                    │
│                                                                   ▼                    │
│   3. Evaluates Conditional Access Rules ──────────────► [ Microsoft Entra ID ]         │
│                                                                   │                    │
│   4. Issues JWT Bearer Token (containing claims & roles) ◄────────┘                    │
│      │                                                                                 │
│      ▼                                                                                 │
│   5. Presents JWT Token to Azure Resource Manager (ARM)                                │
│      │                                                                                 │
│      ▼                                                                                 │
│   6. ARM validates signature & verifies RBAC scope ──► [ ACCESS GRANTED ]              │
└────────────────────────────────────────────────────────────────────────────────────────┘
```

---

## ⚖️ 4. Identity Types: Human User vs. Service Principal vs. Managed Identity

| Dimension | Human User | Service Principal | Managed Identity |
| :--- | :--- | :--- | :--- |
| **Who Owns Credentials** | The human user (Password + Authenticator App). | The developer / DevOps engineer (Client Secret or Certificate). | **Azure Platform Automatically** (Zero secrets to manage). |
| **Secret Rotation** | Self-service password reset (SSPR) or corporate policy. | Manual rotation in Azure Portal / Key Vault before expiration. | **Automatic:** Azure rotates certificates internally. |
| **Risk of Leaked Secrets** | Phishing, credential stuffing. | Accidentally committing `client_secret` into public GitHub. | **ZERO:** No password or secret string exists anywhere in code. |
| **Primary Use Case** | Human engineers, admins, business users. | External CI/CD (GitHub Actions, Jenkins, Terraform Cloud). | **Azure-Native Workloads** (VMs, AKS, Functions, App Service). |

---

## 🛡️ 5. Advanced Security Governance: Conditional Access & PIM

### 1. Entra ID Licensing Tiers
- **Free:** Included with Azure subscription; user/group management, basic security defaults.
- **P1 (Premium 1):** Adds **Conditional Access**, dynamic user groups, and advanced hybrid synchronization.
- **P2 (Premium 2):** Adds **Privileged Identity Management (PIM)** and risk-based Identity Protection.

### 2. Context-Aware Conditional Access (P1 / P2)
Conditional Access acts as an intelligent firewall for identities:

```text
IF:
  User belongs to "Data Engineers"
  AND Application = "Azure Management Portal"
  AND Location = NOT "Corporate Headquarter Network"
THEN:
  Require Multi-Factor Authentication (MFA)
  AND Require Compliant Managed Device
```

### 3. Privileged Identity Management (PIM - P2 Tier)
PIM eliminates permanent standing administrative privileges:

```text
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                        JUST-IN-TIME (JIT) PRIVILEGED ACCESS                            │
│                                                                                        │
│   Normal Standing State:  Engineer ──► Permanent "Reader" Role (Cannot break anything) │
│                                                                                        │
│   Incident Occurs:        Engineer requests "Contributor" role for 4 hours             │
│                           ├── Submits ticket justification                             │
│                           ├── Requires approval from SecOps Team Lead                  │
│                           ├── Requires step-up MFA challenge                           │
│                           │                                                            │
│   Approved:               Role activates dynamically for exactly 4 hours               │
│   Post-Incident:          Role EXPIRES automatically; reverts back to Reader!          │
└────────────────────────────────────────────────────────────────────────────────────────┘
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Prev Module (01-Azure-Fundamentals-Management-Groups-and-Subscriptions)](../01-Azure-Fundamentals-Management-Groups-and-Subscriptions/12-Quick-Revision.md) | [Index](../../../README.md) | [02 - Entra ID Users Groups and Administrative Units →](./02-Entra-ID-Users-Groups-and-Administrative-Units.md) |
