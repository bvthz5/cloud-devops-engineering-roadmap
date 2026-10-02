# 08 - CDN & Anycast: Troubleshooting Guide

## 1. Inspecting Edge Cache Headers with `curl`

```bash
curl -Iv https://www.example.com/assets/logo.png
```

Key Diagnostic Headers:
- **Cloudflare:** `CF-Cache-Status: HIT` (or `MISS`, `EXPIRED`, `DYNAMIC`, `BYPASS`).
- **AWS CloudFront:** `X-Cache: Hit from cloudfront` (or `Miss from cloudfront`).
- **Age:** `Age: 342` (Indicates the asset has lived in the CDN cache for 342 seconds).

---

## 2. Troubleshooting Anycast Flapping

If clients experience intermittent TCP resets on an Anycast IP:
- **BGP Route Flapping:** An intermediate ISP is toggling routes between two edge PoPs. Because TCP state is bound to one server, packets landing on the second PoP trigger an immediate `TCP RST`.
- **Diagnostic:** Run `mtr -z` from affected regions to identify fluctuating AS hops.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [07 - Real-World Scenarios](./07-Real-World-Scenarios.md) | [README](./README.md) | [09 - Interview Q&A](./09-Interview-QA.md) |
