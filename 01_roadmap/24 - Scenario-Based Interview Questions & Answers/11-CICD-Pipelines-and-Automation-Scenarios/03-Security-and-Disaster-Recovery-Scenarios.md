# CI/CD Pipelines & Automation Scenarios: Security & Disaster Recovery Scenarios

> **Interview Focus:** Real-world incident response, root-cause analysis (RCA), diagnostic toolchains, and articulate candidate verbal pitches.

---

## 📌 Scenario 7: Database Migration Job in CI/CD Causes Production Table Lock & Downtime

### 🚨 The Production Scenario
An automated CI/CD pipeline deploys a database migration adding a column with a default value to a 50-million row Postgres table. The migration holds an ACCESS EXCLUSIVE lock, blocking all production read/write transactions and bringing down the web application.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
The migration script ran 'ALTER TABLE orders ADD COLUMN status VARCHAR(20) DEFAULT 'pending''. In older PostgreSQL versions (or on unindexed tables), this operation rewrites the table and holds an exclusive lock until completion, queuing thousands of incoming queries until connection pools exhausted.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Immediately identify and cancel the blocking ALTER TABLE statement via pg_stat_activity.
- Separate schema migrations from application binary deployments in CI/CD.
- Enforce zero-downtime migration guidelines: add nullable column first, backfill data in batches, then apply NOT NULL constraint with VALIDATE CONSTRAINT.
- Configure 'lock_timeout' and 'statement_timeout' in all automated migration scripts.
- Use tools like Liquibase, Flyway, or pg-roll for declarative, safe zero-downtime schema evolution.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Find blocking migration query in PostgreSQL
SELECT pid, query, state, age(clock_timestamp(), query_start) FROM pg_stat_activity WHERE state != 'idle' ORDER BY age DESC;

# Safely cancel blocking migration query to restore application traffic
SELECT pg_cancel_backend(<PID>);

# Execute migration with strict lock timeout to prevent queue pile-up
SET lock_timeout = '3s'; ALTER TABLE orders ADD COLUMN status VARCHAR(20);

# Check schema migration history status
flyway info -url=jdbc:postgresql://db:5432/app

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "Coupling database migrations directly with container deployment in CI/CD without lock safeguards is a major operational risk. I separate database migrations into an independent, canary-gated pipeline stage. All SQL migrations must include 'SET lock_timeout = 3s' and adhere to expand-contract patterns: add nullable columns first, backfill asynchronously in micro-batches, and add constraints without table rewrites."

---

## 📌 Scenario 8: Flaky Integration Tests Stall Pull Request Velocity by 30%

### 🚨 The Production Scenario
Over 30% of pull request CI runs fail due to intermittent Selenium/Cypress UI and API integration test failures. Developers waste hours re-running pipelines, eroding confidence in CI test gates.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
Tests relied on hardcoded sleep timers (`time.sleep(5)`) rather than explicit event-driven condition waits, shared shared state across parallel test threads, and communicated directly with external third-party sandbox APIs subject to rate-limiting.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Implement automated test telemetry to track flakiness metrics per test suite over time.
- Replace static thread sleeps with explicit condition polling (e.g., waitForSelector, waitUntilPresent).
- Mock external third-party dependencies using WireMock, Prism, or ephemeral mock containers.
- Isolate test environments: provision dedicated ephemeral database containers per test runner thread using Testcontainers.
- Quarantine flaky tests to an asynchronous non-blocking test suite while engineering teams refactor them.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Identify slowest and frequently failing tests in Python suites
pytest --durations=10 --maxfail=3

# Launch lightweight mock server to isolate integration tests from external APIs
docker run -d -p 8080:8080 wiremock/wiremock:latest

# Audit history of flaky test patch attempts
git log --grep='fix: flaky' -n 10 --oneline

# Run quarantined flaky tests independently
gh workflow run pr-checks.yaml -f test_suite=quarantined

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "Flaky tests destroy developer velocity and build trust. I isolate tests by replacing shared staging environments with ephemeral, isolated dependencies using Testcontainers. We replace hardcoded sleeps with explicit polling assertions, mock external third-party APIs, and implement an automated quarantine policy: any test that fails intermittently twice in 48 hours is moved out of the blocking PR path until repaired."

---

## 📌 Scenario 9: CI Pipeline Fails with 'Docker Hub Pull Rate Limit Exceeded (HTTP 429)'

### 🚨 The Production Scenario
During a high-concurrency deployment window, dozens of CI/CD pipelines across the enterprise fail with: 'toomanyrequests: You have reached your unauthenticated pull rate limit of 100 requests every 6 hours'.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
CI worker nodes were pulling public base images directly from Docker Hub without authentication. Because the entire office and NAT gateway shared a single public outbound IP, the unauthenticated limit was exhausted within minutes.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Configure authenticated Docker Hub credentials or upgrade to enterprise subscriptions.
- Mirror critical public base images (e.g., node, python, alpine, golang) into private internal registries (AWS ECR, Azure ACR, or Harbor).
- Implement an in-cluster caching pull-through proxy (e.g., Harbor proxy cache or ECR Public pull-through cache).
- Update all Dockerfiles to reference internal registry mirrors rather than upstream public hubs.
- Implement image scanning and vulnerability lifecycle management in the internal mirror.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Mirror public image to private enterprise registry
crane copy docker.io/library/alpine:3.19 internal-registry.corp/library/alpine:3.19

# Configure AWS ECR pull-through cache for Docker Hub
aws ecr create-pull-through-cache-rule --ecr-repository-prefix docker-hub --upstream-registry-url registry-1.docker.io

# Pull base image through enterprise caching proxy
docker pull internal-registry.corp/docker-hub/library/python:3.12-slim

# Check remaining Docker Hub pull rate limit headers
curl -sI https://registry-1.docker.io/v2/ratelimitpreview/test/manifests/latest

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "Relying on direct Docker Hub pulls in enterprise CI/CD is an operational fragility anti-pattern. I implement an enterprise pull-through cache proxy using AWS ECR or Harbor. Pipelines only reference the internal cache. Images are fetched once, stored internally, and served locally with zero rate limits, sub-second pull speeds, and automatic vulnerability scanning."

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← CI/CD Pipelines & Automation Scenarios: Architecture & Scaling Scenarios](02-Architecture-and-Scaling-Scenarios.md) | [Index](../../../README.md) | [CI/CD Pipelines & Automation Scenarios: Rapid-Fire Drills & Master Cheat Sheet →](04-Rapid-Fire-Interview-Drills-and-Cheat-Sheet.md) |

