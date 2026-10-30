# 03 - Native HTTP/3, QUIC and 0-RTT

## 1. Why HTTP/3 over QUIC Matters

HTTP/1.1 and HTTP/2 operate over TCP. If a single packet is lost on a degraded network (e.g., mobile switching cell towers), TCP pauses **all multiplexed streams** until the missing packet is retransmitted (**Head-of-Line Blocking**).

**HTTP/3** runs over **UDP** using the **QUIC** protocol:
- **Zero Head-of-Line Blocking**: Streams are truly independent; losing a packet on Stream A does not stall Stream B!
- **Connection Migration**: Connections survive client IP changes (e.g., switching from Wi-Fi to 5G) without resetting the TCP connection.
- **0-RTT Resumption**: Clients reconnect and send HTTP requests immediately without a handshake round trip.

```text
HTTP/2 over TCP:
[ HTTP/2 Streams ] ──► [ TCP Stack ] ──► (Packet Drop) ──► ALL Streams Frozen!

HTTP/3 over QUIC:
[ Stream A ] [ Stream B ] [ Stream C ] ──► [ QUIC / UDP ] ──► (Drop on A) ──► B and C stream unaffected!
```

Caddy enables HTTP/3 **by default** on all HTTPS listeners.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 02 - Automatic HTTPS and Internal PKI](./02-Automatic-HTTPS-and-Internal-PKI.md) | [Index](../../../README.md) | [04 - Caddyfile Syntax Directives and Snippets →](./04-Caddyfile-Syntax-Directives-and-Snippets.md) |
