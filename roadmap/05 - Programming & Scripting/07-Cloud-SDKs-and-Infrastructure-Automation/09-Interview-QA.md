# 09 - Interview Questions & Architectural Scenarios

### Q1: In AWS Boto3, what is the architectural difference between a Client and a Resource?
**Answer**: A **Client** is a low-level, direct 1:1 mapping to the AWS REST/Query API. It returns plain Python dictionaries representing raw wire responses, is thread-safe, and supports paginators and waiters. A **Resource** is an object-oriented abstraction that wraps service entities into Python objects with sub-resources and lazily-loaded attributes. AWS has frozen new feature development on Resources, making Clients the standard for enterprise automation.

### Q2: How does Azure's `DefaultAzureCredential` resolve authentication across environments?
**Answer**: It checks sequentially: 1) Environment variables, 2) Workload identity tokens, 3) Managed identities (VM/AKS metadata server), 4) Azure CLI cache (`az login`), and 5) PowerShell credentials. This enables zero-code modification between local debugging and production execution.

### Q3: What happens if an API call exceeds rate limits and how should a Cloud SDK handle it?
**Answer**: The cloud provider returns HTTP 429 (`Too Many Requests`) or service-specific throttling codes like `RequestLimitExceeded` / `ThrottlingException`. The SDK should implement exponential backoff with full jitter to avoid synchronous retry storms.

### Q4: Why should Paginators be used instead of manual pagination loops?
**Answer**: Manual loops require handling different token key names across services (`NextToken`, `ContinuationToken`, `NextMarker`), tracking truncation booleans (`IsTruncated`), and handling edge cases where empty contents are returned alongside a continuation token. Paginators abstract this into clean Python generators.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [08 - Troubleshooting](./08-Troubleshooting.md) | [README](./README.md) | [10 - Hands-On Practice](./10-Hands-On-Practice.md) |
