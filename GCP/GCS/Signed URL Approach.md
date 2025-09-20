
---

## 1. Generate a **Signed Upload URL**

```js
// Generate signed URL for upload
app.post('/api/files/signed-upload', async (req, res) => {
  try {
    await ensureBucketExists();

    const { fileName, contentType } = req.body;
    if (!fileName) {
      return res.status(400).json({ error: 'fileName is required' });
    }

    const filePath = `ksa-test/${fileName}`;
    const file = storage.bucket(bucketName).file(filePath);

    const [url] = await file.getSignedUrl({
      version: 'v4',
      action: 'write',
      expires: Date.now() + 15 * 60 * 1000, // 15 minutes
      contentType: contentType || 'application/octet-stream',
    });

    res.json({
      success: true,
      url,
      filePath,
      method: 'PUT',
      expiresIn: '15m',
    });
  } catch (error) {
    console.error('Signed upload error:', error);
    res.status(500).json({ success: false, error: error.message });
  }
});
```

👉 Client-side (browser, Postman, or frontend app), you can `PUT` the file directly:

```bash
curl -X PUT -H "Content-Type: image/png" --upload-file ./my.png "<SIGNED_URL>"
```

---

## 2. Generate a **Signed Download URL**

```js
// Generate signed URL for download
app.get('/api/files/signed-download/:fileName', async (req, res) => {
  try {
    await ensureBucketExists();

    const { fileName } = req.params;
    const filePath = `ksa-test/${fileName}`;
    const file = storage.bucket(bucketName).file(filePath);

    const [url] = await file.getSignedUrl({
      version: 'v4',
      action: 'read',
      expires: Date.now() + 15 * 60 * 1000, // 15 minutes
    });

    res.json({
      success: true,
      url,
      filePath,
      method: 'GET',
      expiresIn: '15m',
    });
  } catch (error) {
    console.error('Signed download error:', error);
    res.status(500).json({ success: false, error: error.message });
  }
});
```

👉 Client-side:

```bash
curl -L "<SIGNED_URL>" -o downloaded.png
```

---

## 🔒 Key Points

- **Security**: Signed URLs expire after the time you set (15m above). You can make it shorter/longer.
    
- **Permissions**: The service account your API runs under must have `storage.objects.get` and `storage.objects.create` permissions on the bucket.
    
- **No tmp storage needed**: Client uploads go straight to GCS, Cloud Run/Functions never handle large file bodies.
    

---
