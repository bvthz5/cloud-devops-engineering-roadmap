# 11 — Multiple Choice Questions: Linux Networking

Self-assessment multiple-choice questions with answers and technical rationales.

---

### Q1. What does the command `ip route get 8.8.8.8` display?
- [ ] A) It downloads Google DNS settings
- [ ] B) It simulates and displays the exact egress interface, gateway, and source IP the kernel will select for traffic destined to 8.8.8.8
- [ ] C) It tests ping latency to 8.8.8.8
- [ ] D) It adds a new route to 8.8.8.8

<details>
<summary><b>View Answer</b></summary>
<b>Correct Answer: B</b><br>
<code>ip route get &lt;ip&gt;</code> performs an immediate route table evaluation against the Linux FIB (Forwarding Information Base) and outputs the outgoing interface, source IP, and gateway.
</details>

---

### Q2. Which TCP state indicates that the remote peer initiated connection teardown, and the local application must now call `close()` on its socket?
- [ ] A) TIME_WAIT
- [ ] B) CLOSE_WAIT
- [ ] C) FIN_WAIT_2
- [ ] D) SYN_RECV

<details>
<summary><b>View Answer</b></summary>
<b>Correct Answer: B</b><br>
In <code>CLOSE_WAIT</code>, the kernel has received and acknowledged the remote host's FIN packet, and is waiting for the local user-space program to close the socket descriptor.
</details>

---

### Q3. On a listening TCP socket, what does a non-zero `Recv-Q` value in `ss -tulpn` indicate?
- [ ] A) Incoming network packets are arriving faster than line rate
- [ ] B) The application is slow or frozen and failing to accept completed 3-way handshakes from the kernel backlog queue
- [ ] C) The remote client has dropped the connection
- [ ] D) The socket is ready for SSL encryption

<details>
<summary><b>View Answer</b></summary>
<b>Correct Answer: B</b><br>
For listening sockets, <code>Recv-Q</code> is the number of connections in the accept queue. If greater than 0, the application is failing to invoke <code>accept()</code> fast enough.
</details>

---

### Q4. Which configuration option in `/etc/resolv.conf` can cause high DNS query amplification when querying external domains in Kubernetes?
- [ ] A) `timeout:5`
- [ ] B) `attempts:3`
- [ ] C) `ndots:5`
- [ ] D) `edns0`

<details>
<summary><b>View Answer</b></summary>
<b>Correct Answer: C</b><br>
<code>ndots:5</code> instructs the resolver to append search domains sequentially for any query containing fewer than 5 dots before attempting the absolute domain query.
</details>

---

### Q5. What is the standard MTU of Ethernet frames?
- [ ] A) 1450 bytes
- [ ] B) 1500 bytes
- [ ] C) 9000 bytes
- [ ] D) 65535 bytes

<details>
<summary><b>View Answer</b></summary>
<b>Correct Answer: B</b><br>
Standard Ethernet Layer 2 frames support a Maximum Transmission Unit (MTU) of 1500 bytes for the Layer 3 payload.
</details>

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 10 - Hands On Practice](./10-Hands-On-Practice.md) | [Index](../../../README.md) | [12 - Quick Revision →](./12-Quick-Revision.md) |
