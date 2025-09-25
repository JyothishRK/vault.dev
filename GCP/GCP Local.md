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
gcloud functions deploy notify-sails-queue --runtime=nodejs20 --trigger-topic=me-2-uat-queue--data-sync --region=me-central2 --entry-point=sailsNotifyPubSub --source=. --set-build-env-vars=NPM_CONFIG_LEGACY_PEER_DEPS=true --vpc-connector=me-2-uat-vpc-conn-redis --egress-settings=all
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


Env
```
FEATURE_FLAG_URL: devapi.v-comply.com
LOG_EXECUTION_ID: 'true'
NPM_CONFIG_LEGACY_PEER_DEPS: 'true'
PROJECT_ID: me-2-uat
SECRET_NAME: me-2-uat-secret
```

### Report Download Excel

```
gcloud functions deploy report-download-excel --runtime=nodejs20 --trigger-http --allow-unauthenticated --region=me-central2 --entry-point=handler --source=. --set-build-env-vars=NPM_CONFIG_LEGACY_PEER_DEPS=true --vpc-connector=me-2-uat-vpc-conn-redis --egress-settings=private-ranges-only
```


### Globals

```
gcloud functions deploy globals --runtime=nodejs20 --trigger-http --allow-unauthenticated --region=me-central2 --entry-point=handler --source=. --set-build-env-vars=NPM_CONFIG_LEGACY_PEER_DEPS=true --vpc-connector=me-2-uat-vpc-conn-redis --egress-settings=private-ranges-only





last row = F
header => Remove from array
Comment 7, 8
resp owner, visibility key