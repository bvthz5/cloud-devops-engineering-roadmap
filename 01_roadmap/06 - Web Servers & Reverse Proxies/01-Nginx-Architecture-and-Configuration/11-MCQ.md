# 11 - Multiple-Choice Assessment (MCQ)

### 1. In Nginx, which location modifier takes the highest evaluation priority?
- [ ] A) `^~`
- [x] B) `=`
- [ ] C) `~*`
- [ ] D) Standard prefix without modifier

<details>
<summary><b>Explanation</b></summary>
The <code>=</code> modifier specifies an exact match and has absolute highest priority. If matched, Nginx immediately stops search and processes the request.
</details>

---

### 2. Which Linux system call does Nginx employ on modern Linux kernels to achieve zero-copy file transmission?
- [ ] A) `fork()`
- [ ] B) `mmap()`
- [x] C) `sendfile()`
- [ ] D) `epoll_create()`

<details>
<summary><b>Explanation</b></summary>
<code>sendfile()</code> allows data transfer directly from kernel page cache to socket buffer, bypassing user-space memory copies.
</details>

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 10 - Hands On Practice](./10-Hands-On-Practice.md) | [Index](../../../README.md) | [12 - Quick Revision →](./12-Quick-Revision.md) |
