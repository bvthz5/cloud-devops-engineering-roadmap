# 02 — DNS Record Types Deep Dive

DNS stores different types of information inside specific Resource Records (RRs).

---

## 1. Primary DNS Record Types

| Record Type | Purpose / Description | Example Value |
| :--- | :--- | :--- |
| **`A`** | Maps a domain to an **IPv4 address** | `example.com. IN A 93.184.216.34` |
| **`AAAA`** | Maps a domain to an **IPv6 address** | `example.com. IN AAAA 2606:2800:220:1:248:1893:25c8:1946` |
| **`CNAME`** | Canonical Name: Alias pointing to another domain | `www.example.com. IN CNAME example.com.` |
| **`ALIAS` / `ANAME`** | Virtual alias at the root/apex domain (Cloud-specific)| Maps root domain `company.com` to cloud load balancers |
| **`MX`** | Mail Exchange: Specifies mail server and priority | `company.com. IN MX 10 mail.company.com.` |
| **`TXT`** | Plaintext metadata (Domain verification, SPF, DKIM) | `"v=spf1 include:_spf.google.com ~all"` |
| **`PTR`** | Pointer: **Reverse DNS** (IP address to domain name)| `34.216.184.93.in-addr.arpa. IN PTR example.com.` |
| **`SRV`** | Service Record: Port and hostname for services | `_sip._tcp.example.com. IN SRV 10 60 5060 sip.example.com.` |
| **`NS`** | Nameserver: Delegates a DNS zone to authoritative server| `company.com. IN NS ns1.digitalocean.com.` |
| **`SOA`** | Start of Authority: Primary master, serial number, TTLs| `ns1.company.com. hostmaster.company.com. 2026100201 ...` |
| **`CAA`** | Certification Authority Authorization: Restricts TLS CAs | `company.com. IN CAA 0 issue "letsencrypt.org"` |

---

## 2. Why Can't You Put a CNAME on a Zone Apex (`@`)?

According to RFC 1912, **a CNAME record cannot coexist with any other record for the same name**.
Because a root apex domain (e.g. `company.com`) **must** have `NS` and `SOA` records, placing a `CNAME` at `@` violates the DNS specification.
Cloud DNS providers created **ALIAS / Flattened CNAME** records to resolve this issue by dynamically returning an `A` record containing the target's current IP address.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 01 - DNS Architecture and Hierarchical Tree](./01-DNS-Architecture-and-Hierarchical-Tree.md) | [Index](../../../README.md) | [03 - The Complete DNS Resolution Walkthrough →](./03-The-Complete-DNS-Resolution-Walkthrough.md) |
