# 01 - Cloud SDK Architecture: Client vs Resource Models

## 1. Low-Level Client vs High-Level Resource Models

Cloud providers expose HTTP/JSON or gRPC endpoints. Cloud SDKs bridge programming languages to these remote APIs via two distinct architectural abstraction layers:

```text
+-----------------------------------------------------------------------------------+
|                           Developer Application Code                              |
+-----------------------------------------+-----------------------------------------+
                                          |
        ┌─────────────────────────────────┴─────────────────────────────────┐
        ▼                                                                   ▼
[ Resource Model / Object Abstraction ]                 [ Low-Level Client Model ]
  - Object-oriented entities (e.g., s3.Bucket)            - 1:1 mapping to remote wire REST API
  - Lazily loaded attributes                              - Returns pure dictionaries / raw responses
  - High memory overhead                                  - Explicit pagination and error handling
  - E.g.: boto3.resource('s3')                            - Thread-safe and light memory footprint
                                                          - E.g.: boto3.client('s3')
        └─────────────────────────────────┬─────────────────────────────────┘
                                          |
                                          ▼
                         [ HTTP Engine / Connection Pool ]
                           - urllib3 / requests / aiohttp
                           - TLS Handshake & Connection Reuse
                           - Request Signing (AWS SigV4, Azure Bearer, GCP OAuth2)
                                          |
                                          ▼
                        [ Cloud Provider REST/gRPC API ]
```

### AWS Boto3 Architectural Comparison
```python
import boto3

# 1. Low-Level Client (Recommended for Enterprise/Production)
s3_client = boto3.client('s3')
response = s3_client.list_objects_v2(Bucket='prod-telemetry-data')
for obj in response.get('Contents', []):
    print(obj['Key'], obj['Size'])

# 2. High-Level Resource (Convenient for quick scripts, but deprecated in many newer AWS services)
s3_resource = boto3.resource('s3')
bucket = s3_resource.Bucket('prod-telemetry-data')
for obj in bucket.objects.all():
    print(obj.key, obj.size)
```

---

## 2. Thread Safety and Connection Pooling

### Connection Pools (`urllib3`)
Cloud SDK clients manage an internal connection pool per host. If your Python automation script uses multi-threading (`concurrent.futures.ThreadPoolExecutor`), each thread should share a single client instance or configure pool sizes appropriately.

```python
import boto3
from botocore.config import Config

# Configure client connection pool for high concurrency
config = Config(
    max_pool_connections=50,  # Match or exceed ThreadPoolExecutor max_workers
    retries={
        'max_attempts': 5,
        'mode': 'adaptive'     # Dynamically adjusts rate based on throttling
    },
    connect_timeout=5,
    read_timeout=15
)

client = boto3.client('ec2', config=config)
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Prev Module (06-PowerShell-Core-for-Cloud-and-DevOps)](../06-PowerShell-Core-for-Cloud-and-DevOps/12-Quick-Revision.md) | [Index](../../../README.md) | [02 - AWS Boto3 Deep Dive Paginators Waiters and Config →](./02-AWS-Boto3-Deep-Dive-Paginators-Waiters-and-Config.md) |
