## CreateMultipartUpload

**Data & Analytics > Data Lake Storage > API Guide > Multipart > CreateMultipartUpload**

Initiates a multipart upload and generates an upload ID to upload large objects. The upload ID is
valid for up to one hour.

### Request

```http
POST /{bucket}/{objectKey}?uploads HTTP/1.1
x-amz-checksum-algorithm: ChecksumAlgorithm
x-amz-checksum-type: ChecksumType
```

#### Request Header

For the common header information for Data Lake Storage API, see the Data Lake
Storage [API Request Header Guide](api-guide-common).

| 필드                       | 필수 여부 | 설명                                                                                                                                       |
|--------------------------|-------|------------------------------------------------------------------------------------------------------------------------------------------|
| x-amz-checksum-algorithm | N     | 오브젝트의 체크섬 생성에 사용할 알고리즘을 지정. 유효한 값: `CRC32 \| CRC32C \| SHA1 \| SHA256 \| CRC64NVME \| SHA512 \| MD5 \| XXHASH64 \| XXHASH3 \| XXHASH128` |
| x-amz-checksum-type      | N     | 오브젝트의 체크섬 값을 계산하는 방식을 지정. 유효한 값: `COMPOSITE \| FULL_OBJECT`                                                                              |

#### Request Parameter

| Name                | Category | Type   | Required | Description         |
|---------------------|----------|--------|----------|---------------------|
| bucket              | Path     | String | Y        | Bucket name         |
| objectKey           | Path     | String | Y        | Object name         |
| Content-Type        | Header   | String | N        | Object content type |
| x-amz-storage-class | Header   | String | N        | Storage class       |
| x-amz-meta-\*       | Header   | String | N        | Custom metadata     |

### Response

```http
HTTP/1.1 200 OK
x-amz-checksum-algorithm: ChecksumAlgorithm
x-amz-checksum-type: ChecksumType

<?xml version="1.0" encoding="UTF-8"?>
<InitiateMultipartUploadResult>
  <Bucket>String</Bucket>
  <Key>String</Key>
  <UploadId>String</UploadId>
</InitiateMultipartUploadResult>
```

#### Response Header

| 필드                       | 설명                       |
|--------------------------|--------------------------|
| x-amz-checksum-algorithm | 오브젝트의 체크섬 생성에 사용한 알고리즘 값 |
| x-amz-checksum-type      | 오브젝트의 체크섬 값을 계산한 방식      |

#### Response Body

| Name                                   | Type   | Description                     |
|----------------------------------------|--------|---------------------------------|
| InitiateMultipartUploadResult          | Object | Results of the multipart upload |
| InitiateMultipartUploadResult.Bucket   | String | Target bucket name              |
| InitiateMultipartUploadResult.Key      | String | Object key                      |
| InitiateMultipartUploadResult.UploadId | String | Multipart upload ID             |
