# 🟧 AWS Services & Architecture Types — Kids-Mind Short Notes

> **Format:** Simple 1-line definitions, real-life analogies, and visual comparison tables. Covers every AWS service family and all internal types (EC2 instance types, EBS volume types, S3 storage tiers, ELB types, VPC gateway types, RDS engines, and IAM policies).

---

## 🧭 1. EC2 Instance Families & Types Explained Simply

An EC2 instance is a virtual server. AWS groups them into **Families** with specific letters, numbers, and suffixes.

```text
       ┌─── Family: "c" = Compute Optimized
       │ ┌─ Generation: "6" = 6th Gen hardware (newer = faster & cheaper)
       │ │ ┌─ Processor / Feature: "g" = AWS Graviton (ARM-based CPU)
       ▼ ▼ ▼
       c 6 g . 2 x l a r g e
               ▲
               └── Sizing multiplier (2xlarge = 8 vCPUs, 16 GB RAM)
```

### 🏷️ Suffix Decoder Table:
| Suffix Letter | Meaning | Why Choose It? |
| :---: | :--- | :--- |
| **`a`** | AMD EPYC Processor | ~10% cheaper than Intel instances for identical workloads. |
| **`g`** | AWS Graviton (ARM64) | Best price-to-performance ratio; up to 40% cheaper for Linux apps. |
| **`i`** | Intel Xeon Processor | Maximum compatibility with legacy x86-64 binary software. |
| **`n`** | Enhanced Networking | Up to 100 Gbps network bandwidth; great for high-packet apps. |
| **`d`** | Direct-attached NVMe SSD | Ultra-fast temporary local scratch disk (cleared on shutdown). |

---

### 💻 Master EC2 Instance Types Table:

| Instance Family | Series Letters | Kids-Mind 1-Line Definition | Real-Life Analogy | Best Use Cases |
| :--- | :---: | :--- | :--- | :--- |
| **General Purpose (Burstable)** | **`t3`, `t4g`** | Low-cost servers that save CPU credits when idle and spend them during quick traffic bursts. | A hybrid car that charges its battery while idling at a red light. | Small websites, dev/test servers, microservices, jump boxes. |
| **General Purpose (Balanced)** | **`m5`, `m6i`, `m7g`** | Standard balanced servers with an equal ratio of CPU cores and RAM (1 vCPU to 4 GB RAM). | A reliable family sedan; balanced for daily city driving. | Production web APIs, backend application servers, container worker nodes. |
| **Compute Optimized** | **`c5`, `c6i`, `c7g`** | High-clock-speed CPU servers with low RAM ratio (1 vCPU to 2 GB RAM) for heavy math calculations. | A Formula 1 race car; built strictly for raw speed and engine power. | Video encoding, batch data crunching, gaming servers, machine learning inference. |
| **Memory Optimized** | **`r5`, `r6i`, `r7g`, `x2gd`** | Massive RAM servers (1 vCPU to 8 GB RAM) designed to keep huge datasets in memory. | A giant warehouse with endless storage racks for instant retrieval. | In-memory caches (Redis, Memcached), high-load production databases, SAP HANA. |
| **Storage / I/O Optimized** | **`i3`, `i4i`, `d2`, `d3`** | Servers with tens of thousands of local NVMe SSD IOPS or petabytes of dense HDD storage. | A bullet train cargo carrier designed to move massive physical freight fast. | NoSQL databases (Cassandra, MongoDB), Elasticsearch, data warehousing. |
| **Accelerated / GPU** | **`g4`, `g5`, `p4`, `p5`** | Workstations loaded with high-end NVIDIA GPUs for AI training and 3D graphics rendering. | A Hollywood VFX production studio workstation. | Deep learning training (LLMs), stable diffusion, 3D computer animation. |

---

## 💽 2. EBS (Elastic Block Store) Volume Types

EBS is a virtual hard disk plugged into an EC2 server. AWS offers 5 major volume types:

```text
┌────────────────────────────────────────────────────────────────────────┐
│                        EBS VOLUME TYPE HIERARCHY                       │
│                                                                        │
│   ┌─────────────────────┐  ┌──────────────────┐  ┌──────────────────┐  │
│   │    GENERAL SSD      │  │ PROVISIONED SSD  │  │   MAGNETIC HDD   │  │
│   │    (gp2 / gp3)      │  │   (io1 / io2)    │  │   (st1 / sc1)    │  │
│   │                     │  │                  │  │                  │  │
│   │ • 3,000 IOPS base   │  │ • Up to 256k IOPS│  │ • Sequential I/O │  │
│   │ • OS boot volumes   │  │ • Critical DBs   │  │ • Big Data / Log │  │
│   │ • gp3 is 20% cheaper│  │ • Sub-ms latency │  │ • Cannot boot OS │  │
│   └─────────────────────┘  └──────────────────┘  └──────────────────┘  │
└────────────────────────────────────────────────────────────────────────┘
```

| Volume Type | API Name | Max IOPS | Max Throughput | Cost & Best Use Case |
| :--- | :---: | :---: | :---: | :--- |
| **General Purpose SSD (Latest)** | **`gp3`** | **16,000** | **1,000 MB/s** | **Default choice!** Baseline 3,000 IOPS & 125 MB/s free; customize IOPS without buying extra GBs. 20% cheaper than gp2. |
| **General Purpose SSD (Legacy)** | **`gp2`** | 16,000 | 250 MB/s | Old generation. IOPS tied directly to disk size (3 IOPS per GB). Upgrade to `gp3`! |
| **Provisioned IOPS SSD** | **`io2`** | **256,000** | **4,000 MB/s** | Sub-millisecond latency; 99.999% durability. Critical enterprise relational databases (Oracle, SAP, SQL Server). |
| **Throughput Optimized HDD** | **`st1`** | 500 | 500 MB/s | Low-cost magnetic disk for large sequential data streams (Kafka, Hadoop, data warehouses). Cannot boot OS. |
| **Cold HDD** | **`sc1`** | 250 | 250 MB/s | Lowest cost block storage for cold data that is rarely accessed. Cannot boot OS. |
| **Instance Store (Ephemeral)** | *Hardware* | **Millions** | **Hardware speed** | Physically inside the host server. **Temporary data only**; lost forever if instance stops! |

---

## 🪣 3. S3 Storage Classes (Tiers) & Cost Optimization

Amazon S3 is an object store (files + metadata). Choosing the right storage class saves up to 95% on cloud bills:

```text
┌──────────────────────────────────────────────────────────────────────────┐
│                   S3 STORAGE CLASS COST vs RETRIEVAL                     │
│                                                                          │
│   [S3 Standard] ──(30 days)──► [S3 Standard-IA] ──(90 days)──► [Glacier] │
│     $0.023/GB                    $0.0125/GB                   $0.0036/GB │
│   Frequent Access              Infrequent Access             Archival    │
│   Milliseconds                 Milliseconds                  Hours       │
└──────────────────────────────────────────────────────────────────────────┘
```

| Storage Class | Availability | Retrieval Time | Min Storage Duration | 1-Line Definition (Kids Mind) |
| :--- | :---: | :---: | :---: | :--- |
| **S3 Standard** | 99.99% | Milliseconds | None | Instant access for frequently accessed active websites, images, and user uploads. |
| **S3 Intelligent-Tiering** | 99.9% | Milliseconds | None | **Magic automated cost saver!** Moves files between frequent and infrequent tiers automatically with zero fee. |
| **S3 Standard-IA** | 99.9% | Milliseconds | 30 days | For data accessed once or twice a month (e.g., month-end financial reports, disaster recovery). |
| **S3 One Zone-IA** | 99.5% | Milliseconds | 30 days | Stored in only 1 AZ (20% cheaper). Good for reproducible data, secondary backup copies. |
| **S3 Glacier Instant Retrieval** | 99.9% | **Milliseconds** | 90 days | Archival data that is rarely accessed (once a quarter) but needed immediately when requested. |
| **S3 Glacier Flexible Retrieval** | 99.99% | **1 min to 5 hours** | 90 days | Deep archive for legal compliance and company records; retrieval takes up to a few hours. |
| **S3 Glacier Deep Archive** | 99.99% | **12 hours** | 180 days | **Cheapest storage on Earth** (~$0.00099/GB/month). Long-term compliance (7-year healthcare/tax records). |

---

## ⚖️ 4. Elastic Load Balancer (ELB) Types

AWS provides 4 types of managed load balancers to distribute user traffic:

```text
┌──────────────────────────────────────────────────────────────────────────┐
│                         ELB LOAD BALANCER TYPES                          │
│                                                                          │
│    ┌──────────────────┐    ┌──────────────────┐    ┌─────────────────┐   │
│    │    ALB (L7)      │    │    NLB (L4)      │    │    GLB (L3)     │   │
│    │  (HTTP/HTTPS)    │    │   (TCP/UDP)      │    │  (IP Packets)   │   │
│    │                  │    │                  │    │                 │   │
│    │ • Path routing   │    │ • Millions req/s │    │ • 3rd party     │   │
│    │ • SSL Offloading │    │ • Ultra-low ms   │    │   Firewalls     │   │
│    │ • Target: ECS/K8s│    │ • Static EIP     │    │ • IDS / IPS     │   │
│    └──────────────────┘    └──────────────────┘    └─────────────────┘   │
└──────────────────────────────────────────────────────────────────────────┘
```

| Type | Layer | Protocols | Static IP? | 1-Line Definition (Kids Mind) | Best Use Case |
| :--- | :---: | :---: | :---: | :--- | :--- |
| **ALB (Application Load Balancer)** | **Layer 7** (Application) | HTTP, HTTPS, gRPC, WebSocket | ❌ No (DNS name only) | Smart traffic cop that inspects HTTP headers, hostnames, and URL paths (`/api` vs `/login`). | Web applications, microservices, container workloads (ECS / EKS). |
| **NLB (Network Load Balancer)** | **Layer 4** (Transport) | TCP, UDP, TLS | ✅ Yes (Elastic IPs per AZ) | Ultra-fast highway interchange routing millions of raw network packets per second at ultra-low latency. | Real-time gaming, financial trading, VoIP, high-throughput TCP streaming. |
| **GLB (Gateway Load Balancer)** | **Layer 3** (Network) | IP packets (GENEVE) | ❌ Transparent | Security inspection checkpoint routing entire IP traffic streams to fleets of 3rd-party firewalls. | Palo Alto, Check Point, Fortinet virtual firewall appliances and IDS/IPS inspection. |
| **CLB (Classic Load Balancer)** | Layer 4 / 7 | HTTP, HTTPS, TCP | ❌ No | **Legacy 1st-generation load balancer.** Deprecated for new designs. | Legacy EC2-Classic systems only. |

---

## 🚪 5. AWS VPC Gateway & Networking Types

Connecting your isolated VPC to other networks requires specific gateways:

| Gateway / Connection Type | 1-Line Definition (Kids Mind) | Real-Life Analogy | Inbound or Outbound? |
| :--- | :--- | :--- | :---: |
| **Internet Gateway (IGW)** | The bidirectional two-way front door connecting public subnets to the public internet. | The front door of your house opening to the public street. | Both (In + Out) |
| **NAT Gateway** | Managed AWS gateway letting private servers reach internet for updates while blocking incoming traffic. | A one-way revolving door: you can walk out, but strangers cannot walk in. | Outbound Only |
| **VPC Peering** | Direct fiber link connecting two VPCs using private IPs; no internet involved. | A private enclosed footbridge connecting two neighboring office towers. | Both (Direct) |
| **AWS Transit Gateway (TGW)** | A central cloud hub router connecting hundreds of VPCs and on-premises data centers together. | A major international airport connecting 50 regional spoke airports. | Central Hub |
| **VPC Gateway Endpoint** | Free private connection directly from your VPC route table to **S3** and **DynamoDB** without touching internet! | A private delivery tunnel under your basement directly into Amazon's store. | Outbound to S3/DDB |
| **VPC Interface Endpoint (PrivateLink)**| An elastic network interface (ENI) with a private IP used to access other AWS services or vendor SaaS. | A private dedicated phone line directly to the vendor's office. | Inbound/Outbound |
| **Virtual Private Gateway (VGW)** | The VPC side endpoint for creating an encrypted IPSec VPN tunnel back to your office. | An encrypted walkie-talkie connecting your satellite office to headquarters. | Encrypted VPN |
| **AWS Direct Connect (DX)** | A dedicated physical fiber optic cable running from your company data center straight into AWS. | A dedicated private bullet train track between your building and AWS. | High-speed dedicated |

---

## 🗄️ 6. AWS Database Types (Relational vs NoSQL)

| Database Service | Type | Storage Engine | Scalability | When to Use |
| :--- | :---: | :--- | :--- | :--- |
| **Amazon RDS** | Relational (SQL) | MySQL, PostgreSQL, MariaDB, Oracle, SQL Server | Vertical compute scaling; automated read replicas. | Standard relational apps requiring complex joins, transactions (ACID), and schema consistency. |
| **Amazon Aurora** | Cloud-Native Relational | Compatible with MySQL & PostgreSQL | **6x replication across 3 AZs;** Auto-scales up to 128 TB disk; Aurora Serverless v2 scales CPU in 0.5-sec increments. | High-throughput enterprise production systems; 3x to 5x performance of standard RDS. |
| **Amazon DynamoDB** | NoSQL Key-Value & Document | Proprietary distributed engine | Single-digit millisecond latency at **ANY scale;** infinite auto-scaling; global tables. | High-volume shopping carts, gaming leaderboards, session stores, user profiles. |
| **Amazon DocumentDB** | NoSQL Document | MongoDB compatible | Clustered storage backed by Aurora architecture. | Migrating MongoDB workloads to fully managed AWS infrastructure. |
| **Amazon ElastiCache** | In-Memory Data Store | Redis or Memcached | Sub-millisecond response times in RAM. | Caching SQL database queries, session tokens, real-time counters. |
| **Amazon Redshift** | Columnar Data Warehouse | PostgreSQL-based OLAP | Petabyte scale; parallel SQL querying. | Business intelligence (BI), analytics, big data reporting dashboards. |

---

## 🔐 7. AWS IAM Policy Types

IAM policies define permissions. AWS supports 6 distinct types:

| Policy Type | Attached To | 1-Line Definition (Kids Mind) |
| :--- | :--- | :--- |
| **Identity-Based Policy** | IAM User, Group, or Role | Specifies what the identity can do (e.g., "Developer Alice can read S3 bucket X"). |
| **Resource-Based Policy** | S3 Bucket, SQS Queue, KMS Key | Attached directly to the resource; specifies who can touch it (e.g., S3 Bucket Policy). |
| **Permission Boundary** | IAM User or Role | An advanced guardrail that sets the **maximum possible permissions** an identity can ever hold. |
| **Service Control Policy (SCP)** | AWS Organizations OU or Account | Organization-wide master rule; limits maximum permissions for entire AWS accounts (even root!). |
| **Session Policy** | Temporary STS Session | Passed dynamically when assuming a role via AWS CLI/SDK to further restrict credentials. |
| **Access Control List (ACL)** | S3 Object or Bucket (Legacy) | Legacy cross-account permission list; AWS recommends keeping disabled in favor of bucket policies. |

---

## 📦 8. Containers & Serverless Types (ECS vs EKS vs Lambda)

```text
┌────────────────────────────────────────────────────────────────────────┐
│                        AWS CONTAINER ECOSYSTEM                         │
│                                                                        │
│   ORCHESTRATOR:    [Amazon ECS] (AWS Native)   [Amazon EKS] (Kubernetes) │
│                          │                             │               │
│                          ▼                             ▼               │
│   COMPUTE ENGINE:  [EC2 Instances]   OR   [AWS Fargate] (Serverless)   │
│                    (You manage OS)        (Zero server management)     │
└────────────────────────────────────────────────────────────────────────┘
```

- **ECS (Elastic Container Service):** Simple, AWS-native container orchestrator. Fast, lightweight, and deeply integrated with AWS ALB and IAM roles.
- **EKS (Elastic Kubernetes Service):** Industry-standard open-source Kubernetes managed by AWS. Best for multi-cloud setups and teams already using Helm.
- **Launch Types:**
  - **EC2 Launch Type:** You own and manage the worker VMs (OS patching, Docker updates, AMI hardening).
  - **AWS Fargate Launch Type:** **Serverless container compute.** You specify CPU and RAM per container; AWS runs them without exposing any underlying VMs!
- **AWS Lambda Packaging Types:**
  - **Zip Archive:** Code bundle up to 250 MB uncompressed; standard for Python, Node.js, Go.
  - **Container Image:** Docker container up to 10 GB size; great for heavy dependencies like PyTorch, Pandas, and OpenCV.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Terraform & Ansible (IaC) Short Notes](./08-Terraform-and-Ansible-IaC-Short-Notes.md) | [Index](../README.md) | [Azure Services & Types Short Notes →](./10-Azure-Services-and-Types-Deep-Dive-Short-Notes.md) |
