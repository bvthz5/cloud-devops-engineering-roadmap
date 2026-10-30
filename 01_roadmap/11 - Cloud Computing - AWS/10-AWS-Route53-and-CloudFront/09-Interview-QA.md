# 09 - Interview Q&A: Route53 & CloudFront

### Q: Why use an ALIAS record instead of a CNAME record for a zone apex domain (example.com)?
**Answer:** DNS standards prohibit CNAME records at the zone apex (`example.com`). Route53 ALIAS records solve this by acting as apex virtual alias records pointing directly to AWS resources (ALB/CloudFront) with zero performance penalty.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 08 - Troubleshooting](./08-Troubleshooting.md) | [Index](../../../README.md) | [10 - Hands On Practice →](./10-Hands-On-Practice.md) |
