# CI/CD Pipelines & Automation Scenarios: Architecture & Scaling Scenarios

> **Interview Focus:** Real-world incident response, root-cause analysis (RCA), diagnostic toolchains, and articulate candidate verbal pitches.

---

## 📌 Scenario 4: Dependency Vulnerability Scan (Trivy/Snyk) Fails CI Build on Zero-Day CVE

### 🚨 The Production Scenario
All developer pull request builds fail at the security scan stage because Trivy flags a critical CVE in a transitive Linux package (e.g., openssl or glibc) for which no upstream patch is yet released.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
The CI pipeline enforces a strict security gate: 'exit-code 1 if severity == CRITICAL'. When a zero-day vulnerability is published in the CVE database without an available vendor patch, the pipeline completely blocks all organizational deployments.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Evaluate vulnerability exploitability in the application's runtime context using VEX (Vulnerability Exploitability eXchange) or CVSS vector analysis.
- If the vulnerability is non-exploitable in the current deployment, configure a temporary, time-bound Trivy ignore rule (.trivyignore) with a documented Jira ticket reference.
- Require DevSecOps team approval for any security gate exception.
- Update base container images to minimal distroless or Wolfi images to dramatically reduce CVE surface area.
- Schedule daily automated scans to alert immediately when an official vendor patch becomes available.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Run Trivy scan filtering out vulnerabilities without available upstream patches
trivy image --severity CRITICAL --ignore-unfixed my-app:latest

# Generate structured vulnerability report for security review
trivy image --vuln-type os,library --format json -o vuln-report.json my-app:latest

# Add documented exception waiver
echo 'CVE-2026-1234 # Temporarily waived by Security Team until upstream patch release - SEC-982' >> .trivyignore

# Verify image cryptographic signature and provenance
cosign verify --key cosign.pub my-app:latest

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "A rigid security gate that blocks unfixable upstream zero-days halts business delivery. I configure vulnerability scanners with '--ignore-unfixed' for standard developer PRs while maintaining a separate daily security audit. For confirmed non-exploitable zero-days, we implement a documented, time-bounded .trivyignore exception requiring DevSecOps approval, and migrate base images to Chainguard or distroless."

---

## 📌 Scenario 5: Multi-Architecture Docker Builds (amd64 / arm64) Slow Pipeline to 45 Minutes

### 🚨 The Production Scenario
After introducing AWS Graviton (ARM64) nodes to reduce EC2 costs, the CI/CD pipeline builds multi-arch Docker images using QEMU emulation. Build times balloon from 4 minutes to 45 minutes, creating a massive deployment bottleneck.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
QEMU user-mode emulation translates CPU instructions in software, incurring an 8x to 10x performance penalty for CPU-heavy compilation tasks (e.g., C/Go compilation, npm build) on x86 CI runners.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Eliminate QEMU software emulation by utilizing native multi-architecture builder nodes.
- Configure Docker Buildx with a remote builder node pool containing both native amd64 and native arm64 instances.
- Leverage cross-compilation toolchains (e.g., Go `GOARCH=arm64`, Rust `target`) where binary compilation runs natively on x86 host without QEMU.
- Implement layer caching using GitHub Actions cache (`type=gha`) or Amazon ECR remote cache (`type=registry`).
- Parallelize architecture builds and merge manifests using `docker buildx imagetools create`.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Create native AMD64 builder instance
docker buildx create --name native-builder --driver docker-container --platform linux/amd64

# Attach native ARM64 worker node to builder pool
docker buildx create --name native-builder --append --driver docker-container --platform linux/arm64 --node arm-builder ssh://ubuntu@arm-runner.internal

# Execute high-speed native multi-arch build with remote registry cache
docker buildx build --builder native-builder --platform linux/amd64,linux/arm64 -t my-app:latest --cache-to type=registry,ref=repo/cache --cache-from type=registry,ref=repo/cache --push .

# Verify multi-architecture manifest list
docker buildx imagetools inspect my-app:latest

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "QEMU software emulation is completely impractical for production CI/CD because of CPU instruction translation overhead. I replace QEMU with Docker Buildx configured across native heterogenous worker nodes—one x86 runner and one ARM64 runner. Each platform compiles natively at full hardware speed, cutting multi-arch build times from 45 minutes down to 3 minutes."

---

## 📌 Scenario 6: Pipeline Supply Chain Attack - NPM Dependency Typosquatting

### 🚨 The Production Scenario
A CI/CD build automatically pulls a newly introduced NPM package whose name differed by one letter from an internal utility. During the build, an obfuscated post-install script exfiltrates pipeline environment variables and AWS credentials.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
The project lacked a package-lock.json integrity check, and the CI environment permitted unrestricted outbound internet access. The developer mistakenly installed a malicious typosquatted package that executed arbitrary code during npm install.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Immediately revoke all secrets and tokens exposed in the compromised build environment.
- Enforce 'npm ci --ignore-scripts' in CI pipelines to prevent arbitrary code execution during dependency resolution.
- Configure private package proxy/firewall (e.g., Sonatype Nexus, JFrog Artifactory, AWS CodeArtifact) with repository firewalls blocking unvetted packages.
- Enforce package lockfile immutability and commit verification.
- Isolate CI build runners in private subnets with restricted egress proxy filtering.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Install dependencies deterministically without executing malicious post-install lifecycle scripts
npm ci --ignore-scripts

# Perform dependency security audit
npm audit --audit-level=high

# Scan repository filesystem and lockfiles for malicious packages
pip-audit || trivy fs --scanners vuln .

# Cryptographically sign build image to establish verifiable provenance
cosign sign --key cosign.key my-secure-image:v1.0.0

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "Supply chain attacks exploit default trust in package managers. I enforce three non-negotiable pipeline controls: first, use 'npm ci --ignore-scripts' so third-party packages cannot execute shell scripts during install; second, route all dependency fetches through an enterprise proxy like Artifactory with an active package firewall; third, use OIDC short-lived tokens so compromised build environments expose no static credentials."

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← CI/CD Pipelines & Automation Scenarios: Production Incidents & Triage Scenarios](01-Production-Incidents-and-Triage-Scenarios.md) | [Index](../../../README.md) | [CI/CD Pipelines & Automation Scenarios: Security & Disaster Recovery Scenarios →](03-Security-and-Disaster-Recovery-Scenarios.md) |

