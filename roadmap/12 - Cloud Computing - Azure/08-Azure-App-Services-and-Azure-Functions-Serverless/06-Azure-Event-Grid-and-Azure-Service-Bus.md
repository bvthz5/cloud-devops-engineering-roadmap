# 06 - Enterprise Messaging: Azure Service Bus vs. Azure Event Grid vs. Event Hubs

> **Focus:** Asynchronous communication architectures on Azure. Deep dive into Messages vs. Events, Service Bus Queues & Topics, Peek-Lock semantics, Dead-Letter Queues (DLQ), Event Grid reactive event distribution, and Event Hubs high-throughput telemetry streaming.

---

## 📬 1. What, Why, and History of Azure Messaging & Eventing

### Messages vs. Events: The Core Distinction
- **A Message carries raw intent or instructions:** The sender expects the recipient to perform specific work or transactions (e.g., "Process Order #4091"). The system must guarantee delivery, ordering, and transactional safety.
- **An Event announces a state change that already happened:** The sender does not care who listens or what they do with the fact (e.g., "CustomerPhotoUploaded.jpg Created").

### Why do Cloud Architectures Require Decoupled Messaging?
1. **Temporal Decoupling:** Senders can dispatch thousands of orders even if backend inventory or payment processing systems are temporarily offline for maintenance.
2. **Load Leveling (Buffer Peaks):** Prevents downstream databases from crashing during Black Friday flash sales by queueing requests and processing at a steady, sustainable rate.
3. **Fan-Out Distribution:** A single event (e.g., "AccountCreated") triggers multiple independent services simultaneously (send welcome email, create billing account, provision cloud resources).

### 📜 History & Evolution:
- **2010:** Microsoft launches **Azure Service Bus** (originally Windows Azure AppFabric Service Bus) as an enterprise-grade message broker supporting AMQP 1.0 and WS-Relay.
- **2014:** **Azure Event Hubs** introduced to handle massive big-data streaming ingestion (millions of events per second) with Apache Kafka API compatibility.
- **2017:** **Azure Event Grid** launched as the industry's first cloud-native reactive event router, natively wired into every Azure control-plane event.

---

## ⚖️ 2. Architectural Comparison: Service Bus vs. Event Grid vs. Event Hubs

```text
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                              AZURE MESSAGING ECOSYSTEM                                 │
├────────────────────────────────────────────────────────────────────────────────────────┤
│ 1. AZURE SERVICE BUS (Enterprise Message Broker)                                       │
│    • High-value transactions: Banking, order fulfillment, e-commerce checkouts.        │
│    • Guarantees: At-least-once or Exactly-once (Sessions), Dead-Letter Queuing.         │
│                                                                                        │
│ 2. AZURE EVENT GRID (Reactive Discrete Event Routing)                                  │
│    • Reacting to system state changes: Blob created, VM stopped, User signed up.       │
│    • Push-Push model over HTTP/REST (CloudEvents standard), sub-second latency.         │
│                                                                                        │
│ 3. AZURE EVENT HUBS (Big Data Telemetry Streaming)                                    │
│    • Continuous data streaming: IoT sensor metrics, application clickstreams, logs.    │
│    • Partitioned consumer model; Kafka wire-protocol compatible.                       │
└────────────────────────────────────────────────────────────────────────────────────────┘
```

| Criteria | Azure Service Bus | Azure Event Grid | Azure Event Hubs |
| :--- | :--- | :--- | :--- |
| **Data Nature** | **Messages** (Contractual payload) | **Discrete Events** (Fact notifications) | **Streaming Series** (Continuous time logs) |
| **Primary Protocol** | **AMQP 1.0**, SBMP, HTTPS | **HTTP Webhooks / CloudEvents** | **AMQP / Apache Kafka** |
| **Delivery Model** | **Pull (Polling / Peek-Lock)** | **Push (HTTP Webhook call)** | **Pull (Partition Read Pointer)** |
| **Message Ordering** | Strict FIFO with Message Sessions | Not guaranteed | Guaranteed per partition key |
| **Payload Size Limit**| 256 KB (Std) / 100 MB (Premium) | 1 MB (Standard) | 1 MB (Standard) / 20 MB (Premium) |
| **Dead-Letter Queue** | Built-in native Dead-Letter Queue | Dead-lettering to Azure Storage | None (Retained on log disk buffer) |

---

## 🏗️ 3. Azure Service Bus Mechanics: Peek-Lock & Dead-Lettering

```text
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                               SERVICE BUS PEEK-LOCK FLOW                               │
├────────────────────────────────────────────────────────────────────────────────────────┤
│ 1. Worker reads message with Peek-Lock mode.                                           │
│ 2. Message is hidden from other workers for lock duration (e.g., 60 seconds).          │
│ 3. Success: Worker calls CompleteMessage() ➔ Message deleted permanently from Queue.   │
│ 4. Failure: Worker calls AbandonMessage() ➔ Message immediately visible to others.     │
│ 5. Max Delivery Count Reached (e.g., 10 failed retries):                               │
│    ➔ Automatically moved to Dead-Letter Queue (DLQ: myqueue/$DeadLetterQueue)!         │
└────────────────────────────────────────────────────────────────────────────────────────┘
```

---

## 💻 4. Hands-on CLI: Deploying Service Bus & Event Grid

```bash
# 1. Create a Service Bus Namespace and Queue
az servicebus namespace create \
  --name sb-corp-prod \
  --resource-group rg-messaging \
  --location eastus \
  --sku Standard

az servicebus queue create \
  --name orders-queue \
  --namespace-name sb-corp-prod \
  --resource-group rg-messaging \
  --enable-dead-lettering-on-message-expiration true \
  --max-delivery-count 10

# 2. Create an Event Grid Topic
az eventgrid topic create \
  --name egt-customer-events \
  --resource-group rg-messaging \
  --location eastus

# 3. Create an Event Grid Subscription routing to an Azure Function
az eventgrid event-subscription create \
  --name sub-process-user \
  --source-resource-id $(az eventgrid topic show -n egt-customer-events -g rg-messaging --query id -o tsv) \
  --endpoint $(az functionapp function show -g rg-apps -n func-api-prod --function-name ProcessUserEvent --query id -o tsv) \
  --endpoint-type azurefunction
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 05 - Azure Functions Hosting Plans Consumption Premium Flex](./05-Azure-Functions-Hosting-Plans-Consumption-Premium-Flex.md) | [Index](../../../README.md) | [07 - Real World Scenarios →](./07-Real-World-Scenarios.md) |
