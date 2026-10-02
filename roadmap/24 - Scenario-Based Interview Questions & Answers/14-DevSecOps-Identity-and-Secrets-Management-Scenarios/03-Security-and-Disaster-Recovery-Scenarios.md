# DevSecOps, Identity & Secrets Management Scenarios: Security & Disaster Recovery Scenarios

> **Interview Focus:** Real-world incident response, root-cause analysis (RCA), diagnostic toolchains, and articulate candidate verbal pitches.

---

## 📌 Scenario 7: Open Source Package Supply Chain Typosquatting in Production Base Image

### 🚨 The Production Scenario
A nightly container vulnerability scanner detects that a production container image deployed last week contains a malicious binary communicating with a known command-and-control (C2) IP address.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
The base Dockerfile used a public upstream base image tag (`node:latest`) that was poisoned, or a transitive dependency pulled from a public registry had been compromised via developer account takeover.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Immediately isolate the affected pods using NetworkPolicies to sever all outbound network connectivity.
- Deploy the previous verified clean container image release.
- Analyze container memory and filesystem forensics using Falco and runtime security tools.
- Enforce Software Bill of Materials (SBOM) generation (using Syft) and SBOM validation in the CI/CD pipeline.
- Migrate base images to hardened, enterprise-managed distroless base images (e.g., Google Distroless or Chainguard) with pinned sha256 digests.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Run Falco runtime detection engine to inspect suspicious network calls
falco -c /etc/falco/falco.yaml

# Generate Software Bill of Materials (SBOM) for container image
syft my-app:latest -o spdx-json=sbom.json

# Scan generated SBOM for known vulnerabilities and malicious dependencies
grype sbom:./sbom.json

# Pin base image by immutable SHA256 digest
docker pull node@sha256:4bc4264c1264c... # Enforce immutable digest pinning

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "Tagging base images like 'node:latest' is an open door to supply chain poisoning. I respond immediately by severing pod egress via Cilium NetworkPolicies and rolling back to a known clean digest. To prevent future incidents, I mandate two rules: every base image must be pinned by immutable SHA256 digest from a trusted internal registry, and every build must generate an SBOM with Syft that is verified before deployment."

---

## 📌 Scenario 8: Secrets Sprawl - Hardcoded Production Secrets Detected in Compiled Frontend Bundles

### 🚨 The Production Scenario
A security researcher reports that a compiled production React JavaScript bundle accessible to public web browsers contains AWS secret access keys and database credentials.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
A developer configured environment variables without the `REACT_APP_` or `NEXT_PUBLIC_` prefix guard, or imported a backend configuration file into a client-side component, causing Webpack/Vite to inline production environment variables directly into public JavaScript bundles.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Immediately revoke and rotate all compromised keys and credentials in AWS and database systems.
- Audit the client-side JavaScript codebase to identify imports referencing backend secret configs.
- Refactor code: backend secrets must strictly reside in backend API server runtimes, never accessible to client-side build tools.
- Implement secret scanning with GitGuardian or Gitleaks in CI pipelines to block builds containing secret patterns.
- Deploy a hotfix with clean bundle compilation.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Scan repository for leaked credentials and hardcoded secrets
gitleaks detect --source . --verbose

# Verify public frontend bundle for leaked AWS access keys
curl -s https://www.example.com/main.bundle.js | grep -E '(AKIA[0-9A-Z]{16})'

# Immediately deactivate compromised AWS credentials
aws iam update-access-key --access-key-id AKIAIOSFODNN7EXAMPLE --status Inactive

# Audit built frontend distribution files for secret leaks
npm run build && grep -rn 'SECRET' dist/

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "Frontend build tools like Webpack bundle everything imported in client code. If a backend config is imported, secrets get compiled into public JavaScript. I immediately revoke the leaked keys at the cloud provider level. Then, I enforce strict architectural separation: frontend code only accesses backend endpoints via secure HTTP-only cookies, and Gitleaks is integrated into CI to abort builds if API keys are detected."

---

## 📌 Scenario 9: TLS Certificate Expiration Blackout on Internal Microservice gRPC Traffic

### 🚨 The Production Scenario
At midnight, internal microservices suddenly fail to communicate over gRPC with `transport: authentication handshake failed: x509: certificate has expired`. Cross-service orders fail 100%.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
Internal microservices used custom mTLS certificates managed manually or generated with a 1-year validity period. No automated certificate manager (Cert-Manager) or certificate expiry alerting was configured.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Immediately issue new TLS certificates using internal CA or OpenSSL and reload microservices.
- Deploy Cert-Manager in Kubernetes to automate certificate lifecycle management.
- Integrate Cert-Manager with Let's Encrypt or HashiCorp Vault PKI backend for automated issuance and renewal.
- Configure certificate renewal at 30 days prior to expiration.
- Create Prometheus alerting rules on `certmanager_certificate_expiration_timestamp_seconds`.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Check expiration date of TLS certificate file
openssl x509 -in /etc/tls/tls.crt -noout -enddate

# List all Cert-Manager managed certificates in cluster
kubectl get certificates -A

# Inspect status of pending certificate requests
kubectl get certificaterequests -A

# Query certificates expiring within 14 days
curl -s http://prometheus:9090/api/v1/query?query='certmanager_certificate_expiration_timestamp_seconds - time() < 86400 * 14'

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "Manual TLS certificate management is a guarantee of future outages. After restoring connectivity with an emergency certificate re-issue, I deploy Cert-Manager integrated with Vault PKI. Cert-Manager continuously monitors certificate validity and automatically renews and rotates certificates 30 days before expiration, backed by Prometheus alerts that fire if any cert has less than 14 days of validity remaining."

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← DevSecOps, Identity & Secrets Management Scenarios: Architecture & Scaling Scenarios](02-Architecture-and-Scaling-Scenarios.md) | [Index](../../../README.md) | [DevSecOps, Identity & Secrets Management Scenarios: Rapid-Fire Drills & Master Cheat Sheet →](04-Rapid-Fire-Interview-Drills-and-Cheat-Sheet.md) |

