```
sudo docker exec -it redis redis-cli -a ksa-uat@vc
```

```
gcloud compute networks vpc-access connectors create me-2-uat-vpc-conn-redis ^
  --region=me-central2 ^
  --network=me-2-uat-vpc ^
  --range=10.8.0.0/28 ^
  --min-instances=2 ^
  --max-instances=10
```

```
gcloud functions deploy redisHandler --gen2 --runtime=nodejs20 --region=me-central2 --entry-point=redisHandler --source=. --trigger-http --allow-unauthenticated --vpc-connector=me-2-uat-vpc-conn-redis
```

