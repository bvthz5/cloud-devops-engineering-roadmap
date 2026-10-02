# 01 — Structured Data, Serialization, and Representations

---

## 1. Structured vs Semi-Structured vs Unstructured Data

In computer science and cloud engineering, data is categorized into three fundamental tiers:

```text
+-------------------------------------------------------------+
|                      Data Categorization                    |
+-------------------------------------------------------------+
| 1. Structured Data:                                         |
|    - Rigid, predefined schema (Tables, Rows, Columns)       |
|    - Relational Databases: PostgreSQL, MySQL, Spanner       |
|                                                             |
| 2. Semi-Structured Data:                                    |
|    - Self-describing schema embedded within the data        |
|    - Hierarchical tags, key-values, and lists               |
|    - JSON, YAML, TOML, XML                                  |
|                                                             |
| 3. Unstructured Data:                                       |
|    - No predefined data model or schema                     |
|    - Raw server logs, video files, audio streams, raw text  |
+-------------------------------------------------------------+
```

DevOps engineers operate predominantly in the **Semi-Structured** realm: configuration files, API payloads, infrastructure-as-code manifests, and telemetry metrics.

---

## 2. Serialization and Deserialization

Programs store complex data structures (nested maps, object instances, pointers, arrays) in active physical RAM. However, physical network cables (TCP/IP) and secondary storage (NVMe/SSD) can only transmit and persist sequential **streams of bytes or characters**.

```text
                     DATA SERIALIZATION & DESERIALIZATION

   IN-MEMORY DATA STRUCTURE                         PERSISTED / NETWORK STREAM
┌───────────────────────────────┐                  ┌───────────────────────────────┐
│ Memory Address: 0x7fff4010    │                  │ UTF-8 Text / Byte Stream:     │
│ Struct {                      │   SERIALIZE      │ {"host":"db.internal",        │
│   Host: "db.internal",        ├─────────────────►│  "port":5432,                 │
│   Port: 5432,                 │                  │  "ssl":true}                  │
│   SSL: true                   │                  └───────────────┬───────────────┘
│ }                             │   DESERIALIZE                    │
└───────────────────────────────┘◄─────────────────────────────────┘
                                                   Transmitted over Network (HTTP)
                                                   or Written to Disk (config.json)
```

- **Serialization (Marshalling / Encoding):** The process of translating an in-memory data object into a standardized format (JSON, YAML, Protobuf) suitable for transmission across a network or storage on disk.
- **Deserialization (Unmarshalling / Decoding):** The reverse process: reading a raw byte or character stream from disk or network and reconstructing the original in-memory data structure in RAM.

---

## 3. Data Representation Models

Semi-structured data formats organize information using three universal foundational primitives:

1. **Scalars (Primitives):** Individual atomic values:
   - Strings: `"production"`
   - Numbers: `8080`, `3.14159`
   - Booleans: `true`, `false`
   - Null: `null` (absence of value)
2. **Mappings (Dictionaries / Key-Value Hashes / Objects):**
   Unordered collections of unique key-value pairs (`{"key": "value"}`).
3. **Sequences (Lists / Arrays):**
   Ordered linear collections of values (`["item1", "item2", "item3"]`).

---

## 4. Configuration Files in DevOps & Cloud Infrastructure

A **Configuration File** is a human- or machine-readable file that defines the parameters, environmental settings, and operational behavior of software applications, daemons, and cloud infrastructure.

### Why Configuration Files Replaced Hardcoding:
1. **Separation of Code and Configuration:** Enables running the exact same immutable container image across Development, Staging, and Production environments simply by changing configuration values.
2. **Declarative Infrastructure:** Defines the desired target state of a system (e.g., "Run 3 replicas of the web app") rather than scripting imperative procedural steps ("Start VM 1, start VM 2, start VM 3").
3. **Auditability & GitOps:** Storing configuration files in Git version control provides a complete, cryptographically signed audit log of every change made to production infrastructure.

---

## 5. Declarative vs Imperative Configuration

```text
IMPERATIVE (Procedural "HOW")                DECLARATIVE (Target State "WHAT")
1. Connect to AWS EC2 via SSH               Apply manifest:
2. Run 'apt update'                         kind: Deployment
3. Run 'apt install -y nginx'               spec:
4. Copy custom nginx.conf                         replicas: 3
5. Run 'systemctl restart nginx'                  image: nginx:1.25
* Brittle, non-idempotent, prone to drift   * Self-healing, idempotent, auditable
```

In the next sections, we will explore the four dominant data formats used to express declarative configurations: **JSON, YAML, XML, and TOML**.
