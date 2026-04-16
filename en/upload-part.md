## UploadPart

**Data & Analytics > Data Lake Storage > API Guide > Multipart > UploadPart**

Uploads the parts of the multipart upload. Before uploading, you must call the CreateMultipartUpload API to generate a multipart upload ID.

### Request

```http
PUT /{bucket}/{objectKey}?partNumber={partNumber}&uploadId={uploadId} HTTP/1.1

body
```

### Request Parameter

For the common header information for Data Lake Storage API, see the Data Lake Storage [API Request Header Guide](https://docs.beta-nhncloud.com/en/Data%20&%20Analytics/Data%20Lake%20Storage/ko/api-guide-common/).

| Name | Category | Type | Required | Description |
| --- | --- | --- | --- | --- |
| bucket | Path | String | Y | Bucket name |
| objectKey | Path | String | Y | Object name |
| x-amz-storage-class | Header | String | N | Storage class |
| partNumber | Parameter | Integer | Y | Part number (1 to 10,000) |
| uploadId | Parameter | String | Y | Multipart upload ID |

### Request Body
| Name | Type | Required | Description |
| --- | --- | --- | --- |
| Body | Binary | Y | Part binary data, up to 5 GiB can be uploaded |

### Response

```http
HTTP/1.1 200 OK
```
