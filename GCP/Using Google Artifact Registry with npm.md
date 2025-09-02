

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