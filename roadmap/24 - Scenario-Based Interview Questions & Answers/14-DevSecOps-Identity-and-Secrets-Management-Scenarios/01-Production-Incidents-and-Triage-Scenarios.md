# DevSecOps, Identity & Secrets Management Scenarios: Production Incidents & Triage Scenarios

> **Interview Focus:** Real-world incident response, root-cause analysis (RCA), diagnostic toolchains, and articulate candidate verbal pitches.

---

## 📌 Scenario 1: HashiCorp Vault Cluster Sealed After Network Partition & Storage Outage

### 🚨 The Production Scenario
Following an unexpected restart of the underlying storage backend (Consul or Integrated Raft), the Vault cluster enters 'Sealed' state across all nodes. All microservices fail to retrieve database credentials and crash.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
Vault initializes in a sealed state by design to protect encryption keys in memory. When the storage or host restarts, Vault cannot access the barrier encryption key without unsealing. If Auto-Unseal via Cloud KMS (AWS KMS, Azure Key Vault, GCP KMS) is not configured, Vault remains completely locked until Shamir keys are manually entered.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Verify Vault seal status using `vault status` across all cluster nodes.
- If using manual Shamir keys, gather quorum key holders to run `vault operator unseal`.
- To eliminate manual dependency in production, configure Auto-Unseal with AWS KMS, Azure Key Vault, or GCP Cloud KMS.
- Verify storage connectivity: ensure Raft consensus is healthy via `vault operator raft list-peers`.
- Verify Vault active node status and resume application secret leasing.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Check Vault seal status, cluster name, and storage engine
vault status

# Submit Shamir key shard to unseal Vault
vault operator unseal <UNSEAL_KEY_PORTION>

# Inspect integrated Raft storage consensus peers and leader
vault operator raft list-peers

# Check Vault pod status in Kubernetes
kubectl get pods -l app.kubernetes.io/name=vault -n vault

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "A sealed Vault is an immediate P0 outage because no application can read secrets. For immediate recovery, we unseal using quorum Shamir keys. The permanent architectural fix is Auto-Unseal: we delegate barrier unsealing to AWS KMS or Azure Key Vault via IAM. When a node restarts, it automatically requests KMS decryption and unseals in milliseconds with zero human intervention."

---

## 📌 Scenario 2: Kyverno / OPA Gatekeeper Policy Rejection Blocks Critical Production Hotfix

### 🚨 The Production Scenario
An on-call engineer attempts to deploy an emergency hotfix to production, but the Kubernetes API server rejects the deployment: `admission webhook 'validation.kyverno.svc' denied the request: runAsNonRoot must be true`.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
The production cluster enforces a strict Kyverno or OPA Gatekeeper Pod Security Standard (PSS) policy requiring all containers to run as non-root with read-only root filesystems. The hotfix Docker image was built without a `USER 10001` directive, triggering admission denial.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Inspect the admission controller denial message to identify the violated policy and rule.
- Update the Dockerfile or Kubernetes deployment spec: define `securityContext.runAsNonRoot: true` and `securityContext.runAsUser: 10001`.
- If immediate emergency deployment is mandatory and code changes require build time, evaluate break-glass policy exemptions with Security team sign-off.
- Use Kyverno CLI (`kyverno test`) locally in CI pipelines to catch policy violations before pushing to Git.
- Deploy the compliant hotfix and verify admission approval.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# List active Kyverno ClusterPolicies
kubectl get clusterpolicies -A

# List active OPA Gatekeeper constraint violations
kubectl get constraints -A

# Validate manifest locally against Kyverno policies via CLI
kyverno apply /path/to/policy.yaml --resource /path/to/deployment.yaml

# Inspect supported Kubernetes securityContext parameters
kubectl explain pod.spec.securityContext

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "Admission controllers act as the final security gate at the Kubernetes API. When an emergency hotfix is blocked, we don't disable the policy. We update the pod's securityContext to specify runAsNonRoot: true and runAsUser. To prevent this during outages, we enforce Kyverno CLI and Trivy policy testing in developer CI so non-compliant specs fail at the PR stage rather than during production emergency rollouts."

---

## 📌 Scenario 3: Container Supply Chain Signature Verification (Cosign) Denies Image Admission

### 🚨 The Production Scenario
A new release deployed via ArgoCD fails to schedule pods with: `imagePolicyWebhook: image verification failed: no valid signatures found matching public key`.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
The production cluster enforces Cosign / Sigstore signature verification via Sigstore Policy Controller or Kyverno. The CI build pipeline built and pushed the container image to the registry but failed during the `cosign sign` step due to an expired OIDC token or missing KMS signing key, producing an unsigned artifact.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Verify image signature in the container registry using `cosign verify`.
- Inspect the CI/CD pipeline step where image signing was executed: check signing certificate validity and OIDC token status.
- Re-execute cryptographic signing of the container image using the authorized production key or keyless Sigstore workflow.
- Verify the policy controller admits the signed image in Kubernetes.
- Add pipeline checks that verify image signatures prior to updating GitOps manifests.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Verify container image cryptographic signature locally
cosign verify --key cosign.pub my-registry.corp/app:v2.1.0

# Cryptographically sign container image using cloud KMS
cosign sign --key awskms://arn:aws:kms:us-east-1:...:key/... my-registry.corp/app:v2.1.0

# Inspect Sigstore policy controller status
kubectl get policycontrollerextensions -A

# Check admission controller denial events
kubectl describe pod -l app=my-app -n prod

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "Image verification guarantees that only artifacts built by our verified CI/CD pipeline run in production. When admission fails with 'no valid signatures', I verify the image with 'cosign verify'. If the signing step in CI failed, we sign the image using our KMS-backed key, verify the admission webhook accepts it, and add a pipeline assertion ensuring no deployment manifest is committed unless signature verification passes in CI."

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Monitoring, Observability & Telemetry Scenarios: Rapid-Fire Drills & Master Cheat Sheet](../13-Monitoring-Observability-and-Telemetry-Scenarios/04-Rapid-Fire-Interview-Drills-and-Cheat-Sheet.md) | [Index](../../../README.md) | [DevSecOps, Identity & Secrets Management Scenarios: Architecture & Scaling Scenarios →](02-Architecture-and-Scaling-Scenarios.md) |

