
---
## NotifySails - me-2-uat-queue--sails-notify

```
gcloud functions deploy me-2-uat-queue--sails-notify --runtime=nodejs20 --trigger-topic=me-2-uat-queue--data-sync --region=me-central2 --entry-point=sailsNotifyPubSub --source=. --env-vars-file=env.deploy --vpc-connector=me-2-uat-vpc-conn-redis --egress-settings=private-ranges-only
```

---

## me-2-uat-queue--user-bulk-upload

```
gcloud functions deploy me-2-uat-queue--user-bulk-upload --runtime=nodejs20 --trigger-topic=me-2-uat-queue--user-bulk-upload --region=me-central2 --entry-point=BulkUserCreatePubSub --source=. --env-vars-file=env.deploy --vpc-connector=me-2-uat-vpc-conn-redis --egress-settings=private-ranges-only
```

---

## me-2-uat-queue--user-profile-update-delete

```
gcloud functions deploy me-2-uat-queue--user-profile-update-delete --runtime=nodejs20 --trigger-topic=me-2-uat-queue--user-profile-update-delete --region=me-central2 --entry-point=userProfileUpdateDeletePubSub --source=. --env-vars-file=env.deploy --vpc-connector=me-2-uat-vpc-conn-redis --egress-settings=private-ranges-only
```


---

## me-2-uat-queue--user-profile-update-delete

```
gcloud functions deploy me-2-uat-queue--user-profile-update-delete --runtime=nodejs20 --trigger-topic=me-2-uat-queue--user-profile-update-delete --region=me-central2 --entry-point=userProfileUpdateDeletePubSub --source=. --env-vars-file=env.deploy --vpc-connector=me-2-uat-vpc-conn-redis --egress-settings=private-ranges-only