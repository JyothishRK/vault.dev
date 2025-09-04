
## 🛠️ Step 1: VM + Redis Setup

1. **Spin up a VM** (Compute Engine).
    
    - Pick a machine type that matches your workload (e.g. `e2-medium` is fine for light Redis).
        
    - Place it in the same **VPC + region** as your Cloud Functions for lower latency.
        
2. **Install Docker (on the VM):**
    
    ```sh
    sudo apt update
    sudo apt install -y docker.io
    sudo systemctl start docker
    sudo systemctl enable docker
    ```
    
3. **Run Redis container:**
    
    ```sh
    sudo docker run -d --name redis \
      -p 6379:6379 \
      redis:7-alpine
    ```
    
    Now Redis listens on port **6379** inside your VM.
    

---

## 🛠️ Step 2: Networking for Access

### Option A: Only for Backend App (same VM)

If **only your backend in the VM** needs Redis → you’re done ✅.  
Use `localhost:6379` inside your app.

### Option B: Functions also need Redis

Gen 2 Functions run in **Cloud Run** and connect via **VPC Connector**.

1. **Reserve a static internal IP** for Redis VM:
    
    ```sh
    gcloud compute addresses create redis-ip \
      --region=REGION \
      --subnet=default
    ```
    
2. **Assign the internal IP to VM’s NIC** (or just note down VM’s internal IP from Console → VM → Network details).
    
3. **Open Firewall for Redis port (6379):**
    
    ```sh
    gcloud compute firewall-rules create allow-redis \
      --allow tcp:6379 \
      --source-ranges=10.0.0.0/8 \
      --target-tags=redis-vm
    ```
    
    (Replace `10.0.0.0/8` with your **VPC CIDR**. Add `--network=default` if needed.)
    
4. **Attach VPC Connector** to your Gen 2 Function:
    
    ```sh
    gcloud functions deploy processJob \
      --gen2 \
      --runtime=nodejs20 \
      --region=REGION \
      --source=. \
      --entry-point=processJob \
      --trigger-http \
      --vpc-connector=projects/PROJECT_ID/locations/REGION/connectors/MY-CONNECTOR
    ```
    
    Now the function can reach Redis VM via **internal IP:6379**.
    

---

## 🛠️ Step 3: Security & Best Practices

- **Enable Redis AUTH** → run Redis with a password:
    
    ```sh
    sudo docker run -d --name redis \
      -p 6379:6379 \
      redis:7-alpine redis-server --requirepass "yourStrongPassword"
    ```
    
- **Do NOT expose Redis to public internet**. Keep it internal.
    
- For **TLS**: wrap Redis behind a proxy (like stunnel) if you need encrypted traffic.
    

---

## 🚀 Access Summary

- **Backend app in VM** → use `localhost:6379`
    
- **Gen 2 Function** → use `redis://REDIS_INTERNAL_IP:6379` (through VPC Connector)
    

---
