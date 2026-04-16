## AbortMultipartUpload

**Data & Analytics > Data Lake Storage > API Guide > Multipart > AbortMultipartUpload**

Aborts an in-progress multipart upload.

### Request

```http
DELETE /{bucket}/{objectKey}?uploadId={uploadId} HTTP/1.1
```

### Request Parameter

For the common header information for Data Lake Storage API, see the Data Lake Storage [API Request Header Guide](https://docs.beta-nhncloud.com/en/Data%20&%20Analytics/Data%20Lake%20Storage/ko/api-guide-common/).

| Name | Category | Type | Required | Description |
| --- | --- | --- | --- | --- |
| bucket | Path | String | Y | Bucket name |
| objectKey | Path | String | Y | Object name |
| x-amz-storage-class | Header | String | N | Storage class |
| uploadId | Parameter | String | Y | Multipart upload ID |

### Response

```http
HTTP/1.1 204 No Content
```
