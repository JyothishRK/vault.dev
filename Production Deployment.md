
## Lambdas:

## 1. Organisation:

```
gcloud functions deploy organisation --runtime=nodejs20 --trigger-http --allow-unauthenticated --region=me-central2 --entry-point=handler --source=. --set-build-env-vars=NPM_CONFIG_LEGACY_PEER_DEPS=true --vpc-connector=me-2-uat-vpc-conn-redis --egress-settings=private-ranges-only
```