# 02 - Multi-AZ Deployments vs Read Replicas

| Property | Multi-AZ Deployment | Read Replicas |
|---|---|---|
| **Purpose** | High Availability & Disaster Recovery | Read Scalability & Performance |
| **Replication** | **Synchronous** to standby instance | **Asynchronous** to replica instances |
| **Endpoints** | Single DNS endpoint (Auto-failover) | Separate DNS endpoints per replica |
| **Cross-Region?** | No (Within same Region across 2 AZs) | **YES** (Can span across AWS Regions) |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 01 - Relational Database Service RDS Architecture](./01-Relational-Database-Service-RDS-Architecture.md) | [Index](../../../README.md) | [03 - Amazon Aurora Serverless Global Databases and Storage →](./03-Amazon-Aurora-Serverless-Global-Databases-and-Storage.md) |
