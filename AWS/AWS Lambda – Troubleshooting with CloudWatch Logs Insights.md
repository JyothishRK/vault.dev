
---

## Objective

Identify and troubleshoot errors from AWS Lambda logs using CloudWatch Logs Insights by querying logs for error messages, stack traces, and relevant metadata.

---

## Accessing CloudWatch Logs for a Lambda Function

1. Open the AWS Management Console.
    
2. Navigate to **CloudWatch → Logs → Log groups**.
    
3. Locate the log group for your Lambda function, typically:
    
    ```
    /aws/lambda/<lambda-function-name>
    ```
    
4. Click the log group → **View in Logs Insights**.
    

---

## Basic Query – Search for Errors

```sql
fields @timestamp, @message
| filter @message like /Error/
| sort @timestamp desc
| limit 500
```

**What this does:**

- Extracts the **timestamp** and **message** fields.
    
- Filters logs containing `"Error"` (case-sensitive).
    
- Sorts results latest first.
    
- Limits output to 500 entries.
    

---

## Enhanced Queries

### 1. Case-insensitive Error Search (Recommended)

```sql
fields @timestamp, @message
| filter @message like /(?i)error/
| sort @timestamp desc
| limit 500
```

- `(?i)` → matches `error`, `Error`, `ERROR`, etc.
    

---

### 2. Filter by Lambda RequestId & Log Level

Useful to correlate logs from a single invocation.

```sql
fields @timestamp, requestId, @message
| filter @message like /(?i)(error|exception)/
| sort @timestamp desc
| limit 500
```

---

### 3. Detect Timeouts or Throttles

```sql
fields @timestamp, @message
| filter @message like /Task timed out|Rate Exceeded|Throttling/
| sort @timestamp desc
| limit 100
```

---

### 4. Extract JSON from Structured Logs

If Lambda logs JSON-formatted messages:

```sql
fields @timestamp, @message
| parse @message /"level": "(?<level>[^"]+)", "msg": "(?<msg>[^"]+)"/
| filter level = "error"
| sort @timestamp desc
| limit 100
```

---

## Troubleshooting Tips

- Use absolute time ranges for production debugging.
    
- Save and share queries for re-use with the team.
    
- If using Lambda Powertools or structured logging libraries, leverage JSON parsing for cleaner insights.
    
- For continuous monitoring:
    
    - Set up CloudWatch Alarms on error metrics.
        
    - Integrate with AWS Lambda Insights, Datadog, or New Relic.
        

---

## References

- [AWS Docs – CloudWatch Logs Insights Query Syntax](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/CWL_QuerySyntax.html)
    
- [AWS Lambda Logging Best Practices](https://docs.aws.amazon.com/lambda/latest/dg/nodejs-logging.html)
    

---