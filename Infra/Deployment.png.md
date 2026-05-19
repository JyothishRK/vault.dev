Existing Deployment Process:
-----

* Lambda:
---
- Repository: b-all

- Deployment Steps:
  - Navigate to the respective function directory
  - Run:
    sls deploy --function <function-name> --stage <stage>

- For stage-based Lambda deployments:
  - The above deployment approach is sufficient

- For alias-based Lambda deployments:
  - Deploy the latest function code
  - Publish a new Lambda version
  - Update the corresponding alias to point to the newly published version


* Sails:
---
- Repository: vc-backend-v2

- Deployment Steps:
  - Build the Docker image locally using the corresponding environment configuration
  - Tag and push the image to ECR
  - SSH into the VM
  - Pull the latest tagged image
  - Run the updated container