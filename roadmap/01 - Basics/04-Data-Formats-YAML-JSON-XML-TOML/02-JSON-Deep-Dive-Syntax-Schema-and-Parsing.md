# 02 — JSON Deep Dive: Syntax, Schema, and Parsing

---

## 1. What Is JSON?

**JSON (JavaScript Object Notation)** is an open-standard, lightweight, text-based data interchange format defined by **RFC 8259** and **ECMA-404**. Although derived from JavaScript, JSON is completely language-independent and universally supported across virtually every programming language on earth.

In cloud and DevOps engineering, JSON is the **native dialect of REST APIs**, Docker daemon configuration, Terraform state files (`.tfstate`), and AWS CloudTrail audit logs.

---

## 2. JSON Strict Syntax Rules

Unlike JavaScript objects or Python dictionaries, standard JSON enforces strict grammatical constraints:

1. **Keys MUST Be Strings in Double Quotes:**
   `"name": "nginx"` is valid. `'name': "nginx"` or `name: "nginx"` is an invalid syntax error!
2. **Double Quotes Strictly Enforced for Strings:**
   Single quotes (`'...'`) are completely forbidden for strings.
3. **No Trailing Commas:**
   A trailing comma after the last key-value pair or array item (`{"a": 1, }` or `[1, 2, ]`) is invalid.
4. **No Comments Permitted:**
   Standard RFC 8259 JSON **does not support comments** (neither `//` nor `/* */`).
5. **Restricted Value Types:**
   Only six data types are allowed: Object, Array, String, Number, Boolean, Null.

---

## 3. The 6 Native JSON Data Types

```json
{
  "string_example": "Production US-East",
  "number_integer": 8080,
  "number_float": 99.95,
  "number_scientific": 1.5e6,
  "boolean_true": true,
  "boolean_false": false,
  "null_value": null,
  "array_example": [
    "us-east-1a",
    "us-east-1b",
    "us-east-1c"
  ],
  "nested_object": {
    "replicas": 3,
    "auto_scale": true
  }
}
```

### Numerical Caveats in JSON
- JSON numbers have no fixed precision limits defined in the spec, but IEEE 754 double-precision floating-point limits apply in most parsers. Numbers larger than $2^{53} - 1$ (`9007199254740991`) may suffer truncation when parsed.
- `NaN` (Not-a-Number) and `Infinity` are strictly forbidden in valid JSON.

---

## 4. JSON Schema: Contract-Driven Data Validation

**JSON Schema** is a declarative standard used to annotate and validate the structure, constraints, and data types of JSON documents.

### Production Example: Kubernetes Container Specification Schema
Save to `container-schema.json`:
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "ContainerConfig",
  "type": "object",
  "required": ["name", "image", "port"],
  "additionalProperties": false,
  "properties": {
    "name": {
      "type": "string",
      "minLength": 3,
      "maxLength": 63,
      "pattern": "^[a-z0-9]([-a-z0-9]*[a-z0-9])?$"
    },
    "image": {
      "type": "string"
    },
    "port": {
      "type": "integer",
      "minimum": 1,
      "maximum": 65535
    },
    "env": {
      "type": "array",
      "items": {
        "type": "object",
        "required": ["key", "value"],
        "properties": {
          "key": { "type": "string" },
          "value": { "type": "string" }
        }
      }
    }
  }
}
```

### Validating JSON Documents against Schema via CLI
```bash
# Install ajv-cli (Another JSON Schema Validator)
npm install -g ajv-cli

# Validate document
ajv validate -s container-schema.json -d app-config.json
```

---

## 5. JSON Parsing, Serialization, and Deserialization

### In Python:
```python
import json

# Python in-memory dictionary
data = {
    "service": "payments-api",
    "replicas": 3,
    "enabled": True
}

# 1. Serialization (Dict -> JSON String):
json_string = json.dumps(data, indent=2)
print(json_string)

# 2. Deserialization (JSON String -> Python Dict):
parsed_dict = json.loads(json_string)
print(f"Service name: {parsed_dict['service']}")
```

### In Go:
```go
package main

import (
	"encoding/json"
	"fmt"
)

type Config struct {
	Service  string `json:"service"`
	Replicas int    `json:"replicas"`
	Enabled  bool   `json:"enabled"`
}

func main() {
	rawJSON := `{"service":"auth","replicas":2,"enabled":true}`
	var cfg Config

	// Deserialization (Unmarshalling)
	json.Unmarshal([]byte(rawJSON), &cfg)
	fmt.Printf("Service: %s, Replicas: %d\n", cfg.Service, cfg.Replicas)

	// Serialization (Marshalling)
	bytes, _ := json.MarshalIndent(cfg, "", "  ")
	fmt.Println(string(bytes))
}
```

---

## 6. CLI JSON Wrangling with `jq`

```bash
# Pretty-print JSON from curl
curl -s https://api.github.com/zen | jq .

# Validate JSON file syntax from command line
jq empty config.json && echo "Valid JSON" || echo "Syntax Error!"
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [01 - Structured Data Serialization and Representations](./01-Structured-Data-Serialization-and-Representations.md) | [README](./README.md) | [03 - YAML Deep Dive Syntax Anchors and Gotchas](./03-YAML-Deep-Dive-Syntax-Anchors-and-Gotchas.md) |
