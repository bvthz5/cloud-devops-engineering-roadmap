# 01 - VPN Protocols: IPsec, WireGuard, and OpenVPN

## 1. Protocol Comparison Matrix

| Feature | IPsec (IKEv2) | WireGuard | OpenVPN |
|---|---|---|---|
| **OSI Layer** | Layer 3 (Network) | Layer 3 (Network) | Layer 3 / Layer 2 (TUN/TAP) |
| **Kernel vs User Space** | In-Kernel (Fast) | In-Kernel (Extremely Fast) | User Space (tun driver context switches) |
| **Codebase Size** | ~400,000+ lines (StrongSwan) | ~4,000 lines (Auditable) | ~100,000+ lines |
| **Cryptography** | AES-GCM, SHA256, DH groups | ChaCha20, Poly1305, Curve25519 | OpenSSL ciphers (AES, RSA, ECDSA) |
| **Transport Protocol** | UDP 500, UDP 4500 (NAT-T), ESP (IP 50) | UDP only (custom port, e.g. 51820) | TCP or UDP (commonly UDP 1194) |
| **Roaming / Handover** | MOBIKE supported | Seamless cryptographic roaming | Connection re-handshake required |
| **Standard Enterprise Use** | Cloud Site-to-Site VPNs (AWS/Azure) | Modern Kubernetes/DevOps meshes | Legacy Client-to-Site employee VPN |

---

## 2. IPsec: Tunnel vs. Transport Mode

```
TRANSPORT MODE (Host-to-Host):
[Original IP Header] [ESP Header] [TCP / Payload] [ESP Trailer] [ESP Auth]
* Encrypts only the payload. Original IP source & destination remain visible.

TUNNEL MODE (Network-to-Network / Site-to-Site Gateways):
[NEW Outer IP Header] [ESP Header] [Original IP Header] [TCP / Payload] [ESP Trailer] [ESP Auth]
* Encapsulates and encrypts the ENTIRE original IP packet. Used for VPC-to-On-Prem VPNs.
```

---

## 3. WireGuard Architecture

WireGuard utilizes **Cryptokey Routing**:
- Every peer has a static public key and an assigned private IP inside the VPN tunnel.
- The tunnel interface maintains an association table: `Public Key <---> AllowedIPs`.
- When sending a packet to `10.100.0.5`, WireGuard encrypts it with the public key associated with `10.100.0.5` and encapsulates it into a UDP packet.
- No chatty keepalives: In the absence of data, peers remain completely silent and undetectable to port scanners.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [README](./README.md) | [README](./README.md) | [02 - Cloud VPC Architecture](./02-Cloud-VPC-Architecture-and-Subnet-Topology.md) |
