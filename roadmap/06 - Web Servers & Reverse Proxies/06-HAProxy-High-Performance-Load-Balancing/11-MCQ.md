# 11 - Multiple-Choice Assessment (MCQ)

### 1. Which HAProxy directive specifies the number of OS threads spawned by the worker process?
- [ ] A) `worker_processes`
- [ ] B) `threads`
- [x] C) `nbthread`
- [ ] D) `max_threads`

<details>
<summary><b>Explanation</b></summary>
<code>nbthread</code> sets the number of concurrent operating system threads spawned by HAProxy (typically set to <code>nbthread auto</code> or the number of CPU cores).
</details>

---

### 2. Which Linux system call is used by HAProxy in TCP mode to transfer data between sockets without copying into user space?
- [ ] A) `read()`
- [x] B) `splice()`
- [ ] C) `mmap()`
- [ ] D) `epoll_wait()`

<details>
<summary><b>Explanation</b></summary>
The <code>splice()</code> system call enables zero-copy data transfer between two file/socket descriptors via kernel memory pipes.
</details>

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [10 - Hands-On Practice](./10-Hands-On-Practice.md) | [README](./README.md) | [12 - Quick Revision](./12-Quick-Revision.md) |
