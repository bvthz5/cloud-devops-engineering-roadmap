# Google Cloud Platform (GCP) Infrastructure Scenarios: Production Incidents & Triage Scenarios

> **Interview Focus:** Real-world incident response, root-cause analysis (RCA), diagnostic toolchains, and articulate candidate verbal pitches.

---

## 📌 Scenario 1: GKE Workload Identity Fails with 'Could Not Refresh Access Token: 403 Forbidden'

### 🚨 The Production Scenario
Pods running inside Google Kubernetes Engine (GKE) attempting to query Cloud Storage or Secret Manager fail with: 'Google::Apis::ClientError: Access denied: 403 Forbidden' despite the GCP Service Account possessing Owner/Editor permissions.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
Workload Identity requires three-way binding: GKE cluster must have Workload Identity enabled, the Kubernetes Service Account (KSA) must have the annotation 'iam.gke.io/gcp-service-account', and the GCP Service Account (GSA) must have the IAM role 'roles/iam.workloadIdentityUser' granting access to 'serviceAccount:<PROJECT_ID>.svc.id.goog[<NAMESPACE>/<KSA>]'. Any missing link breaks token exchange.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Verify cluster Workload Identity pool is configured using 'gcloud container clusters describe'.
- Inspect KSA annotations using kubectl get serviceaccount <KSA> -o yaml.
- Verify IAM policy binding on the GSA for 'roles/iam.workloadIdentityUser'.
- Verify pod spec specifies 'serviceAccountName: <KSA>' in the deployment manifest.
- Test token retrieval from inside the pod via the GKE metadata server.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Verify GKE cluster workloadIdentityConfig
gcloud container clusters describe gke-prod --zone us-central1-a --query 'workloadIdentityConfig'

# Check KSA IAM annotation
kubectl get sa app-ksa -n default -o jsonpath='{.metadata.annotations}'

# Check roles/iam.workloadIdentityUser binding on GSA
gcloud iam service-accounts get-iam-policy app-gsa@my-proj.iam.gserviceaccount.com

# Verify pod metadata server returns GSA email
kubectl exec -it app-pod -- curl -H 'Metadata-Flavor: Google' 'http://169.254.169.254/computeMetadata/v1/instance/service-accounts/default/email'

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "Workload Identity links Kubernetes service accounts to GCP service accounts without long-lived keys. When token exchange fails, I check the three-way handshake: the KSA annotation pointing to the GSA email, the GSA IAM binding granting 'roles/iam.workloadIdentityUser' to the KSA identity URI, and the pod's serviceAccountName field. If any of these differ by a single character, the metadata server refuses the token exchange."

---

## 📌 Scenario 2: Cloud Load Balancing 502 Bad Gateway with 'backend_timeout' or 'failed_to_connect_to_backend'

### 🚨 The Production Scenario
A Global External Application Load Balancer in GCP intermittently returns HTTP 502 Bad Gateway to users. Cloud Logging shows 'statusDetails: backend_timeout' while backend Compute Engine instance groups show 100% CPU.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
GCP Cloud Load Balancer default backend service timeout is 30 seconds. Heavy backend compute operations exceeded this limit, or backend web servers (e.g., Apache/Nginx) had keep-alive timeouts shorter than the load balancer's keep-alive timeout, causing the backend to close TCP connections unexpectedly.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Query Cloud Logging filtering for 'httpRequest.status=502' and extract 'jsonPayload.statusDetails'.
- Check Nginx/Gunicorn keep-alive timeout: must be set greater than 610 seconds to exceed GCP CLB's 600-second keep-alive.
- Inspect backend service health check configuration and port alignment.
- Examine firewall rules: verify that GCP Load Balancer probe IP ranges ('130.211.0.0/22', '35.191.0.0/16') are allowed ingress to backend instances.
- Increase Backend Service timeout if long-running requests are expected, or scale MIG (Managed Instance Group).

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Query CLB 502 logs in Cloud Logging
gcloud logging read 'resource.type="http_load_balancer" AND httpRequest.status=502' --limit 10 --format json

# Inspect backend health status across all zones
gcloud compute backend-services get-health app-backend-service --global

# Verify GCP CLB health check IP ranges are allowed
gcloud compute firewall-rules list --filter='network=prod-vpc AND targetTags:gke-node' --format='table(name,allowed,sourceRanges)'

# Increase backend service response timeout
gcloud compute backend-services update app-backend-service --global --timeout=60s

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "In GCP, a 502 Bad Gateway from Cloud Load Balancing is caused by three things: firewall rules blocking the probe ranges 130.211.0.0/22 and 35.191.0.0/16, backend processing exceeding the backend service timeout, or a keep-alive race condition where the web server closes idle sockets before the load balancer does. Setting the backend keep-alive to 650 seconds eliminates the race condition."

---

## 📌 Scenario 3: Private Google Access Failure Blocks Cloud Storage Downloads from Private Compute VMs

### 🚨 The Production Scenario
Compute Engine instances in a private subnet without external IP addresses fail to download dependencies from Google Cloud Storage (`gsutil cp gs://my-bucket/pkg.tar.gz .`), timing out after multiple retries.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
The VPC subnet hosting the instances did not have 'Private Google Access' enabled. Without external public IPs, VMs cannot reach Google public APIs unless Private Google Access is enabled on the subnet, allowing internal routes to Google's private VIPs (199.36.153.8/30 or private.googleapis.com).

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Inspect the VM instance network interface: confirm 'natIP' is absent (purely private).
- Check subnet configuration for 'privateIpGoogleAccess'.
- Enable Private Google Access on the subnet using gcloud.
- Verify DNS resolution on the VM: ensure 'storage.googleapis.com' resolves properly.
- Check egress firewall rules: ensure outbound traffic to Google API ranges is not blocked by a broad deny rule.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Check Private Google Access status on subnet
gcloud compute networks subnets describe snet-private-backend --region=us-central1 --format='get(privateIpGoogleAccess)'

# Enable Private Google Access on private subnet
gcloud compute networks subnets update snet-private-backend --region=us-central1 --enable-private-ip-google-access

# Confirm VM has no external public IP
gcloud compute instances describe vm-worker --zone=us-central1-a --format='get(networkInterfaces[0].accessConfigs)'

# Check for overriding egress firewall deny rules
gcloud compute firewall-rules list --filter='direction=EGRESS AND action=DENY'

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "Compute Engine instances without external public IPs cannot reach Google APIs by default. Enabling 'Private Google Access' on the subnet instructs Google's software-defined network (Andromeda) to route requests destined for googleapis.com directly to internal Google service endpoints without going through internet gateways."

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Azure Cloud Infrastructure & Governance Scenarios: Rapid-Fire Drills & Master Cheat Sheet](../09-Azure-Cloud-Infrastructure-and-Governance-Scenarios/04-Rapid-Fire-Interview-Drills-and-Cheat-Sheet.md) | [Index](../../../README.md) | [Google Cloud Platform (GCP) Infrastructure Scenarios: Architecture & Scaling Scenarios →](02-Architecture-and-Scaling-Scenarios.md) |

