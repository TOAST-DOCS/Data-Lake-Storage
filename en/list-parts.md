## ListParts

**Data & Analytics > Data Lake Storage > API Guide > Multipart > ListParts**

Retrieves a list of parts in a multipart upload.

### Request

```http
GET /{bucket}/{objectKey}?uploadId={uploadId} HTTP/1.1
```

### Request Parameter

For the common header information for Data Lake Storage API, see the Data Lake Storage [API Request Header Guide](api-guide-common).

| Name | Category | Type | Required | Description |
| --- | --- | --- | --- | --- |
| bucket | Path | String | Y | Bucket name |
| objectKey | Path | String | Y | Object name |
| x-amz-storage-class | Header | String | N | Storage class |
| uploadId | Parameter | String | Y | Multipart upload ID |

### Response

```http
HTTP/1.1 200 OK

<?xml version="1.0" encoding="UTF-8"?>
<ListPartsResult>
  <Bucket>String</Bucket>
  <Key>String</Key>
  <UploadId>String</UploadId>
  <StorageClass>String</StorageClass>
  <PartNumberMarker>Integer</PartNumberMarker>
  <NextPartNumberMarker>Integer</NextPartNumberMarker>
  <MaxParts>Integer</MaxParts>
  <IsTruncated>Boolean</IsTruncated>
  <Part>
    <PartNumber>Integer</PartNumber>
    <LastModified>Timestamp</LastModified>
    <ETag>String</ETag>
    <Size>Long</Size>
  </Part>
</ListPartsResult>
```

| Name | Type | Description |
| --- | --- | --- |
| ListPartsResult | Object | Result of part list retrieval |
| ListPartsResult.Bucket | String | Bucket name |
| ListPartsResult.Key | String | Object key |
| ListPartsResult.UploadId | String | Multipart upload ID |
| ListPartsResult.StorageClass | String | Storage class |
| ListPartsResult.PartNumberMarker | Integer | Starting point for retrieval |
| ListPartsResult.NextPartNumberMarker | Integer | Starting point for next retrieval |
| ListPartsResult.MaxParts | Integer | Maximum number of parts |
| ListPartsResult.IsTruncated | Boolean | Whether additional pages exist |
| ListPartsResult.Part | Object | Part information |
| ListPartsResult.Part.PartNumber | Integer | Part number |
| ListPartsResult.Part.LastModified | Timestamp | Last modified time (in ISO 8601 format) |
| ListPartsResult.Part.ETag | String | Unique identifier of the part object |
| ListPartsResult.Part.Size | Long | Part size (bytes) |
