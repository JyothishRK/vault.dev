

---



## Project Structure

Your deployment consists of these key files:

- **Dockerfile** – Container configuration
    
- **deploy-cloudrun.bat** – Deployment script
    
- **.dockerignore** – Files to exclude from container
    
- **package.json** – Node.js dependencies and configuration
    

---

## Dockerfile Configuration

```dockerfile
FROM node:20-slim

# Install necessary packages for file operations
RUN apt-get update && apt-get install -y \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /app

# Copy package files first (for better caching)
COPY package*.json ./

# Install production dependencies only
RUN npm install --omit=dev

# Copy application code
COPY . .

# Expose the port Cloud Run expects
EXPOSE 8080

# Start the application
CMD ["node", "index.js"]
```

---

## Deployment Script (Windows Batch)

```bat
@echo off
echo Deploying to Cloud Run...
echo.

echo Building and deploying containerized application...
gcloud run deploy YOUR-SERVICE-NAME ^
  --source . ^
  --region=YOUR-REGION ^
  --allow-unauthenticated ^
  --memory=512Mi ^
  --cpu=1 ^
  --timeout=540 ^
  --concurrency=80 ^
  --max-instances=10

echo.
echo Deployment completed!
echo.
echo Your Cloud Run service URL will be displayed above.
pause
```

---

## .dockerignore Configuration

```
# Exclude unnecessary files from Docker build
node_modules/
npm-debug.log
.git/
.gitignore
README.md
*.md
.env
.env.local
.env.*.local
*.log
*.txt
*.bat
*.sh
uploads/
test-*.js
deploy*.bat
key.json
.gcloudignore
```

---

## Key Configuration Points

- **Port Configuration**: Cloud Run expects your app to listen on port `8080` (or use `process.env.PORT`).
    
- **Memory & CPU**: `512Mi` memory and `1 CPU` are good starting points for most Node.js apps.
    
- **Timeout**: `540 seconds` (9 minutes) for long-running operations.
    
- **Concurrency**: `80` concurrent requests per instance.
    
- **Authentication**: `--allow-unauthenticated` for public APIs.
    

---

## Application Requirements

Your Node.js app should:

- Listen on `process.env.PORT || 8080`.
    
- Handle graceful shutdowns.
    
- Use `/tmp` directory for temporary files (Cloud Run filesystem is read-only except `/tmp`).
    
- Include proper error handling and logging.
    

---

## Deployment Command Breakdown

- `--source .` – Deploy from current directory (builds container automatically).
    
- `--region=me-central2` – Deploy to specific region.
    
- `--memory=512Mi` – Allocate 512MB RAM.
    
- `--cpu=1` – Allocate 1 CPU unit.
    
- `--timeout=540` – 9-minute request timeout.
    
- `--concurrency=80` – Handle up to 80 concurrent requests per instance.
    
- `--max-instances=10` – Scale up to 10 instances maximum.
    

---

## Prerequisites

- Google Cloud CLI installed and authenticated.
    
- Project configured with:
    
    ```sh
    gcloud config set project YOUR-PROJECT-ID
    ```
    
- Cloud Run API enabled.
    
- Proper IAM permissions for deployment.
    

---

This setup provides a **robust, scalable containerized deployment to Google Cloud Run** with automatic scaling and load balancing.

---
