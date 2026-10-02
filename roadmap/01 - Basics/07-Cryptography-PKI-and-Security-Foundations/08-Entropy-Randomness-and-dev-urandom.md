# 08 — Entropy, Randomness, and /dev/urandom

Cryptographic security depends entirely on unpredictable random numbers. Generating SSH keys, TLS session tokens, and passwords requires high-quality **Entropy**.

---

## 1. Linux Kernel Entropy Gathering

The Linux kernel gathers environmental noise from unpredictable hardware events:
- Keystroke timing and mouse movements
- Hardware interrupt timings (NIC packet arrivals)
- Disk I/O seek times
- CPU hardware random number generator (`RDRAND` instruction)

---

## 2. `/dev/random` vs `/dev/urandom`

| Device | Type | Behavior | Recommendation |
| :--- | :--- | :--- | :--- |
| **`/dev/random`** (Legacy) | Blocking | If entropy pool drops low, reads **block and halt the process** | Legacy; do not use |
| **`/dev/urandom`** | Non-blocking | Uses a CSPRNG (ChaCha20). **Never blocks** | **Universal Best Practice** |

> [!NOTE]
> Since Linux kernel 5.6 (2020), `/dev/random` and `/dev/urandom` behave identically: both block only once during initial boot until 128 bits of entropy are gathered, and then never block again.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [07 - Password Security & KDFs](./07-Password-Security-Salts-and-KDFs.md) | [README](./README.md) | [09 - Real-World Scenarios](./09-Real-World-Scenarios.md) |
