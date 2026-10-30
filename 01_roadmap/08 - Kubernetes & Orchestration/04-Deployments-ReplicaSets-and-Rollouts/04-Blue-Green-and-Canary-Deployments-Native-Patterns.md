# 04 - Blue-Green and Canary Deployments: Native Patterns

## 1. Native Blue-Green Deployment Pattern

In a Blue-Green deployment, two identical production environments exist simultaneously. Traffic is switched instantly via the Service's `selector`:

```text
                   +------------------------+
                   |  Service: web-service  |
                   |  selector:             |
                   |    app: web            |
                   |    version: blue  ─────┼────────┐
                   +------------------------+        │
                                                     v
                                             +---------------+
                                             |  Blue Pods    | (Active v1.0)
                                             +---------------+

                                             +---------------+
                                             |  Green Pods   | (Idle v2.0 - Tested)
                                             +---------------+
```

```bash
# Instant traffic flip to Green:
kubectl patch service web-service -p '{"spec":{"selector":{"version":"green"}}}'
```

---

## 2. Native Canary Deployment Pattern

Deploying two separate Deployments behind a single Service with a ratio of pod replicas:
- `web-v1` Deployment: 9 replicas (`app: web`, `track: stable`)
- `web-v2` Deployment: 1 replica (`app: web`, `track: canary`)
- Service selector: `app: web` (automatically routes 10% of traffic to v2!).

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 03 - Rollbacks Revision History and Change Cause](./03-Rollbacks-Revision-History-and-Change-Cause.md) | [Index](../../../README.md) | [05 - Pausing Resuming and Scaling Deployments →](./05-Pausing-Resuming-and-Scaling-Deployments.md) |
