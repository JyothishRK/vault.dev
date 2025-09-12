
## Overview

This guide documents the complete migration process from AWS SQS to GCP Pub/Sub for serverless functions. It covers all code changes, configuration updates, and deployment procedures.

---

## Key Differences Between AWS SQS and GCP Pub/Sub

|Aspect|AWS SQS|GCP Pub/Sub|
|---|---|---|
|**Message Format**|`message.body` (string)|`message.data` (base64 encoded)|
|**Message Processing**|Manual acknowledgment required|Automatic acknowledgment on success|
|**Error Handling**|Manual retry logic|Automatic retry with exponential backoff|
|**Trigger Type**|SQS trigger|Pub/Sub topic trigger|
|**Deployment**|`--trigger-event`|`--trigger-topic`|

---

## Code Changes Required

### 1. Message Processing Logic

**Before (AWS SQS):**

```javascript
exports.handler = async (event) => {
  try {
    for (const record of event.Records) {
      const messageBody = JSON.parse(record.body);
      // Process message
    }
    return { statusCode: 200, body: 'Success' };
  } catch (error) {
    console.error('Error:', error);
    throw error;
  }
};
```

**After (GCP Pub/Sub):**

```javascript
exports.sailsNotifyPubSub = async (message, context) => {
  try {
    var get_body = {};
    if (message && message.data) {
      const payload = JSON.parse(Buffer.from(message.data, 'base64').toString());
      get_body = payload;
    } else if (message && message.body) {
      get_body = JSON.parse(message.body);
    }

    const response = await processMessage(get_body);
    return response;
  } catch (err) {
    console.log("Error in PubSub processing", err);
    throw err;
  }
};
```

### 2. Secret Management Migration

**Before (AWS Secrets Manager):**

```javascript
const AWS = require('aws-sdk');
const secret_manager = new AWS.SecretsManager();

async function getSecretString() {
  try {
    const params = { SecretId: 'VComply_Global' };
    const result = await secret_manager.getSecretValue(params).promise();
    return result.SecretString;
  } catch (e) {
    console.log("secret manager error", e);
    return {}.toString();
  }
}
```

**After (GCP Secret Manager):**

```javascript
const { SecretManagerServiceClient } = require('@google-cloud/secret-manager');

const fetchSecretManagerDetails = async () => {
  try {
    const client = new SecretManagerServiceClient();
    const projectId = process.env.GOOGLE_CLOUD_PROJECT || 'me-2-uat';
    const secretName = `${projectId}-secret`;
    const secretPath = `projects/${projectId}/secrets/${secretName}/versions/latest`;

    try {
      const [version] = await client.accessSecretVersion({ name: secretPath });
      const secretValue = version.payload.data.toString();
      console.log("Fetch from GCP Secret Manager");
      return JSON.parse(secretValue);
    } catch (error) {
      console.error(`Error accessing secret ${secretPath}:`, error);
      const defaultConfig = fetchDefaultConfig();
      console.log("Fallback to defaultConfig");
      return defaultConfig;
    }
  } catch (err) {
    console.error("Error in fetchSecretManagerDetails", err);
    return { status: 500, data: { message: "Error in fetchSecretManagerDetails", err } };
  }
};
```

### 3. Package.json Updates

**Add GCP Dependencies:**

```json
{
  "dependencies": {
    "@google-cloud/secret-manager": "^4.0.0"
  },
  "devDependencies": {
    "@google-cloud/functions-framework": "^3.0.0"
  },
  "engines": {
    "node": ">=18"
  }
}
```

**Remove AWS Dependencies:**

```json
{
  "dependencies": {
    // "aws-sdk": "^2.x.x",
    // "aws-lambda": "^1.x.x"
  }
}
```

---

## Configuration Updates

### 1. Environment Variables

Create `.env` file:

```env
FEATURE_FLAG_URL=devapi.v-comply.com
NPM_CONFIG_LEGACY_PEER_DEPS=true
PROJECT_ID=me-2-uat
SECRET_NAME=me-2-uat-secret
LOG_EXECUTION_ID=true
```

### 2. Function Configuration

- Function name: `notifySailsOnDataSync`
    
- Entry point: `sailsNotifyPubSub`
    
- Topic: `me-2-uat-queue--data-sync`
    

---

## Deployment Process

### 1. Prerequisites

```bash
gcloud auth login
gcloud config set project me-2-uat
```

### 2. Create Pub/Sub Topic

```bash
gcloud pubsub topics create me-2-uat-queue--data-sync
```

### 3. Deploy Function (Bash)

```bash
cd "path/to/your/function"
gcloud functions deploy notifySailsOnDataSync \
  --runtime=nodejs20 \
  --trigger-topic=me-2-uat-queue--data-sync \
  --region=me-central2 \
  --entry-point=sailsNotifyPubSub \
  --source=. \
  --env-vars-file=.env \
  --vpc-connector=me-2-uat-vpc-conn-redis \
  --egress-settings=all
```

### 4. Deploy Function (PowerShell)

```powershell
cd "d:\GCP\gcp-b-all\SQS\Sails-Notify-SQS"
gcloud functions deploy notifySailsOnDataSync --runtime=nodejs20 --trigger-topic=me-2-uat-queue--data-sync --region=me-central2 --entry-point=sailsNotifyPubSub --source=. --env-vars-file=.env --vpc-connector=me-2-uat-vpc-conn-redis --egress-settings=all
```

### 5. Deploy Function (CMD.exe)

```cmd
cd /d "d:\GCP\gcp-b-all\SQS\Sails-Notify-SQS"
gcloud functions deploy notifySailsOnDataSync --runtime=nodejs20 --trigger-topic=me-2-uat-queue--data-sync --region=me-central2 --entry-point=sailsNotifyPubSub --source=. --env-vars-file=.env --vpc-connector=me-2-uat-vpc-conn-redis --egress-settings=all
```

---

## Environment Variables Setup

|Variable|Description|Example|
|---|---|---|
|`FEATURE_FLAG_URL`|Feature flag service URL|`devapi.v-comply.com`|
|`PROJECT_ID`|GCP Project ID|`me-2-uat`|
|`SECRET_NAME`|Secret Manager secret name|`me-2-uat-secret`|
|`LOG_EXECUTION_ID`|Enable execution logging|`true`|
|`NPM_CONFIG_LEGACY_PEER_DEPS`|NPM config|`true`|

---

## Testing and Validation


**Publish Test Message:**

```bash
gcloud pubsub topics publish me-2-uat-queue--data-sync \
  --message='{"test": "data", "eventName": "test:event"}'
```

**Check Logs:**

```bash
gcloud functions logs read notifySailsOnDataSync --region=me-central2
```

---

## Troubleshooting

**1. Secret Manager Access Denied**

```bash
gcloud secrets add-iam-policy-binding me-2-uat-secret \
  --member="serviceAccount:me-2-uat@appspot.gserviceaccount.com" \
  --role="roles/secretmanager.secretAccessor"
```

**2. VPC Connector Issues**

```bash
gcloud compute networks vpc-access connectors list --region=me-central2
```

**3. Message Format Fix**

```javascript
const message = {
  data: Buffer.from(JSON.stringify(payload)).toString('base64')
};
```

**4. Increase Timeout**

```bash
gcloud functions deploy notifySailsOnDataSync \
  --timeout=540s \
  --region=me-central2
```

---

## Additional Resources

- [GCP Cloud Functions](https://cloud.google.com/functions/docs)
    
- [GCP Pub/Sub](https://cloud.google.com/pubsub/docs)
    
- [GCP Secret Manager](https://cloud.google.com/secret-manager/docs)
    

---
