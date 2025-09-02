


## 1. Overview

This guide explains how to deploy and redeploy the `organisation` Cloud Function (Gen2) on **Google Cloud Platform** using **Node.js 20 runtime**.  
It also covers configuring access to **private npm packages** stored in **Google Artifact Registry**.

---

## 2. Prerequisites

1. Install the [Google Cloud SDK](https://cloud.google.com/sdk/docs/install).
    
2. Authenticate with Google account:
    
    ```sh
    gcloud auth login
    ```
    
3. Set the active project:
    
    ```sh
    gcloud config set project PROJECT_ID
    ```
    
4. Ensure you have access to the private Artifact Registry repository.
    
5. Create a `.npmrc` file in your project root.
    

---

## 3. `.npmrc` Configuration

Create or update `.npmrc` in your project root with the following:

```ini
legacy-peer-deps=true
//me-central2-npm.pkg.dev/:_authToken=YOUR_GCP_ACCESS_TOKEN
@your-org:registry=https://me-central2-npm.pkg.dev/seismic-axiom-466215-c9/tables-schema-repo/
always-auth=true
```

Get the token using:

```sh
gcloud auth print-access-token
```

Replace `YOUR_GCP_ACCESS_TOKEN` in `.npmrc` with the value returned.

---

## 4. Initial Deployment

Run the following command to deploy the function for the first time:

```sh
gcloud functions deploy organisation \
  --gen2 \
  --runtime=nodejs20 \
  --trigger-http \
  --allow-unauthenticated \
  --region=me-central2 \
  --source . \
  --entry-point=organisation \
  --set-build-env-vars NPM_CONFIG_LEGACY_PEER_DEPS=true
```

---

## 5. Updating / Redeploying

After making code changes, redeploy the function using the same command:

```sh
gcloud functions deploy organisation \
  --gen2 \
  --runtime=nodejs20 \
  --trigger-http \
  --allow-unauthenticated \
  --region=me-central2 \
  --source . \
  --entry-point=organisation \
  --set-build-env-vars NPM_CONFIG_LEGACY_PEER_DEPS=true
```

> **Tip**: Only the changed code will be uploaded. GCP rebuilds the container with updated dependencies (using `.npmrc`).

---

## 6. Verification

1. List deployed functions:
    
    ```sh
    gcloud functions list --region=me-central2
    ```
    
2. Get the HTTPS trigger URL:
    
    ```sh
    gcloud functions describe organisation --region=me-central2 --format="value(serviceConfig.uri)"
    ```
    
3. Test the function:
    
    ```sh
    curl https://YOUR_CLOUD_FUNCTION_URL
    ```
    

---
