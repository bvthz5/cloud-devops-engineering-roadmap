# 01 - Anycast Routing Architecture and BGP

## 1. Unicast vs. Anycast

- **Unicast:** One IP address maps to one single physical network interface anywhere in the world.
- **Anycast:** One single IP address (e.g. `1.1.1.1` or `8.8.8.8`) is announced via BGP simultaneously from **hundreds of different datacenters** across the planet!

```
                      [ Client in Tokyo ]
                               │
                               ▼ BGP chooses lowest AS-Path
                  +---------------------------+
                  | Tokyo Edge PoP (1.1.1.1)  |
                  +---------------------------+

                      [ Client in Frankfurt ]
                               │
                               ▼ BGP chooses lowest AS-Path
                  +-------------------------------+
                  | Frankfurt Edge PoP (1.1.1.1)  |
                  +-------------------------------+
```

---

## 2. Why Anycast is Invulnerable to Centralized DDoS
In a standard Unicast setup, an attacker flooding 500 Gbps easily saturates a single datacenter uplink.
In **Anycast**:
- A global botnet of 1,000,000 infected machines sends 1 Tbps of traffic to `1.1.1.1`.
- Because of BGP local routing, the attack traffic is **automatically partitioned geographically** across 250+ edge datacenters.
- Each edge PoP only absorbs a manageable ~4 Gbps slice, completely diffusing the attack!

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [README](./README.md) | [README](./README.md) | [02 - CDN Architecture](./02-CDN-Architecture-PoPs-and-Edge-Caching.md) |
