# 06 — DNSSEC and Modern DNS Security

Traditional DNS is unencrypted and unauthenticated, making it vulnerable to **Cache Poisoning / Spoofing** (tricking a resolver into caching a fraudulent IP for `bank.com`).

---

## 1. DNSSEC (DNS Security Extensions)

DNSSEC adds cryptographic authentication to DNS responses using digital signatures:
- **`RRSIG` (Resource Record Signature):** Digital signature for a record set.
- **`DNSKEY`:** Public key used to verify `RRSIG`.
- **`DS` (Delegation Signer):** Hash of the child's `DNSKEY` placed in the parent zone, establishing a **Chain of Trust** up to the Root Zone.
- **Note:** DNSSEC does **not encrypt** queries; it guarantees authenticity and integrity.

---

## 2. Encrypted DNS: DoH vs DoT

- **DoT (DNS over TLS):** Encrypts DNS queries using TLS on dedicated **Port 853**.
- **DoH (DNS over HTTPS):** Wraps DNS queries inside standard HTTPS traffic on **Port 443**. Difficult for network firewalls to block or spy on.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 05 - Split Horizon DNS and Private Hosted Zones](./05-Split-Horizon-DNS-and-Private-Hosted-Zones.md) | [Index](../../../README.md) | [07 - Real World Scenarios →](./07-Real-World-Scenarios.md) |
