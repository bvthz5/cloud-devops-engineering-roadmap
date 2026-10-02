# Data Formats — YAML, JSON, XML & TOML Core Concepts

## 1. Serialization & Deserialization

- **Serialization:** Converting memory objects into structured text formats (JSON, YAML, TOML) for network transmission or disk storage.
- **Deserialization:** Parsing text config back into memory objects.

```text
Memory Object / Struct  ─── Serialization ───>  YAML / JSON String
Memory Object / Struct  <── Deserialization ───  YAML / JSON String
```

---

## 2. JSON (JavaScript Object Notation)

JSON is strict, compact, and ideal for machine-to-machine APIs.

### Supported Data Types
1. **Object:** `{ "key": "value" }`
2. **Array:** `[ 1, 2, 3 ]`
3. **String:** `"text"` (Must use double quotes!)
4. **Number:** `42` or `3.14`
5. **Boolean:** `true` or `false`
6. **Null:** `null`

### Strict JSON Rules
- Keys must be wrapped in double quotes `"key"`.
- No trailing commas allowed (`{ "a": 1, }` is invalid!).
- Standard JSON does NOT support comments (`//` or `/* */`).

```json
{
  "application": "payment-service",
  "version": 1.2,
  "ports": [8080, 8081],
  "production": true,
  "tls_config": null
}
```

---

## 3. YAML (YAML Ain't Markup Language)

YAML is whitespace-sensitive, human-readable, and the industry standard for Kubernetes, Ansible, and Docker Compose.

### Key Rules & Syntax
- **Indentation:** Uses spaces (typically 2 spaces). Tabs are STRICTLY FORBIDDEN!
- **Lists / Sequences:** Prefixed with `- `.
- **Multiline Strings:**
  - `|` (Literal block): Preserves newlines.
  - `>` (Folded block): Replaces newlines with spaces.
- **Boolean Gotchas:** Always quote country codes like `"NO"` so YAML doesn't parse it as `false`!

```yaml
# Kubernetes Deployment Manifest Example
apiVersion: apps/v1
kind: Deployment
metadata:
  name: web-app
spec:
  replicas: 3
  selector:
    matchLabels:
      app: web
  template:
    metadata:
      labels:
        app: web
    spec:
      containers:
        - name: nginx
          image: nginx:1.25-alpine
          ports:
            - containerPort: 80
          env:
            - name: ENV_NAME
              value: "production"
          command:
            - /bin/sh
            - -c
          args:
            - |
              echo "Starting Nginx..."
              nginx -g 'daemon off;'
```

---

## 4. XML (Extensible Markup Language)

Verbose, tag-based markup format using elements and attributes. Used extensively in legacy enterprise Java apps (`pom.xml`), SOAP APIs, and Android builds.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration xmlns="http://example.com/schema">
    <application name="auth-service" port="8443">
        <database type="postgresql">
            <host>db.internal</host>
            <port>5432</port>
        </database>
    </application>
</configuration>
```

---

## 5. TOML (Tom's Obvious Minimal Language)

Explicit, human-friendly format organized into tables `[table]` and arrays of tables `[[table]]`. Popular in Rust (`Cargo.toml`), Python (`pyproject.toml`), and Hugo.

```toml
[package]
name = "devops-tool"
version = "0.1.0"
authors = ["Binil <binil@example.com>"]

[database]
server = "192.168.1.1"
ports = [ 8001, 8002, 8003 ]
enabled = true

[[services]]
name = "frontend"
port = 3000

[[services]]
name = "backend"
port = 8080
```

---

## 6. Comprehensive Format Comparison Matrix

| Feature | YAML | JSON | TOML | XML |
|---|---|---|---|---|
| **Human Readability** | Extremely High | High | Very High | Moderate |
| **Comments Support** | Yes (`#`) | No standard | Yes (`#`) | Yes (`<!-- -->`) |
| **Indentation Rule** | Spaces (No Tabs) | Formatting flexible | Formatting flexible | Formatting flexible |
| **DevOps Adoption** | Kubernetes, Ansible, CI/CD | APIs, CloudTrail, Config | Cargo, Pyproject | Java, Maven, Legacy |
| **Parsing Overhead** | Higher (Complex) | Ultra-Fast | Fast | Moderate to High |

---

## 7. Configuration vs. Secrets Management

- **Normal Config:** Non-sensitive operational values (`PORT=8080`, `LOG_LEVEL=debug`, `REPLICAS=3`). Stored directly in Git repository manifests.
- **Sensitive Secrets:** Passwords, API keys, private certificates, DB connection strings. **NEVER COMMIT TO GIT!**
- **Protection Tools:** Kubernetes Secrets (base64 encoded, encrypt at rest), HashiCorp Vault, AWS Secrets Manager, Azure Key Vault, GCP Secret Manager, Sealed Secrets.
