# 06 - Database Latency and Connection Prober Template

```python
#!/usr/bin/env python3
# db_latency_prober.py
import time
import psycopg2
import os

DB_HOST = os.environ.get("DB_HOST", "localhost")
DB_USER = os.environ.get("DB_USER", "postgres")
DB_PASS = os.environ.get("DB_PASS", "postgres")
DB_NAME = os.environ.get("DB_NAME", "production")

def probe_database():
    start = time.perf_counter()
    try:
        conn = psycopg2.connect(
            host=DB_HOST,
            user=DB_USER,
            password=DB_PASS,
            dbname=DB_NAME,
            connect_timeout=3
        )
        cursor = conn.cursor()
        cursor.execute("SELECT count(*) FROM pg_stat_activity WHERE state = 'active';")
        active_conns = cursor.fetchone()[0]
        duration_ms = (time.perf_counter() - start) * 1000

        print(f"[OK] DB Probe: {duration_ms:.2f}ms | Active Connections: {active_conns}")
        cursor.close()
        conn.close()
    except Exception as e:
        print(f"[CRITICAL] Database connection failed: {e}")

if __name__ == "__main__":
    probe_database()
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [05 - Slack Alert Dispatcher](./05-Slack-and-PagerDuty-Alert-Dispatcher-Template.md) | [README](./README.md) | [07 - Real-World Scenarios](./07-Real-World-Scenarios.md) |
