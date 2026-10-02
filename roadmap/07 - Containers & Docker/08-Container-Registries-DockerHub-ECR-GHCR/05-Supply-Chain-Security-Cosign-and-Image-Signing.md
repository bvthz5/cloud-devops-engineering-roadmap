# 05 - Supply Chain Security: Cosign and Image Signing

## 1. Sigstore Cosign: Cryptographic Container Signatures

How do you guarantee that the container image running in production was actually built by your CI/CD pipeline and not tampered with by a malicious insider or compromised registry?

**Cosign** signs the image digest cryptographically and stores the signature directly inside the OCI registry as an attached artifact:

```bash
# 1. Sign container image with private key
cosign sign --key cosign.key myregistry.com/api:v1.2.0

# 2. Verify signature before deployment
cosign verify --key cosign.pub myregistry.com/api:v1.2.0
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [04 - Authentication & IAM](./04-Authentication-IAM-Roles-and-Token-Exchanges.md) | [README](./README.md) | [06 - Storage Optimization & GC](./06-Registry-Storage-Optimization-and-Garbage-Collection.md) |
