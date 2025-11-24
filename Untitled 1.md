
---

# ✅ **1. High-Level Architecture (GCP)**

```
API Endpoint (Node.js / Express)
        ↓
Publish message → GCP Pub/Sub Topic
        ↓
Subscriber (Cloud Function / Cloud Run)
        ↓
Execute Trigger Operation (Role Update, Group Create, etc.)
        ↓
Update Trackers Collection
```

This gives you:

✔ decoupled  
✔ scalable  
✔ retry-safe  
✔ event-driven automation

---

# ✅ **2. Generalised Pub/Sub Message Format**

We must design a _universal_ message payload so ANY trigger can use it:

```json
{
  "triggerName": "Trigger_t_member",
  "eventType": "member.update",
  "documentId": "65f89d12ab3c21",
  "before": {},
  "after": {},
  "payload": {}, 
  "initiatedBy": "system|user",
  "timestamp": "2025-11-18T12:30:00Z",
  "traceId": "uuid-123",
  "retryCount": 0
}
```

### Explanation:

|Field|Purpose|
|---|---|
|**triggerName**|Identifies source trigger (role, member, group)|
|**eventType**|Standardized event name (`member.create`, `role.update`, etc.)|
|**documentId**|The `_id` which changed in Mongo|
|**before/after**|Optional; diff of the update|
|**payload**|Full API payload if needed by consumer|
|**initiatedBy**|Who performed the action|
|**timestamp**|Event time|
|**traceId**|For debugging/log correlation|
|**retryCount**|Increment on Pub/Sub dead-letter retries|

---

# ✅ **3. Tracker Collection (Generalized + Scalable)**

This will track completion status of _any_ trigger event.

### **Collection name:** `trigger_tracker`

### ✔ **Schema (Generalized)**

```json
{
  "_id": "uuid",
  "triggerName": "Trigger_t_member",
  "eventType": "member.update",
  "documentId": "65f89d12ab3c21",
  "status": "completed | pending | failed | retrying",
  "attempts": 1,
  "error": null,
  "messageRefId": "pubsub-message-id",
  "createdAt": "2025-11-18T12:30:00Z",
  "updatedAt": "2025-11-18T12:30:20Z"
}
```

### Status Lifecycle:

|Status|Meaning|
|---|---|
|**pending**|Message published but not processed yet|
|**retrying**|Consumer failing; retry in progress|
|**completed**|Event processed successfully|
|**failed**|All retries exhausted|

---

# ✅ **4. Publisher (Inside API code)**

Whenever a DB operation occurs, API will publish:

```js
const message = {
  triggerName: "Trigger_t_member",
  eventType: "member.update",
  documentId: doc._id.toString(),
  before: previousData,
  after: updatedData,
  payload: requestBody,
  initiatedBy: userId || "system",
  timestamp: new Date().toISOString(),
  traceId: uuid(),
  retryCount: 0
}

await pubsub.topic("vc-trigger-events").publishMessage({
  json: message
});
```

And create a **tracker entry** immediately:

```js
await TriggerTracker.create({
  _id: message.traceId,
  triggerName: message.triggerName,
  eventType: message.eventType,
  documentId: message.documentId,
  status: "pending",
  attempts: 0,
  createdAt: new Date(),
});
```

---

# ✅ **5. Consumer (Cloud Function / Cloud Run)**

The consumer picks, processes, and updates status.

### Generic Consumer Logic:

```js
exports.handleTriggerEvent = async (message) => {
  const data = JSON.parse(Buffer.from(message.data, 'base64').toString());

  const { triggerName, eventType, documentId, traceId } = data;

  try {
    // Mark tracker as retrying/pending
    await TriggerTracker.updateOne(
      { _id: traceId },
      { $inc: { attempts: 1 }, status: "retrying" }
    );

    // Route to handler
    await routeEvent(triggerName, eventType, data);

    // Mark as completed
    await TriggerTracker.updateOne(
      { _id: traceId },
      { status: "completed", updatedAt: new Date() }
    );

  } catch (err) {
    const attempts = await TriggerTracker.findOne({ _id: traceId }).attempts;

    if (attempts >= 5) {
      // Hard fail
      await TriggerTracker.updateOne(
        { _id: traceId },
        { status: "failed", error: err.message }
      );
    } else {
      throw err; // Allow Pub/Sub retry
    }
  }
};
```

---

# ✅ **6. Routing Logic (Generalized)**

Every event goes through one centralized router:

```js
async function routeEvent(triggerName, eventType, data) {

  const handlerMap = {
    "Trigger_t_member": {
      "member.create": handleMemberCreate,
      "member.update": handleMemberUpdate,
      "member.activate": handleMemberActivation,
      "member.unsubscribeToSubscribe": handleMemberSubscribe
    },
    "Trigger_vc_roles": {
      "role.create": handleRoleCreate,
      "role.update": handleRoleUpdate,
      "role.activate": handleRoleActivation
    },
    "Trigger_vc_usergroups": {
      "group.create": handleGroupCreate,
      "group.update": handleGroupUpdate,
      "group.activate": handleGroupActivation
    }
  };

  const handler = handlerMap?.[triggerName]?.[eventType];

  if (!handler) throw new Error(`No handler found for: ${triggerName} → ${eventType}`);

  return handler(data);
}
```

---

**Resource Links:**

A complete Roadmap you can follow throughout the month: [https://roadmap.sh/ai-engineer](https://roadmap.sh/ai-engineer "https://roadmap.sh/ai-engineer")

More Guides:

- Basics:  [https://www.tableau.com/data-insights/ai/what-is](https://www.tableau.com/data-insights/ai/what-is "https://www.tableau.com/data-insights/ai/what-is")
- Timeline & In-depth: [https://medium.com/@amitvsolutions/artificial-intelligence-101-the-complete-beginners-guide-to-arti…](https://medium.com/@amitvsolutions/artificial-intelligence-101-the-complete-beginners-guide-to-artificial-intelligence-99b357949940 "https://medium.com/@amitvsolutions/artificial-intelligence-101-the-complete-beginners-guide-to-artificial-intelligence-99b357949940")

**Extras for interest:**

- Generative Pre-trained Transformers:
    1. [https://www.datacamp.com/tutorial/how-transformers-work](https://www.datacamp.com/tutorial/how-transformers-work "https://www.datacamp.com/tutorial/how-transformers-work")
    2. [https://www.geeksforgeeks.org/artificial-intelligence/introduction-to-generative-pre-trained-transf…](https://www.geeksforgeeks.org/artificial-intelligence/introduction-to-generative-pre-trained-transformer-gpt/ "https://www.geeksforgeeks.org/artificial-intelligence/introduction-to-generative-pre-trained-transformer-gpt/")
    3. Extra: Deep Dive in Base Transformer Architecture: [Attention is all you need by Ashish Vaswani et al](https://arxiv.org/abs/1706.03762 "https://arxiv.org/abs/1706.03762")
- Current Trends in AGI & AI Agents: [https://agilayer.com/cutting%E2%80%91edge-trends-in-agi-ai-agents-june-2025/](https://agilayer.com/cutting%E2%80%91edge-trends-in-agi-ai-agents-june-2025/ "https://agilayer.com/cutting%e2%80%91edge-trends-in-agi-ai-agents-june-2025/")
- NGENT: [https://arxiv.org/html/2504.21433](https://arxiv.org/html/2504.21433 "https://arxiv.org/html/2504.21433")
- Emotion AI: [https://research.aimultiple.com/affective-computing/](https://research.aimultiple.com/affective-computing/ "https://research.aimultiple.com/affective-computing/")
- Basic CNN Model Project to visualize the process: [https://colab.research.google.com/drive/1bTf3KAUy5heMrKgfIxABE1VTMEKdQRds?usp=sharing](https://colab.research.google.com/drive/1bTf3KAUy5heMrKgfIxABE1VTMEKdQRds?usp=sharing "https://colab.research.google.com/drive/1btf3kauy5hemrkgfixabe1vtmekdqrds?usp=sharing")

**Next Session SneakPeak:**

- [https://medium.com/@meenn396/differences-between-llm-deep-learning-machine-learning-and-ai-3c7eb1c8…](https://medium.com/@meenn396/differences-between-llm-deep-learning-machine-learning-and-ai-3c7eb1c87ef8 "https://medium.com/@meenn396/differences-between-llm-deep-learning-machine-learning-and-ai-3c7eb1c87ef8")

AI Engineer Roadmap

Step by step guide to becoming an AI Engineer in 2025