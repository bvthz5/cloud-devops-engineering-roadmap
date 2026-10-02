# 12 — Hands-On Practice Labs: Data Formats and Configuration Engineering

Hands-on, command-driven laboratory exercises to practice YAML anchors, JSON schema enforcement, multi-format conversion pipelines, and secret encryption.

---

## Lab 1: Refactoring Duplicate Configurations with YAML Anchors & Merge Keys

### Objective
Eliminate redundant configurations in a multi-environment Docker Compose file by implementing reusable YAML memory anchors (`&`) and merge keys (`<<:`).

### Step 1: Create the Redundant Configuration
```bash
mkdir -p /tmp/formats_lab && cd /tmp/formats_lab
cat << 'EOF' > docker-compose-bloated.yaml
version: '3.8'
services:
  web-frontend:
    image: node:20-alpine
    restart: always
    environment:
      NODE_ENV: production
      LOG_LEVEL: info
      METRICS_ENABLED: "true"
    logging:
      driver: "json-file"
      options:
        max-size: "100m"
        max-file: "3"
    ports:
      - "3000:3000"

  api-backend:
    image: node:20-alpine
    restart: always
    environment:
      NODE_ENV: production
      LOG_LEVEL: info
      METRICS_ENABLED: "true"
      PORT: 8080
    logging:
      driver: "json-file"
      options:
        max-size: "100m"
        max-file: "3"
    ports:
      - "8080:8080"
EOF
```

### Step 2: Refactor Using Anchors and Merge Keys
Create `docker-compose-clean.yaml`:
```yaml
version: '3.8'

# Define reusable base service definition
x-common-logging: &default-logging
  driver: "json-file"
  options:
    max-size: "100m"
    max-file: "3"

x-base-node-service: &base-service
  image: node:20-alpine
  restart: always
  logging: *default-logging
  environment: &default-env
    NODE_ENV: production
    LOG_LEVEL: info
    METRICS_ENABLED: "true"

services:
  web-frontend:
    <<: *base-service
    ports:
      - "3000:3000"

  api-backend:
    <<: *base-service
    environment:
      <<: *default-env
      PORT: 8080
    ports:
      - "8080:8080"
```

### Step 3: Verify Semantic Equivalence
Use `yq` or Python to ensure both files produce identical expanded JSON data structures:
```bash
python3 -c "import yaml, json; print(json.dumps(yaml.safe_load(open('docker-compose-clean.yaml')), indent=2))"
```

---

## Lab 2: JSON Schema Validation in Python

### Objective
Enforce strict validation rules on incoming JSON payloads to reject malformed configurations automatically.

### Step 1: Create Schema and Test Payload
```bash
cat << 'EOF' > schema.json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "type": "object",
  "required": ["service_name", "port", "replicas"],
  "properties": {
    "service_name": { "type": "string", "minLength": 3 },
    "port": { "type": "integer", "minimum": 1, "maximum": 65535 },
    "replicas": { "type": "integer", "minimum": 1, "maximum": 100 }
  },
  "additionalProperties": false
}
EOF

cat << 'EOF' > valid.json
{
  "service_name": "auth-service",
  "port": 8080,
  "replicas": 3
}
EOF
```

### Step 2: Run Validation via Python
```python
import json
import jsonschema

# Load schema and payload
schema = json.load(open("schema.json"))
valid_data = json.load(open("valid.json"))

# Validate
try:
    jsonschema.validate(instance=valid_data, schema=schema)
    print("✓ Payload conforms strictly to JSON Schema!")
except jsonschema.exceptions.ValidationError as e:
    print(f"✗ Validation Error: {e.message}")
```

---

## Lab 3: Cross-Format Translations with `yq`

### Objective
Convert structured data seamlessly across JSON, YAML, TOML, and XML using `yq`.

### Commands to Execute
```bash
# 1. Convert JSON to YAML
yq -p=json -o=yaml valid.json > converted.yaml
cat converted.yaml

# 2. Convert YAML to TOML
yq -p=yaml -o=toml converted.yaml > converted.toml
cat converted.toml

# 3. Convert TOML back to formatted JSON
yq -p=toml -o=json converted.toml
```

---

## Lab 4: In-Place Manifest Modification for CI/CD

### Objective
Simulate a continuous deployment pipeline step updating container image tags in a Kubernetes deployment manifest without manual editing.

```bash
cat << 'EOF' > deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: payments-api
spec:
  replicas: 2
  template:
    spec:
      containers:
        - name: app
          image: payments-api:v1.0.0
EOF

# Update container image in-place (-i) during CI/CD build:
NEW_TAG="payments-api:v1.2.4"
yq -i ".spec.template.spec.containers[0].image = \"${NEW_TAG}\"" deployment.yaml

# Verify modification:
cat deployment.yaml
```

---

## Lab 5: Value-Level Encryption with Mozilla SOPS and `age`

### Objective
Encrypt sensitive values inside a YAML file while retaining plaintext keys for Git version control.

### Step 1: Install and Generate `age` Key
```bash
# Generate age encryption key pair
age-keygen -o key.txt
export SOPS_AGE_KEY_FILE=$(pwd)/key.txt
AGE_PUBLIC_KEY=$(grep "public key:" key.txt | cut -d: -f2 | tr -d ' ')
```

### Step 2: Encrypt YAML Secret
```bash
cat << 'EOF' > raw-secret.yaml
apiVersion: v1
kind: Secret
metadata:
  name: api-keys
data:
  stripe_key: "sk_live_98124018491204"
  db_password: "production_super_secret"
EOF

# Encrypt with SOPS using age public key:
sops --encrypt --age "${AGE_PUBLIC_KEY}" raw-secret.yaml > secret.enc.yaml

# Inspect encrypted file:
cat secret.enc.yaml
# Notice: 'name' and 'apiVersion' are readable, but values in 'data' are encrypted!
```

### Step 3: Decrypt On-The-Fly
```bash
# Decrypt stream to stdout:
sops --decrypt secret.enc.yaml
```

Clean up:
```bash
cd /tmp && rm -rf /tmp/formats_lab
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 11 - Interview QA](./11-Interview-QA.md) | [Index](../../../README.md) | [13 - MCQ →](./13-MCQ.md) |
