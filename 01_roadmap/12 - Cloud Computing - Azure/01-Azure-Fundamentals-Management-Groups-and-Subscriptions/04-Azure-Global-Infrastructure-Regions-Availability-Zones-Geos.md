# 04 - Azure Global Infrastructure: History, Architecture, 60+ Regions & Global Datacenters Directory

> **Focus:** The complete architectural history of Microsoft Azure, physical datacenter engineering, global WAN fiber backbone, subsea cables, Availability Zone internals, regional pairs, and an exhaustive directory of 60+ regions and 300+ datacenters worldwide.

---

## 📜 1. History & Evolution of Microsoft Azure and Its Infrastructure

Understanding the origins and engineering evolution of Microsoft Azure explains why its global infrastructure is architected the way it is today:

```text
┌────────────────────────────────────────────────────────────────────────────────────────────────────────┐
│                                   CHRONOLOGICAL EVOLUTION OF AZURE                                      │
├────────────────────────────────────────────────────────────────────────────────────────────────────────┤
│ 2008: "Project Red Dog" announced by Ray Ozzie & Amitabh Srivastava at PDC in Los Angeles.           │
│       Kernel architect Dave Cutler adapts the Windows NT kernel to build the cloud hypervisor.         │
│                                                   ▼                                                    │
│ 2010: Windows Azure officially launches GA (February 1, 2010) as a PaaS platform with Web & Worker    │
│       roles, Cloud Services, and Azure Storage.                                                        │
│                                                   ▼                                                    │
│ 2012: IaaS Virtual Machines introduced on Azure Service Management (ASM / "Classic" control plane).     │
│                                                   ▼                                                    │
│ 2014: Rebranded from "Windows Azure" to "Microsoft Azure" under Satya Nadella (March 25, 2014).          │
│       Azure Resource Manager (ARM) introduced, deprecating classic XML-based management.               │
│                                                   ▼                                                    │
│ 2017: Project Olympus open-source datacenter hardware announced with Open Compute Project (OCP).       │
│       Launch of Availability Zones (AZs) for high availability across physically isolated sites.       │
│                                                   ▼                                                    │
│ 2018: Project Natick: 864 underwater datacenter servers submerged off Scotland for 2 years with 8x    │
│       lower failure rate than land-based datacenters.                                                  │
│                                                   ▼                                                    │
│ 2020: Azure Arc launched, extending Azure management to on-premises, AWS, and GCP environments.        │
│                                                   ▼                                                    │
│ 2023-2026: Massive AI Supercomputing expansion; hundreds of thousands of NVIDIA H100/H200/B200 GPUs    │
│       interconnected with 3.2 Tbps Quantum-2 InfiniBand for OpenAI ChatGPT and Microsoft Copilot;      │
│       Deployment of custom Microsoft Azure Maia 100 AI chips and Cobalt 100 ARM CPUs.                  │
└────────────────────────────────────────────────────────────────────────────────────────────────────────┘
```

### 🎯 Key Architectural Eras:
1. **The Classic Era (2008–2014) — Azure Service Management (ASM):** Resources were tightly coupled inside cloud services, managed through XML configuration files and monolithic management certificates. Each service was an isolated silo without fine-grained RBAC.
2. **The ARM Era (2014–Present) — Azure Resource Manager:** Declarative JSON/Bicep deployments, unified RBAC, tagging, Resource Groups, and idempotent state management.
3. **The Hardware Acceleration Era (2017–Present) — Project Cerberus & Azure Boost:** Offloading virtualization overhead (networking, storage IOPS) from host CPUs onto custom SmartNICs and System-on-Chip (SoC) hardware, delivering sub-millisecond network latency and line-rate throughput.

---

## 🏗️ 2. Physical Datacenter Architecture & Engineering

Microsoft designs and operates some of the most advanced hyperscale datacenters in the world:

```text
┌─────────────────────────────────────────────────────────────────────────────────────────┐
│                               HYPERSCALE DATACENTER CAMPUS                              │
│                                                                                         │
│   ┌─────────────────────────────────────────────────────────────────────────────────┐   │
│   │ POWER & COOLING INFRASTRUCTURE                                                  │   │
│   │ • Dual independent utility power substations + Diesel/HVO Biofuel Generators    │   │
│   │ • Uninterruptible Power Supply (UPS) battery banks (Lithium-ion flywheels)      │   │
│   │ • Adiabatic evaporative cooling and closed-loop direct-to-chip liquid cooling   │   │
│   └─────────────────────────────────────────────────────────────────────────────────┘   │
│                                           │                                             │
│                                           ▼                                             │
│   ┌─────────────────────────────────────────────────────────────────────────────────┐   │
│   │ PHYSICAL SERVER RACKS (Project Olympus & Project Cerberus)                      │   │
│   │ • 42U/48U Open Compute Project (OCP) standard server chassis                    │   │
│   │ • Custom Microsoft Azure Cobalt 100 ARM CPUs & AMD EPYC / Intel Xeon processors │   │
│   │ • NVIDIA HGX H100/H200 & Microsoft Maia 100 AI acceleration clusters           │   │
│   │ • Azure Boost SmartNICs providing hardware-accelerated SDN & NVMe-over-Fabrics  │   │
│   └─────────────────────────────────────────────────────────────────────────────────┘   │
│                                           │                                             │
│                                           ▼                                             │
│   ┌─────────────────────────────────────────────────────────────────────────────────┐   │
│   │ OPTICAL NETWORK MESH (Spine-Leaf Clos Architecture)                             │   │
│   │ • 400 Gbps & 800 Gbps optical transceiver interconnects                         │   │
│   │ • SONiC (Software for Open Networking in the Cloud) Linux network switch OS    │   │
│   └─────────────────────────────────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────────────────────────────────┘
```

### 🔬 Groundbreaking Innovations:
- **Project Natick (Underwater Datacenters):** Microsoft proved that sealing server racks in nitrogen-filled waterproof capsules underwater reduces hardware failure by 800% due to no oxygen corrosion, stable oceanic temperatures, and zero human interference.
- **Project Silica:** Archival storage etching data into quartz glass plates using femtosecond lasers, surviving for 10,000+ years impervious to electromagnetic pulses (EMP), heat, and water.
- **Project Cerberus:** A NIST 800-193 compliant cryptographic microcontroller soldered directly onto motherboards, enforcing hardware root-of-trust and preventing firmware tampering before boot.
- **100% Carbon-Neutral & Renewable Commitment:** Microsoft aims to be carbon negative by 2030 and remove all historical carbon emissions since its founding (1975) by 2050.

---

## 🌐 3. Global Network Backbone & Submarine Fiber Cables

Microsoft operates one of the **top 3 largest private fiber-optic networks on Earth**:
- **175,000+ Miles (280,000+ km)** of terrestrial dark fiber and subsea cables.
- **200+ Edge Points of Presence (PoPs)** delivering Azure Front Door, Azure CDN, and Microsoft 365.
- **Anycast BGP Routing:** Traffic enters Microsoft's private network at the nearest edge PoP to the user and stays entirely on Microsoft's high-speed fiber backbone, never traversing the public internet.

```text
┌─────────────────────────────────────────────────────────────────────────────────────────┐
│                               MAJOR MICROSOFT SUBSEA CABLES                             │
├─────────────────────────────────────────────────────────────────────────────────────────┤
│ • MAREA: 4,104 miles across the Atlantic (Virginia Beach, USA ➔ Bilbao, Spain).         │
│   Delivers over 200 Terabits per second (Tbps) of bandwidth across 8 fiber pairs.       │
│ • DUNANT: 4,000 miles across the Atlantic (Virginia Beach, USA ➔ Saint-Hilaire, France).│
│   Uses space-division multiplexing (SDM) with 12 fiber pairs delivering 250 Tbps.       │
│ • AMITIE: Connecting Massachusetts, USA to Le Porge, France and Bude, UK (16 pairs).    │
│ • GRACE HOPPER: New York, USA ➔ Bude, UK and Bilbao, Spain.                             │
│ • HAVFRUE: New Jersey, USA ➔ Ireland, Denmark, and Norway.                              │
│ • JUPITER: Trans-Pacific cable connecting Oregon, USA ➔ Maruyama & Shima, Japan.        │
│ • PLCN: Pacific Light Cable Network connecting Los Angeles ➔ Taiwan and Philippines.    │
└─────────────────────────────────────────────────────────────────────────────────────────┘
```

---

## 🏢 4. Regions vs. Availability Zones Architecture

```text
       ┌────────────────────────────────────────────────────────────────────────┐
       │                        AZURE GEOGRAPHY (e.g., USA)                     │
       │                                                                        │
       │   ┌─────────────────────────────┐    ┌─────────────────────────────┐   │
       │   │   PRIMARY REGION            │    │   PAIRED REGION (300+ miles)│   │
       │   │   (e.g., East US)           │    │   (e.g., West US)           │   │
       │   │                             │    │                             │   │
       │   │  ┌───────┐ ┌───────┐ ┌───────┐   │  ┌───────┐ ┌───────┐ ┌───────┐   │   │
       │   │  │  AZ 1 │ │  AZ 2 │ │  AZ 3 │   │  │  AZ 1 │ │  AZ 2 │ │  AZ 3 │   │   │
       │   │  │  🏢   │ │  🏢   │ │  🏢   │   │  │  🏢   │ │  🏢   │ │  🏢   │   │   │
       │   │  └───────┘ └───────┘ └───────┘   │  └───────┘ └───────┘ └───────┘   │   │
       │   │     ▲         ▲         ▲    │   │                             │   │
       │   │     └─────────┴─────────┘    │   │                             │   │
       │   │    Sub-2ms roundtrip latency │   │                             │   │
       │   └───────────────┬─────────────┘   └──────────────┬──────────────┘   │
       │                   │                                │                  │
       │                   └─────── Synchronous GRS ────────┘                  │
       │                             Replication                               │
       └────────────────────────────────────────────────────────────────────────┘
```

### 🛡️ SLA Guarantees:
| Architecture | Configuration | Azure SLA Guarantee | Allowable Annual Downtime |
| :--- | :--- | :---: | :---: |
| **Single VM (Premium SSD/Ultra)** | No redundancy, standalone VM | **99.9%** | ~8 hours 45 minutes |
| **Availability Set** | Split across Fault Domains & Update Domains | **99.95%** | ~4 hours 22 minutes |
| **Availability Zones (AZs)** | Deployed across 2 or more distinct AZs | **99.99%** | ~52 minutes 35 seconds |
| **Multi-Region Cross-Zone** | Global Load Balancer (Front Door/Traffic Mgr) | **99.999% (5 Nines)** | ~5 minutes 15 seconds |

---

## 🗺️ 5. Exhaustive Directory of ALL 60+ Azure Regions Worldwide

Below is the definitive global directory of Azure regions, detailing their physical location, availability zone support, regional pairs, and regulatory compliance status:

### 🇺🇸 Americas (North America & Latin America)
| Region Display Name | Region Identifier | Physical Location | Availability Zones | Paired Region | Launch Year |
| :--- | :--- | :--- | :---: | :--- | :---: |
| **East US** | `eastus` | Boydton & Richmond, Virginia, USA | **3 AZs** | West US | 2010 |
| **East US 2** | `eastus2` | Boydton, Virginia, USA | **3 AZs** | Central US | 2014 |
| **Central US** | `centralus` | West Des Moines, Iowa, USA | **3 AZs** | East US 2 | 2010 |
| **North Central US** | `northcentralus` | Chicago, Illinois, USA | Non-zonal | South Central US | 2010 |
| **South Central US** | `southcentralus` | San Antonio, Texas, USA | **3 AZs** | North Central US | 2010 |
| **West US** | `westus` | San Jose & Santa Clara, California, USA | Non-zonal | East US | 2012 |
| **West US 2** | `westus2` | Quincy & Moses Lake, Washington, USA | **3 AZs** | West Central US | 2016 |
| **West US 3** | `westus3` | Phoenix & El Mirage, Arizona, USA | **3 AZs** | East US | 2021 |
| **West Central US** | `westcentralus` | Cheyenne, Wyoming, USA | Non-zonal | West US 2 | 2016 |
| **Canada Central** | `canadacentral` | Toronto, Ontario, Canada | **3 AZs** | Canada East | 2016 |
| **Canada East** | `canadaeast` | Quebec City, Quebec, Canada | Non-zonal | Canada Central | 2016 |
| **Brazil South** | `brazilsouth` | Campinas & São Paulo, Brazil | **3 AZs** | South Central US | 2014 |
| **Brazil Southeast** | `brazilsoutheast` | Rio de Janeiro, Brazil | Non-zonal | Brazil South | 2020 |
| **Mexico Central** | `mexicocentral` | Querétaro, Mexico | **3 AZs** | East US | 2024 |
| **Chile Central** | `chilecentral` | Santiago, Chile | **3 AZs** | Non-paired | Announced |

### 🇪🇺 Europe
| Region Display Name | Region Identifier | Physical Location | Availability Zones | Paired Region | Launch Year |
| :--- | :--- | :--- | :---: | :--- | :---: |
| **North Europe** | `northeurope` | Dublin, Ireland | **3 AZs** | West Europe | 2010 |
| **West Europe** | `westeurope` | Middenmeer & Amsterdam, Netherlands | **3 AZs** | North Europe | 2010 |
| **UK South** | `uksouth` | London & Slough, United Kingdom | **3 AZs** | UK West | 2016 |
| **UK West** | `ukwest` | Cardiff & Newport, Wales, UK | Non-zonal | UK South | 2016 |
| **France Central** | `francecentral` | Paris, France | **3 AZs** | France South | 2018 |
| **France South** | `francesouth` | Marseille, France | Non-zonal | France Central | 2018 |
| **Germany West Central**| `germanywestcentral`| Frankfurt, Germany | **3 AZs** | Germany North | 2019 |
| **Germany North** | `germanynorth` | Berlin, Germany | Non-zonal | Germany West Central | 2019 |
| **Switzerland North** | `switzerlandnorth` | Zurich, Switzerland | **3 AZs** | Switzerland West | 2019 |
| **Switzerland West** | `switzerlandwest` | Geneva, Switzerland | Non-zonal | Switzerland North | 2019 |
| **Norway East** | `norwayeast` | Oslo, Norway | **3 AZs** | Norway West | 2019 |
| **Norway West** | `norwaywest` | Stavanger, Norway | Non-zonal | Norway East | 2019 |
| **Sweden Central** | `swedencentral` | Gävle, Sandviken & Staffanstorp, Sweden | **3 AZs** | Non-paired | 2021 |
| **Poland Central** | `polandcentral` | Warsaw, Poland | **3 AZs** | Non-paired | 2023 |
| **Italy North** | `italynorth` | Milan, Italy | **3 AZs** | Non-paired | 2023 |
| **Spain Central** | `spaincentral` | Madrid, Spain | **3 AZs** | Non-paired | 2024 |
| **Austria East** | `austriaeast` | Vienna, Austria | **3 AZs** | Non-paired | Announced |
| **Belgium Central** | `belgiumcentral` | Brussels, Belgium | **3 AZs** | Non-paired | Announced |
| **Denmark East** | `denmarkeast` | Copenhagen, Denmark | **3 AZs** | Non-paired | Announced |
| **Finland Central** | `finlandcentral` | Helsinki, Finland | **3 AZs** | Non-paired | Announced |
| **Greece Central** | `greececentral` | Athens, Greece | **3 AZs** | Non-paired | Announced |

### 🌏 Asia Pacific & Oceania
| Region Display Name | Region Identifier | Physical Location | Availability Zones | Paired Region | Launch Year |
| :--- | :--- | :--- | :---: | :--- | :---: |
| **East Asia** | `eastasia` | Hong Kong SAR | **3 AZs** | Southeast Asia | 2010 |
| **Southeast Asia** | `southeastasia` | Singapore | **3 AZs** | East Asia | 2010 |
| **Central India** | `centralindia` | Pune, Maharashtra, India | **3 AZs** | South India | 2015 |
| **South India** | `southindia` | Chennai, Tamil Nadu, India | Non-zonal | Central India | 2015 |
| **West India** | `westindia` | Mumbai, Maharashtra, India | Non-zonal | South India | 2015 |
| **India South Central**| `indiasouthcentral`| Hyderabad, Telangana, India | **3 AZs** | Non-paired | Announced |
| **Japan East** | `japaneast` | Tokyo & Saitama, Japan | **3 AZs** | Japan West | 2014 |
| **Japan West** | `japanwest` | Osaka, Japan | Non-zonal | Japan East | 2014 |
| **Korea Central** | `koreacentral` | Seoul, South Korea | **3 AZs** | Korea South | 2017 |
| **Korea South** | `koreasouth` | Busan, South Korea | Non-zonal | Korea Central | 2017 |
| **Australia East** | `australiaeast` | Sydney, New South Wales, Australia | **3 AZs** | Australia Southeast | 2014 |
| **Australia Southeast**| `australiasoutheast`| Melbourne, Victoria, Australia | Non-zonal | Australia East | 2014 |
| **Australia Central** | `australiacentral` | Canberra, ACT, Australia (Government) | Non-zonal | Australia Central 2 | 2018 |
| **Australia Central 2**| `australiacentral2`| Canberra, ACT, Australia | Non-zonal | Australia Central | 2018 |
| **New Zealand North** | `newzealandnorth` | Auckland, New Zealand | **3 AZs** | Non-paired | 2024 |
| **Malaysia West** | `malaysiawest` | Kuala Lumpur, Malaysia | **3 AZs** | Non-paired | 2024 |
| **Indonesia Central** | `indonesiacentral` | Jakarta, Indonesia | **3 AZs** | Non-paired | 2024 |
| **Taiwan North** | `taiwannorth` | Taipei, Taiwan | **3 AZs** | Non-paired | Announced |

### 🌍 Middle East & Africa
| Region Display Name | Region Identifier | Physical Location | Availability Zones | Paired Region | Launch Year |
| :--- | :--- | :--- | :---: | :--- | :---: |
| **UAE North** | `uaenorth` | Dubai, United Arab Emirates | **3 AZs** | UAE Central | 2019 |
| **UAE Central** | `uaecentral` | Abu Dhabi, UAE | Non-zonal | UAE North | 2019 |
| **Qatar Central** | `qatarcentral` | Doha, Qatar | **3 AZs** | Non-paired | 2022 |
| **Israel Central** | `israelcentral` | Tel Aviv & Modi'in, Israel | **3 AZs** | Non-paired | 2023 |
| **Saudi Arabia East** | `saudiarabiaeast` | Dammam / Riyadh, Saudi Arabia | **3 AZs** | Non-paired | Announced |
| **South Africa North** | `southafricanorth` | Johannesburg, South Africa | **3 AZs** | South Africa West | 2019 |
| **South Africa West** | `southafricawest` | Cape Town, South Africa | Non-zonal | South Africa North | 2019 |

---

## 🔒 6. Sovereign, Air-Gapped & Government Clouds

For strict national security, regulatory, and defense requirements, Microsoft operates completely separated cloud partitions:

```text
┌─────────────────────────────────────────────────────────────────────────────────────────┐
│                              ISOLATED SOVEREIGN CLOUD DOMAINS                           │
├─────────────────────────────────────────────────────────────────────────────────────────┤
│ 1. AZURE GOVERNMENT (US):                                                               │
│    • Regions: usgovvirginia, usgovtexas, usgovarizona                                   │
│    • Reserved exclusively for US Federal, State, Local government agencies & cleared    │
│      defense industrial base (DIB) contractors.                                         │
│    • Personnel: Microsoft operations staff must be US citizens with background checks.  │
│    • Compliance: FedRAMP High, ITAR, CJIS, DoD SRG Impact Level 4 & 5.                 │
│                                                                                         │
│ 2. AZURE DOD (US DEPARTMENT OF DEFENSE):                                                │
│    • Regions: usdodeast, usdodcentral                                                   │
│    • Air-gapped and dedicated physically for US military defense workloads.             │
│    • Compliance: DoD SRG Impact Level 6 (Secret / Classified workloads).                │
│                                                                                         │
│ 3. AZURE CHINA (21VIANET):                                                              │
│    • Regions: chinanorth, chinaeast, chinanorth2, chinaeast2, chinanorth3, chinaeast3   │
│    • Operated and maintained exclusively by Shanghai Blue Cloud Technology Co.          │
│      (21Vianet), legally satisfying China Telecommunications Regulations.               │
│    • Physically and logically isolated from global Azure Entra ID; has its own separate │
│      portal (portal.azure.cn) and management endpoints.                                 │
└─────────────────────────────────────────────────────────────────────────────────────────┘
```

---

## 💻 7. Practical CLI Commands: Exploring Regions and Zones

### List All Available Azure Regions with Latency & Geography:
```bash
# List all regions and their display names
az account list-locations --query "[].{Name:name, DisplayName:displayName, RegionalDisplayName:regionalDisplayName}" -o table

# Check if Availability Zones are supported in a specific region
az account list-locations --query "[?name=='eastus'].availabilityZoneMappings" -o json
```

### Verify Service Availability in a Specific Region:
```bash
# Check if a specific VM series (e.g., Standard_D4s_v5) is available in West Europe Zone 1
az vm list-skus --location westeurope --size Standard_D4s_v5 --query "[].{Size:name, Zones:locationInfo[0].zones, Restrictions:restrictions}" -o table
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 03 - Azure Subscriptions Types Limits and Quotas](./03-Azure-Subscriptions-Types-Limits-and-Quotas.md) | [Index](../../../README.md) | [05 - Azure Management Interfaces Portal CLI PowerShell CloudShell →](./05-Azure-Management-Interfaces-Portal-CLI-PowerShell-CloudShell.md) |
