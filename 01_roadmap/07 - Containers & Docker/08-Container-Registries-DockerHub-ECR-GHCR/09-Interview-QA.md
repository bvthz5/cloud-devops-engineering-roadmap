# 09 - Interview Questions & Architectural Scenarios

### Q1: What is the architectural difference between an Image Tag and an Image Digest?
**Answer**: An Image Tag (e.g. `:v1.0.0`) is a mutable, human-friendly pointer that can be updated to point to different images over time. An Image Digest (e.g. `@sha256:abc...`) is an immutable cryptographic hash of the image manifest. Referencing a digest guarantees that the exact byte-for-byte image is pulled, eliminating risks of tag overwriting.

### Q2: How does Sigstore Cosign verify image authenticity?
**Answer**: Cosign signs the cryptographic digest of an image using a private key (or keyless via OIDC identity tokens). It stores the resulting signature directly in the registry as an OCI artifact attached to the image manifest. Verification queries the public key against the signature to prove the image originated from a trusted entity without tampering.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 08 - Troubleshooting](./08-Troubleshooting.md) | [Index](../../../README.md) | [10 - Hands On Practice →](./10-Hands-On-Practice.md) |
