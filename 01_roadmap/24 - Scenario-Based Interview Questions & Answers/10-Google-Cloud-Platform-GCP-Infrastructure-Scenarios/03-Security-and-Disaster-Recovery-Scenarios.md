# Google Cloud Platform (GCP) Infrastructure Scenarios: Security & Disaster Recovery Scenarios

> **Interview Focus:** Real-world incident response, root-cause analysis (RCA), diagnostic toolchains, and articulate candidate verbal pitches.

---

## 📌 Scenario 7: Cloud Run Cold Starts Cause HTTP 504 Timeouts for Real-Time Mobile Clients

### 🚨 The Production Scenario
Mobile app users report frequent 504 timeouts when invoking a Python ML inference microservice hosted on Google Cloud Run following periods of low traffic.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
Cloud Run scaled instance count to 0 during idle periods. When a new request arrived, the container took 12 seconds to cold start: downloading heavy Python ML libraries (PyTorch, TensorFlow) and initializing model weights into memory before listening on the port.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Configure 'min-instances: 1' (or more) on the Cloud Run service to keep warm containers constantly running.
- Optimize Dockerfile: pre-compile Python bytecode and leverage multi-stage builds to minimize image size.
- Enable Cloud Run Startup CPU Boost to allocate maximum CPU during container startup initialization.
- Implement lazy loading for heavy machine learning model weights.
- Configure health check startup probes to ensure Cloud Run does not send user traffic until the model is loaded.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Inspect min-instances and startup settings
gcloud run services describe ml-inference --region us-central1 --format='get(spec.template.metadata.annotations)'

# Set minimum instances to 2 to eliminate cold starts
gcloud run services update ml-inference --region us-central1 --min-instances 2

# Enable Startup CPU Boost for fast container initialization
gcloud run services update ml-inference --region us-central1 --cpu-boost

# Verify startup probe timing
gcloud logging read 'resource.type="cloud_run_revision" AND textPayload=~"Startup probe"' --limit 10

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "Cloud Run scale-to-zero is cost-effective but causes latency spikes for heavy workloads like Python ML microservices. I eliminate cold starts by setting min-instances to 1 or 2, which maintains warm execution environments. Additionally, I enable Cloud Run Startup CPU Boost to give containers temporary multi-core burst power during boot, cutting container initialization time by over 50%."

---

## 📌 Scenario 8: Cloud SQL Primary High Disk Utilization Emergency (98% Full)

### 🚨 The Production Scenario
A Cloud SQL MySQL instance reaches 98% disk capacity. Alerts warn that when disk capacity reaches 100%, the instance will automatically become read-only and shut down database processes.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
Automatic storage increase was disabled or capped, and large binary logs (binlogs) accumulated because a replica stalled or failed to acknowledge replication positions, preventing binary log purge.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Immediately trigger an emergency storage increase via gcloud to give the database breathing room.
- Inspect binary log retention settings and purge expired binlogs.
- Verify replication status of all read replicas: identify stalled replicas holding binlog purge locks.
- Enable 'Automatic Storage Increase' with a reasonable maximum limit.
- Configure Cloud Monitoring alerts at 75% and 85% storage utilization.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Check current disk allocation size
gcloud sql instances describe sql-prod --format='get(settings.dataDiskSizeGb,settings.dataDiskType)'

# Increase disk size immediately by 200 GB to avert shutdown
gcloud sql instances patch sql-prod --data-disk-size-gb=500

# Enable automatic storage increase with safety cap
gcloud sql instances patch sql-prod --enable-bin-log --storage-auto-increase --max-disk-size-gb=2000

# Purge aged binary logs inside MySQL prompt
PURGE BINARY LOGS BEFORE NOW() - INTERVAL 3 DAY;

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "Running out of disk on Cloud SQL can bring production down because instances enter read-only panic state. Step one is immediate: increase disk size via gcloud patch since disk expansion is online with zero downtime. Step two is investigating why disk grew: check for orphaned binary logs held by stalled replicas, purge expired logs, and permanently enable automatic storage increase."

---

## 📌 Scenario 9: Cloud Build Pipeline Pipeline Timeout Due to Ephemeral Disk Exhaustion

### 🚨 The Production Scenario
CI/CD builds running in Google Cloud Build fail intermittently during large Docker image compilation with: 'No space left on device' after 15 minutes of execution.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
Standard Cloud Build worker pools execute on VMs with default 100 GB boot disks. The build compiled multiple large microservices and pulled gigabytes of base images and caches without pruning intermediate layers, exhausting local disk space.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Switch the build to use Cloud Build Private Pools or custom machine types with custom disk sizes.
- Specify a larger disk size (e.g., 200 GB or 500 GB) in the Cloud Build configuration options.
- Implement multi-stage Docker builds to discard build dependencies from the final image.
- Use Kaniko or Docker Buildx cache with Google Cloud Storage or Artifact Registry cache backends instead of local disk.
- Add cleanup steps in cloudbuild.yaml to remove intermediate build artifacts.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Submit build with custom disk specification
gcloud builds submit --config=cloudbuild.yaml --substitutions=_DISK_SIZE=200GB

# List available disk performance types
gcloud compute disk-types list --zone=us-central1-a

# Check Docker disk usage and clean stale layers
docker system df && docker system prune -af

# Verify Artifact Registry cached images
gcloud artifacts docker images list us-central1-docker.pkg.dev/my-proj/repo

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "Default Cloud Build workers have fixed disk limits that fail under heavy multi-container builds. I resolve this by configuring custom machine types and disk sizes in the build options block of cloudbuild.yaml, or provisioning a Cloud Build Private Pool. In the pipeline, I use Kaniko with remote Artifact Registry caching so layers stream to remote storage rather than consuming local runner disk."

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Google Cloud Platform (GCP) Infrastructure Scenarios: Architecture & Scaling Scenarios](02-Architecture-and-Scaling-Scenarios.md) | [Index](../../../README.md) | [Google Cloud Platform (GCP) Infrastructure Scenarios: Rapid-Fire Drills & Master Cheat Sheet →](04-Rapid-Fire-Interview-Drills-and-Cheat-Sheet.md) |

