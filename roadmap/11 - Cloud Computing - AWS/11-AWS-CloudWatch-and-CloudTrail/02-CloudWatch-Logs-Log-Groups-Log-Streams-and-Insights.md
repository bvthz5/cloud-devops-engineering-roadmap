# 02 - CloudWatch Logs & Logs Insights

Query logs interactively using CloudWatch Logs Insights syntax:
```sql
fields @timestamp, @message, status
| filter status = 500
| sort @timestamp desc
| limit 20
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [01 - CloudWatch Metrics](./01-Amazon-CloudWatch-Metrics-Alarms-and-Dashboards.md) | [README](./README.md) | [03 - AWS CloudTrail](./03-AWS-CloudTrail-Management-and-Data-Events.md) |
