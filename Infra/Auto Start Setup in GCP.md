---

excalidraw-plugin: parsed
tags: [excalidraw]

---
==⚠  Switch to EXCALIDRAW VIEW in the MORE OPTIONS menu of this document. ⚠== You can decompress Drawing data with the command palette: 'Decompress current Excalidraw file'. For more info check in plugin settings under 'Saving'


# Excalidraw Data

## Text Elements

Start Environment

    User
      ↓
    Slack Captain Command
      ↓
    AWS Lambda
      ↓
    GCP Compute Engine API
      ↓
    Start VM
      ↓
    Docker + PM2 + Systemd Services Auto Restore
      ↓
    UAT Environment Available

Stop Environment

    User
      ↓
    Slack Captain Command
      ↓
    AWS Lambda
      ↓
    GCP Compute Engine API
      ↓
    Stop VM

Status Check

    User
      ↓
    Slack Captain Command
      ↓
    AWS Lambda
      ↓
    GCP Compute Engine API
      ↓
    Get VM Status
      ↓
    Status Returned to Slack ^KafRsJWS

Overall Flow

    /captain start PF2 UAT
                ↓
           AWS Lambda
                ↓
        GCP Compute API
                ↓
             Start VM
                ↓
      Docker Containers Start
                ↓
        PM2 Apps Restore
                ↓
      Systemd Services Start
                ↓
       Environment Ready ^OnXzByuU

Setup:
---

- Create dedicated service account:
    - Captain-controller
        - Permissions: Compute Instance Admin(v1)
        (OR)
        - Custom Role with permissions:
            - compute.instances.get
            - compute.instances.start
            - compute.instances.stop

- Create the service account key for Captain-controller (JSON).
- Store the key in AWS Secret Manager.
- Lambda flow:
    - Read Secret
    - Create JWT Token
    - Obtain Oauth Token
    - Call Compute Engine API
- Compute Engine APIs:
    - Start: POST https://compute.googleapis.com/compute/v1/projects/{PROJECT}/zones/{ZONE}/instances/{INSTANCE}/start
    - Stop: POST
https://compute.googleapis.com/compute/v1/projects/{PROJECT}/zones/{ZONE}/instances/{INSTANCE}/stop
    - Status: GET
https://compute.googleapis.com/compute/v1/projects/{PROJECT}/zones/{ZONE}/instances/{INSTANCE}
 ^d6Bv46sp

VM Startup Recovery
---

On the VM itself:

Docker:
--

docker update --restart unless-stopped $(docker ps -aq)

Verify:
--
docker inspect container_name | grep RestartPolicy

⸻
PM2
--

pm2 save
pm2 startup
pm2 save

Verify:
--
systemctl status 

⸻
Java Services
--
Convert anything started with nohup java -jar xxx.jar into:

systemctl enable service-name

and verify:

systemctl status service-name
 ^d02fQhIa

%%
## Drawing
```compressed-json
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
```
%%