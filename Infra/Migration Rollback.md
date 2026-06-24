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
 ^YEgau7dM

%%
## Drawing
```compressed-json
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
```
%%