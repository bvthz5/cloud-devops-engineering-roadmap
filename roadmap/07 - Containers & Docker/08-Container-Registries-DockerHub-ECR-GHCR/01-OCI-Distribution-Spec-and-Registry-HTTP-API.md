# 01 - OCI Distribution Spec and Registry HTTP API

## 1. Anatomy of an OCI Image in a Registry

An image in an OCI-compliant registry is not a single file; it is a collection of content-addressable cryptographic blobs:

```text
[ Image Tag: v1.0.0 ] ──► Points to ──► [ Manifest JSON (application/vnd.oci.image.manifest.v1+json) ]
                                                   │
                 ┌─────────────────────────────────┴─────────────────────────────────┐
                 ▼                                                                   ▼
       [ Config Blob (JSON) ]                                             [ Layer Blobs (tar.gzip) ]
       (Environment, Entrypoint, Architecture)                              - Layer 1 (sha256:abc...)
                                                                           - Layer 2 (sha256:def...)
```

### Digest-Based Immutable Pulling
Tags (like `:latest` or `:v1.0.0`) are mutable pointers. To guarantee pulling the exact byte-for-byte binary, reference the cryptographic digest:
```bash
docker pull mycompany/api@sha256:4b9123456789abcdef...
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [README](./README.md) | [README](./README.md) | [02 - Enterprise Registries ECR, GHCR & Harbor](./02-Enterprise-Registries-ECR-GHCR-and-Harbor.md) |
