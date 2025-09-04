
---

## 1. Objective

To replicate **AWS SQS Producer → Queue → Consumer** pattern within **Google Cloud Platform** using **Pub/Sub** (closest managed equivalent). This POC demonstrates:

- Message publishing by a producer.
    
- Message consumption and processing by a consumer.
    
- Configuration to mimic SQS features such as FIFO, visibility timeout, and dead-letter queues.
    

---

## 2. Architecture Overview

### AWS SQS Flow

```
Producer → SQS Queue → Consumer
```

### Equivalent GCP Flow

```
Producer Function (Gen2) → Pub/Sub Topic → Pub/Sub Subscription → Consumer Function (Gen2)
```

---

## 3. Mapping SQS Features to GCP Pub/Sub

|**SQS Feature**|**GCP Pub/Sub Equivalent**|
|---|---|
|Queue|Topic + Subscription|
|FIFO|Ordering Key in Pub/Sub|
|Visibility Timeout|Ack Deadline in Subscription|
|Dead-letter Queue (DLQ)|Dead-letter Topic + Dead-letter Subscription|
|Message Retention|Subscription message retention settings|
|Delivery Attempts|Retry Policy in Subscription|

---

## 4. Components

1. **Pub/Sub Topic**
    
    - Acts as the “Queue”.
        
    - Example: `projects/[PROJECT_ID]/topics/orders-topic`
        
2. **Producer Cloud Function (Gen2)**
    
    - Publishes messages to the Pub/Sub Topic.
        
    - Can set `orderingKey` for FIFO behavior.
        
3. **Pub/Sub Subscription**
    
    - Attached to the topic.
        
    - Configurable ack deadline (visibility timeout).
        
    - Configurable retry + dead-letter policy.
        
    - Example: `projects/[PROJECT_ID]/subscriptions/orders-sub`
        
4. **Dead-letter Topic & Subscription**
    
    - Handles failed messages after N retry attempts.
        
    - Example: `orders-dlq-topic`, `orders-dlq-sub`.
        
5. **Consumer Cloud Function (Gen2)**
    
    - Triggered by subscription.
        
    - Processes the message, acknowledges success, or message is retried / moved to DLQ.
        

---

## 5. Setup Steps

### Step 1: Create Pub/Sub Topic

```bash
gcloud pubsub topics create orders-topic
```

### Step 2: Create Dead-letter Topic

```bash
gcloud pubsub topics create orders-dlq-topic
```

### Step 3: Create Subscription with DLQ + Ack Deadline

```bash
gcloud pubsub subscriptions create orders-sub \
  --topic=orders-topic \
  --ack-deadline=30 \              # Visibility timeout (seconds)
  --dead-letter-topic=orders-dlq-topic \
  --max-delivery-attempts=5 \
  --enable-message-ordering
```

### Step 4: Deploy Producer Function

```bash
gcloud functions deploy producerFn \
  --gen2 \
  --runtime=nodejs20 \
  --region=me-central2 \
  --trigger-http \
  --allow-unauthenticated
```

Sample Producer Code (Node.js):

```js
const {PubSub} = require('@google-cloud/pubsub');
const pubsub = new PubSub();

exports.producerFn = async (req, res) => {
  const message = req.body || { orderId: Date.now() };
  const dataBuffer = Buffer.from(JSON.stringify(message));

  await pubsub.topic('orders-topic').publishMessage({
    data: dataBuffer,
    orderingKey: "order-1"  // ensures FIFO
  });

  res.json({ success: true, message });
};
```

### Step 5: Deploy Consumer Function

```bash
gcloud functions deploy consumerFn \
  --gen2 \
  --runtime=nodejs20 \
  --region=me-central2 \
  --trigger-topic=orders-topic
```

Sample Consumer Code (Node.js):

```js
exports.consumerFn = async (message, context) => {
  try {
    const payload = JSON.parse(Buffer.from(message.data, 'base64').toString());
    console.log("Processing order:", payload);

    // Process logic here...
    // Ack happens automatically if function succeeds.
  } catch (err) {
    console.error("Error processing message:", err);
    throw err; // Message will be retried / sent to DLQ
  }
};
```

---

## 6. Testing

1. Call Producer Function (via HTTP):
    
    ```bash
    curl -X POST https://REGION-PROJECT.cloudfunctions.net/producerFn \
    -H "Content-Type: application/json" \
    -d '{"orderId":"1234"}'
    ```
    
2. Verify logs in Consumer Function:
    
    ```bash
    gcloud functions logs read consumerFn
    ```
    
3. Check DLQ for failed messages:
    
    ```bash
    gcloud pubsub subscriptions pull orders-dlq-sub --limit=10
    ```
    

---

## 7. Benefits

- Fully managed, serverless queue system.
    
- Scales automatically.
    
- Supports retries, DLQ, and FIFO.
    
- Simple migration path from SQS → Pub/Sub.
    

---

