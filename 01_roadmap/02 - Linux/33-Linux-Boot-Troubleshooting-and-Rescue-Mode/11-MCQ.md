# 11 — Multiple Choice Questions: Boot Troubleshooting

---

### Q1. Why is `touch /.autorelabel` required after resetting a root password using `rd.break` on RHEL/Rocky systems?
- [ ] A) To update the hardware clock
- [ ] B) To instruct SELinux to scan and restore security contexts on modified authentication files (such as `/etc/shadow`) during reboot
- [ ] C) To format swap space
- [ ] D) To trigger an automated file system check (fsck)

<details>
<summary><b>View Answer</b></summary>
<b>Correct Answer: B</b><br>
When booting with <code>rd.break</code>, SELinux is not active. Any changes to <code>/etc/shadow</code> lack proper SELinux contexts. <code>/.autorelabel</code> forces SELinux to relabel all files upon restart, preventing access denial.
</details>

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 10 - Hands On Practice](./10-Hands-On-Practice.md) | [Index](../../../README.md) | [12 - Quick Revision →](./12-Quick-Revision.md) |
