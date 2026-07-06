## Multipart
**Data & Analytics > Data Lake Storage > Amazon S3 호환 API 가이드 > Multipart**


## AbortMultipartUpload

Aborts an in-progress multipart upload.

### Request

```http
DELETE /{bucket}/{objectKey}?uploadId={uploadId} HTTP/1.1
```

#### Request Header

For the common header information for Data Lake Storage API, see the Data Lake Storage [API Request Header Guide](api-guide-common).

#### Request Parameter

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


## CompleteMultipartUpload

Combine the uploaded parts to save the object and complete the multipart upload.

### Request

```http
POST /{bucket}/{objectKey}?uploadId={uploadId} HTTP/1.1
x-amz-checksum-crc32: ChecksumCRC32
x-amz-checksum-crc32c: ChecksumCRC32C
x-amz-checksum-crc64nvme: ChecksumCRC64NVME
x-amz-checksum-sha1: ChecksumSHA1
x-amz-checksum-sha256: ChecksumSHA256
x-amz-checksum-sha512: ChecksumSHA512
x-amz-checksum-md5: ChecksumMD5
x-amz-checksum-xxhash64: ChecksumXXHASH64
x-amz-checksum-xxhash3: ChecksumXXHASH3
x-amz-checksum-xxhash128: ChecksumXXHASH128
x-amz-checksum-type: ChecksumType

<?xml version="1.0" encoding="UTF-8"?>
<CompleteMultipartUpload xmlns="http://s3.amazonaws.com/doc/2006-03-01/">
  <Part>
    <ChecksumCRC32>string</ChecksumCRC32>
    <ChecksumCRC32C>string</ChecksumCRC32C>
    <ChecksumCRC64NVME>string</ChecksumCRC64NVME>
    <ChecksumMD5>string</ChecksumMD5>
    <ChecksumSHA1>string</ChecksumSHA1>
    <ChecksumSHA256>string</ChecksumSHA256>
    <ChecksumSHA512>string</ChecksumSHA512>
    <ChecksumXXHASH128>string</ChecksumXXHASH128>
    <ChecksumXXHASH3>string</ChecksumXXHASH3>
    <ChecksumXXHASH64>string</ChecksumXXHASH64>
    <PartNumber>Integer</PartNumber>
    <ETag>String</ETag>
  </Part>
</CompleteMultipartUpload>
```

#### Request Header

For the common header information for Data Lake Storage API, see the Data Lake
Storage [API Request Header Guide](api-guide-common).

| 이름                       | 필수 여부 | 설명                                                                                                                                                        |
|--------------------------|-------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| x-amz-checksum-crc32     | N     | 데이터 무결성 검증을 위한 헤더. 오브젝트의 32비트 `CRC32` 체크섬 값을 Base64로 인코딩한 값                                                                                               |
| x-amz-checksum-crc32c    | N     | 데이터 무결성 검증을 위한 헤더. 오브젝트의 32비트 `CRC32C` 체크섬 값을 Base64로 인코한 값                                                                                               |
| x-amz-checksum-crc64nvme | N     | 데이터 무결성 검증을 위한 헤더. 오브젝트의 64비트 `CRC64NVME` 체크섬 값을 Base64로 인코딩한 값                                                                                           |
| x-amz-checksum-sha1      | N     | 데이터 무결성 검증을 위한 헤더. 오브젝트의 160비트 `SHA1` 다이제스트 값을 Base64로 인코딩한 값                                                                                             |
| x-amz-checksum-sha256    | N     | 데이터 무결성 검증을 위한 헤더. 오브젝트의 256비트 `SHA256` 다이제스트 값을 Base64로 인코딩한 값                                                                                           |
| x-amz-checksum-sha512    | N     | 데이터 무결성 검증을 위한 헤더. 오브젝트의 512비트 `SHA512` 다이제스트 값을 Base64로 인코딩한 값                                                                                           |
| x-amz-checksum-md5       | N     | 데이터 무결성 검증을 위한 헤더. 오브젝트의 128비트 `MD5` 다이제스트 값을 Base64로 인코딩한 값                                                                                              |
| x-amz-checksum-xxhash64  | N     | 데이터 무결성 검증을 위한 헤더. 오브젝트의 64비트 `XXHASH64` 체크섬 값을 Base64로 인코딩한 값                                                                                            |
| x-amz-checksum-xxhash3   | N     | 데이터 무결성 검증을 위한 헤더. 오브젝트의 64비트 `XXHASH3` 체크섬 값을 Base64로 인코딩한 값                                                                                             |
| x-amz-checksum-xxhash128 | N     | 데이터 무결성 검증을 위한 헤더. 오브젝트의 128비트 `XXHASH128` 체크섬 값을 Base64로 인코딩한 값                                                                                          |
| x-amz-checksum-type      | N     | 멀티파트 업로드에서 파트별 체크섬을 결합하여 오브젝트 수준의 체크섬을 생성하는 방식. `CreateMultipartUpload` 요청에서 지정한 체크섬 타입과 일치하지 않을 경우 `BadDigest` 오류가 반환. 유효한 값: `COMPOSITE \| FULL_OBJECT` |

#### Request Parameter

| Name                | Category  | Type   | Required | Description         |
|---------------------|-----------|--------|----------|---------------------|
| bucket              | Path      | String | Y        | Bucket name         |
| objectKey           | Path      | String | Y        | Object name         |
| x-amz-storage-class | Header    | String | N        | Storage class       |
| uploadId            | Parameter | String | Y        | Multipart upload ID |

#### Request Body

| Name                                           | Type    | Required | Description                                 |
|------------------------------------------------|---------|----------|---------------------------------------------|
| CompleteMultipartUpload                        | Object  | Y        | Request to complete multipart upload        |
| CompleteMultipartUpload.Part                   | Object  | N        | Part list                                   |
| CompleteMultipartUpload.Part.PartNumber        | Integer | N        | Part number                                 |
| CompleteMultipartUpload.Part.ETag              | String  | N        | Unique identifier of the object             |
| CompleteMultipartUpload.Part.ChecksumCRC32     | String  | N        | 파트의 32비트 `CRC32` 체크섬 값을 Base64로 인코딩한 값      |
| CompleteMultipartUpload.Part.ChecksumCRC32C    | String  | N        | 파트의 32비트 `CRC32C` 체크섬 값을 Base64로 인코딩한 값     |
| CompleteMultipartUpload.Part.ChecksumCRC64NVME | String  | N        | 파트의 64비트 `CRC64NVME` 체크섬 값을 Base64로 인코딩한 값  |
| CompleteMultipartUpload.Part.ChecksumSHA1      | String  | N        | 파트의 160비트 `SHA1` 체크섬 값을 Base64로 인코딩한 값      |
| CompleteMultipartUpload.Part.ChecksumSHA256    | String  | N        | 파트의 256비트 `SHA256` 체크섬 값을 Base64로 인코딩한 값    |
| CompleteMultipartUpload.Part.ChecksumSHA512    | String  | N        | 파트의 512비트 `SHA512` 체크섬 값을 Base64로 인코딩한 값    |
| CompleteMultipartUpload.Part.ChecksumMD5       | String  | N        | 파트의 128비트 `MD5` 체크섬 값을 Base64로 인코딩한 값       |
| CompleteMultipartUpload.Part.ChecksumXXHASH64  | String  | N        | 파트의 64비트 `XXHASH64` 체크섬 값을 Base64로 인코딩한 값   |
| CompleteMultipartUpload.Part.ChecksumXXHASH3   | String  | N        | 파트의 64비트 `XXHASH3` 체크섬 값을 Base64로 인코딩한 값    |
| CompleteMultipartUpload.Part.ChecksumXXHASH128 | String  | N        | 파트의 128비트 `XXHASH128` 체크섬 값을 Base64로 인코딩한 값 |

### Response

```http
HTTP/1.1 200 OK

<?xml version="1.0" encoding="UTF-8"?>
<CompleteMultipartUploadResult>
  <Location>String</Location>
  <Bucket>String</Bucket>
  <Key>String</Key>
  <ETag>String</ETag>
  <ChecksumCRC32>string</ChecksumCRC32>
  <ChecksumCRC32C>string</ChecksumCRC32C>
  <ChecksumCRC64NVME>string</ChecksumCRC64NVME>
  <ChecksumSHA1>string</ChecksumSHA1>
  <ChecksumSHA256>string</ChecksumSHA256>
  <ChecksumSHA512>string</ChecksumSHA512>
  <ChecksumMD5>string</ChecksumMD5>
  <ChecksumXXHASH64>string</ChecksumXXHASH64>
  <ChecksumXXHASH3>string</ChecksumXXHASH3>
  <ChecksumXXHASH128>string</ChecksumXXHASH128>
  <ChecksumType>string</ChecksumType>
</CompleteMultipartUploadResult>
```

#### Response Body

| Name                                            | Type   | Description                                    |
|-------------------------------------------------|--------|------------------------------------------------|
| CompleteMultipartUploadResult                   | Object | Multipart upload completion result             |
| CompleteMultipartUploadResult.Location          | String | Path of the created object                     |
| CompleteMultipartUploadResult.Bucket            | String | Target bucket name                             |
| CompleteMultipartUploadResult.Key               | String | Generated object key                           |
| CompleteMultipartUploadResult.ETag              | String | Unique identifier of the final combined object |
| CompleteMultipartUploadResult.ChecksumCRC32     | String | 파트의 32비트 `CRC32` 체크섬 값을 Base64로 인코딩한 값         |
| CompleteMultipartUploadResult.ChecksumCRC32C    | String | 파트의 32비트 `CRC32C` 체크섬 값을 Base64로 인코딩한 값        |
| CompleteMultipartUploadResult.ChecksumCRC64NVME | String | 파트의 64비트 `CRC64NVME` 체크섬 값을 Base64로 인코딩한 값     |
| CompleteMultipartUploadResult.ChecksumSHA1      | String | 파트의 160비트 `SHA1` 체크섬 값을 Base64로 인코딩하여 인코딩한 값   |
| CompleteMultipartUploadResult.ChecksumSHA256    | String | 파트의 256비트 `SHA256` 체크섬 값을 Base64로 인코딩한 값       |
| CompleteMultipartUploadResult.ChecksumSHA512    | String | 파트의 512비트 `SHA512` 체크섬 값을 Base64로 인코딩한 값       |
| CompleteMultipartUploadResult.ChecksumMD5       | String | 파트의 128비트 `MD5` 체크섬 값을 Base64로 인코딩한 값          |
| CompleteMultipartUploadResult.ChecksumXXHASH64  | String | 파트의 64비트 `XXHASH64` 체크섬 값을 Base64로 인코딩한 값      |
| CompleteMultipartUploadResult.ChecksumXXHASH3   | String | 파트의 64비트 `XXHASH3` 체크섬 값을 Base64로 인코딩한 값       |
| CompleteMultipartUploadResult.ChecksumXXHASH128 | String | 파트의 128비트 `XXHASH128` 체크섬 값을 Base64로 인코딩한 값    |


## CreateMultipartUpload

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


## ListParts

Retrieves a list of parts in a multipart upload.

### Request

```http
GET /{bucket}/{objectKey}?uploadId={uploadId} HTTP/1.1
```

#### Request Header

For the common header information for Data Lake Storage API, see the Data Lake Storage [API Request Header Guide](api-guide-common).

#### Request Parameter

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

#### Response Body

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


## UploadPart

Uploads the parts of the multipart upload. Before uploading, you must call the CreateMultipartUpload API to generate a multipart upload ID.

### Request

```http
PUT /{bucket}/{objectKey}?partNumber={partNumber}&uploadId={uploadId} HTTP/1.1
Content-Length: ContentLength
Content-MD5: ContentMD5
x-amz-sdk-checksum-algorithm: ChecksumAlgorithm
x-amz-checksum-crc32: ChecksumCRC32
x-amz-checksum-crc32c: ChecksumCRC32C
x-amz-checksum-crc64nvme: ChecksumCRC64NVME
x-amz-checksum-sha1: ChecksumSHA1
x-amz-checksum-sha256: ChecksumSHA256
x-amz-checksum-sha512: ChecksumSHA512
x-amz-checksum-md5: ChecksumMD5
x-amz-checksum-xxhash64: ChecksumXXHASH64
x-amz-checksum-xxhash3: ChecksumXXHASH3
x-amz-checksum-xxhash128: ChecksumXXHASH128

body
```

#### Request Header

For the common header information for Data Lake Storage API, see the Data Lake Storage [API Request Header Guide](api-guide-common).

| 필드                       | 필수 여부 | 설명                                          |
|--------------------------|-------|---------------------------------------------|
| Content-Length           | N     | 요청 본문의 크기(바이트). 본문의 크기를 자동으로 결정할 수 없는 경우 사용 |
| Content-MD5              | N     | 파트 데이터의 128비트 `MD5` 다이제스트 값을 Base64로 인코딩한 값 |
| x-amz-checksum-crc32     | N     | 파트의 32비트 `CRC32` 체크섬 값을 Base64로 인코딩한 값      |
| x-amz-checksum-crc32c    | N     | 파트의 32비트 `CRC32C` 체크섬 값을 Base64로 인코딩한 값     |
| x-amz-checksum-crc64nvme | N     | 파트의 64비트 `CRC64NVME` 체크섬 값을 Base64로 인코딩한 값  |
| x-amz-checksum-sha1      | N     | 파트의 160비트 `SHA1` 다이제스트 값을 Base64로 인코딩한 값    |
| x-amz-checksum-sha256    | N     | 파트의 256비트 `SHA256` 다이제스트 값을 Base64로 인코딩한 값  |
| x-amz-checksum-sha512    | N     | 파트의 512비트 `SHA512` 다이제스트 값을 Base64로 인코딩한 값. |
| x-amz-checksum-md5       | N     | 파트의 128비트 `MD5` 다이제스트 값을 Base64로 인코딩한 값     |
| x-amz-checksum-xxhash64  | N     | 파트의 64비트 `XXHASH64` 체크섬 값을 Base64로 인코딩한 값   |
| x-amz-checksum-xxhash3   | N     | 파트의 64비트 `XXHASH3` 체크섬 값을 Base64로 인코딩한 값    |
| x-amz-checksum-xxhash128 | N     | 파트의 128비트 `XXHASH128` 체크섬 값을 Base64로 인코딩한 값 |

#### Request Parameter

| Name | Category | Type | Required | Description |
| --- | --- | --- | --- | --- |
| bucket | Path | String | Y | Bucket name |
| objectKey | Path | String | Y | Object name |
| x-amz-storage-class | Header | String | N | Storage class |
| partNumber | Parameter | Integer | Y | Part number (1 to 10,000) |
| uploadId | Parameter | String | Y | Multipart upload ID |

#### Request Body

| Name | Type | Required | Description |
| --- | --- | --- | --- |
| Body | Binary | Y | Part binary data, up to 5 GiB can be uploaded |

### Response

```http
HTTP/1.1 200 OK
ETag: ETag
x-amz-checksum-crc32: ChecksumCRC32
x-amz-checksum-crc32c: ChecksumCRC32C
x-amz-checksum-crc64nvme: ChecksumCRC64NVME
x-amz-checksum-sha1: ChecksumSHA1
x-amz-checksum-sha256: ChecksumSHA256
x-amz-checksum-sha512: ChecksumSHA512
x-amz-checksum-md5: ChecksumMD5
x-amz-checksum-xxhash64: ChecksumXXHASH64
x-amz-checksum-xxhash3: ChecksumXXHASH3
x-amz-checksum-xxhash128: ChecksumXXHASH128
```

#### Response Header

| 필드                       | 설명                                          |
|--------------------------|---------------------------------------------|
| ETag                     | 업로드된 파트의 엔티티 태그                             |
| x-amz-checksum-crc32     | 파트의 32비트 `CRC32` 체크섬 값을 Base64로 인코딩한 값      |
| x-amz-checksum-crc32c    | 파트의 32비트 `CRC32C` 체크섬 값을 Base64로 인코딩한 값     |
| x-amz-checksum-crc64nvme | 파트의 64비트 `CRC64NVME` 체크섬 값을 Base64로 인코딩한 값  |
| x-amz-checksum-sha1      | 파트의 160비트 `SHA1` 체크섬 값을 Base64로 인코딩한 값      |
| x-amz-checksum-sha256    | 파트의 256비트 `SHA256` 체크섬 값을 Base64로 인코딩한 값    |
| x-amz-checksum-sha512    | 파트의 512비트 `SHA512` 체크섬 값을 Base64로 인코딩한 값    |
| x-amz-checksum-md5       | 파트의 128비트 `MD5` 체크섬 값을 Base64로 인코딩한 값       |
| x-amz-checksum-xxhash64  | 파트의 64비트 `XXHASH64` 체크섬 값을 Base64로 인코딩한 값   |
| x-amz-checksum-xxhash3   | 파트의 64비트 `XXHASH3` 체크섬 값을 Base64로 인코딩한 값    |
| x-amz-checksum-xxhash128 | 파트의 128비트 `XXHASH128` 체크섬 값을 Base64로 인코딩한 값 |
