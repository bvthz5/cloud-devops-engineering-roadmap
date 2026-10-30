# 11 — Technical Interview Questions & Answers (Junior to Staff SRE)

A rigorous compilation of technical interview questions testing data formats, serialization, declarative configuration, security, and templating from Junior DevOps to Senior/Staff SRE levels.

---

### Q1: Why does Kubernetes use YAML for its manifests rather than JSON, even though the Kubernetes API server internally communicates using JSON?
**Level:** Junior / Intermediate  
**Answer:**
- **Internally:** The Kubernetes API server is an HTTP REST API that serializes and deserializes payloads strictly as **JSON** over the wire for machine performance and simplicity.
- **For Humans:** Kubernetes CLI tools (`kubectl`) accept **YAML** because YAML is optimized for human authoring:
  1. It supports **comments** (`#`), allowing engineers to document architecture rationale inline.
  2. It supports **multi-document streams** (`---`), allowing multiple related resources (e.g., a Deployment, Service, and ConfigMap) to be bundled into a single file.
  3. It eliminates visual noise (braces, quotes, trailing commas).
  4. `kubectl` automatically converts YAML manifests to JSON before transmitting them to the Kubernetes API server.

---

### Q2: What is the "Norway Problem" in YAML, and how do you protect against it?
**Level:** Intermediate  
**Answer:**
- **The Issue:** Under the YAML 1.1 specification, unquoted strings like `NO`, `no`, `N`, `false`, `off`, `yes`, `y`, `true`, `on` are automatically inferred as **Boolean** values (`false` or `true`).
- When a configuration contains country codes (e.g., `country: NO`), an unquoted `NO` is parsed as the boolean `false` rather than the ISO country code string `"NO"`, causing schema validation failures and broken business logic.
- **Protection:**
  1. **Strict Quoting:** Always enclose strings that could resemble booleans or numbers in single or double quotes: `'NO'`.
  2. **YAML 1.2:** Adopt parsers that implement YAML 1.2, which restricted booleans strictly to `true` and `false`.
  3. **Linting:** Enforce `yamllint` with rule `truthy: {check-keys: true}` in pre-commit CI/CD gates.

---

### Q3: Explain the difference between the YAML Literal Block scalar (`|`) and Folded Block scalar (`>`).
**Level:** Intermediate  
**Answer:**
- **Literal Block (`|`):** Preserves all internal line breaks and whitespace verbatim. It is typically used for embedding shell scripts, multi-line SQL queries, or configuration files inside Kubernetes ConfigMaps or CI/CD pipelines.
- **Folded Block (`>`):** Replaces internal single line breaks with spaces, "folding" multiple lines of text into a single long paragraph. It is used for long descriptions, prose, and release notes.

---

### Q4: Why is using `yaml.load()` in Python considered a severe remote code execution (RCE) security vulnerability?
**Level:** Senior  
**Answer:**
- Traditional `yaml.load()` in libraries like PyYAML supports arbitrary object instantiation through custom YAML tags (e.g., `!!python/object/apply:os.system ['rm -rf /']`).
- If an application accepts untrusted YAML input from an external user or API and parses it with `yaml.load()`, an attacker can execute arbitrary shell commands inside the host container.
- **Mitigation:** Always use **`yaml.safe_load()`**, which restricts deserialization strictly to basic scalar types, lists, and dictionaries, completely disallowing custom object execution.

---

### Q5: How do YAML Anchors (`&`) and Merge Keys (`<<`) work, and what is their primary use case in DevOps?
**Level:** Intermediate / Senior  
**Answer:**
- **Anchors (`&name`):** Marks a YAML mapping or sequence with an identifier, storing the object definition in the parser's memory.
- **Aliases (`*name`):** References the stored anchor, inserting the referenced block into another location.
- **Merge Keys (`<<: *name`):** Merges all key-value pairs from the anchored map into the current dictionary, allowing specific keys to be overridden.
- **DevOps Use Case:** Eliminates duplication in complex configurations (such as Docker Compose files, GitLab CI jobs, and Prometheus alert rules), adhering to the DRY (Don't Repeat Yourself) principle.

---

### Q6: Why does standard JSON (RFC 8259) explicitly disallow comments?
**Level:** Junior / Intermediate  
**Answer:**
Douglas Crockford (the creator of JSON) intentionally omitted comments from the JSON specification to **prevent parsing divergence and dialect fragmentation**. In formats like HTML and XML, developers began placing execution directives, parser instructions, and pre-processor annotations inside comments, causing parsers to behave inconsistently. JSON was designed to be a minimal, unambiguous data interchange standard rather than a document layout language.

---

### Q7: Compare TOML and YAML for application configuration files. When would you choose one over the other?
**Level:** Senior  
**Answer:**
- **Choose TOML when:**
  - The configuration is primarily flat or has shallow hierarchies (e.g., package manager configurations like Rust `Cargo.toml`, Python `pyproject.toml`, or daemon settings like `containerd`).
  - You want to eliminate indentation-related bugs.
  - You require first-class native support for RFC 3339 date/time timestamps.
- **Choose YAML when:**
  - The data model is deeply nested and hierarchical (e.g., Kubernetes manifests with metadata, spec, template, containers, volume mounts).
  - You need multi-document streaming in a single file (`---`).
  - You need anchors and merge keys for DRY templates.

---

### Q8: What is the XML "Billion Laughs" attack, and how is it mitigated?
**Level:** Senior / Staff SRE  
**Answer:**
- The **Billion Laughs Attack** is a Denial-of-Service (DoS) vulnerability targeting XML parsers that resolve Document Type Definitions (DTDs) and XML entities.
- An attacker submits a tiny (1 KB) XML document containing 10 nested entities, each referencing the previous entity 10 times. When parsed, memory consumption expands exponentially into gigabytes of RAM, causing CPU saturation and crashing the server with an OOM panic.
- **Mitigation:** Configure XML parsers to completely disable DTD processing (`disallow-doctype-decl = true`) and disable external entity resolution (XXE protection).

---

### Q9: What is Mozilla SOPS, and how does its encryption model differ from standard PGP or GPG?
**Level:** Senior  
**Answer:**
- Traditional encryption tools (GPG, OpenSSL) encrypt an entire file into a raw binary ciphertext blob. This completely destroys Git version control usability—you cannot view Git diffs, merge branches, or see which keys were modified.
- **Mozilla SOPS (Secrets OPerationS)** performs **Value-Level Encryption**:
  - It parses the structured format (YAML, JSON, ENV) and encrypts **only the values**, leaving the keys, hierarchy, and metadata in plaintext.
  - This allows engineers to safely commit encrypted files to Git (GitOps), review meaningful pull request diffs, and inspect structure without exposing plaintext credentials.
  - Supports cloud KMS keys (AWS KMS, GCP KMS, Azure Key Vault) and Age/PGP keys.

---

### Q10: What is the difference between Schema Validation and Syntax Validation?
**Level:** Intermediate  
**Answer:**
- **Syntax Validation:** Verifies that a document adheres to the basic grammar of the serialization language (e.g., "Are the quotes balanced?", "Are there illegal tabs in YAML?", "Are commas placed correctly in JSON?"). Tools: `jq empty`, `yamllint`.
- **Schema Validation:** Verifies that a syntactically valid document conforms to specific structural constraints, required fields, and data types expected by an application domain (e.g., "Does this Kubernetes Deployment contain a `spec.template.spec.containers` array?", "Is the port an integer between 1 and 65535?"). Tools: `JSON Schema`, `XSD`, `kubeconform`.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 10 - Troubleshooting](./10-Troubleshooting.md) | [Index](../../../README.md) | [12 - Hands On Practice →](./12-Hands-On-Practice.md) |
