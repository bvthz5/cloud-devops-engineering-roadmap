# ☁️ Cloud Computing (AWS, Azure, GCP) — Complete Kids-Mind Short Notes

> **Format:** Simple 1-line definitions, advantages, real-life analogies, and command breakdown. Covers every core cloud concept, compute, storage, networking (VPC), IAM, and FinOps across AWS, Azure, and GCP.

---

## 🧠 1. Cloud Core Fundamentals (1-Line Definitions)

| Concept | Simple 1-Line Definition (Kids Mind) | Real-Life Analogy | Main Advantage |
| :--- | :--- | :--- | :--- |
| **Cloud Computing** | Renting computers, storage, and networks over the internet from big tech companies instead of buying physical hardware. | Renting power from an electric company instead of running a generator in your basement. | No expensive upfront hardware costs; pay only for what you use. |
| **IaaS (Infrastructure as a Service)** | Renting raw virtual hardware (VMs, hard drives, networks); you install and manage the operating system and apps. | Renting an empty apartment unfurnished; you bring your own furniture. | Maximum control over operating system, kernel, and software stack. |
| **PaaS (Platform as a Service)** | A managed platform where the cloud provider manages the OS and runtime; you just upload your code. | Renting a fully furnished hotel room; you just bring your clothes. | Zero OS patching or server maintenance; focus 100% on writing application code. |
| **SaaS (Software as a Service)** | A complete finished software application accessed directly via a web browser. | Eating at a restaurant; food is cooked, served, and cleaned for you. | Zero code, zero servers, zero maintenance (e.g., Gmail, Slack, Office 365). |
| **Public Cloud** | Cloud resources owned by a vendor (AWS/Azure/GCP) and shared securely among millions of customers. | Taking a public passenger subway train. | Infinite global scale, low cost, zero hardware to maintain. |
| **Private Cloud** | Cloud infrastructure built exclusively for one single enterprise inside their private data center. | Driving your own personal private car. | Absolute data control, regulatory compliance, total privacy. |
| **Hybrid Cloud** | Connecting an on-premises private data center directly to the public cloud with a private VPN or fiber link. | Driving your car to the train station and taking the train into the city. | Keep sensitive financial/healthcare data on-premises, burst web traffic to cloud. |
| **Region** | A physical geographical area around the world containing multiple isolated data center buildings. | A major metropolitan city (e.g., North Virginia, Frankfurt, Tokyo). | Place servers close to your end users to minimize latency. |
| **Availability Zone (AZ)** | One or more discrete, physical data center facilities within a Region with redundant power, networking, and cooling. | Independent physical buildings across town connected by high-speed fiber cables. | If one data center floods or loses power, your app runs uninterrupted in another AZ. |
| **Edge Location / PoP** | Mini data centers situated in hundreds of cities worldwide to cache website content close to visitors. | Local neighborhood vending machines stocking snacks. | Lightning-fast website and video loading speeds worldwide via CDNs. |

---

## 🗺️ 2. The Big 3 Cloud Providers Mapping Table

| Cloud Capability | Amazon Web Services (AWS) | Microsoft Azure | Google Cloud Platform (GCP) | Simple 1-Line Purpose |
| :--- | :--- | :--- | :--- | :--- |
| **Virtual Machines** | **EC2** (Elastic Compute Cloud) | **Azure Virtual Machines** | **Compute Engine** | Rent virtual CPU and RAM to run Linux or Windows. |
| **Serverless Functions**| **AWS Lambda** | **Azure Functions** | **Cloud Functions** | Run snippets of code on demand without running any servers. |
| **Managed Kubernetes** | **EKS** (Elastic K8s Service) | **AKS** (Azure K8s Service) | **GKE** (Google K8s Engine) | Fully managed production-grade Kubernetes control plane. |
| **Object Storage** | **Amazon S3** | **Azure Blob Storage** | **Google Cloud Storage** | Infinite bucket to store images, videos, backups, and logs. |
| **Block Storage (VM Disk)**| **Amazon EBS** | **Azure Managed Disks** | **Persistent Disk** | Virtual SSD/HDD hard drive plugged directly into a VM. |
| **Shared File System** | **Amazon EFS** | **Azure Files** | **Cloud Filestore** | Shared network folder (NFS/SMB) accessible by 1,000 servers. |
| **Relational Database** | **Amazon RDS / Aurora** | **Azure SQL Database** | **Cloud SQL / Spanner** | Automated MySQL, Postgres, and SQL Server with auto-backups. |
| **NoSQL Database** | **DynamoDB** | **Cosmos DB** | **Cloud Firestore / Bigtable** | Single-millisecond key-value and document store at scale. |
| **Virtual Private Network**| **Amazon VPC** | **Azure Virtual Network (VNet)**| **Google Cloud VPC** | Your private, isolated software-defined network in the cloud. |
| **DNS Service** | **Route 53** | **Azure DNS** | **Cloud DNS** | Highly available domain name routing and health checks. |
| **Identity & Access** | **AWS IAM** | **Microsoft Entra ID (Azure AD)**| **Cloud IAM** | Manages user accounts, service credentials, and permissions. |
| **Cloud Monitoring** | **Amazon CloudWatch** | **Azure Monitor** | **Google Cloud Monitoring** | Collects metrics, CPU alarms, and application log streams. |
| **Audit Trails** | **AWS CloudTrail** | **Azure Activity Log** | **Cloud Audit Logs** | Records every single API call: who deleted what and when. |

---

> 💡 **Dedicated Cloud Architecture Deep Dives:** For comprehensive, dedicated breakdowns of all EC2/EBS/S3/ELB types and Azure VM/Disks/Blob/Networking types, explore [Guide 09: AWS Types Deep Dive](./09-AWS-Services-and-Types-Deep-Dive-Short-Notes.md) and [Guide 10: Azure Types Deep Dive](./10-Azure-Services-and-Types-Deep-Dive-Short-Notes.md).

## 💻 3. Compute Services: Virtual Machines & Serverless

### Virtual Machines (AWS EC2 / Azure VM / GCP Compute Engine)
- **1-Line Definition:** A simulated physical computer running in the cloud with custom CPU, RAM, and disk configuration.
- **Instance Families (AWS Examples):**
  - **`t3`, `t4g` (Burstable General Purpose):** Inexpensive; ideal for web dev, testing, and small microservices.
  - **`m6i`, `m7g` (Balanced General Purpose):** Equal balance of CPU and memory for production workloads.
  - **`c6i`, `c7g` (Compute Optimized):** High CPU clock speeds for batch processing, video encoding, and gaming servers.
  - **`r6i`, `r7g` (Memory Optimized):** Massive RAM allocations for in-memory databases (Redis, Memcached) and data warehouses.
  - **`p4`, `g5` (GPU Accelerated):** Dedicated NVIDIA GPUs for Machine Learning, AI training, and graphics rendering.
- **AMI (Amazon Machine Image):** A pre-configured operating system template (Ubuntu, Amazon Linux, Windows Server) used to boot an EC2 instance.
- **User Data:** A bash script that executes automatically with root privileges during the very first boot of a new VM (used to install Docker or clone Git repos).
- **Inspect Instances via CLI (`aws ec2 describe-instances`):**
  - What details you get: `InstanceId` (e.g., `i-0a1b2c3d4e`), `State: running`, `PublicIpAddress: 54.210.12.8`, `PrivateIpAddress: 10.0.1.25`, `InstanceType: t3.micro`.

---

### Serverless Compute (AWS Lambda / Azure Functions / Cloud Functions)
- **1-Line Definition:** An execution model where you upload application code and the cloud provider provisions, runs, and scales the containers on demand.
- **Advantage:** Pay strictly for the exact milliseconds your code runs; scales to zero when idle ($0 cost).
- **Cold Start:** The brief delay ($100–800\text{ ms}$) on the very first request while the cloud provider spins up a brand-new container runtime for your code.
- **Key Limitations:** Maximum execution timeout (15 minutes for AWS Lambda); stateless (cannot save local files permanently).
- **Event Triggers:** Lambda runs automatically when an image is uploaded to S3, an API Gateway receives an HTTP call, or a message lands in an SQS queue.

---

## 🗄️ 4. Cloud Storage Breakdown (S3 vs EBS vs EFS)

```text
┌────────────────────────────────────────────────────────────────────────┐
│                        CLOUD STORAGE TYPES                             │
│                                                                        │
│   ┌────────────────────┐  ┌────────────────────┐  ┌─────────────────┐  │
│   │   OBJECT STORAGE   │  │   BLOCK STORAGE    │  │  FILE STORAGE   │  │
│   │     (Amazon S3)    │  │    (Amazon EBS)    │  │  (Amazon EFS)   │  │
│   │                    │  │                    │  │                 │  │
│   │ • Infinite size    │  │ • Fast local drive │  │ • Shared drive  │  │
│   │ • REST API access  │  │ • Locked to 1 VM   │  │ • 1,000 VMs     │  │
│   │ • Static web/media │  │ • OS & Databases   │  │ • WordPress/NFS │  │
│   └────────────────────┘  └────────────────────┘  └─────────────────┘  │
└────────────────────────────────────────────────────────────────────────┘
```

| Feature | Object Storage (AWS S3) | Block Storage (AWS EBS) | File Storage (AWS EFS) |
| :--- | :--- | :--- | :--- |
| **Simple 1-Line Definition** | An infinite online filing cabinet storing unformatted files via REST APIs. | A virtual hard drive plugged directly into a single VM's motherboard. | A shared network folder mounted simultaneously by hundreds of servers. |
| **Real-Life Analogy** | Storing files in Google Drive or Dropbox. | Plugging an external USB SSD into your laptop. | A shared network office file server. |
| **Protocol / Access** | HTTP / HTTPS (REST API) | Raw block storage (NVMe / SATA) | NFSv4 / SMB network protocol |
| **Max Capacity** | Practically Infinite (5TB max per single object) | Up to 64 TB per volume | Elastic; grows and shrinks automatically |
| **Concurrent Access** | Millions of clients across the entire world | Single EC2 instance (Multi-Attach limited) | Thousands of EC2 instances simultaneously |
| **Common Use Cases** | Photos, videos, HTML assets, data lake files | Boot drive (`/`), database files (`/var/lib/mysql`)| Shared content folders, WordPress uploads |

### S3 Storage Tiers & Cost Optimization:
- **S3 Standard:** Frequent access, lowest latency, high availability (99.999999999% 11 9's durability).
- **S3 Standard-IA (Infrequent Access):** Cheaper storage, but charges a small retrieval fee per GB.
- **S3 Glacier Flexible Retrieval:** Deep archival storage for compliance; retrieval takes 1–5 hours; ultra-low cost ($0.004/GB/month).
- **S3 Glacier Deep Archive:** The cheapest cloud storage on Earth ($0.00099/GB/month); retrieval takes 12 hours.

---

## 🌐 5. Cloud Networking & Virtual Private Cloud (VPC)

A VPC is your own isolated, private data center inside the cloud provider's network.

```text
┌──────────────────────────────────────────────────────────────────────────┐
│                   VIRTUAL PRIVATE CLOUD (10.0.0.0/16)                    │
│                                                                          │
│  ┌───────────────────────┐                    ┌───────────────────────┐  │
│  │     PUBLIC SUBNET     │                    │    PRIVATE SUBNET     │  │
│  │     (10.0.1.0/24)     │                    │     (10.0.2.0/24)     │  │
│  │                       │                    │                       │  │
│  │   ┌───────────────┐   │                    │   ┌───────────────┐   │  │
│  │   │  Web / ALB    │   │                    │   │  Database     │   │  │
│  │   │ (Public IP)   │   │                    │   │ (No Public IP)│   │  │
│  │   └───────┬───────┘   │                    │   └───────▲───────┘   │  │
│  │           │           │                    │           │           │  │
│  │   ┌───────▼───────┐   │                    │           │           │  │
│  │   │  NAT Gateway  │───┼────────────────────┼───────────┘           │  │
│  │   └───────┬───────┘   │ (Outbound Updates) │                       │  │
│  └───────────┼───────────┘                    └───────────────────────┘  │
│              ▼                                                           │
│     [Internet Gateway] (IGW)                                             │
└──────────────┬───────────────────────────────────────────────────────────┘
               ▼
        PUBLIC INTERNET
```

### Core VPC Components (1-Line Definitions):
- **VPC (Virtual Private Cloud):** An isolated virtual network dedicated to your cloud account with a custom IP range (e.g., `10.0.0.0/16`).
- **Subnet:** A smaller slice of a VPC's IP range tied to a specific Availability Zone.
- **Public Subnet:** A subnet whose Route Table sends `0.0.0.0/0` traffic directly to an **Internet Gateway (IGW)**; servers can have public IPs.
- **Private Subnet:** A subnet with NO direct route to an Internet Gateway; servers have no public IPs and cannot be directly reached from the internet.
- **Internet Gateway (IGW):** The bidirectional front door connecting your public subnet to the internet.
- **NAT Gateway (Network Address Translation):** A managed gateway in the public subnet that lets private servers download security patches from the internet while blocking anyone outside from initiating a connection in!
- **Route Table:** A set of routing rules directing network packets from subnets to the IGW, NAT Gateway, or VPC peering connections.
- **Security Group (Instance Firewall):** A **stateful** virtual firewall attached directly to a VM's virtual network card; controls inbound and outbound port traffic.
- **Network ACL (NACL - Subnet Firewall):** A **stateless** subnet boundary firewall evaluated before traffic touches Security Groups; requires explicit inbound and outbound rule pairs.

### Stateful (Security Group) vs Stateless (NACL):
- **Stateful (Security Group):** If you allow inbound traffic on port 80, the return outbound traffic is **automatically allowed**, regardless of outbound rules!
- **Stateless (NACL):** Return traffic must be explicitly allowed in the outbound rules table (ephemeral ports $1024–65535$).

---

### Load Balancers: ALB vs NLB
- **Application Load Balancer (ALB - Layer 7):**
  - **1-Line Definition:** An intelligent HTTP/HTTPS load balancer that routes traffic based on URL paths (`/api` vs `/static`) or host headers (`api.app.com`).
  - **Features:** Terminates SSL/TLS certificates, supports WebSocket, integrates natively with ECS and EKS.
- **Network Load Balancer (NLB - Layer 4):**
  - **1-Line Definition:** An ultra-fast, ultra-low-latency load balancer operating at the TCP/UDP transport layer.
  - **Features:** Capable of handling millions of requests per second; provides static Elastic IP addresses.

---

## 🔐 6. Identity & Access Management (IAM)

IAM controls WHO can access WHAT resources in your cloud account.

```text
┌──────────────────────────────────────────────────────────────────────────┐
│                             IAM ARCHITECTURE                             │
│                                                                          │
│   ┌──────────────┐         ┌──────────────┐         ┌────────────────┐   │
│   │   IAM USER   │────────►│  IAM GROUP   │────────►│   IAM POLICY   │   │
│   │ (Human dev)  │         │ (Developers) │         │ (Permissions)  │   │
│   └──────────────┘         └──────────────┘         └────────────────┘   │
│                                                             ▲            │
│   ┌──────────────┐                                          │            │
│   │   IAM ROLE   │──────────────────────────────────────────┘            │
│   │ (EC2/Lambda) │   (Temporary STS Tokens - Zero Stored Passwords!)     │
│   └──────────────┘                                                       │
└──────────────────────────────────────────────────────────────────────────┘
```

### Core IAM Concepts (1-Line Definitions):
- **Root Account:** The master email account that created the cloud subscription; has god-mode privileges and must be locked away with MFA!
- **IAM User:** A permanent credential identity created for an individual human engineer (username + password + console access).
- **IAM Group:** A collection of IAM users (e.g., `DevOps-Admins`, `Junior-Devs`) allowing you to attach permissions to many people at once.
- **IAM Role:** A temporary credential identity assumed by machines (EC2, Lambda) or federated users; uses temporary rotating STS tokens (zero hardcoded secrets!).
- **IAM Policy:** A JSON document explicitly stating what actions are allowed or denied on specific cloud resources.
- **Principle of Least Privilege:** Security golden rule: grant ONLY the exact minimum permissions an employee or application needs to do its job.

### Anatomy of an IAM Policy JSON:
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowS3ReadWrite",
      "Effect": "Allow",
      "Action": [
        "s3:GetObject",
        "s3:PutObject"
      ],
      "Resource": "arn:aws:s3:::my-production-app-bucket/*"
    },
    {
      "Sid": "ExplicitDenyDelete",
      "Effect": "Deny",
      "Action": "s3:DeleteObject",
      "Resource": "arn:aws:s3:::my-production-app-bucket/*"
    }
  ]
}
```
- **Effect:** `Allow` or `Deny` (An explicit `Deny` always trumps an `Allow`).
- **Action:** The API calls permitted (e.g., `s3:GetObject`, `ec2:StartInstances`).
- **Resource:** The ARN (Amazon Resource Name) identifying the target resource.

---

## 📊 7. Monitoring, Auditing & Observability

| Tool | Cloud Provider | Simple 1-Line Definition | What It Tracks |
| :--- | :--- | :--- | :--- |
| **CloudWatch Metrics** | AWS | Collects numeric time-series data from servers. | CPU %, Disk I/O, Network packets, 5XX HTTP errors. |
| **CloudWatch Alarms** | AWS | Triggers automated actions if a metric breaches a threshold. | Sends Slack alert if CPU $> 85\%$ for 5 minutes. |
| **CloudWatch Logs** | AWS | Centralized log ingestion and search engine. | Application stdout, container logs, Linux syslog. |
| **CloudTrail** | AWS | The security governance surveillance camera recording every API call. | Who deleted the production database, at what time, from what IP! |
| **Azure Monitor** | Azure | Unified telemetry dashboard for Azure infrastructure. | VM health, Application Insights, Log Analytics queries. |
| **Cloud Operations** | GCP | Integrated logging and monitoring suite for Google Cloud. | GKE container logs, latency traces, uptime checks. |

---

## 💰 8. Cloud Financial Management (FinOps) & Pricing Models

| Purchasing Model | Discount vs On-Demand | Commitment | Best Use Case |
| :--- | :---: | :---: | :--- |
| **On-Demand Instances** | **$0\%$ (Baseline)** | None (Pay per second) | Short-term spikes, unpredictable experiments, new app development. |
| **Savings Plans** | **Up to $72\%$** | 1 or 3 Years (Commit to \$/hour spend) | Steady-state production workloads with flexible instance families. |
| **Reserved Instances (RI)** | **Up to $72\%$** | 1 or 3 Years (Commit to specific instance) | Permanent 24/7 databases and core backend infrastructure. |
| **Spot Instances** | **Up to $90\%$** | None (Cloud can reclaim with 2-min warning) | Fault-tolerant batch processing, CI/CD runners, rendering, stateless worker pods. |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Kubernetes (K8s) Short Notes](./05-Kubernetes-K8s-Short-Notes.md) | [Index](../README.md) | [Git & CI/CD Short Notes →](./07-Git-and-CICD-Short-Notes.md) |
