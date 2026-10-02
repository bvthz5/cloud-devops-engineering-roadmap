# 08 - Container Registries (DockerHub, ECR, GHCR)

Container Registries are the central distribution hubs for containerized software. Beyond simply hosting Docker images, enterprise registries manage fine-grained IAM authentication, automated lifecycle policies, cross-region replication, image immutability, vulnerability scanning, and cryptographic supply-chain signing with Cosign.

---

## Module Index

| # | Topic | Description |
|---|---|---|
| 01 | [OCI Distribution Spec & Registry API](./01-OCI-Distribution-Spec-and-Registry-HTTP-API.md) | OCI Distribution Specification, HTTP API v2, manifests, blobs, sha256 digests. |
| 02 | [Enterprise Registries: ECR, GHCR & Harbor](./02-Enterprise-Registries-ECR-GHCR-and-Harbor.md) | AWS ECR lifecycle policies, GitHub Container Registry (GHCR), self-hosted Harbor. |
| 03 | [Image Tagging Strategies & Immutability](./03-Image-Tagging-Strategies-and-Immutability.md) | Why `:latest` is forbidden in production, semantic versioning, Git SHA tags, immutable tags. |
| 04 | [Authentication, IAM & Credential Helpers](./04-Authentication-IAM-Roles-and-Token-Exchanges.md) | `docker login`, AWS ECR token exchanges, GHCR personal tokens, credential stores. |
| 05 | [Supply Chain Security: Cosign & Image Signing](./05-Supply-Chain-Security-Cosign-and-Image-Signing.md) | Sigstore Cosign, keyless signing via OIDC, cryptographic verification in Kubernetes. |
| 06 | [Storage Optimization & Garbage Collection](./06-Registry-Storage-Optimization-and-Garbage-Collection.md) | Cleaning orphaned untagged manifests, registry garbage collection, multi-arch manifests. |
| 07 | [Real-World Scenarios](./07-Real-World-Scenarios.md) | Enterprise post-mortems: Overwritten `:latest` tag outage, ECR auth token expiration in CI. |
| 08 | [Troubleshooting & Diagnostic Runbooks](./08-Troubleshooting.md) | Debugging 401 Unauthorized, Docker Hub rate limits (429), manifest digest verification. |
| 09 | [Interview Questions & Architectural Scenarios](./09-Interview-QA.md) | 10 Senior SRE/DevOps interview scenarios on registry architecture, Cosign, and OCI specs. |
| 10 | [Hands-On Practice Labs](./10-Hands-On-Practice.md) | Lab 1: Deploying private local registry; Lab 2: Signing image with Cosign; Lab 3: ECR auth script. |
| 11 | [Multiple-Choice Assessment (MCQ)](./11-MCQ.md) | 10 technical scenario MCQs with deep architectural rationales. |
| 12 | [Quick-Revision & Enterprise Cheat Sheet](./12-Quick-Revision.md) | High-density reference for registry CLI commands, ECR login patterns, and Cosign flags. |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [07 - Container Security](../07-Container-Security-and-Image-Scanning/README.md) | [README](./README.md) | [01 - OCI Distribution Spec](./01-OCI-Distribution-Spec-and-Registry-HTTP-API.md) |
