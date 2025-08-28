
---

## 1. Active Accounts in `gcloud`

- **List all authenticated accounts**
    
    ```sh
    gcloud auth list
    ```
    
- **Set an active account**
    
    ```sh
    gcloud config set account ACCOUNT_EMAIL
    ```
    
    Example:
    
    ```sh
    gcloud config set account user1@gmail.com
    ```
    
- **Authenticate a new account**
    
    ```sh
    gcloud auth login
    ```
    

---

## 2. Switching Projects

- **List all accessible projects**
    
    ```sh
    gcloud projects list
    ```
    
- **Set a default project**
    
    ```sh
    gcloud config set project PROJECT_ID
    ```
    
    Example:
    
    ```sh
    gcloud config set project seismic-axiom-466215-c9
    ```
    

---

## 3. Deploying Cloud Functions (Gen2)

To deploy a Node.js 20 runtime HTTP-triggered function in region `me-central2`:

```sh
gcloud functions deploy gcs-poc ^
  --gen2 ^
  --runtime=nodejs20 ^
  --trigger-http ^
  --allow-unauthenticated ^
  --region=me-central2
```

---

