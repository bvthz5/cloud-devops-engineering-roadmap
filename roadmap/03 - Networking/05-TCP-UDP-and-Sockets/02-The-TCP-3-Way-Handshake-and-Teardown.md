# 02 — The TCP 3-Way Handshake and Teardown

Before data can flow over TCP, endpoints establish synchronization parameters.

---

## 1. The 3-Way Handshake (Establishment)

```text
Client                                                  Server (LISTEN)
  │                                                           │
  │ ─── 1. SYN (Seq=X, Options: MSS, WS, SACK) ─────────────> │ Server enters SYN_RCVD
  │                                                           │
  │ <── 2. SYN-ACK (Seq=Y, Ack=X+1) ───────────────────────── │ Client enters ESTABLISHED
  │                                                           │
  │ ─── 3. ACK (Seq=X+1, Ack=Y+1) ──────────────────────────> │ Server enters ESTABLISHED
  │                                                           │
  │ <================== Connection Open ====================> │
```

---

## 2. The 4-Way Teardown (Connection Termination)

```text
Endpoint A (Active Close)                               Endpoint B (Passive Close)
  │                                                           │
  │ ─── 1. FIN (Seq=U) ─────────────────────────────────────> │ Enters CLOSE_WAIT
  │ <── 2. ACK (Ack=U+1) ──────────────────────────────────── │
  │                                                           │
  │ Enters FIN_WAIT_2                                         │ [App calls close()]
  │ <── 3. FIN (Seq=V) ────────────────────────────────────── │
  │ ─── 4. ACK (Ack=V+1) ───────────────────────────────────> │ Enters CLOSED
  │                                                           │
  │ Enters TIME_WAIT (Waits 2*MSL = 60s)                      │
  │ Enters CLOSED                                             │
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [01 - TCP Architecture](./01-TCP-Architecture-and-Reliability-Guarantees.md) | [README](./README.md) | [03 - TCP States & Queues](./03-TCP-Connection-States-and-Socket-Queues.md) |
