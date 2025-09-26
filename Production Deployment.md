```
FEATURE_FLAG_URL: api.v-comply.com
LOG_EXECUTION_ID: 'true'
NPM_CONFIG_LEGACY_PEER_DEPS: 'true'
PROJECT_ID: me-2-prod
SECRET_NAME: me-2-prod-secrets
```
## Lambdas:

## 1. Organisation:

```
gcloud functions deploy organisation --runtime=nodejs20 --trigger-http --allow-unauthenticated --region=me-central2 --entry-point=handler --source=. --set-build-env-file=env.deploy --vpc-connector=me-2-prod-vpc-connector --egress-settings=private-ranges-only
```