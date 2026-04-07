---

excalidraw-plugin: parsed
tags: [excalidraw]

---
==⚠  Switch to EXCALIDRAW VIEW in the MORE OPTIONS menu of this document. ⚠== You can decompress Drawing data with the command palette: 'Decompress current Excalidraw file'. For more info check in plugin settings under 'Saving'


# Excalidraw Data

## Text Elements
Triggers:
----
1. Member
2. Roles
3. Usergroups



 ^Vb3wGOJB

* Member trigger:
----
Legacy triggers:
--
1. Trigger_t_member

Operations:
--
1. Update
2. Replace (No business logic present)

Fields:
--
1. role_count
2. roles
3. usergroup_id

Action:
--
1. Based on the roles and user groups attached, license_type and the read_only_flag is updated in the t_member collection.
2. If power user:
    - license_type : "power users"
    - read_only_flag: 0
3. If light user:
    - license_type: "light users"
    - read_only_flag: 1
    t_my_settings:
    - duediligence_email_alert: 1 ^HiPdtApx

* Role trigger:
----
Legacy triggers:
--
1. Trigger_vc_roles

Operations:
--
1. Update

Fields:
--
1. roleActions
2. licenseType

Action:
--
1. In case of licenseType update, based on the license type: power user or light user, relevant operations are performed.
2. If power user:
    - For all users (t_member)
    - license_type: "power users"
    - read_only_flag: 0
3. If light user:
    - If the member doesn't have power user role (t_member):
    - license_type: "light users"
    - read_only_flag: 1
    t_my_settings:
    - duediligence_email_alert: 1
4. Logging the event in t_report_schedule_events_sent ^abutUuR8

* User group trigger:
----
Legacy triggers:
--
1. Trigger_vc_usergroups1
2. Trigger_vc_usergroups_insert

Operations:
--
1. Update
2. Insert

Fields:
--
1. N/A

Action:
--
1. Based on the role type linked to the user group, the users linked to the group will be updated. Based on the license, the following fields will be updated:
    - license_type
    - read_only_flag
   t_my_settings:
    - duediligence_email_alert: 1 (in case of light users)
2. Based on the role type linked, the t_organisation.organisation_admin key is updated.
 ^W4gtrosq

Flow:
----
API call -> SQS (Trigger Logic) -> CRON (Recovery Layer)

Current Scope:
- The triggers related to following entities will be picked up:
    - Member
    - Roles
    - User groups
- The related APIs will have a SQS call, the operations will be for a single entity.
- Cron will act as a recovery layer

Not in Scope:
- Batch Processing ^NyRwHzir

Updated Schema:
----

{
  "_id": "string",

  "triggerName": "Trigger_vc_roles",
  "eventType": "role.event",

  "documentId": "691bf941178880811da42eb6",
  "organisation_id": 38,

  "operation": "update",

  "status": "pending",            // pending | processing | completed | failed
  "attempts": 0,
  "maxAttempts": 5,

  "payload": {
    "changeEvent": {
      "operationType": "update",
      "documentKey": {
        "_id": "691bf941178880811da42eb6"
      },
      "updateDescription": {
        "updatedFields": {},
        "removedFields": []
      }
    },

    "subTasks": {
      "items": [],
      "currentIndex": 0
    }
  },

  "createdAt": "ISODate",
  "updatedAt": "ISODate"
}
 ^uYU9YQwA

Timeline (Tentative):
----
7th April
--
* New Schema Finalization: 3:00pm
* Review of the planned items: 4:00pm
* Member trigger development: 4:00pm - 6:00pm
* Member trigger UAT deployment: 6:30pm

8th April
--
* Roles trigger development: 10:00am - 1:00pm
* Roles trigger UAT deployment: 2:30pm
* User groups trigger development: 3:00pm - 5:00pm
* User groups trigger UAT deployment: 5:30pm

UAT testing to be closed before 9th April: 1:00 pm


 ^pWRICNNa

%%
## Drawing
```compressed-json
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
```
%%