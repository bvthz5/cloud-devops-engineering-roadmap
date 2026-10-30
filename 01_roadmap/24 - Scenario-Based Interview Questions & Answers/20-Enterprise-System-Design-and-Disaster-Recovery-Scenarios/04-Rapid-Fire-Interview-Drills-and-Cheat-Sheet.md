# Enterprise System Design & Disaster Recovery Scenarios: Rapid-Fire Drills & Master Cheat Sheet

> **Interview Focus:** Real-world incident response, root-cause analysis (RCA), diagnostic toolchains, and articulate candidate verbal pitches.

---

## 📌 Scenario 10: Enterprise Zero-Trust Architecture Migration Breaks Legacy Service-to-Service APIs

### 🚨 The Production Scenario
Following the implementation of a company-wide Zero-Trust Network Access (ZTNA) policy mandating mutual TLS and OAuth2 client credentials for every internal hop, 25 legacy internal batch microservices fail with `401 Unauthorized`.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
Legacy services relied on perimeter network security (private subnet IP whitelisting) and lacked OAuth2 token acquisition logic or dynamic mTLS client certificates.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Deploy Envoy sidecar proxies or SPIFFE/SPIRE agents alongside legacy services to handle mutual TLS and OAuth2 token injection transparently at Layer 4/7.
- The sidecar intercept outbound requests, acquires an OAuth2 token from the identity provider (Okta/Keycloak/Auth0), injects the `Authorization: Bearer <TOKEN>` header, and initiates mTLS.
- Configure migration grace period with SPIFFE ID verification in permissive mode.
- Validate service identity and RBAC authorization policies.
- Retire legacy IP whitelisting once 100% of internal traffic is verified via cryptographic identity.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Verify SPIRE agent health and workload attestation status
spire-agent healthcheck

# Inspect SPIFFE identity registration entry
spire-server entry show -spiffeID spiffe://corp.internal/ns/prod/sa/legacy-service

# Test mutual TLS authentication with SPIFFE X.509 SVID
curl -v -k --cert /etc/spire/certs/svid.crt --key /etc/spire/certs/svid.key https://internal-api.corp/orders

# Inspect service mesh authorization policies enforcing zero-trust
kubectl get authorizationpolicies -A

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "Zero-trust mandates that network locality never implies trust. You cannot rewrite 25 legacy services overnight to handle OAuth2 and mTLS. I solve this using the sidecar pattern with SPIFFE/SPIRE and Envoy. The sidecar attests the workload's cryptographic identity, handles certificate rotation, and injects OAuth tokens transparently, achieving complete Zero-Trust compliance with zero application code changes."

---

## ⚡ Rapid-Fire Diagnostic Cheat Sheet

| Symptom / Scenario | First Command to Run | Root Cause Hypothesis |
| :--- | :--- | :--- |
| **Scenario 1: Active-Active Multi-Region Database Write Conflict Resolution** | `aws dynamodb describe-global-table --global-table-name Customers` | Multi-master active-active replication without deterministic conflict resolution... |
| **Scenario 2: Ransomware Infection Recovery - Cold Storage Air-Gapped Vault Restore** | `aws backup describe-backup-vault --backup-vault-name AirGappedComplianceVault` | Backups were accessible through standard enterprise IAM credentials and stored o... |
| **Scenario 3: Zero-Downtime Data Center to Cloud Database Migration (10TB Data)** | `aws dms describe-replication-tasks --filters Name=replication-task-id,Values=db-migration-task` | A simple offline backup-and-restore would require 18 hours of downtime to dump, ... |
| **Scenario 4: Regional Blackout Recovery - RTO < 5 Minutes, RPO < 1 Minute Across Regions** | `aws rds failover-global-cluster --global-cluster-identifier global-db --target-db-cluster-identifier-arn arn:aws:rds:us-west-2:123:cluster:aurora-west` | Failover failed in past drills because secondary infrastructure was deployed col... |
| **Scenario 5: Global Anycast BGP Route Leaks Cause Latency Spikes for 50% of Global Users** | `whois -h whois.radb.net '198.51.100.0/24'` | A transit ISP accidentally leaked BGP autonomous system (AS) routes, advertising... |
| **Scenario 6: Microservices Circuit Breaker Thundering Herd Post-Outage Recovery** | `kubectl get destinationrules payment-service -o yaml \| grep -A 10 outlierDetection` | Circuit breakers lacked a 'Half-Open' rate-limiting throttle and retry requests ... |
| **Scenario 7: Distributed Cache-Aside Race Condition & Dirty Reads in Financial Ledger** | `redis-cli del user_balance:12345` | A race condition between database update and cache invalidation: Thread A update... |
| **Scenario 8: API Gateway DDoS Protection Saturation - Layer 7 Slowloris Attack** | `ss -tan state syn-recv \| wc -l` | A Layer 7 Slowloris DDoS attack: attackers opened thousands of HTTP connections ... |
| **Scenario 9: Bulk Data Ingestion Pipeline Memory Exhaustion via Unbounded Outbox Pattern** | `SELECT count(*), status FROM outbox_events GROUP BY status;` | The outbox polling query executed `SELECT * FROM outbox_events WHERE status = 'p... |
| **Scenario 10: Enterprise Zero-Trust Architecture Migration Breaks Legacy Service-to-Service APIs** | `spire-agent healthcheck` | Legacy services relied on perimeter network security (private subnet IP whitelis... |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Enterprise System Design & Disaster Recovery Scenarios: Security & Disaster Recovery Scenarios](03-Security-and-Disaster-Recovery-Scenarios.md) | [Index](../../../README.md) | [Complete (Roadmap Home) →](../../../README.md) |

