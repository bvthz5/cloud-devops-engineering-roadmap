# 05 — Trees, Graphs, DAGs, and Merkle Trees

Non-linear hierarchical and graph data structures power databases, version control systems, and infrastructure automation.

---

## 1. B-Trees & B+ Trees (Database Indexing)

Relational databases (MySQL InnoDB, PostgreSQL) and filesystems (ext4, XFS, NTFS) use **B+ Trees** for on-disk indexing:
- Optimized for storage where reading a disk block is expensive.
- High fanout (each node can hold hundreds of keys), keeping tree depth very shallow (typically 3–4 levels for billions of rows).
- Lookups, range queries (`BETWEEN 10 AND 50`), and insertions execute in **$O(\log N)$** disk I/O operations.

---

## 2. Directed Acyclic Graphs (DAGs)

A Graph with directed edges and **no cycles** (you can never loop back to the same node).

```text
[ VPC ] ──► [ Subnet ] ──► [ Security Group ] ──► [ EC2 Instance ]
```

### Where DAGs are used in DevOps:
- **Terraform / OpenTofu:** Builds an internal DAG of all cloud resources. Evaluates which resources are independent so it can provision them in parallel, and which must wait for dependencies.
- **Git Commit History:** Every Git commit points to its parent commit(s), forming a DAG.
- **Data Pipelines:** Apache Airflow, Dagster, and dbt structure data transformations as DAGs.

---

## 3. Merkle Trees (Cryptographic Hash Trees)

A tree where every non-leaf node is the cryptographic hash of its child nodes:
- Enables rapid, efficient verification that two large datasets match.
- Used in **Git** (tree objects), **Cassandra / DynamoDB** (anti-entropy replica sync), and **Bitcoin/Ethereum**.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [04 - Stacks, Queues & Brokers](./04-Stacks-Queues-and-Message-Brokers.md) | [README](./README.md) | [06 - Rate Limiting Algorithms](./06-Rate-Limiting-Algorithms-in-API-Gateways.md) |
