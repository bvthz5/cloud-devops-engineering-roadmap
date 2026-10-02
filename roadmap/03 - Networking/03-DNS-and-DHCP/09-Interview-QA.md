# 09 — DNS & DHCP Interview Q&A

10 technical interview questions for DevOps, SRE, and Infrastructure roles.

---

### Q1: What is the difference between a CNAME record and an ALIAS record?
**Answer:**
A `CNAME` record is an official DNS standard record that points one hostname to another. However, RFC 1912 forbids placing a `CNAME` on the root apex domain (e.g. `company.com`) because it conflicts with `SOA` and `NS` records.
An `ALIAS` (or ANAME) record is a cloud-provider feature (AWS Route 53, Cloudflare). It can be used at the root apex domain because the DNS provider's nameservers automatically resolve the target hostname internally and return a standard `A` or `AAAA` record to the client.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [08 - Troubleshooting](./08-Troubleshooting.md) | [README](./README.md) | [10 - Hands-On Practice](./10-Hands-On-Practice.md) |
