## CreateMultipartUpload

**Data & Analytics > Data Lake Storage > API Guide > Multipart > CreateMultipartUpload**

Initiates a multipart upload and generates an upload ID to upload large objects. The upload ID is valid for up to one hour.

### Request

```http
POST /{bucket}/{objectKey}?uploads HTTP/1.1
```

### Request Parameter

For the common header information for Data Lake Storage API, see the Data Lake Storage [API Request Header Guide](api-guide-common).

| Name | Category | Type | Required | Description |
| --- | --- | --- | --- | --- |
| bucket | Path | String | Y | Bucket name |
| objectKey | Path | String | Y | Object name |
| Content-Type | Header | String | N | Object content type |
| x-amz-storage-class | Header | String | N | Storage class |
| x-amz-meta-\* | Header | String | N | Custom metadata |

### Response

```http
HTTP/1.1 200 OK

<?xml version="1.0" encoding="UTF-8"?>
<InitiateMultipartUploadResult>
  <Bucket>String</Bucket>
  <Key>String</Key>
  <UploadId>String</UploadId>
</InitiateMultipartUploadResult>
```

| Name | Type | Description |
| --- | --- | --- |
| InitiateMultipartUploadResult | Object | Results of the multipart upload |
| InitiateMultipartUploadResult.Bucket | String | Target bucket name |
| InitiateMultipartUploadResult.Key | String | Object key |
| InitiateMultipartUploadResult.UploadId | String | Multipart upload ID |
