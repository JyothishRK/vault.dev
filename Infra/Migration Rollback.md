---

excalidraw-plugin: parsed
tags: [excalidraw]

---
==⚠  Switch to EXCALIDRAW VIEW in the MORE OPTIONS menu of this document. ⚠== You can decompress Drawing data with the command palette: 'Decompress current Excalidraw file'. For more info check in plugin settings under 'Saving'


# Excalidraw Data

## Text Elements
AWS Clients:
-----
* Applications:
    - PF1 - app.v-comply.com
    - PF3 (Legacy) - platform.v-comply.com
    - PF3 (Migration) - central.v-comply.com
    - PF4 - portal.v-comply.com
* Regions:
    - us-east-1, eu-west-1.
* Current state of customers:
    - Using the legacy application in PF1 and PF3.
* Post migration state:
    - Clients moved to PF4 and PF3 (Migration Setup)
* Order of Execution for migration:
    - Client account disabled after communication.
    - Mongo -> Mongo Migration.
    - Mongo -> Postgres Migration.
    - S3 -> S3 Migration.
    - Data verification.
* In case of rollback:
    - Deactivate the new account in central.v-comply.com (PF3 Migration), portal.v-comply.com (PF4)
    - Use the accounts present in app.v-comply.com(PF1), platform.v-comply.com(PF3 Legacy)
* Outcomes:
    - Client will use the existing application.
    - No data loss.
 ^LvOctxfX

GCP Clients:
-----
* Applications:
    - PF2 - app-prod-gcp.v-comply.com
* Regions:
    - me-central2 (Dammam)
* Current state of customers:
    - Using migrated system (Compliance + Policy)
* Post migration state:
    - Will use the migrated system with Organization as well (Compliance + Policy + Organization)
* Order of Execution for migration:
    - Client account disabled after communication.
    - Postgres DB Backup snapshot
    - Mongo -> Mongo Migration.
    - Mongo -> Postgres Migration.
    - GCS -> GCS Migration.
    - New application deployment
    - Re-routing traffic
        - Login
        - Organization
        - Settings
    - Data verification.
* In case of rollback:
    - Rollback the deployments and reroute the traffic to angular applications
    - Rollback DB to older schemas using the snapshot
* Outcomes:
    - Client will use the existing application.
    - No data loss.  ^YEgau7dM

%%
## Drawing
```compressed-json
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=
```
%%