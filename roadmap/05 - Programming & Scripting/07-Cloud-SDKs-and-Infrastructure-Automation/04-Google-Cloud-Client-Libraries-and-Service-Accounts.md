# 04 - Google Cloud Client Libraries and Service Accounts

## 1. Application Default Credentials (ADC) in GCP

Google Cloud utilizes **Application Default Credentials (ADC)** to resolve authentication:
1. `GOOGLE_APPLICATION_CREDENTIALS` environment variable pointing to a service account JSON file.
2. User credentials set via `gcloud auth application-default login`.
3. Compute Engine / GKE metadata server identity tokens.

```python
from google.cloud import storage
import google.auth

# Explicit authentication resolution inspection
credentials, project_id = google.auth.default(
    scopes=['https://www.googleapis.com/auth/cloud-platform']
)
print(f"Authenticated as project: {project_id}")

# Client automatically adopts ADC credentials
storage_client = storage.Client(credentials=credentials, project=project_id)

buckets = list(storage_client.list_buckets())
for b in buckets:
    print(f"Bucket: {b.name}, StorageClass: {b.storage_class}")
```

---

## 2. GKE Cluster Management and Node Pool Automation

```python
from google.cloud import container_v1

client = container_v1.ClusterManagerClient()

project_id = "my-gcp-prod-project"
zone = "us-central1-a"
parent = f"projects/{project_id}/locations/{zone}"

# List all GKE clusters in region
response = client.list_clusters(parent=parent)
for cluster in response.clusters:
    print(f"Cluster: {cluster.name}, Status: {cluster.status.name}, Version: {cluster.current_master_version}")
    for pool in cluster.node_pools:
        print(f"  Node Pool: {pool.name}, Size: {pool.initial_node_count}, AutoScaling: {pool.autoscaling.enabled}")
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [03 - Azure SDK for Python](./03-Azure-SDK-for-Python-and-Identity-Libraries.md) | [README](./README.md) | [05 - Automated Cloud Cost Optimization](./05-Automated-Cloud-Cost-Optimization-and-Janitor-Scripts.md) |
