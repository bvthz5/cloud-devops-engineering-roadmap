# 09 - StatefulSets and DaemonSets

> **Learning Methodology:**
> **Understand → See → Practice → Troubleshoot → Interview → Revise**

## 📌 Module Syllabus
1. `01-StatefulSet-Architecture-Stable-Network-and-Storage-Identity.md` — Stateful workloads: sticky network IDs (`pod-0`), ordinal indexes, and headless service coupling.
2. `02-VolumeClaimTemplates-and-Per-Replica-Storage.md` — Automated dedicated persistent disk creation per replica and retention during scaling.
3. `03-OrderedReady-vs-Parallel-Pod-Management-Policies.md` — Scaling strategies: sequential ordered startup/teardown vs `Parallel` pod management.
4. `04-Clustered-Database-Deployment-Patterns-MySQL-Postgres.md` — Production database patterns: leader election, primary-replica replication, and dynamic failovers.
5. `05-DaemonSet-Architecture-and-Node-Level-Agents.md` — Node infrastructure workloads: log collectors (Fluentbit), CNI daemons, monitoring agents (Node Exporter).
6. `06-DaemonSet-Update-Strategies-and-HostPort-Considerations.md` — Rolling updates on DaemonSets, `maxUnavailable`, node selectors, tolerations, and `hostPort` binds.
7. `07-Real-World-Scenarios.md` — Production post-mortems: split-brain in StatefulSet database clusters, and DaemonSet rollout locking entire nodes.
8. `08-Troubleshooting.md` — Diagnostic decision tree for stuck StatefulSet ordinals and failed volume attachments.
9. `09-Interview-QA.md` — 10 Senior SRE/DevOps interview scenarios on StatefulSets and DaemonSets.
10. `10-Hands-On-Practice.md` — Production lab: deploying a 3-node HA Redis/PostgreSQL cluster with StatefulSet and headless discovery.
11. `11-MCQ.md` — 10 scenario-based multiple choice questions with collapsible answers.
12. `12-Quick-Revision.md` — High-density StatefulSet and DaemonSet cheat sheet.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [Module 08 - Storage & Volumes](../08-Storage-PV-PVC-and-StorageClasses/README.md) | [README](./README.md) | [01 - StatefulSet Architecture](./01-StatefulSet-Architecture-Stable-Network-and-Storage-Identity.md) |
