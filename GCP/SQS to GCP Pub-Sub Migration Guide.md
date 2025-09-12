

## Overview
This guide documents the changes required to migrate from AWS SQS to GCP Pub/Sub for the data sync functionality across Lambda functions. The migration maintains backward compatibility while enabling GCP Pub/Sub integration.

## Changes Required

### 1. Package Dependencies
Add the GCP Pub/Sub client to `package.json`:

```json
{
  "dependencies": {
    "@google-cloud/pubsub": "^4.0.0"
  }
}
```

### 2. Import Statement
Add the Pub/Sub import at the top of your main file:

```javascript
const { PubSub } = require('@google-cloud/pubsub');
```

### 3. Helper Function
Add this helper function before your main `sailsNotifySqsPayload` function:

```javascript
// Helper function to publish to GCP Pub/Sub
const publishToPubSub = async (payload, stage) => {
    try {
        const pubsub = new PubSub();
        let topicName = "";
        
        // Stage-based topic selection
        if(stage == "ksa-uat"){
            topicName = 'me-2-uat-queue--data-sync';
	    }
	    else if(stage == "ksa-prod"){
		    topicName = 'me-2-prod-queue--data-sync';
	    }
        
        // Convert payload to Buffer as expected by the consumer
        const dataBuffer = Buffer.from(JSON.stringify(payload));
        
        const messageId = await pubsub.topic(topicName).publish(dataBuffer);
        console.log(`Message ${messageId} published to topic ${topicName}`);
        
        return messageId;
    } catch (error) {
        console.error('Error publishing to Pub/Sub:', error);
        throw error;
    }
};
```

### 4. Main Function Changes
Replace your existing `sailsNotifySqsPayload` function:

**BEFORE (SQS):**
```javascript
exports.sailsNotifySqsPayload = async (args) => {
    try {
        if (args && Object.keys(args).length > 0) {
            let stage = require("./index").domain_stage;            
            let QueueName = ''
            if (stage == 'ksa-uat' || stage == 'test') {
                QueueName = 'sailsNotifySqsPayload_SQS_dev'
            } else if (stage == 'beta') {
                QueueName = 'sailsNotifySqsPayload_SQS_beta'
            } else if ((stage == 'ksa-prod') || (stage == 'app') || (stage == 'prod-01')) {
                QueueName = 'sailsNotifySqsPayload_SQS_prod'
            }
            let payloadObj = {
                queueName: QueueName,
                data: args.data,
                eventName : args.eventName,
                stage: stage
            }
            //SQS Payload start
            let payload = {
                "DelaySeconds": 0,
                "MessageBody": "Sails Notify ",
                "MessageAttributes": {
                    "payload": {
                        "DataType": "String",
                        "StringValue": JSON.stringify(payloadObj)
                    }
                },
                "QueueName": payloadObj.queueName
            }
            //SQS Payload End
            return payload;
        }
    } catch (error) {
        console.log(error, "error in sailsNotifySqsPayload function")
        return {}
    }
};
```

**AFTER (Pub/Sub):**
```javascript
exports.sailsNotifySqsPayload = async (args) => {
    try {
        if (args && Object.keys(args).length > 0) {
            let stage = require("./index").domain_stage;            
            
            // GCP Pub/Sub payload structure - simplified for the consumer
            let payload = {
                data: args.data,
                eventName: args.eventName,
                stage: stage
            }
            
            // Publish to GCP Pub/Sub
            await publishToPubSub(payload, stage);
            
            return payload;
        }
    } catch (error) {
        console.log(error, "error in sailsNotifySqsPayload function")
        return {}
    }
};
```

### 5. Update Related Functions
Update any `notifyAndPostToSQS` function to remove SQS posting:

**BEFORE:**
```javascript
exports.notifyAndPostToSQS = async (data, actionFrom) => {
  try {
    const sqs_url = `https://${this.apiURL}/sqs_post`;
    const requestPayload = {
      eventName: actionFrom,
      data: data,
    };
    const payload = await this.sailsNotifySqsPayload(requestPayload);
    if (payload && Object.keys(payload).length > 0) {
      await this.postRequest(payload, sqs_url);
    } else {
      console.log("Payload error for sailsNotify in " + actionFrom);
    }
  } catch (error) {
    console.log(error);
    return {};
  }
};
```

**AFTER:**
```javascript
exports.notifyAndPostToSQS = async (data, actionFrom) => {
  try {
    const requestPayload = {
      eventName: actionFrom,
      data: data,
    };
    const payload = await this.sailsNotifySqsPayload(requestPayload);
    if (payload && Object.keys(payload).length > 0) {
      console.log("Successfully published to Pub/Sub for " + actionFrom);
    } else {
      console.log("Payload error for sailsNotify in " + actionFrom);
    }
  } catch (error) {
    console.log(error);
    return {};
  }
};
```

## Key Changes Summary

1. **Simplified Payload**: Removed complex SQS message structure
2. **Direct Publishing**: Messages are published directly to Pub/Sub instead of being returned for SQS posting
3. **Stage-based Topics**: Different topics for different environments
4. **Buffer Encoding**: Payload is converted to Buffer for Pub/Sub compatibility
5. **Backward Compatibility**: Function signatures remain the same

## Testing

1. Install the new dependency: `npm install`
2. Deploy the function
3. Test with the GCP Pub/Sub topic: `me-2-uat-queue--data-sync`
4. Verify messages are received by the SQS/Sails-Notify-SQS consumer

## Benefits

- **Simplified Architecture**: Direct Pub/Sub publishing
- **Better Performance**: No intermediate SQS step
- **GCP Native**: Leverages GCP Pub/Sub features
- **Minimal Changes**: Maintains existing function interfaces
- **Environment Support**: Automatic topic selection based on stage