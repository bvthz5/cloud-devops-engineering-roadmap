# 02 - AWS Regions, Availability Zones Architecture & Global Datacenter Directory

> **Focus:** Comprehensive global infrastructure directory, all 34+ commercial and sovereign regions, 108+ Availability Zones, zone count per country, AZ physical separation, inter-AZ low-latency fiber links, and immutable AZ ID mapping.

---

## 🌍 1. The Global Footprint by the Numbers

As of 2025/2026, the AWS Global Footprint spans:
- **34+ Launched Commercial & Sovereign Regions** across 6 continents.
- **108+ Availability Zones (AZs)** (every region has at least 3 AZs).
- **600+ CloudFront Points of Presence (Edge Locations)** and 30+ Regional Edge Caches.
- **35+ AWS Local Zones** in major metropolitan areas worldwide.
- **245+ Countries and Territories** served.

---

## 🗺️ 2. Comprehensive Directory of ALL AWS Regions Worldwide

Every AWS region has a unique geographical code and consists of multiple, physically separated Availability Zones:

### 🇺🇸 1. North America (United States & Canada)
| Region Name | Code | City / Location | Availability Zones | Launch Year |
| :--- | :--- | :--- | :---: | :---: |
| **US East (N. Virginia)** | `us-east-1` | Northern Virginia, USA | **6 AZs** | 2006 |
| **US East (Ohio)** | `us-east-2` | Columbus, Ohio, USA | **3 AZs** | 2016 |
| **US West (N. California)**| `us-west-1` | San Francisco Bay Area, USA | **3 AZs** | 2009 |
| **US West (Oregon)** | `us-west-2` | Boardman / Umatilla, Oregon, USA | **4 AZs** | 2011 |
| **AWS GovCloud (US-East)** | `us-gov-east-1` | Eastern USA (ITAR / FedRAMP High) | **3 AZs** | 2018 |
| **AWS GovCloud (US-West)** | `us-gov-west-1` | Western USA (ITAR / FedRAMP High) | **3 AZs** | 2011 |
| **Canada (Central)** | `ca-central-1` | Montreal, Quebec, Canada | **3 AZs** | 2016 |
| **Canada West (Calgary)** | `ca-west-1` | Calgary, Alberta, Canada | **3 AZs** | 2023 |

### 🇪🇺 2. Europe
| Region Name | Code | Country & City | Availability Zones | Launch Year |
| :--- | :--- | :--- | :---: | :---: |
| **Europe (Ireland)** | `eu-west-1` | Dublin, Ireland | **3 AZs** | 2008 |
| **Europe (London)** | `eu-west-2` | London, United Kingdom | **3 AZs** | 2016 |
| **Europe (Frankfurt)** | `eu-central-1` | Frankfurt, Germany | **3 AZs** | 2014 |
| **Europe (Paris)** | `eu-west-3` | Paris, France | **3 AZs** | 2017 |
| **Europe (Stockholm)** | `eu-north-1` | Stockholm, Sweden | **3 AZs** | 2018 |
| **Europe (Milan)** | `eu-south-1` | Milan, Italy | **3 AZs** | 2020 |
| **Europe (Spain)** | `eu-south-2` | Aragon, Spain | **3 AZs** | 2022 |
| **Europe (Zurich)** | `eu-central-2` | Zurich, Switzerland | **3 AZs** | 2022 |

### 🌏 3. Asia Pacific
| Region Name | Code | Country & City | Availability Zones | Launch Year |
| :--- | :--- | :--- | :---: | :---: |
| **Asia Pacific (Tokyo)** | `ap-northeast-1` | Tokyo, Japan | **4 AZs** | 2011 |
| **Asia Pacific (Seoul)** | `ap-northeast-2` | Seoul, South Korea | **4 AZs** | 2016 |
| **Asia Pacific (Osaka)** | `ap-northeast-3` | Osaka, Japan | **3 AZs** | 2018 |
| **Asia Pacific (Singapore)**| `ap-southeast-1` | Singapore | **3 AZs** | 2010 |
| **Asia Pacific (Sydney)** | `ap-southeast-2` | Sydney, Australia | **3 AZs** | 2012 |
| **Asia Pacific (Mumbai)** | `ap-south-1` | Mumbai, Maharashtra, India | **3 AZs** | 2016 |
| **Asia Pacific (Hyderabad)**| `ap-south-2` | Hyderabad, Telangana, India | **3 AZs** | 2022 |
| **Asia Pacific (Jakarta)** | `ap-southeast-3` | Jakarta, Indonesia | **3 AZs** | 2021 |
| **Asia Pacific (Melbourne)**| `ap-southeast-4` | Melbourne, Australia | **3 AZs** | 2023 |
| **Asia Pacific (Malaysia)** | `ap-southeast-5` | Kuala Lumpur, Malaysia | **3 AZs** | 2024 |
| **Asia Pacific (Hong Kong)**| `ap-east-1` | Hong Kong SAR | **3 AZs** | 2019 |

### 🌍 4. South America, Middle East & Africa
| Region Name | Code | Country & City | Availability Zones | Launch Year |
| :--- | :--- | :--- | :---: | :---: |
| **South America (São Paulo)**| `sa-east-1` | São Paulo, Brazil | **3 AZs** | 2011 |
| **Middle East (Bahrain)** | `me-south-1` | Manama, Bahrain | **3 AZs** | 2019 |
| **Middle East (UAE)** | `me-central-1` | Dubai / Abu Dhabi, UAE | **3 AZs** | 2022 |
| **Israel (Tel Aviv)** | `il-central-1` | Tel Aviv, Israel | **3 AZs** | 2023 |
| **Africa (Cape Town)** | `af-south-1` | Cape Town, South Africa | **3 AZs** | 2020 |

---

## 🏢 3. Availability Zone Internals & AZ ID Mapping

- **Physical Separation:** Each AZ consists of one or more physical datacenters located typically 10 to 60 miles apart.
- **Isolated Power & Utilities:** Every zone connects to independent power substations, diesel backup generators, and isolated municipal water cooling.
- **Sub-Millisecond Dark Fiber:** AZs are interconnected via redundant private dark fiber loops delivering $< 1	ext{ ms}$ round-trip network latency.

```text
The Cross-Account AZ Name Randomization Problem:
Account A: [us-east-1a] ────────► Physical Datacenter 1 (AZ ID: use1-az1)
Account B: [us-east-1a] ────────► Physical Datacenter 4 (AZ ID: use1-az4)

Always use immutable AZ IDs (use1-az1, use1-az2) when architecting shared VPCs!
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 01 - AWS Global Infrastructure Overview](./01-AWS-Global-Infrastructure-Overview.md) | [Index](../../../README.md) | [03 - Edge Locations CloudFront and AWS Global Accelerator →](./03-Edge-Locations-CloudFront-and-AWS-Global-Accelerator.md) |
