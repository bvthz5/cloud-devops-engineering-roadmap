# 04 - Edge Compute: Cloudflare Workers, Lambda@Edge, Fastly Compute

## 1. Architectural Revolution: V8 Isolates vs. Containers

Traditional cloud serverless (AWS Lambda) launches lightweight Docker microVMs (Firecracker), requiring 100ms+ cold starts.
Modern Edge Compute (**Cloudflare Workers**, **Fastly Compute**):
- Employs **Google V8 JavaScript Engine Isolates** or **WebAssembly (Wasm)**.
- **Zero Cold Start (< 5 ms):** Thousands of isolated execution contexts share a single multi-threaded process.
- Executes code in 300+ edge locations worldwide within 50ms of any global user.

```
Traditional Cloud:
User (London) ─── 120ms Network Trip ───► AWS Region (us-east-1) ──► Node.js Lambda

Edge Compute:
User (London) ─── 2ms Trip ───► London Edge PoP (V8 Isolate executes JWT Auth) ──► Return
```

---

## 2. Prime Use Cases for Edge Compute
- **Zero-Trust JWT Auth Verification:** Validate tokens and reject unauthorized requests before they ever touch your central cloud infrastructure.
- **Dynamic A/B Testing:** Split user traffic based on cookies without client-side page layout flicker.
- **Geo-Fencing & Localization:** Direct users to local country subdomains based on CDN headers (`cf-ipcountry`).

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [03 - Cache-Control](./03-Cache-Control-Headers-and-Invalidation-Strategies.md) | [README](./README.md) | [05 - DDoS Mitigation](./05-DDoS-Mitigation-Rate-Limiting-and-WAF-at-the-Edge.md) |
