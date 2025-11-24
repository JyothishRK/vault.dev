
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

