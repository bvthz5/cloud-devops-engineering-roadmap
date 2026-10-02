# 07 — Configuration Management, Environments, and Templating

---

## 1. The Twelve-Factor App: Factor III (Config)

The gold standard for modern cloud-native software architecture—the **Twelve-Factor App**—mandates a strict separation of configuration from code:

> **The Twelve-Factor Methodology (Factor III):**  
> *"Store config in the environment."*  
> Code must be completely neutral across environments. A codebase should be capable of being made open source at any moment without compromising credentials, database endpoints, or internal URLs.

---

## 2. Configuration vs Secrets: A Crucial Security Boundary

One of the most dangerous anti-patterns in DevOps is treating application configuration and cryptographic secrets as the same entity.

```text
+-------------------------------------------------------------+
|              Configuration vs Secrets Distinction           |
+-------------------------------------------------------------+
| Dimension       | Configuration           | Secrets         |
+-----------------+-------------------------+-----------------+
| Definition      | Non-sensitive parameters| Highly sensitive|
|                 | governing application   | authentication  |
|                 | behavior & endpoints    | credentials     |
| Examples        | Log level, PORT, DB_HOST| DB passwords,   |
|                 | cache TTL, timeout      | API tokens, TLS |
| Stored In Git?  | **YES** (Plaintext Git) | **NEVER in Git**|
| Access Control  | Accessible to all devs  | Strict RBAC     |
| Rotation Impact | Application restart     | Key revocation  |
+-------------------------------------------------------------+
```

---

## 3. Environment-Specific Configuration Lifecycle

Modern software progresses through a series of isolated deployment environments before reaching end users:

```text
[ Development ] ──► [ Testing / QA ] ──► [ Staging (Pre-Prod) ] ──► [ Production ]
Local Docker        CI/CD Runners        Exact Prod Replica          Real Customers
Low resources       Automated tests      Performance testing         High Availability
Verbose debug logs  Ephemeral fixtures   Sanitized real data         Minimal error logs
```

### Environment Configuration Tier Matrix

| Parameter | Development (`dev`) | Staging (`stage`) | Production (`prod`) |
| :--- | :--- | :--- | :--- |
| **Log Level** | `DEBUG` / `TRACE` | `INFO` | `WARN` / `ERROR` |
| **Database Host** | `localhost:5432` | `stage-db.internal` | Multi-AZ Cloud RDS Cluster |
| **Replicas** | `1` | `2` | `10` (with HPA autoscaling) |
| **SSL / TLS** | Self-signed / disabled | Let's Encrypt staging | Public CA Certificate |
| **Telemetry** | Local console | Test Datadog agent | Production APM & PagerDuty |

---

## 4. Configuration Templating Engines

Rather than duplicating 500-line YAML files across four environments (which leads to configuration drift), DevOps engineers use **Configuration Templating Engines** to generate environment-specific manifests from a single parameterized template.

```text
Parameter Values (values-prod.yaml)  +  Base Template (deployment.yaml.j2)
                               │
                               ▼ Templating Engine (Helm / Jinja2 / envsubst)
                     Generated Manifest (YAML)
                               │
                               ▼
                    Kubernetes API / Server
```

---

## 5. Templating Technologies in Depth

### 1. `envsubst` (Lightweight Shell Variable Interpolation)
The simplest POSIX-compliant templating tool. It reads a template from `stdin` and replaces shell variables with active environment values:

```bash
# Template: nginx.conf.template
cat << 'EOF' > nginx.conf.template
server {
    listen ${PORT};
    server_name ${DOMAIN};
}
EOF

# Render template using active environment variables:
export PORT=8080
export DOMAIN="api.prod.org"
envsubst '${PORT} ${DOMAIN}' < nginx.conf.template > nginx.conf
```

### 2. Jinja2 (Python / Ansible Standard)
Used extensively in Ansible and Python infrastructure automation. Supports loops, conditionals, and filters:
```jinja2
# template.yaml.j2
database:
  host: {{ db_host }}
  port: {{ db_port | default(5432) }}
  {% if environment == "production" %}
  pool_size: 50
  ssl_mode: require
  {% else %}
  pool_size: 5
  ssl_mode: disable
  {% endif %}
```

### 3. Helm (The Kubernetes Package Manager)
Combines Go text templates with YAML parameter values:
```yaml
# templates/deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: {{ .Release.Name }}-api
spec:
  replicas: {{ .Values.replicaCount }}
  template:
    spec:
      containers:
        - name: app
          image: "{{ .Values.image.repository }}:{{ .Values.image.tag }}"
```

---

## 6. Configuration Validation & Linting Pipeline

Automated linting ensures configuration files are syntactically and semantically valid before they reach production:

```bash
# 1. YAML syntax and styling validation
yamllint -c .yamllint.yaml deployment.yaml

# 2. Kubernetes schema conformance testing
kubeconform -strict -summary deployment.yaml

# 3. JSON Schema validation
ajv validate -s schema.json -d data.json
```

---

## 7. Configuration Best Practices Checklist

- [ ] **Single Source of Truth:** All infrastructure configuration is committed to Git (GitOps).
- [ ] **No Hardcoded Endpoints:** All external URLs, ports, and feature flags are parameterized.
- [ ] **Zero Secrets in Git:** Plaintext passwords, tokens, and private keys are never committed.
- [ ] **Schema Validation in CI/CD:** Every pull request runs automated schema validation (`yamllint`, `kubeconform`).
- [ ] **Immutable Infrastructure:** Containers and VMs are not modified in-place; configuration changes trigger new rolling deployments.
