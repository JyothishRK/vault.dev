
---
# 📘 **Architecture Document — Trigger Processing Pipeline (Pub/Sub + Tracker + Cron Worker)**

## **1. Overview**

This architecture enables reliable, scalable, event-driven automation for system triggers such as:

- Member Create / Update / Activate / Deactivate / Subscription Activate
    
- Role Create / Update / Activate / Deactivate
    
- Group Create / Update / Activate / Deactivate
    

The architecture ensures:

- Asynchronous processing
    
- Guaranteed retries
    
- Zero duplication
    
- High scalability
    
- Auditability
    
- Safe recovery through a Cron Worker
    

---

# **2. High-Level Architecture Flow (Final Version)**

```
API Endpoint (Node.js / Express)
        ↓
Insert entry into Tracker Collection (status = pending, locked = false)
        ↓
Publish event → GCP Pub/Sub Topic
        ↓
Pub/Sub Consumer (Cloud Function / Cloud Run)
        ↓
Locks tracker entry (locked = true)
        ↓
Executes trigger handler (role/member/group logic)
        ↓
Updates tracker (completed / retrying / failed, locked=false)
        ↓
----- Recovery Layer -----
Cron Worker (runs every 2 min)
        ↓
Finds tasks: status in (pending, retrying), locked=false, attempts < maxAttempts
        ↓
Locks tracker entry (locked = true)
        ↓
Executes the same trigger handler
        ↓
Updates tracker (completed / retrying / failed, locked=false)
```

The **Pub/Sub Consumer handles real-time processing**  
The **Cron Worker handles failed or missed tasks**  
Both share the same logic and locking rules.

---

# **3. Components**

## **3.1 API Layer**

- Creates a tracker entry (`pending`, `attempts=0`)
    
- Publishes event to Pub/Sub topic
    
- No processing happens in the API layer
    

Ensures API response speed and decoupling.

---

## **3.2 Tracker Collection**

This stores:

- Task metadata
    
- Current status
    
- Attempts count
    
- Lock state
    
- Payload needed for processing
    
- Error history
    

It serves as the **single source of truth** for the entire pipeline.

---

## **3.3 Pub/Sub Topic**

- Asynchronous event delivery mechanism
    
- Supports large-scale distributed execution
    
- Provides at-least-once delivery
    
- Does **not** handle retries; retries are controlled by tracker
    

---

## **3.4 Pub/Sub Consumer (Cloud Function / Cloud Run)**

### _Responsibilities:_

1. Receive the Pub/Sub message
    
2. Atomically **lock** the tracker entry
    
3. If lock fails → another worker is processing → **ACK & exit**
    
4. Execute the event handler
    
5. Update tracker
    
6. Unlock (locked=false)
    

### _Why locking is critical?_

Prevents:

- Duplicate processing
    
- Cron + consumer collision
    
- Horizontal scaling conflicts
    

---

## **3.5 Cron Worker (Runs Every 2 Minutes)**

### _Purpose:_

Acts as a **recovery layer** for:

- Messages lost by Pub/Sub
    
- Messages that failed in consumer
    
- Messages stuck due to processing errors
    
- Pending tasks missed by consumer (cold start, timeouts, failures)
    

### _Workflow:_

1. Query tracker for tasks:
    
    ```
    status in (pending, retrying)
    AND locked=false
    AND attempts < maxAttempts
    ```
    
2. Attempt to lock the task (`locked=true`)
    
3. If lock succeeds → process
    
4. Update tracker
    
5. Unlock
    

### _Guarantees:_

- Full durability
    
- No task is left unprocessed
    
- No task is executed twice
    
- Retry logic stays consistent
    

---

# **4. Tracker Schema (Minimal + Scalable + Lock-safe)**

```json
{
  "_id": "string",                      // UUID traceId
  "triggerName": "string",              // e.g., Trigger_t_member
  "eventType": "string",                // e.g., member.update
  "documentId": "string",               // Mongo ID of the record

  "payload": {},                        // Dynamic JSON for processing

  "status": "pending | processing | retrying | completed | failed",
  "attempts": 0,
  "maxAttempts": 5,

  "locked": false,                      // Prevents concurrent processing
  "lastError": "string|null",

  "createdAt": "ISODate",
  "updatedAt": "ISODate"
}
```

This schema stays stable even if 100 new triggers are added.

---

# **5. State Machine for Reliability**

```
pending (locked=false)
        ↓ lock
processing (locked=true)
        ↓ unlock
completed (locked=false)
retrying (locked=false)
failed (locked=false)
```

The task stays eligible for retry until:

```
attempts >= maxAttempts
```

After that, it's **failed permanently**.

---

# **6. Why Cron + Pub/Sub Consumer Together?**

|Responsibility|Pub/Sub Consumer|Cron Worker|
|---|---|---|
|Real-time processing|✅ Yes|❌|
|Guaranteed retry|❌ (not reliable)|✅ Yes|
|Recovery from lost messages|❌|✅|
|Exponential scaling|✅|Limited|
|Safety against duplicates|Via locking|Via locking|
|Ensures all pending tasks are processed|❌|✅|

### Together:

➡ Real-time speed **and** guaranteed reliability.

---

# **7. Example Processing Flow**

### When everything works normally:

```
API → Tracker (pending) → Pub/Sub → Consumer → Completed
```

### When consumer fails:

```
API → Tracker (pending)
        ↓
Pub/Sub → Consumer → error → retrying
        ↓
Cron picks retrying → processes → completed
```

### When Pub/Sub drops message:

```
API → Tracker (pending)
        ↓
Cron (2 min later) picks pending → processes → completed
```

### When consumer & cron run at same time:

```
Consumer tries to lock → succeeds
Cron tries to lock → fails (locked=true)
Cron skips
```

---

    

