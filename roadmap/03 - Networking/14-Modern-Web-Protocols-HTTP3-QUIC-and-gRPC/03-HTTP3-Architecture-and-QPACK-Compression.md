# 03 - HTTP/3 Architecture and QPACK Compression

## 1. True Stream Independence

In HTTP/3, each stream is handled independently at the transport layer by QUIC:
- If packet loss occurs on Stream 1, **Stream 2, 3, and 4 continue processing at full speed without waiting!**
- Eliminates Head-of-Line blocking completely.

---

## 2. QPACK: Out-of-Order Header Compression

HTTP/2 used HPACK, which required strictly ordered packet delivery to maintain the shared compression dictionary.
Because QUIC streams can arrive out of order, HTTP/3 uses **QPACK**:
- Employs separate encoder and decoder control streams.
- Allows header compression without creating cross-stream blocking dependencies.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 02 - QUIC Protocol Internals 0 RTT and Connection Migration](./02-QUIC-Protocol-Internals-0-RTT-and-Connection-Migration.md) | [Index](../../../README.md) | [04 - gRPC Architecture Protocol Buffers and Streaming →](./04-gRPC-Architecture-Protocol-Buffers-and-Streaming.md) |
