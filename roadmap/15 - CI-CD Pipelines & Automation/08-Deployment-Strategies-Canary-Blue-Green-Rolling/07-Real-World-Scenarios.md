# Real-World Scenarios - Deployment Strategies (Canary, Blue-Green, Rolling)

> **Module**: Deployment Strategies (Canary, Blue-Green, Rolling)

---

## 🏢 Scenario 1: Zero-Downtime Blue-Green Deployment for Mission-Critical Microservice

### Background
A payment processing platform requires zero downtime during software updates. Traditional rolling updates risk partial transaction failures if temporary version incompatibility occurs.

### Solution Architecture
1. **Blue-Green Provisioning**: Maintain duplicate production environments (Blue = active traffic, Green = idle new release).
2. **Staging Smoke Testing**: Deploy new container release to the Green environment and execute automated integration tests without affecting live users.
3. **Instant Router Switchover**: Update ALB/Ingress routing rules to direct 100% of user traffic to Green within milliseconds. Keep Blue alive for 1 hour for instant zero-downtime rollback capability.

---

## 🏢 Scenario 2: Centralized Artifact Management & Air-Gapped Dependency Mirroring

### Background
A enterprise financial institution requires strict air-gapped security. Build servers cannot access public internet package repositories (`npm`, `PyPI`, `Maven Central`).

### Solution Architecture
1. **JFrog Artifactory / Sonatype Nexus Proxy**: Setup internal proxy repositories connected to approved public mirrors with automated vulnerability scanning (Xray/Nexus IQ).
2. **Security Gating**: Automatically quarantine dependencies containing High/Critical CVEs before developers or CI runners can download them.
3. **CI Pipeline Integration**: Configure `.npmrc`, `settings.xml`, and `pip.conf` across all build runners to enforce routing exclusively through the internal repository.
