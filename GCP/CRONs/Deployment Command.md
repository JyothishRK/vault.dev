--
## 1. Cron-RC-Transfer

### Initial Deployment with env:

```
gcloud functions deploy me-2-uat-cron--rc-transfer --runtime=nodejs20 --trigger-http --allow-unauthenticated --region=me-central2 --entry-point=handler --source=. --env-vars-file=env.deploy --vpc-connector=me-2-uat-vpc-conn-redis --egress-settings=private-ranges-only

```

### Later Deployments

```
gcloud functions deploy me-2-uat-cron--rc-transfer --runtime=nodejs20 --trigger-http --allow-unauthenticated --region=me-central2 --entry-point=handler --source=. --set-build-env-vars=NPM_CONFIG_LEGACY_PEER_DEPS=true --vpc-connector=me-2-uat-vpc-conn-redis --egress-settings=private-ranges-only

```

--

## 2. CreateBulkUserCron

### Initial Deployment with env:

```
gcloud functions deploy me-2-uat-cron--bulk-user-create --runtime=nodejs20 --trigger-http --allow-unauthenticated --region=me-central2 --entry-point=handler --source=. --env-vars-file=env.deploy --vpc-connector=me-2-uat-vpc-conn-redis --egress-settings=private-ranges-only

```

### Later Deployments

```
gcloud functions deploy me-2-uat-cron--bulk-user-create --runtime=nodejs20 --trigger-http --allow-unauthenticated --region=me-central2 --entry-point=handler --source=. --set-build-env-vars=NPM_CONFIG_LEGACY_PEER_DEPS=true --vpc-connector=me-2-uat-vpc-conn-redis --egress-settings=private-ranges-only

```

---

## 3. Cron-role-change-effect

### Initial Deployment with env:

```
gcloud functions deploy me-2-uat-cron--role-change-effect --runtime=nodejs20 --trigger-http --allow-unauthenticated --region=me-central2 --entry-point=handler --source=. --env-vars-file=env.deploy --vpc-connector=me-2-uat-vpc-conn-redis --egress-settings=private-ranges-only

```

### Later Deployments

```
gcloud functions deploy me-2-uat-cron--role-change-effect --runtime=nodejs20 --trigger-http --allow-unauthenticated --region=me-central2 --entry-point=handler --source=. --set-build-env-vars=NPM_CONFIG_LEGACY_PEER_DEPS=true --vpc-connector=me-2-uat-vpc-conn-redis --egress-settings=private-ranges-only

```

---
## 4. Cron-user-email-change

### Initial Deployment with env:

```
gcloud functions deploy me-2-uat-cron--user-email-change --runtime=nodejs20 --trigger-http --allow-unauthenticated --region=me-central2 --entry-point=handler --source=. --env-vars-file=env.deploy --vpc-connector=me-2-uat-vpc-conn-redis --egress-settings=private-ranges-only

```

### Later Deployments

```
gcloud functions deploy me-2-uat-cron--user-email-change --runtime=nodejs20 --trigger-http --allow-unauthenticated --region=me-central2 --entry-point=handler --source=. --set-build-env-vars=NPM_CONFIG_LEGACY_PEER_DEPS=true --vpc-connector=me-2-uat-vpc-conn-redis --egress-settings=private-ranges-only

```

---
## 5. Cron-User-Transfer

### Initial Deployment with env:

```
gcloud functions deploy me-2-uat-cron--user-transfer --runtime=nodejs20 --trigger-http --allow-unauthenticated --region=me-central2 --entry-point=handler --source=. --env-vars-file=env.deploy --vpc-connector=me-2-uat-vpc-conn-redis --egress-settings=private-ranges-only

```

### Later Deployments

```
gcloud functions deploy me-2-uat-cron--user-transfer --runtime=nodejs20 --trigger-http --allow-unauthenticated --region=me-central2 --entry-point=handler --source=. --set-build-env-vars=NPM_CONFIG_LEGACY_PEER_DEPS=true --vpc-connector=me-2-uat-vpc-conn-redis --egress-settings=private-ranges-only

```

---
## 6. CronSyncGroupUsers

### Initial Deployment with env:

```
gcloud functions deploy me-2-uat-cron--sync-group-users --runtime=nodejs20 --trigger-http --allow-unauthenticated --region=me-central2 --entry-point=handler --source=. --env-vars-file=env.deploy --vpc-connector=me-2-uat-vpc-conn-redis --egress-settings=private-ranges-only

```

### Later Deployments

```
gcloud functions deploy me-2-uat-cron--sync-group-users --runtime=nodejs20 --trigger-http --allow-unauthenticated --region=me-central2 --entry-point=handler --source=. --set-build-env-vars=NPM_CONFIG_LEGACY_PEER_DEPS=true --vpc-connector=me-2-uat-vpc-conn-redis --egress-settings=private-ranges-only

```

---