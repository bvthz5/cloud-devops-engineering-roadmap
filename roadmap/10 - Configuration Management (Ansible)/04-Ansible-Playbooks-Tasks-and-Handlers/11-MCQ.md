# 11 - MCQ: Playbooks & Handlers

#### Q1. Which task directive forces notified handlers to run immediately at that point in the playbook?
- [ ] A) `notify: immediate`
- [x] B) `meta: flush_handlers`
- [ ] C) `ansible.builtin.handler: run`
- [ ] D) `strategy: free`

<details><summary>Explanation</summary>The meta task `meta: flush_handlers` triggers all pending notified handlers immediately before proceeding to the next task.</details>

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [10 - Hands-On Practice](./10-Hands-On-Practice.md) | [README](./README.md) | [12 - Quick Revision](./12-Quick-Revision.md) |
