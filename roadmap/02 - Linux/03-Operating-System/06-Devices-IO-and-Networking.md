# 06 - Devices, I/O Subsystems & Networking

The input/output (I/O) subsystem is responsible for communicating with hardware peripherals spanning extreme variations in transfer speed, latency, and access mechanisms.

---

## 1. The 3 Methods of I/O Handling

How does the CPU coordinate data transfers with peripheral hardware?

```
1. Programmed I/O (Polling / Busy-Waiting):
   CPU continuously loops asking: "Is data ready? Is data ready?"
   • Pros: Minimal latency for ultra-fast devices.
   • Cons: Wastes 100% of CPU cycles doing nothing.

2. Interrupt-Driven I/O:
   CPU tells device to start, then switches to other work.
   When finished, device fires a physical electrical pin: Interrupt Request (IRQ).
   • Pros: CPU remains productive while I/O is in-flight.
   • Cons: Interrupt handling overhead at high packet/transfer rates.

3. Direct Memory Access (DMA):
   Specialized DMA controller transfers entire megabytes of data directly
   between device hardware and physical RAM without routing through CPU registers!
   • Pros: The CPU only receives 1 interrupt when the ENTIRE multi-megabyte block is done.
```

---

## 2. Block Devices vs. Character Devices

Unix/Linux divides device interfaces into two primary categories:

```
┌───────────────────────────────────────┬───────────────────────────────────────┐
│         Block Devices (`b`)           │       Character Devices (`c`)         │
├───────────────────────────────────────┼───────────────────────────────────────┤
│ • Transfers data in fixed-size blocks │ • Transfers unbuffered stream of      │
│   (e.g., 512 bytes or 4096 bytes).    │   individual bytes sequentially.      │
│ • Supports random seeking to any      │ • No random seeking (streaming data). │
│   block offset on the disk.           │                                       │
│ • Heavily cached via kernel Page Cache│ • Data typically consumed immediately.│
│ • Examples: `/dev/sda`, `/dev/nvme0n1`│ • Examples: `/dev/tty`, `/dev/urandom`│
└───────────────────────────────────────┴───────────────────────────────────────┘
```

---

## 3. Network Subsystem & Interrupt Handling

Network interface cards (NICs) represent a unique challenge: they cannot be treated as standard block or character files because packets arrive asynchronously over the wire.

### Network Devices in Linux:
- Managed via the socket API (`socket()`, `bind()`, `listen()`, `connect()`).
- Data flows through kernel packet buffers called **`sk_buff`** structures.
- Does not have entries in `/dev`; exposed as network interfaces (e.g., `eth0`, `wlan0`, `docker0`) in `/sys/class/net/` and configured via `ip` commands.

### Avoiding "Interrupt Livelock" with NAPI:
If a 100 Gbps network card fired a CPU hardware interrupt for every arriving packet, the CPU would spend 100% of its time processing interrupts, starving user applications (**Interrupt Livelock**).
Modern Linux uses **NAPI (New API)**:
1. First packet triggers a hardware interrupt.
2. The kernel switches the network driver into **polling mode** to drain the ring buffer.
3. Once the packet burst subsides, the driver re-enables hardware interrupts.
---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [05 - File and Storage Management](./05-File-and-Storage-Management.md) | [README](./README.md) | [07 - Kernel User Space System Calls](./07-Kernel-User-Space-System-Calls.md) |
