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
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

bnvpAA==
```
%%