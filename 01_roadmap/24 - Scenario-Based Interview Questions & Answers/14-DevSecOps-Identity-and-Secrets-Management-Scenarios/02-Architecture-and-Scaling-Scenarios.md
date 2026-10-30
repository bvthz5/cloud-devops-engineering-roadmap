# DevSecOps, Identity & Secrets Management Scenarios: Architecture & Scaling Scenarios

> **Interview Focus:** Real-world incident response, root-cause analysis (RCA), diagnostic toolchains, and articulate candidate verbal pitches.

---

## 📌 Scenario 4: AWS IAM Role Credential Exfiltration via Container SSRF Attack

### 🚨 The Production Scenario
A vulnerable microservice with a Server-Side Request Forgery (SSRF) flaw allows an external attacker to query the AWS EC2 Instance Metadata Service (IMDS) at `http://169.254.169.254/latest/meta-data/iam/security-credentials/`, exfiltrating the node's IAM role credentials.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
The EC2 worker nodes were running IMDSv1 (which allows simple HTTP GET requests without token authentication) with `HttpTokens=optional` and `HttpPutResponseHopLimit=2`, allowing pods running inside container network namespaces to reach the host metadata IP.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Immediately rotate and invalidate the exposed IAM role and session credentials via AWS IAM.
- Enforce IMDSv2 globally across all EC2 instances: set `HttpTokens=required` (requires session token via HTTP PUT).
- Set `HttpPutResponseHopLimit=1` on EC2 worker nodes so that packets crossing the container network namespace boundary (hop > 1) are immediately dropped by the IP stack.
- Enforce EKS IAM Roles for Service Accounts (IRSA) or EKS Pod Identities, removing sensitive IAM permissions from the worker node instance profile entirely.
- Block pod access to 169.254.169.254 via Calico / Cilium egress NetworkPolicies.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Enforce IMDSv2 and set hop limit to 1 to block container SSRF
aws ec2 modify-instance-metadata-options --instance-id i-0123456789abcdef0 --http-tokens required --http-put-response-hop-limit 1 --http-endpoint enabled

# Revoke active compromised IAM sessions
aws iam revoke-policy-sessions --role-name NodeInstanceRole

# Test IMDSv2 token acquisition requirement
curl -H 'X-aws-ec2-metadata-token-ttl-seconds: 60' -X PUT 'http://169.254.169.254/latest/api/token'

# Deploy Cilium/Calico policy blocking egress to 169.254.169.254
kubectl apply -f block-imds-networkpolicy.yaml

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "IMDS SSRF attacks are entirely preventable with two configurations: First, mandate IMDSv2 so an attacker cannot use simple GET requests. Second, set the EC2 HttpPutResponseHopLimit to 1. Because container networking represents an IP hop, the kernel drops the packet before it leaves the node network stack. Finally, pods must use IRSA or Pod Identities so the underlying node has zero IAM privileges to steal."

---

## 📌 Scenario 5: Kubernetes RBAC ClusterRole Privilege Escalation Vulnerability Discovered

### 🚨 The Production Scenario
A security audit discovers that a developer ServiceAccount was granted `create` and `patch` permissions on `pods/exec` and `rolebindings` at cluster scope, allowing any developer to elevate their privileges to full `cluster-admin`.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
Over-privileged wildcards (`*`) were used in custom ClusterRole definitions, violating the Principle of Least Privilege and enabling privilege escalation paths.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Audit all ClusterRoleBindings and RoleBindings using security audit tools like `rakkess` or `kube-bench`.
- Revoke wildcards and high-risk verbs (`impersonate`, `bind`, `escalate`, `create pods/exec`) from non-admin principals.
- Enforce namespace isolation: bind roles via RoleBinding (namespaced) rather than ClusterRoleBinding.
- Integrate automated RBAC auditing into CI pipelines using tools like `audit2rbac` and `kube-linter`.
- Implement Just-In-Time (JIT) access management with audit trails for privileged operations.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Audit complete effective permissions of the target ServiceAccount
kubectl auth can-i --list --as=system:serviceaccount:dev:developer-sa

# Generate visual access matrix of ServiceAccount permissions across all resources
rakkess --sa dev:developer-sa

# Revoke over-privileged ClusterRoleBinding
kubectl delete clusterrolebinding rogue-dev-binding

# List all entities granted cluster-admin access
kubectl get rolebindings,clusterrolebindings -A -o json | jq '.items[] | select(.roleRef.name=="cluster-admin") | .metadata.name'

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "In Kubernetes RBAC, granting 'pods/exec' or 'rolebindings' is effectively granting root access because a user can exec into privileged pods or bind themselves to cluster-admin. I audit permissions using 'rakkess' and 'kubectl auth can-i', eliminate wildcard rules, restrict developer roles strictly to namespaced scopes, and implement automated RBAC linters in CI to block over-privileged role creation."

---

## 📌 Scenario 6: Vault Dynamic Database Credentials Revocation Storm Crashes PostgreSQL

### 🚨 The Production Scenario
Every 15 minutes, PostgreSQL database active connections hit max_connections (1,000), rejecting all new application connections. PostgreSQL logs report hundreds of `DROP USER` statements executing simultaneously.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
Vault dynamic secret backend was configured with a short Time-To-Live (TTL = 15m) for database credentials. Hundreds of microservice pods requested individual dynamic users. When the lease expired, Vault attempted to drop hundreds of database roles simultaneously while active queries were running, creating catalog lock contention in Postgres.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Increase Vault database secret lease TTL to a sustainable duration (e.g., 24 hours).
- Implement Vault secret caching and connection pooling (e.g., PgBouncer) so pods share pooled connections rather than generating unique database users per pod.
- Configure Vault to revoke credentials gracefully with staggered revocation windows.
- Tune PostgreSQL `max_connections` and deploy PgBouncer in transaction pooling mode.
- Monitor Vault lease counts in Prometheus (`vault.expire.leases.active`).

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Check total active dynamic secret leases managed by Vault
vault read sys/leases/count

# Tune Vault database engine lease TTL and connection limits
vault write database/config/postgresql max_open_connections=50 default_lease_ttl=24h max_lease_ttl=72h

# Revoke expired lease prefix in controlled batches
vault lease revoke -prefix database/creds/order-service

# Check connection distribution by Vault dynamic username in Postgres
SELECT count(*), usename FROM pg_stat_activity GROUP BY usename;

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "Vault dynamic secrets are powerful, but short TTLs combined with hundreds of pods generate thousands of short-lived database roles. When Vault revokes them, Postgres catalog locks choke the database. I resolve this by increasing lease TTLs to 24 hours and placing PgBouncer in front of Postgres. Microservices authenticate through pooled connections, keeping database role churn and catalog lock contention at zero."

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← DevSecOps, Identity & Secrets Management Scenarios: Production Incidents & Triage Scenarios](01-Production-Incidents-and-Triage-Scenarios.md) | [Index](../../../README.md) | [DevSecOps, Identity & Secrets Management Scenarios: Security & Disaster Recovery Scenarios →](03-Security-and-Disaster-Recovery-Scenarios.md) |

