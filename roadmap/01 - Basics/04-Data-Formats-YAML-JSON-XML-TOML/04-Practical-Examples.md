# Data Formats — Practical Validation, Processing & CLI Tools

## 1. Querying & Formatting JSON with `jq`

```bash
# Pretty-print JSON file
jq . config.json

# Extract specific field value
jq '.application' config.json

# Filter array element
jq '.ports[0]' config.json
```

---

## 2. Querying & Converting YAML with `yq`

```bash
# Read YAML value
yq '.spec.replicas' deployment.yaml

# Convert YAML to JSON
yq -o=json deployment.yaml

# Convert JSON to YAML
yq -p=json -o=yaml config.json
```

---

## 3. Python One-Liner JSON & YAML Syntax Validators

```bash
# Validate JSON file syntax
python3 -m json.tool config.json > /dev/null && echo "Valid JSON"

# Validate YAML syntax with Python PyYAML
python3 -c 'import sys, yaml; yaml.safe_load(open(sys.argv[1]))' deployment.yaml && echo "Valid YAML"
```
