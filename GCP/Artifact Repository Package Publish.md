
---

### 1. **Use `npm publish` in a loop (basic but works)**

If you have a local folder with many packages (each having a `package.json`):

```bat
:: Windows CMD
for /D %%i in (*) do (
  cd %%i
  npm publish --registry=https://me-central2-npm.pkg.dev/YOUR_PROJECT/me-2-uat-ar-packages/
  cd ..
)
```

```bash
# Bash/Linux
for d in */; do
  cd "$d"
  npm publish --registry=https://me-central2-npm.pkg.dev/YOUR_PROJECT/me-2-uat-ar-packages/
  cd ..
done
```

This automates `npm publish` for all subdirectories.

---

### 2. **Download from npmjs and re-upload to Artifact Registry**

If you want to mirror public npm packages into your repo (say `express`, `lodash`, etc.):

- Use `npm pack` to download `.tgz` files.
    
    ```bash
    npm pack express@4.18.2
    ```
    
- Then publish the `.tgz` to Artifact Registry:
    
    ```bash
    npm publish express-4.18.2.tgz --registry=https://me-central2-npm.pkg.dev/YOUR_PROJECT/me-2-uat-ar-packages/
    ```
    

You can script this for multiple packages.

---

### 3. **Set Artifact Registry as a proxy (best for many packages)**

Instead of re-publishing thousands of packages, you can configure your Artifact Registry repo in **proxy mode**.

- GCP will cache packages from the public npm registry automatically the first time you install them.
    
- You only need to upload your **custom vendor packages**, while all public packages resolve via the proxy.
    

Command:

```bash
gcloud artifacts repositories create me-2-uat-ar-packages \
  --repository-format=npm \
  --location=me-central2 \
  --description="Packages repo with npm proxy" \
  --mode=REMOTE_REPOSITORY \
  --remote-repository-config=artifactregistry.googleapis.com/npm-public
```

***

```
for /f "tokens=*" %i in ('gcloud auth print-access-token') do set NPM_TOKEN=%i
```

```
for /D %i in (*) do (if not "%i"=="@ioredis" (echo Processing %i && cd "%i" && npm publish --registry=https://me-central2-npm.pkg.dev/me-2-uat/me-2-uat-ar-packages/ --ignore-scripts 2>&1 | findstr /C:"already exists" >nul && echo ℹ Package %i already exists - skipping || echo ✓ Successfully published %i && cd ..))
```

ansi-escapes

```
cd "D:\Work\gcp-packages"

Rename-Item .npmrc .npmrc.backup -ErrorAction SilentlyContinue

npm install uid-safe@2.1.5

Rename-Item .npmrc.backup .npmrc

cd node_modules\uid-safe

npm publish --registry=https://me-central2-npm.pkg.dev/me-2-uat/me-2-uat-ar-packages/ --ignore-scripts

npm config set //me-central2-npm.pkg.dev/me-2-uat/me-2-uat-ar-packages/:_authToken ya29.a0AS3H6NwX9IreoEJ4DvIGMFPQ43y_5w_l4vdLS7GQQmyz3AlkSQ8FOvdE6xA9DxPALlvCAHDWkOsuuENaVnfg94EjHVRoXsUmDW--2y81nq9n5KwTPkUfiq5_x1bf1XRZp4os4cbhkY8jAwlrrMX_3ybtNULi_8CoB9GttCRCmLeiMHkOlnienGZETzwQFsNUBFbd8oIQP7LmBgaCgYKAY4SARASFQHGX2Mi6W5y6GHEeGZXCaW7VQpl5Q0213
```

