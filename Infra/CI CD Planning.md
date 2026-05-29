---

excalidraw-plugin: parsed
tags: [excalidraw]

---
==⚠  Switch to EXCALIDRAW VIEW in the MORE OPTIONS menu of this document. ⚠== You can decompress Drawing data with the command palette: 'Decompress current Excalidraw file'. For more info check in plugin settings under 'Saving'


# Excalidraw Data

## Text Elements
Github - Production Governance:
-----

* Branch protection:
--
    * CI pass
    * Review Required
    * Approval required

* Github Environments:
--
    * UAT/Staging
    * Production
- Production deployment should require:
    * Manual approval
    * Restricted approval access

* Deployment to UAT/Staging is triggered on PR merge - Triggering the workflow.
* Production Deployment workflow trigger requires additional approval.

    Build once
    ↓
    Deploy to UAT/Staging
    ↓
    Approval
    ↓
    Promote SAME artifact to production
    

* Secret Management:
    - OIDC based Github actions auth

* Application level secrets managed through AWS Secret Manager. ^4ht6NbZc

Expectations:
-----
* immutable builds
* reproducible deployments
* traceable release history
* automated approvals
* security scanning
* environment promotion workflows
* zero manual production deployments ^VJ06XnQ8

Governed and centralized deployments:
-----

* Github actions
* ECR build pipeline
* CodeDeploy setup
* Remove SSH access
* IAM roles
* Deployment approvals ^JQlXKFnO

Improvements:
-----
* Github Actions:
    - Along with Deployment things
    - Add tests(Include build verification) / health check
    - Security scans ^XHIIv5js

Flow for Deployment:
-----
1. Containerized Applications:

            GitHub Actions
                ↓
            Build Docker Images
                ↓
            Push to ECR
                ↓
            CodeDeploy
                ↓
            EC2 + Docker Compose

2. Serverless functions:

            GitHub Actions
                ↓
            Serverless Framework
                ↓
            Lambda Deployments ^W3MxVy2X

Rollback strategies:
-----
* CodeDeploy:
    - Keep previous image version
    - Automatic rollback on deployment validation failure
* CodeDeploy deployment validation:
    - App boot
    - Services connectivity (Redis, Typesense, AMQP - in case of sails)
    - Essential dependencies ^umHk17pp

Security Scanning:
-----
1. Code Scanning
    - SonarQube
2. Dependency Scanning
    - Snyk
    - npm audit
    - OWASP
    - Dependency check
3. Container Image Scanning
    - Trivy
    - Grype
    - AWS ECR Scanning ^uh7wICvO

CI/CD Platform

* GitHub Actions
* ECR
* CodeDeploy
* IAM Roles
* Secrets Manager

⸻

Governance & Controls

* Branch protection
* Required reviews
* Deployment approvals
* GitHub environments
* Restricted production access

⸻

Security & Validation

* Tests
* Build verification
* Code scanning
* Dependency scanning
* Container scanning
* Health checks

⸻

Deployment Strategy

* Build once
* Promote same artifact
* Automated rollback
* Staging → Production promotion ^Bp26GBS7

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
  - Run the updated container ^B9MJDCLV

Action Items:
-----
20th May
---
* Developers account serverless setup
* FE Deployment:
    - Github Actions:
        - Zip appspec and scripts
        - Upload to S3:
            - codedeploy-artifact-registry
    - CodeDeploy
        - VM:
            - Pull from S3 (instead of github)
            - Run appspec.yml file
            - Complete deployment
 ^PzIxEbB6

Frontend CI/CD Architecture:
-----

GitHub Actions
    ↓
Build Docker image
    ↓
Push image to ECR
    ↓
Zip appspec + scripts
    ↓
Upload zip to S3
    ↓
Trigger CodeDeploy
    ↓
EC2 executes deploy.sh
    ↓
Container updated ^mXOAvqTb

Flow
--

GitHub Actions
↓
OIDC Authentication to AWS
↓
Read deploy-config.yml using yq
↓
Loop through Lambda deployments
↓
Run Serverless Deploy Commands
↓
Deploy AWS Lambda Functions

⸻

Benefits
--
* Centralized deployment management
* Scalable onboarding
* Secure authentication
* Reduced workflow duplication
* Standardized deployments ^qt1cDnYc

Core Components
---

GitHub Actions
--
* Automates Lambda deployments
* Centralized reusable workflow

OIDC Authentication
--
* Secure AWS access
* No long-lived AWS keys

Serverless Framework v4
--
* Handles Lambda deployments
* Uses service-specific configurations

deploy-config.yml
--
Centralized deployment configuration

Example:

stage: uat
deployments:
  - path: organisation
    function: Organisation
  - path: assessment
    function: Assessment

yq
--
* Parses YAML config dynamically
* Extracts deployment definitions ^fUtKlAYz

Deployment Command
---
serverless deploy function --function Organisation --stage uat


Adding a New Lambda
---
1. Add entry in deploy-config.yml
2. Commit & Push
3. GitHub Actions auto-deploys ^GajejLdg

AWS Lambda CI/CD Pipeline ^FMaTDi4b

Flow
--

GitHub Actions
↓
OIDC Authentication to AWS
↓
Build Angular App inside Docker
↓
Push Docker Image to Amazon ECR
↓
Create deployment.zip
(appspec.yml + deploy.sh)
↓
Upload Bundle to Amazon S3
↓
Trigger AWS CodeDeploy
↓
Target Server Executes deploy.sh
↓
Pull Latest Docker Image from ECR
↓
Restart Frontend Docker Container


Benefits
--
* Secure authentication using OIDC
* Immutable Docker-based deployments
* Centralized deployment orchestration
* Easy rollback using image versions
* Consistent frontend runtime environment ^rLx7EOmy

Core Components
---
GitHub Actions
--
* Automates frontend build & deployment
* Centralized deployment workflow

OIDC Authentication
--
* Secure AWS access
* No long-lived AWS keys

Docker + Angular
--
* Angular app built inside nginx container
* Consistent runtime environment

Amazon ECR
--
Stores Docker images

Example:
{AWS_ACCOUNT_ID}.dkr.ecr.eu-west-1.amazonaws.com/f-users:latest

Amazon S3
--
Stores deployment artifacts

AWS CodeDeploy
--
Handles deployment orchestration on target server
 ^2ScIE6Uj

deployment.zip Structure
---
deployment.zip
├── appspec.yml
└── scripts/
    └── deploy.sh

appspec.yml
--
Defines deployment lifecycle hooks

hooks:
  ApplicationStart:
    - location: scripts/deploy.sh

deploy.sh Responsibilities
--
* Login to ECR
* Pull latest Docker image
* Stop existing container
* Remove old container
* Start updated container
 ^JvmRulMD

Dockerized Applications (Angular/React/Sails BE) CI/CD Pipeline ^3dZ1iO4z

End-to-End Deployment
--

Developer Pushes Code
↓
GitHub Actions Starts
↓
Build Docker Image
↓
Push to ECR
↓
Upload deployment.zip to S3
↓
Trigger CodeDeploy
↓
Target Server Deploys Latest Container
 ^JFhZAkPH

AWS CI/CD Planning  ^BQUf1SuG

Flow
--

GitHub Actions
↓
OIDC Authentication to AWS
↓
Read deploy-layers-config.yml using yq
↓
Loop through Layer deployments
↓
Install Layer Dependencies
↓
Package Layer as ZIP
↓
Publish Layer Version
↓
Deploy AWS Lambda Layers

⸻

Benefits
--

* Centralized layer deployment management
* Secure AWS authentication using OIDC
* No long-lived AWS credentials
* Automated dependency packaging
* Standardized layer publishing process
* Easy onboarding of new layers
* Reduced workflow duplication
 ^uCZpnfVH

Core Components
---

GitHub Actions
--
* Automates Lambda Layer deployments
* Centralized reusable deployment workflow

OIDC Authentication
--
* Secure AWS access
* Eliminates long-lived AWS keys
* Uses IAM Role assumption

AWS Lambda Layers
--
* Publishes reusable dependencies
* Version-controlled deployments
* Supports multiple runtimes

deploy-layers-config.yml
--
Centralized layer deployment configuration.

Example

region: eu-west-1
deployments:
  - path: common-layer
    layer_name: CommonDependencies
    description: Shared Node.js dependencies
    compatible_runtimes:
      - nodejs22.x
  - path: utils-layer
    layer_name: UtilityFunctions
    description: Shared utility libraries
    compatible_runtimes:
      - nodejs22.x

yq
--
* Parses YAML configuration dynamically
* Extracts deployment definitions
* Reads runtime and layer metadata

ZIP Packaging
--
* Creates deployment package for each layer
* Excludes unnecessary files
* Installs production dependencies only
 ^m2vBiRXO

Deployment Command
--

aws lambda publish-layer-version \
  --layer-name <layer-name> \
  --description "<description>" \
  --zip-file fileb://<layer-name>.zip \
  --compatible-runtimes <runtime>

⸻

Adding a New Layer
--
1. Create the layer directory.
2. Add dependencies (package.json if required).
3. Add an entry in deploy-layers-config.yml.

Example:

- path: notification-layer
  layer_name: NotificationDependencies
  description: Shared notification libraries
  compatible_runtimes:
    - nodejs22.x

4. Commit & Push.
5. GitHub Actions automatically:
    * Installs dependencies
    * Creates ZIP package
    * Publishes a new Layer Version
    * Outputs the Layer Version ARN

⸻
 ^1eZqzZwy

AWS Layers CI/CD Pipeline ^WEWHpZ1j

Flow
--

Developer Pushes Code
↓
GitHub Repository
↓
Cloud Build Trigger Executes
↓
Authenticate using Deployment Service Account
↓
Build Docker Image
↓
Push Docker Image to Artifact Registry
↓
Connect to Target VM
↓
Pull Latest Docker Image
↓
Stop Existing Container
↓
Remove Existing Container
↓
Start Updated Container
↓
Application Available with Latest Release


Benefits
--
* Service Account based authentication
* Fully automated Docker deployments
* Centralized build & deployment workflow
* Immutable container deployments
* Easy rollback using image tags
* Consistent application runtime environment
* Supports Angular, React & Sails applications

⸻

 ^GNzwaC0w

Core Components
---

Cloud Build
--
* Automates build & deployment
* Centralized deployment workflow

Deployment Service Account
--
* Executes Cloud Build jobs
* Provides secure access to GCP resources

Artifact Registry
--
Stores Docker images
Example:
me-central2-docker.pkg.dev/<project-id>/<repository>/<image>:latest

Compute Engine VM
--
Hosts application containers
Executes deployment commands
GitHub Repository
--
Source code repository
Triggers deployments on code push

Cloud Build Trigger
---
--
* Push to Branch Trigger
* Branch Regex: ^ci-cd$
* Repository: GitHub App Integration
* Region: me-central2 (Dammam)

Deployment Service Account

Example:
me-2-uat-deployment@<project-id>.iam.gserviceaccount.com
Used by Cloud Build to:
* Push images to Artifact Registry
* Access Compute Engine
* Deploy containers on target VM

⸻ ^GraIJ4ij

Cloud Build Trigger Configuration
---

Repository Service
--
Cloud Build Repositories

Repository Generation
--
1st Gen

Repository
--
VComplyTechnologies/<repository>

Event
--
Push to Branch

Branch
--
^ci-cd$

Build Configuration
--
Cloud Build Configuration File
Location: Inline YAML

Required IAM Roles
---
* Artifact Registry Writer
* Compute Instance Admin (v1)
* Compute OS Login
* Logs Writer
* Service Account User

Deployment Responsibilities
---

* Authenticate with Artifact Registry
* Pull latest Docker image
* Stop existing container
* Remove old container
* Start updated container
* Verify application health ^WQ8dOjbH

Dockerized Applications (Angular/React/Sails BE) CI/CD Pipeline ^kXUH94oF

Flow
--

GitHub Actions
↓
OIDC Authentication to GCP
↓
Read deploy-config.yml using yq
↓
Loop through Cloud Function Deployments
↓
Run gcloud Deploy Commands
↓
Deploy GCP Cloud Functions

⸻

Benefits
--
* Centralized deployment management
* Scalable onboarding
* Secure authentication
* Reduced workflow duplication
* Standardized deployments
* No service account keys required

⸻ ^hseHzijh

Core Components
---

GitHub Actions
--
* Automates Cloud Function deployments
* Centralized reusable workflow

OIDC Authentication
--
* Secure GCP access
* No long-lived service account keys

Google Cloud Functions
--
* Handles serverless deployments
* Supports HTTP & Event-driven functions

deploy-config.yml
--
Centralized deployment configuration

Example:

stage: uat
deployments:
  - path: organisation
    function: organisation
  - path: assessment
    function: assessment

yq
--
* Parses YAML config dynamically
* Extracts deployment definitions

Workload Identity Federation
--
* GitHub ↔ GCP trust relationship
* Eliminates static credentials

⸻ ^dbEQsPi5

Deployment Command
---

Example:

gcloud functions deploy organisation \
  --gen2 \
  --runtime=nodejs22 \
  --region=me-central2 \
  --source=./organisation \
  --entry-point=handler \
  --trigger-http

⸻

Adding a New Cloud Function
--
1. Add entry in deploy-config.yml
2. Commit & Push
3. GitHub Actions auto-deploys

⸻ ^8ZUtKYTi

GCP Cloud Function CI/CD Pipeline ^63X0d4na

%%
## Drawing
```compressed-json
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

CVQIACVQQAA=
```
%%