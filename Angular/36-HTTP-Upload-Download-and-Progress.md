# HTTP Upload, Download, and Progress Events

## Multipart upload

```ts
const form = new FormData();
form.append('file', file, file.name);
this.http.post('/api/upload', form, { reportProgress: true, observe: 'events' })
```

## Progress

Filter `HttpEventType.UploadProgress` / `DownloadProgress` and read `event.loaded` / `event.total` for percentage UI.

## Download blob

`responseType: 'blob'` and create `URL.createObjectURL` for save-as; **revoke** object URLs when done.

## Large files

Consider **chunked upload**, resumable APIs, or direct-to-S3 presigned URLs to keep your API servers light.

## Interceptors

Centralize **auth headers** and **base URL**; avoid duplicating progress logic in every service.

Next: [Router strategies](37-RouteReuseStrategy-and-Preloading.md).
