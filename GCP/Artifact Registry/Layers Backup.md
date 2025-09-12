Got it 👍 — you’re thinking in **AWS Lambda Layers style**, where you package all dependencies once and reuse them across functions.

In **GCP (Gen 2 Functions / Cloud Run)**, there isn’t a direct "layers" feature, but you can achieve the same effect with these approaches:

---

### 🔹 Option 1: **Zip + upload vendor folder to GCS, then reference**

1. Run `npm install` locally in your project to create a `node_modules/`.
    
2. Zip up just the dependencies:
    
    ```bash
    zip -r node_modules.zip node_modules package.json package-lock.json
    ```
    
3. Upload to GCS:
    
    ```bash
    gsutil cp node_modules.zip gs://your-bucket-name/dependencies/node_modules.zip
    ```
    
4. In your function build process (via `cloudbuild.yaml` or a `Dockerfile`), download & extract it:
    
    ```dockerfile
    ADD https://storage.googleapis.com/your-bucket-name/dependencies/node_modules.zip /workspace/
    RUN unzip /workspace/node_modules.zip -d /workspace/
    ```
    

✅ Advantage: one-time build of dependencies. Functions just reuse it.  
⚠️ Downside: you must keep the zip updated when dependencies change.

---

### 🔹 Option 2: **Artifact Registry Docker Image as a "layer"**

1. Build a **base Docker image** with all dependencies pre-installed:
    
    ```dockerfile
    FROM node:20
    WORKDIR /usr/src/app
    COPY package.json package-lock.json ./
    RUN npm ci --production
    ```
    
    Push this image to Artifact Registry:
    
    ```bash
    docker build -t me-central2-docker.pkg.dev/<project>/base-images/node-deps:latest .
    docker push me-central2-docker.pkg.dev/<project>/base-images/node-deps:latest
    ```
    
2. For each function, use that image as the base:
    
    ```dockerfile
    FROM me-central2-docker.pkg.dev/<project>/base-images/node-deps:latest
    COPY . .
    CMD ["npm", "start"]
    ```
    

✅ Advantage: clean, layer-style reuse.  
⚠️ Requires moving to **Docker-based deployment** (`gcloud functions deploy --gen2 --runtime=nodejs20 --dockerfile=...`).

---

### 🔹 Option 3: **Artifact Registry npm (your current approach)**

- Push each package individually into Artifact Registry npm repos.
    
- Install them via `.npmrc` in each function.  
    ✅ Advantage: more "npm-native", granular control.  
    ⚠️ As you’ve seen, it’s **painful with lots of legacy packages**.
    

---

👉 If your goal is to **replace AWS Lambda Layers** quickly for many legacy packages, **Option 1 (zip to GCS)** is the easiest.  
If you’re planning long-term, **Option 2 (base image in Artifact Registry)** is the most maintainable.

Do you want me to draft a **step-by-step setup for Option 1 (zip + GCS reuse)** so you can try it immediately with your Gen 2 function?