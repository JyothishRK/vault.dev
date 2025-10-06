```
FEATURE_FLAG_URL: api.v-comply.com
LOG_EXECUTION_ID: 'true'
NPM_CONFIG_LEGACY_PEER_DEPS: 'true'
PROJECT_ID: me-2-prod
SECRET_NAME: me-2-prod-secrets
```
# Lambdas:

## 1. Organisation:

```
gcloud functions deploy organisation --runtime=nodejs20 --trigger-http --allow-unauthenticated --region=me-central2 --entry-point=handler --source=. --env-vars-file=env.deploy --vpc-connector=me-2-prod-vpc-connector --egress-settings=private-ranges-only
```

## 2. Globals:

```
gcloud functions deploy globals --runtime=nodejs20 --trigger-http --allow-unauthenticated --region=me-central2 --entry-point=handler --source=. --env-vars-file=env.deploy --vpc-connector=me-2-prod-vpc-connector --egress-settings=private-ranges-only
```

### 3. Policy

```
gcloud functions deploy policy --runtime=nodejs20 --trigger-http --allow-unauthenticated --region=me-central2 --entry-point=handler --source=. --env-vars-file=env.deploy --vpc-connector=me-2-prod-vpc-connector --egress-settings=private-ranges-only
```
## SQS:
### 1. Sails-Notify

```
gcloud functions deploy me-2-prod-queue--sails-notify --runtime=nodejs20 --trigger-topic=me-2-prod-queue--data-sync --region=me-central2 --entry-point=sailsNotifyPubSub --source=. --env-vars-file=env.deploy --vpc-connector=me-2-prod-vpc-connector --egress-settings=private-ranges-only
```

### 2. Report-Download-Excel

```
gcloud functions deploy report-download-excel --runtime=nodejs20 --trigger-topic=me-2-prod-queue--report-download-excel --region=me-central2 --entry-point=reportDownloadExcelPubSub --source=. --env-vars-file=env.deploy --vpc-connector=me-2-prod-vpc-connector --egress-settings=private-ranges-only
```