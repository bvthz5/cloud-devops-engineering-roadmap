# 05 — TOML Deep Dive: Syntax, Tables, and Native Typing

---

## 1. What Is TOML?

**TOML (Tom's Obvious, Minimal Language)** was created in 2013 by Tom Preston-Werner (co-founder of GitHub). It was specifically designed to be an unambiguous configuration file format that maps directly to a hash table while remaining vastly easier for humans to read than JSON and significantly less error-prone than YAML.

In modern cloud and DevOps environments, TOML is the **standard configuration format for:**
- **Container Runtimes:** **containerd** (`/etc/containerd/config.toml`).
- **Rust Ecosystem:** Cargo package manager (`Cargo.toml`).
- **Modern Python:** Standard packaging via PEP 518/621 (`pyproject.toml`, Poetry, Ruff, Flake8).
- **CI/CD Agents:** GitLab Runner configuration (`/etc/gitlab-runner/config.toml`).
- **Static Site Generators:** Hugo (`config.toml`).

---

## 2. Core TOML Syntax Rules

```toml
# Top-level key-value configuration
title = "Production Microservices Cluster"
environment = "production"
timeout_seconds = 30
debug_mode = false
```

1. **Whitespace Insensitive for Hierarchy:** Unlike YAML, TOML does not use indentation to represent nested data structures. You can indent with spaces, tabs, or not at all.
2. **Explicit Table Headers:** Dictionaries (hash maps) are declared using bracketed headers: `[table_name]`.
3. **Strict Typing:** All data types (booleans, strings, floats, dates) have rigid, unambiguous literal formats.
4. **Case Sensitivity:** Keys and strings are case-sensitive.
5. **No Trailing Comma Issues:** Arrays permit trailing commas cleanly.

---

## 3. Native Data Types in TOML

TOML features rich, first-class native typing:

```toml
[data_types]
# Strings
basic_string = "Hello\nWorld"        # Supports escape characters
literal_string = 'C:\Users\binil'     # Raw string: NO escape processing (great for regex & Windows paths)
multiline_string = """
Line 1
Line 2
"""

# Numbers
integer = 42
integer_with_underscores = 1_000_000 # Clean readability for large numbers
float_num = 3.14159
scientific = 1e6
hexadecimal = 0xDEADBEEF
octal = 0o755
binary = 0b11010110

# Booleans (strictly lowercase)
is_active = true
maintenance = false

# Native RFC 3339 Dates & Times (A major advantage over JSON/YAML!)
timestamp = 2026-10-02T07:30:00Z
local_date = 2026-10-02
local_time = 07:30:00
```

---

## 4. Tables vs Nested Tables vs Inline Tables

### 1. Standard Tables (`[table]`)
Declares a new key-value dictionary. All subsequent key-value pairs belong to this table until the next table header appears:
```toml
[database]
server = "192.168.1.1"
ports = [ 8001, 8002, 8003 ]
connection_max = 5000
enabled = true
```

### 2. Nested Sub-Tables (`[parent.child]`)
Creates hierarchical nested structures explicitly without deep whitespace indentation:
```toml
[servers]

[servers.alpha]
ip = "10.0.0.1"
role = "primary"

[servers.beta]
ip = "10.0.0.2"
role = "replica"
```

### 3. Inline Tables
Compact, single-line representation of small objects:
```toml
point = { x = 1, y = 2 }
author = { name = "Alice", role = "Lead SRE" }
```

---

## 5. Arrays of Tables (`[[table_name]]`)

When you need to model a **list of objects / dictionaries**, TOML uses double brackets **`[[ ... ]]`**. Each declaration appends a new object item to the array:

```toml
# List of microservices:
[[services]]
name = "auth-api"
port = 8081
healthcheck = "/healthz"

[[services]]
name = "billing-api"
port = 8082
healthcheck = "/status"

[[services]]
name = "orders-worker"
port = 8083
healthcheck = "/ping"
```

### Equivalent JSON Representation:
```json
{
  "services": [
    { "name": "auth-api", "port": 8081, "healthcheck": "/healthz" },
    { "name": "billing-api", "port": 8082, "healthcheck": "/status" },
    { "name": "orders-worker", "port": 8083, "healthcheck": "/ping" }
  ]
}
```

---

## 6. Parsing & Validating TOML

### In Python (Native `tomllib` in Python 3.11+):
```python
import tomllib  # Built-in standard library in Python 3.11+

with open("config.toml", "rb") as f:
    config = tomllib.load(f)

print(f"Database Server: {config['database']['server']}")
```

### CLI Validation with `yq` or `taplo`:
```bash
# Validate TOML syntax using yq
yq -p=toml config.toml > /dev/null && echo "Valid TOML"

# Convert TOML to JSON
yq -p=toml -o=json config.toml
```
