# 07 - APIs & Webhooks: Real-World Production Scenarios

## Scenario 1: The Double-Billing Webhook Incident

### Incident Summary
A SaaS platform processed customer subscriptions via Stripe webhooks. During a network blip between AWS and Stripe, Stripe retried a `customer.subscription.created` webhook 4 times over 10 minutes. The backend application lacked idempotency checks and processed every retry, creating 4 duplicate charges on 850 customer credit cards!

### Root Cause
The webhook handler treated every incoming POST request as a unique transaction without verifying whether the Stripe Event ID (`evt_xxxx`) had already been recorded.

### Resolution
1. Wrapped all webhook processing in a database unique constraint:
   ```sql
   CREATE TABLE processed_webhook_events (
       event_id VARCHAR(255) PRIMARY KEY,
       processed_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
   );
   ```
2. If `INSERT` throws a unique constraint collision, return `HTTP 200 OK` immediately and skip processing.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 06 - Rate Limiting Exponential Backoff and Jitter](./06-Rate-Limiting-Exponential-Backoff-and-Jitter.md) | [Index](../../../README.md) | [08 - Troubleshooting →](./08-Troubleshooting.md) |
