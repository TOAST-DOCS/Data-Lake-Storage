## CompleteMultipartUpload

**Data & Analytics > Data Lake Storage > API Guide > Multipart > CompleteMultipartUpload**

Combine the uploaded parts to save the object and complete the multipart upload.

### Request

```http
POST /{bucket}/{objectKey}?uploadId={uploadId} HTTP/1.1

<?xml version="1.0" encoding="UTF-8"?>
<CompleteMultipartUpload xmlns="http://s3.amazonaws.com/doc/2006-03-01/">
   <Part>
     <PartNumber>Integer</PartNumber>
     <ETag>String</ETag>
   </Part>
</CompleteMultipartUpload>
```

### Request Parameter

For the common header information for Data Lake Storage API, see the Data Lake Storage [API Request Header Guide](api-guide-common).

| Name | Category | Type | Required | Description |
| --- | --- | --- | --- | --- |
| bucket | Path | String | Y | Bucket name |
| objectKey | Path | String | Y | Object name |
| x-amz-storage-class | Header | String | N | Storage class |
| uploadId | Parameter | String | Y | Multipart upload ID |

### Request Body
| Name | Type | Required | Description |
| --- | --- | --- | --- |
| CompleteMultipartUpload | Object | Y | Request to complete multipart upload |
| CompleteMultipartUpload.Part | Object | N | Part list |
| CompleteMultipartUpload.Part.PartNumber | Integer | N | Part number |
| CompleteMultipartUpload.Part.ETag | String | N | Unique identifier of the object |

### Response

```http
HTTP/1.1 200 OK

<?xml version="1.0" encoding="UTF-8"?>
<CompleteMultipartUploadResult>
  <Location>String</Location>
  <Bucket>String</Bucket>
  <Key>String</Key>
  <ETag>String</ETag>
</CompleteMultipartUploadResult>
```

| Name | Type | Description |
| --- | --- | --- |
| CompleteMultipartUploadResult | Object | Multipart upload completion result |
| CompleteMultipartUploadResult.Location | String | Path of the created object |
| CompleteMultipartUploadResult.Bucket | String | Target bucket name |
| CompleteMultipartUploadResult.Key | String | Generated object key |
| CompleteMultipartUploadResult.ETag | String | Unique identifier of the final combined object |
