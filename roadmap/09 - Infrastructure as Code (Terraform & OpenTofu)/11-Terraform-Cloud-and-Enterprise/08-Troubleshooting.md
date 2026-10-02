# 08 - Troubleshooting

| Issue | Cause | Fix |
|---|---|---|
| Run stuck in "Planning" | Agent pool exhausted | Scale up agents or wait for queue |
| VCS webhook not triggering | Webhook misconfigured | Re-create OAuth connection |
| Sentinel policy false positive | Policy too broad | Refine policy conditions |
| Agent can't reach private resources | Network/firewall rules | Open egress from agent to target |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [07 - Real-World Scenarios](./07-Real-World-Scenarios.md) | [README](./README.md) | [09 - Interview QA](./09-Interview-QA.md) |
