
---

## **1. Overview**

This architecture enables reliable and scalable event-driven automation for system triggers such as:

- Member: Create, Update, Activate, Deactivate, Subscription Activate
    
- Role: Create, Update, Activate, Deactivate
    
- Group: Create, Update, Activate, Deactivate
    

It ensures:

- Asynchronous processing
    
- Guaranteed retries
    
- Zero duplication
    
- High scalability
    
- Full auditability
    
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

![[Pasted image 20251119144224.png]]

---

# **3. Components**

## **3.1 API Layer**

- Creates a tracker entry (pending, attempts=0)
    
- Publishes an event into Pub/Sub
    
- Does **not** execute business logic
    

This keeps API response times fast and decoupled from processing.

---

## **3.2 Tracker Collection**

Stores:

- Task metadata
    
- Current status
    
- Attempts count
    
- Full payload
    

---

## **3.3 Pub/Sub Topic**

- Delivers events asynchronously
    
- Supports large-scale distributed execution
    
- Provides _at-least-once_ delivery
    
- Retries are controlled by the Tracker—not Pub/Sub
    

---

## **3.4 Pub/Sub Consumer (Cloud Function / Cloud Run)**

### **Responsibilities**

1. Receive Pub/Sub message
    
2. Atomically lock the tracker entry
    
3. If lock fails → another worker is processing → ACK & exit
    
4. Execute trigger handler
    
5. Update tracker
    
6. Unlock
    

### **Why locking is critical?**

Prevents:

- Duplicate execution
    
- Cron vs Consumer collisions
    
- Multi-instance conflicts when autoscaling
    

---

## **3.5 Cron Worker (Runs Every 2 Minutes)**

### **Purpose**

Recovery mechanism for:

- Missed Pub/Sub messages
    
- Failed consumer executions
    
- Stuck tasks
    
- Cold start or timeout issues
    

### **Workflow**

1. Query tracker for:
    
    `status in (pending, retrying) AND locked = false AND attempts < maxAttempts`
    
2. Attempt lock
    
3. If lock succeeds → process
    
4. Update tracker
    
5. Unlock
    

### **Guarantees**

- No task is left unprocessed
    
- No duplication
    
- Full reliability even when Pub/Sub fails
    

---

# **4. Tracker Schema (Minimal, Stable & Lock-Safe)**



This schema remains stable even as new triggers are added.

### Payload structure:

```
{
  "_id": "string",
  "triggerName": "string",
  "eventType": "string",
  "documentId": "string",
  "organisation_id": number,
  "payload": {},

  "status": "pending | in-progress | completed | failed",
  "attempts": 0,
  "maxAttempts": 5,

  "createdAt": "ISODate",
  "updatedAt": "ISODate"
}
```

### Example Payloads:

```
{
  "subTasks": null | {
    "items": ["string"],
    "currentIndex": 0
  }
}
```
- User:
    
```
{
  "subTasks": null
}
```


- Group:
    

```
{
  "subTasks": {
    "items": ["member_11", "member_22", "member_33"], //_ids will be stored
    "currentIndex": 0
  }
}
```

- Roles:
    

```
{
  "subTasks": {
    "items": [                     // _ids of the respective collection
      "6745b92f6c1eaa331df81201",
      "6745b92f6c1eaa331df81277",
      "6745b92f6c1eaa331df81333"
    ],
    "currentIndex": 0
  }
}
```

---

# **5. State Machine for Reliability**

Retries continue until:

`attempts >= maxAttempts`

After that, the task becomes permanently **failed**.

---

# **6. Why Use Both Pub/Sub Consumer and Cron Worker?**

|   |   |   |
|---|---|---|
|Responsibility|Pub/Sub Consumer|Cron Worker|
|Real-time execution|✅|❌|
|Guaranteed retry|❌|✅|
|Recovery from lost messages|❌|✅|
|Horizontal scaling|✅|Limited|
|Protection against duplicates|Via locking|Via locking|
|Ensures all pending tasks finish|❌|✅|

### **Combined Result:**

✔ Real-time performance + ✔ Guaranteed reliability + ✔ Zero data loss

---

# **7. Example End-to-End Scenarios**

### **Normal Flow**

`API → Tracker (pending)      → Pub/Sub → Consumer → Completed`

### **Consumer Fails**

`API → Tracker (pending)      → Pub/Sub → Consumer → Error → retrying      → Cron picks → completed`

### **Pub/Sub Drops Message**

`API → Tracker (pending)      → Cron picks after 2 mins → completed`

### **Consumer & Cron Try Same Task**

`Consumer locks → Cron lock fails → Cron skips`

---