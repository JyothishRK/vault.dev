--
## Cron-RC-Transfer

### Initial Deployment with env:

```
gcloud functions deploy me-2-uat-cron--rc-transfer --runtime=nodejs20 --trigger-http --allow-unauthenticated --region=me-central2 --entry-point=handler --source=. --env-vars-file=env.deploy --vpc-connector=me-2-uat-vpc-conn-redis --egress-settings=private-ranges-only

```

### Later Deployments

```
gcloud functions deploy me-2-uat-cron--rc-transfer --runtime=nodejs20 --trigger-http --allow-unauthenticated --region=me-central2 --entry-point=handler --source=. --set-build-env-vars=NPM_CONFIG_LEGACY_PEER_DEPS=true --vpc-connector=me-2-uat-vpc-conn-redis --egress-settings=private-ranges-only

```

--

## CreateBulkUserCron

### Initial Deployment with env:

```
gcloud functions deploy me-2-uat-cron--bulk-user-create --runtime=nodejs20 --trigger-http --allow-unauthenticated --region=me-central2 --entry-point=handler --source=. --env-vars-file=env.deploy --vpc-connector=me-2-uat-vpc-conn-redis --egress-settings=private-ranges-only

```

### Later Deployments

```
gcloud functions deploy me-2-uat-cron--bulk-user-create --runtime=nodejs20 --trigger-http --allow-unauthenticated --region=me-central2 --entry-point=handler --source=. --set-build-env-vars=NPM_CONFIG_LEGACY_PEER_DEPS=true --vpc-connector=me-2-uat-vpc-conn-redis --egress-settings=private-ranges-only

```

--