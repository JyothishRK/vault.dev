
---
## Table of Contents

1. [Winston Implementation Overview](#winston-implementation-overview)

2. [How Winston Works in Theory](#how-winston-works-in-theory)

3. [Current Implementation in Your Codebase](#current-implementation-in-your-codebase)

4. [Using Winston in Migration Scripts](#using-winston-in-migration-scripts)

5. [Best Practices](#best-practices)

6. [Example Migration Script with Winston](#example-migration-script-with-winston)

  

---

  

## Winston Implementation Overview

  

### What is Winston?

Winston is a popular logging library for Node.js that provides:

- **Multiple transports**: Log to files, console, databases, remote services

- **Log levels**: Structured severity levels (silly, verbose, info, warn, error)

- **Formatters**: Customize log message format (JSON, plain text, etc.)

- **Exception handling**: Automatic capture of unhandled exceptions

- **Log rotation**: Automatic file rotation based on size/date

  

---

  

## How Winston Works in Theory

  

### 1. **Logger Creation**

A Winston logger is created with:

- **Level**: Minimum log level to capture (e.g., "info" captures info, warn, error)

- **Format**: How messages are transformed before being logged

- **Transports**: Where logs are written (File, Console, HTTP, etc.)

  

### 2. **Log Levels Hierarchy**

From lowest to highest priority:

```

silly < verbose < debug < info < warn < error

```

  

When you set level to "info", it captures: `info`, `warn`, `error` (but not `debug`, `verbose`, `silly`)

  

### 3. **Transports**

- **File Transport**: Writes logs to files (with optional rotation)

- **Console Transport**: Outputs to stdout/stderr

- **Custom Transports**: Can send to databases, message queues, cloud services (like Sentry)

  

### 4. **Formatters**

Transform log data before it's written:

- **timestamp()**: Adds timestamp to each log entry

- **printf()**: Custom format using template strings

- **json()**: Converts to JSON format

- **combine()**: Chains multiple formatters

  

---

  

## Current Implementation in Your Codebase

  

### Location: `src/config/log.js`

  

Your Winston setup includes:

  

#### 1. **Custom Logger Configuration**

```javascript

const customLogger = createLogger({
    level: "info",
    
    format: combine(
        timestamp({ format: "YYYY-MM-DD hh:mm:ss" }),
        logFormat  // JSON format with timestamp and level
    ),
    
    transports: [
        new transports.File(options.errorFile),  // error.log - only errors
        new transports.File(options.appFile),    // app.log - info and below
    ],

    exceptionHandlers: [new transports.File(options.errorFile)],

    exitOnError: false,
});

```

  
#### 2. **File Transports**

- **`logs/app.log`**: All logs with level `info` and below (excluding errors)

  - Max size: 5MB per file

  - Retains: 5 rotated files

- **`logs/error.log`**: Only errors

  - Max size: 5MB per file

  - Retains: 5 rotated files

  - Handles unhandled exceptions

  

#### 3. **Console Transport**

- Added in **non-production** environments

- Level: `silly` (captures all log levels)

- Useful for development/debugging

  

#### 4. **Sentry Integration**

- Added in **production** environments

- Automatically sends errors to Sentry for monitoring

  

#### 5. **Log Format**

All logs are formatted as JSON:

```json

{
  "timestamp": "2024-01-15 10:30:45",
  "level": "INFO",
  "message": "Your log message here"
}

```
   

#### 6. **Access via Sails**

The logger is accessible through:

- `sails.config.log.custom` - Direct Winston logger instance

- `sails.log.info()`, `sails.log.error()`, etc. - Sails wrapper methods

  

---

  

## Using Winston in Migration Scripts

  

### Option 1: Use Sails Logger (Recommended if script runs in Sails context)

  

```javascript
module.exports = {
    fn: async function (unusedInput, exits) {
        // Access Winston logger via Sails
        const logger = sails.config.log.custom;
        logger.info('Starting migration script');
        logger.info({ policyCount: 100, orgId: 23 }, 'Migration parameters');
        try {
            // Your migration logic
            logger.info('Processing policy 123');
            // ...
            logger.warn({ policyId: 123 }, 'Policy has unusual data format');
            // ...
        } catch (error) {
            logger.error({ error: error.message, stack: error.stack }, 'Migration failed');
            return exits.error(error);
        }
        logger.info('Migration completed successfully');
        return exits.success();
    }
};

```


### Option 2: Import Logger Directly (For standalone scripts)
  

```javascript
// At the top of your script
const path = require('path');
const { createLogger, format, transports } = require('winston');
const { combine, timestamp, printf } = format;

// Create a dedicated logger for this script

const logger = createLogger({
    level: 'info',
    format: combine(
        timestamp({ format: 'YYYY-MM-DD hh:mm:ss' }),
        format.json()
    ),
    transports: [
        // Log to console for immediate feedback
        new transports.Console({
            format: combine(
                format.colorize(),
                format.printf(({ timestamp, level, message, ...meta }) => {
                    const metaStr = Object.keys(meta).length ? JSON.stringify(meta) : '';
                    return `${timestamp} [${level}]: ${message} ${metaStr}`;
                })
            )
        }),
        // Log to file
        new transports.File({
            filename: path.join(process.cwd(), 'logs', 'migration.log'),
            maxsize: 5242880, // 5MB
            maxFiles: 5
        }),

        // Errors to separate file
        new transports.File({
            filename: path.join(process.cwd(), 'logs', 'migration-error.log'),
            level: 'error',
            maxsize: 5242880,
            maxFiles: 5
        })
    ]
});

// Use in your script
logger.info('Starting migration');
logger.error({ error: err }, 'Failed to process record');

```

  

### Option 3: Reuse Existing Logger Configuration

  

```javascript

// Import the logger config
const logConfig = require('../config/log');
const logger = logConfig.log.custom;
  
// Or access via Sails if available

// const logger = sails.config.log.custom;

logger.info('Migration started');

```

  

---

  

## Best Practices

  

### 1. **Use Appropriate Log Levels**

```javascript

logger.silly('Detailed debugging info');      // Very verbose

logger.verbose('Verbose debugging info');      // Verbose debugging

logger.debug('Debug information');            // Debug info

logger.info('General information');           // Informational messages

logger.warn('Warning message');               // Warnings

logger.error('Error occurred');               // Errors

```

  

### 2. **Add Context to Logs**

```javascript

// ❌ Bad: Generic message
logger.info('Processing policy');

// ✅ Good: Contextual information
logger.info({
    policyId: 123,
    organizationId: 23,
    recordCount: 150
}, 'Processing policy migration');

// ✅ Better: Structured for easy filtering/searching
logger.info({
    script: 'migrate-policy-versions',
    action: 'process_policy',
    policyId: 123,
    organizationId: 23,
    recordCount: 150,
    duration: 1250 // milliseconds
}, 'Policy processed successfully');

```

  

### 3. **Log Progress Checkpoints**

```javascript

const total = policies.length;
const checkpoints = [10, 25, 50, 75, 90, 100];
let processed = 0;
  
for (const policy of policies) {
    // Process policy...
    processed++;
    const percentDone = (processed / total) * 100;
    if (checkpoints.includes(Math.floor(percentDone))) {
        logger.info({
            script: 'migrate-policies',
            progress: percentDone,
            processed,
            total,
            remaining: total - processed
        }, `Migration progress: ${percentDone}%`);
    }
}

```

  

### 4. **Log Errors with Full Context**

```javascript

try {
    await migratePolicy(policy);
} catch (error) {
    logger.error({
        script: 'migrate-policies',
        policyId: policy.id,
        organizationId: policy.organizationId,
        error: {
            message: error.message,
            stack: error.stack,
            code: error.code
        },
        inputData: policy // Be careful not to log sensitive data
    }, 'Failed to migrate policy');
}

```

  

### 5. **Log Summary Statistics**

```javascript

const summary = {
    script: 'migrate-policies',
    startTime: startTime.toISOString(),
    endTime: endTime.toISOString(),
    duration: endTime.diff(startTime, 'milliseconds'),
    stats: {
        total: policies.length,
        succeeded: succeededCount,
        failed: failedCount,
        skipped: skippedCount
    },
    errors: failedPolicies.map(f => ({ policyId: f.policyId, error: f.error }))
};

logger.info(summary, 'Migration script completed');
logger.error({ failedCount: summary.stats.failed }, 'Migration had failures');

```

  

### 6. **Avoid Logging Sensitive Data**

```javascript

// ❌ Bad: Logs passwords/tokens
logger.info({ user: { email, password } }, 'User created');

// ✅ Good: Exclude sensitive fields
logger.info({
    user: { email, id: user.id }
}, 'User created');

```

  

### 7. **Use Child Loggers for Scoped Logging**

```javascript

const childLogger = logger.child({

    script: 'migrate-policies',

    organizationId: 23

});

  

// All logs from this child logger will include the context

childLogger.info('Starting migration');

// Logs: { script: 'migrate-policies', organizationId: 23, message: 'Starting migration' }

```

  

---

  

## Example Migration Script with Winston

  

Here's a complete example showing how to refactor your existing migration script:

  

```javascript

const _ = require("lodash");

const fs = require("fs");

const path = require("path");

const moment = require("moment");

  

// Option 1: Use Sails logger (if running in Sails context)

let logger;

  

// Initialize logger

if (typeof sails !== 'undefined' && sails.config && sails.config.log) {

    logger = sails.config.log.custom;

} else {

    // Fallback: Create standalone logger

    const { createLogger, format, transports } = require('winston');

    logger = createLogger({

        level: 'info',

        format: format.combine(

            format.timestamp({ format: 'YYYY-MM-DD hh:mm:ss' }),

            format.json()

        ),

        transports: [

            new transports.Console({

                format: format.combine(

                    format.colorize(),

                    format.printf(({ timestamp, level, message, ...meta }) => {

                        const metaStr = Object.keys(meta).length ? ` ${JSON.stringify(meta)}` : '';

                        return `${timestamp} [${level}]: ${message}${metaStr}`;

                    })

                )

            }),

            new transports.File({

                filename: path.join(process.cwd(), 'logs', 'migration-policy-versions.log'),

                maxsize: 5242880,

                maxFiles: 5

            }),

            new transports.File({

                filename: path.join(process.cwd(), 'logs', 'migration-policy-versions-error.log'),

                level: 'error',

                maxsize: 5242880,

                maxFiles: 5

            })

        ]

    });

}

  

// Create child logger with script context

const scriptLogger = logger.child({

    script: 'migrate-policy-versions',

    version: '1.0'

});

  

module.exports = {

    friendlyName: "KFSHRC: Migrate Policy Versions",

    description: "Create PolicyVersion records per policy using sample metadata, with proper logging",

    inputs: {},

  

    fn: async function (unusedInput, exits) {

        const startTime = moment();

        scriptLogger.info({

            startTime: startTime.toISOString(),

            targetOrgId: 23

        }, 'Starting migration script');

  

        try {

            const targetOrgId = 23;

  

            const loadStart = Date.now();

            const [policies, sourceRecords] = await Promise.all([

                Policy.find({

                    where: {

                        organization: targetOrgId,

                        meta: { "!=": null },

                    },

                    select: ["id", "meta"],

                }),

                loadSampleMetadata(),

            ]);

  

            scriptLogger.info({

                policiesLoaded: policies.length,

                sourceRecordsLoaded: sourceRecords.length,

                loadDuration: Date.now() - loadStart

            }, 'Data loaded successfully');

  

            const byReference = _.groupBy(sourceRecords, (r) =>

                String(r.reference_number || "").trim()

            );

  

            const policyToVersions = buildPolicyToVersionsMap({

                policies,

                byReference,

            });

  

            const policyIds = Object.keys(policyToVersions).map((k) => Number(k));

            scriptLogger.info({

                policiesWithMappedVersions: policyIds.length,

                totalPolicies: policies.length,

                coverage: `${((policyIds.length / policies.length) * 100).toFixed(2)}%`

            }, 'Policy version mapping completed');

  

            let createdPolicies = 0;

            let failedPolicies = 0;

            const failed = [];

  

            const total = policyIds.length;

            const checkpoints = _.range(5, 101, 5);

            let nextCheckpointIndex = 0;

  

            for (let i = 0; i < total; i++) {

                const policyId = policyIds[i];

                const versions = policyToVersions[policyId];

  

                try {

                    await migratePolicyVersions({

                        policyId,

                        versions,

                        organizationId: targetOrgId,

                    });

                    createdPolicies++;

                    // Log every 100th success for tracking (without flooding logs)

                    if ((i + 1) % 100 === 0) {

                        scriptLogger.debug({

                            policyId,

                            versionsCount: versions.length,

                            processed: i + 1,

                            total

                        }, `Processed ${i + 1} policies`);

                    }

                } catch (error) {

                    failedPolicies++;

                    const errorInfo = {

                        policyId,

                        error: error.message,

                        stack: error.stack,

                        versionsAttempted: versions.length

                    };

                    failed.push({ policyId, error: error.message });

                    scriptLogger.error(errorInfo, `Failed to migrate policy ${policyId}`);

                }

  

                const percentDone = ((i + 1) / total) * 100;

                if (

                    nextCheckpointIndex < checkpoints.length &&

                    percentDone >= checkpoints[nextCheckpointIndex]

                ) {

                    scriptLogger.info({

                        progress: checkpoints[nextCheckpointIndex],

                        processed: i + 1,

                        total,

                        remaining: total - (i + 1),

                        succeeded: createdPolicies,

                        failed: failedPolicies,

                        successRate: `${((createdPolicies / (i + 1)) * 100).toFixed(2)}%`

                    }, `Migration progress: ${checkpoints[nextCheckpointIndex]}%`);

                    nextCheckpointIndex++;

                }

            }

  

            const endTime = moment();

            const duration = endTime.diff(startTime, 'milliseconds');

            // Final summary

            const summary = {

                script: 'migrate-policy-versions',

                startTime: startTime.toISOString(),

                endTime: endTime.toISOString(),

                duration: `${duration}ms (${(duration / 1000).toFixed(2)}s)`,

                stats: {

                    total,

                    succeeded: createdPolicies,

                    failed: failedPolicies,

                    successRate: `${((createdPolicies / total) * 100).toFixed(2)}%`

                }

            };

  

            scriptLogger.info(summary, 'Migration script completed');

            if (failed.length > 0) {

                scriptLogger.warn({

                    failedCount: failed.length,

                    failedPolicyIds: failed.map(f => f.policyId)

                }, 'Migration completed with failures');

                // Write failed policies to a separate log entry

                scriptLogger.error({

                    failedPolicies: failed

                }, 'Failed policies details');

            }

  

            // Console output for immediate visibility (still useful for manual runs)

            console.log("\n==== MIGRATION SUMMARY ====\nPolicy Versions");

            console.log(`✅ Policies completed: ${createdPolicies}/${total}`);

            console.log(`❌ Policies failed: ${failedPolicies}/${total}`);

            if (failed.length > 0) {

                console.log("Failed policies:", JSON.stringify(failed, null, 2));

            }

            console.log(`Duration: ${duration}ms`);

            console.log("============================\n");

        } catch (err) {

            scriptLogger.error({

                error: {

                    message: err.message,

                    stack: err.stack

                }

            }, 'Migration script failed with unexpected error');

            console.error("❌ Policy versions migration failed:", err);

            return exits.error(err);

        }

  

        return exits.success();

    },

};

  

// ... rest of your helper functions remain the same

```

  

---

  

## Key Benefits of Using Winston in Migration Scripts

  

1. **Structured Logging**: JSON format makes logs searchable and parseable

2. **Log Rotation**: Automatic file rotation prevents disk space issues

3. **Error Tracking**: Separate error logs make debugging easier

4. **Context Preservation**: Add metadata to logs for better traceability

5. **Production Ready**: Integrates with monitoring tools (Sentry, CloudWatch, etc.)

6. **Consistent Format**: All logs follow the same format across the application

7. **Log Levels**: Filter logs by severity level

8. **Audit Trail**: Permanent record of all migration activities

  

---

  

## Quick Reference

  

### Log Levels

```javascript

logger.silly('message');    // Most verbose

logger.verbose('message');

logger.debug('message');

logger.info('message');     // Standard informational

logger.warn('message');     // Warning

logger.error('message');    // Error

```

  

### Accessing Logger in Scripts

```javascript

// In Sails context

const logger = sails.config.log.custom;

  

// Standalone script

const { createLogger, format, transports } = require('winston');

const logger = createLogger({ /* config */ });

```

  

### Common Patterns

```javascript

// With context

logger.info({ id: 123, name: 'test' }, 'Processing record');

  

// Child logger

const childLogger = logger.child({ script: 'migration' });

  

// Progress tracking

logger.info({ progress: 50, processed: 100, total: 200 }, 'Progress update');

```