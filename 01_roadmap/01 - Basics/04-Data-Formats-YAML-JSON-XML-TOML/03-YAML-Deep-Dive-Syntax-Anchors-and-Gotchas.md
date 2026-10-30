# 03 — YAML Deep Dive: Syntax, Anchors, Merge Keys, and Gotchas

---

## 1. What Is YAML?

**YAML** (originally *Yet Another Markup Language*, retroactively renamed to *YAML Ain't Markup Language*) is a human-friendly, data-centric serialization language.

In the cloud ecosystem, YAML is the **undisputed lingua franca of declarative infrastructure**:
- **Kubernetes:** All Pods, Deployments, Services, and CRDs.
- **Docker Compose:** Multi-container service definitions (`compose.yaml`).
- **CI/CD Pipelines:** GitHub Actions workflows, GitLab CI, CircleCI, Azure Pipelines.
- **Ansible:** Playbooks, tasks, and inventory definitions.

---

## 2. Core YAML Syntax Rules

1. **Indentation Must Use SPACES Only:**  
   **Tabs are strictly illegal in YAML!** Even a single tab character will trigger a syntax parser error (`yaml.scanner.ScannerError: found character '\t' that cannot start any token`). Standard convention is 2 spaces per indentation level.
2. **Case Sensitivity:**  
   Keys and string values are strictly case-sensitive. `Replicas` and `replicas` are distinct keys.
3. **Comments Are Supported:**  
   Any text following `#` is treated as a comment.
4. **Colons Must Be Followed by a Space:**  
   `port: 80` is valid. `port:80` is parsed as a single scalar string, not a key-value mapping!

---

## 3. Documents, Mappings, and Sequences

### Multi-Document Streams (`---` and `...`)
YAML files can contain multiple documents separated by three hyphens (`---`). Three dots (`...`) denote the end of a document:
```yaml
---
# Document 1: Kubernetes Deployment
apiVersion: apps/v1
kind: Deployment
metadata:
  name: web-app
---
# Document 2: Kubernetes Service
apiVersion: v1
kind: Service
metadata:
  name: web-service
...
```

### Mappings (Key-Value Dictionaries)
```yaml
database:
  engine: postgresql
  port: 5432
  ssl_enabled: true
```

### Sequences (Ordered Lists)
Denoted with a hyphen and a space (`- `):
```yaml
allowed_regions:
  - us-east-1
  - eu-west-1
  - ap-southeast-1

# Inline compact JSON-style array syntax (also valid YAML):
allowed_regions: [us-east-1, eu-west-1, ap-southeast-1]
```

---

## 4. Multiline Strings: Literal Block (`|`) vs Folded Block (`>`)

One of the most frequent sources of confusion in Kubernetes ConfigMaps and CI/CD shell scripts is choosing between the Pipe (`|`) and Greater-Than (`>`) multiline block scalar operators.

```text
LITERAL BLOCK (|)                             FOLDED BLOCK (>)
Preserves literal newlines & formatting.       Folds multiple lines into a single string
Used for shell scripts and config files.       (replaces internal newlines with spaces).
                                               Used for long prose and descriptions.
```

```yaml
config:
  # 1. Literal Block (|): Each line break is preserved
  startup_script: |
    #!/usr/bin/env bash
    echo "Starting background worker..."
    exec python3 worker.py

  # 2. Folded Block (>): Lines are joined into a single paragraph
  release_notes: >
    This update introduces performance
    enhancements to the payments API
    and fixes issue #402.
```

### Stripping and Keeping Trailing Newlines (`|-` vs `|+`)
- `|` (Default Clip): Keeps exactly one trailing newline at the end of the block.
- `|-` (Strip): Strips all trailing newlines completely.
- `|+` (Keep): Preserves all trailing blank newlines.

---

## 5. DRY Configuration: Anchors (`&`), Aliases (`*`), and Merge Keys (`<<`)

In complex configurations (e.g., Docker Compose files or CI/CD pipelines), repeating identical resource definitions violates DRY (Don't Repeat Yourself). YAML supports native memory anchors:

```text
&anchor_name   ──► Defines an anchor (saves object definition in memory)
*anchor_name   ──► References the anchor (inserts object)
<<: *anchor    ──► Merges anchor keys into the current mapping
```

### Production Docker Compose Example:
```yaml
version: '3.8'

# Define reusable base service template
x-base-service: &base-service
  image: python:3.11-slim
  restart: always
  environment:
    ENV: production
    DB_HOST: postgres.internal
  logging:
    driver: "json-file"
    options:
      max-size: "50m"

services:
  web-api:
    <<: *base-service
    ports:
      - "8080:8080"
    command: ["uvicorn", "app.main:app", "--host", "0.0.0.0"]

  worker:
    <<: *base-service
    command: ["celery", "-A", "app.tasks", "worker"]
```

---

## 6. Dangerous Gotchas: The "Norway Problem" and Type Inference

In YAML 1.1 (the version implemented by Python's PyYAML and older Kubernetes tools), unquoted boolean values have broad type inference rules.

### The Norway Problem (ISO Country Code `NO`)
Consider a list of international country codes:
```yaml
countries:
  - GB
  - FR
  - DE
  - NO  # <-- DANGER!
```
In YAML 1.1, the string `NO` (along with `no`, `n`, `N`, `y`, `yes`, `true`, `false`, `on`, `off`) is parsed as a **Boolean `false`**!
When parsed by Python, the list becomes:
`['GB', 'FR', 'DE', False]`!

### Golden Defense:
**Always quote strings that look like booleans, numbers, or dates:**
```yaml
countries:
  - 'GB'
  - 'FR'
  - 'DE'
  - 'NO'  # Safely preserved as String "NO"
```

### The Port Number Octal Trap:
```yaml
# Parsed as decimal 80:
http_port: 80

# Starts with 0 -> Parsed in YAML 1.1 as OCTAL!
# 0700 in octal becomes decimal 448!
cache_port: 0700

# Fix: Quote it if you need the literal string:
permission: "0755"
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 02 - JSON Deep Dive Syntax Schema and Parsing](./02-JSON-Deep-Dive-Syntax-Schema-and-Parsing.md) | [Index](../../../README.md) | [04 - XML Deep Dive Syntax Namespaces and Schemas →](./04-XML-Deep-Dive-Syntax-Namespaces-and-Schemas.md) |
