# 09 - Interview Q&A: AWS S3

### Q: What is the difference between SSE-S3 and SSE-KMS encryption?
**Answer:** Both encrypt data at rest using AES-256. SSE-S3 uses S3-managed keys with zero key management cost. SSE-KMS uses AWS KMS keys, allowing key policy access control and CloudTrail audit logging for every decryption request.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [08 - Troubleshooting](./08-Troubleshooting.md) | [README](./README.md) | [10 - Hands-On Practice](./10-Hands-On-Practice.md) |
