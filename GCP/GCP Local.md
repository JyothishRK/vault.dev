```
$env:GOOGLE_APPLICATION_CREDENTIALS = "D:\GCP\gcp-b-all\organisation\keys.json"
```

## memberCreate

1. t_member
2. t_organisation
3. member_subscription
4. vc_account_access

NotifySails - Data Sync

```
gcloud functions deploy notify-sails-queue --runtime=nodejs20 --trigger-topic=me-2-uat-queue--data-sync --region=me-central2 --entry-point=sailsNotifyPubSub --source=. --env-vars-file=env.deploy --vpc-connector=me-2-uat-vpc-conn-redis --egress-settings=all
```

URL: https://me-central2-me-2-uat.cloudfunctions.net/notifySailsOnDataSync

Subscription create - Manual create if needed:

```
gcloud pubsub subscriptions create notify-sails-data-sync-sub `
  --topic=me-2-uat-queue--data-sync `
  --push-endpoint=https://REGION-PROJECT_ID.cloudfunctions.net/notifySailsOnDataSync `
  --ack-deadline=30

```


---

## Lambda Deployments

### 1. Organisation

```
gcloud functions deploy organisation --runtime=nodejs20 --trigger-http --allow-unauthenticated --region=me-central2 --entry-point=handler --source=. --set-build-env-vars=NPM_CONFIG_LEGACY_PEER_DEPS=true --vpc-connector=me-2-uat-vpc-conn-redis --egress-settings=private-ranges-only
```

### Policy

```
gcloud functions deploy policy --runtime=nodejs20 --trigger-http --allow-unauthenticated --region=me-central2 --entry-point=handler --source=. --set-build-env-vars=NPM_CONFIG_LEGACY_PEER_DEPS=true --vpc-connector=me-2-uat-vpc-conn-redis --egress-settings=private-ranges-only
```