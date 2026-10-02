# 03 - Amazon Aurora Architecture

Aurora separates compute and storage layers. Storage is replicated 6 ways across 3 AZs automatically.

- **Aurora Serverless v2**: Scales compute (ACUs) up or down dynamically in fractions of a second.
- **Aurora Global Database**: Cross-region latency under 1 second for global reads.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 02 - RDS High Availability Multi AZ and Read Replicas](./02-RDS-High-Availability-Multi-AZ-and-Read-Replicas.md) | [Index](../../../README.md) | [04 - DynamoDB NoSQL Partition Keys GSIs LSIs and Streams →](./04-DynamoDB-NoSQL-Partition-Keys-GSIs-LSIs-and-Streams.md) |
