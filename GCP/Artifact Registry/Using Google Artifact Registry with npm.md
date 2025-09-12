

## 1. Authentication

### cmd.exe

```cmd
:: Get GCP access token and store it in NPM_TOKEN
for /f "tokens=*" %i in ('gcloud auth print-access-token') do set NPM_TOKEN=%i
```

### bash

```bash
# Get GCP access token and store it in NPM_TOKEN
export NPM_TOKEN=$(gcloud auth print-access-token)
```

---

## 2. Configure `.npmrc`

Create or update `.npmrc` in your project root.

### cmd.exe

```cmd
echo registry=https://registry.npmjs.org/>.npmrc
echo @your-org:registry=https://me-central2-npm.pkg.dev/seismic-axiom-466215-c9/tables-schema-repo/>>.npmrc
echo //me-central2-npm.pkg.dev/seismic-axiom-466215-c9/tables-schema-repo/:_authToken=%NPM_TOKEN%>>.npmrc
```

### bash

```bash
echo "registry=https://registry.npmjs.org/" > .npmrc
echo "@your-org:registry=https://me-central2-npm.pkg.dev/seismic-axiom-466215-c9/tables-schema-repo/" >> .npmrc
echo "//me-central2-npm.pkg.dev/seismic-axiom-466215-c9/tables-schema-repo/:_authToken=${NPM_TOKEN}" >> .npmrc
```

---

## 3. Listing Versions

### cmd.exe

```cmd
npm view @your-org/tables-schema versions --registry=https://me-central2-npm.pkg.dev/seismic-axiom-466215-c9/tables-schema-repo/
```

### bash

```bash
npm view @your-org/tables-schema versions --registry=https://me-central2-npm.pkg.dev/seismic-axiom-466215-c9/tables-schema-repo/
```

---

## 4. Publishing a Package

### cmd.exe

```cmd
npm publish --registry=https://me-central2-npm.pkg.dev/seismic-axiom-466215-c9/tables-schema-repo/
```

### bash

```bash
npm publish --registry=https://me-central2-npm.pkg.dev/seismic-axiom-466215-c9/tables-schema-repo/
```

---

Absolutely! Here's the step-by-step process I follow whenever you provide a package name:

## My Process for Installing and Publishing Packages

### 1. **Create Todo List**
- Create a todo list with two tasks: "Install [package]@[version]" and "Publish [package]@[version] to GCP registry"
- Mark the install task as "in_progress"

### 2. **Navigate to Project Root**
```powershell
cd "D:\Work\gcp-packages"
```

### 3. **Temporarily Disable GCP Registry**
```powershell
Rename-Item .npmrc .npmrc.backup
```
- This step often fails because `.npmrc` doesn't exist, but I still try it
- The purpose is to temporarily switch from the GCP registry to the public npm registry for installation

### 4. **Install Package from Public Registry**
```powershell
npm install [package]@[version]
```
- This downloads the package from the public npm registry into `node_modules`

### 5. **Restore GCP Registry Configuration**
```powershell
Rename-Item .npmrc.backup .npmrc
```
- This restores the `.npmrc` file that points to your GCP registry

### 6. **Navigate to Package Directory**
```powershell
cd node_modules\[package-name]
```

### 7. **Publish to GCP Registry**
```powershell
npm publish --registry=https://me-central2-npm.pkg.dev/me-2-uat/me-2-uat-ar-packages/ --ignore-scripts
```

### 8. **Handle Authentication Issues**
When I encounter 403 Forbidden errors (authentication expired), I:
- Navigate back to project root
- Get a fresh access token: `gcloud auth print-access-token`
- Configure npm with the new token: `npm config set //me-central2-npm.pkg.dev/me-2-uat/me-2-uat-ar-packages/:_authToken [token]`
- Retry the publish command

### 9. **Handle Package Script Issues**
For packages with problematic prepare scripts (like TypeScript compilation):
- Read the `package.json` file
- Remove or modify the `prepare` script that causes issues
- Fix any JSON syntax errors
- Retry publishing

### 10. **Update Todo List**
- Mark both install and publish tasks as "completed"
- Provide a summary of all successfully published packages

## Key Points:
- **Authentication**: Uses `gcloud auth print-access-token` and configures npm with the token
- **Registry**: Publishes to `https://me-central2-npm.pkg.dev/me-2-uat/me-2-uat-ar-packages/`
- **Flags**: Always uses `--ignore-scripts` to avoid running potentially problematic package scripts
- **Error Handling**: Refreshes authentication when tokens expire and fixes package.json issues when needed

This process ensures packages are installed from the public registry but published to your private GCP Artifact Registry.