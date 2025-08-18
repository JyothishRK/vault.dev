
---
This guide outlines the step-by-step process for deploying a backend module to AWS Lambda using the Serverless Framework. Follow the instructions carefully to ensure a successful deployment.

---

### Prerequisites

- AWS credentials configured (via IAM or AWS CLI)
    
- Serverless Framework installed globally (`npm install -g serverless`)
    
- Proper access to the Git branch/module
    
- Required configurations in `serverless.yml` for the service/module
    

---

### Deployment Steps

**1. Switch to the Target Branch**  
Ensure you are on the correct Git branch from which the deployment will be made:

```bash
git checkout <branch-name>
```

**2. Pull the Latest Changes**  
Sync your local repository with the remote branch or base branch:

```bash
git pull origin <branch-name>
# or
git pull origin <base-branch-name>
```

**3. Navigate to the Module Directory**  
Change directory into the module that needs to be deployed:

```bash
cd <module-directory>
```

**4. Deploy the Function**  
Use the Serverless CLI to deploy the specific function to the appropriate stage (e.g., dev):

```bash
sls deploy function --function Compliance --stage dev
```

**Note:** The function name (`Compliance` in this example) should match the function identifier defined in your `serverless.yml` file under the `functions:` section.

**5. Verify Deployment on AWS**  
Go to the AWS Lambda Console and verify that:

- The correct function is updated
    
- The changes reflect as expected
    

**6. Publish a New Version**  
After verifying the deployed function, publish a version with a meaningful description:

- **Manual:** Use AWS Console → Lambda Function → Versions → Publish new version
    

**7. Point the Alias to the New Version**  
Update the alias (corresponding to the environment, e.g., dev, staging, or prod) to point to the new version:

- **Manual:** AWS Console → Lambda → Aliases → Edit alias target version
    

Ensure that the alias now reflects the correct version number of your Lambda function.

---

### Deployment Complete

Your backend function is now successfully deployed to AWS Lambda and linked to the appropriate environment alias.

---

### Notes

- Always validate changes locally before deploying to production environments.
    
- Maintain version history and alias mappings for rollback purposes.
    
- Use tagging and CloudWatch logs for enhanced monitoring and debugging.