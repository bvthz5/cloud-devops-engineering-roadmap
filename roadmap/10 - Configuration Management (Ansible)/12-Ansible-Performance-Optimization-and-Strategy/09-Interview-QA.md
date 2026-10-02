# 09 - Interview Q&A: Performance Optimization

### Q: What is SSH pipelining in Ansible and how does it improve execution speed?
**Answer:** SSH pipelining executes module payloads directly over stdin without copying temporary python script files via SFTP/SCP to disk, significantly reducing SSH round-trips.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [08 - Troubleshooting](./08-Troubleshooting.md) | [README](./README.md) | [10 - Hands-On Practice](./10-Hands-On-Practice.md) |
