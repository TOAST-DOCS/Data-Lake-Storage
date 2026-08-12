<!-- pre-align:aligned sig=f760859bc93e -->

<a id="multipart"></a>
## Multipart { #multipart }
**Data & Analytics > Data Lake Storage > Amazon S3-Compatible API Guide > Multipart**


<a id="abortmultipartupload"></a>
## AbortMultipartUpload { #abortmultipartupload }

Aborts an in-progress multipart upload.

<a id="request"></a>
### Request { #request }

```http
DELETE /{bucket}/{objectKey}?uploadId={uploadId} HTTP/1.1
```

<a id="request-header"></a>
#### Request Header

For the common header information for Data Lake Storage API, see the Data Lake Storage [API Request Header Guide](api-guide-common).

<a id="request-parameter"></a>
#### Request Parameter

| Name | Category | Type | Required | Description |
| --- | --- | --- | --- | --- |
| bucket | Path | String | Y | Bucket name |
| objectKey | Path | String | Y | Object name |
| x-amz-storage-class | Header | String | N | Storage class |
| uploadId | Parameter | String | Y | Multipart upload ID |

<a id="response"></a>
### Response { #response }

```http
HTTP/1.1 204 No Content
```


<a id="completemultipartupload"></a>
## CompleteMultipartUpload { #completemultipartupload }

Combine the uploaded parts to save the object and complete the multipart upload.

<a id="completemultipartupload-request"></a>
### Request { #completemultipartupload-request }

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

<a id="completemultipartupload-request-request-header"></a>
#### Request Header

For the common header information for Data Lake Storage API, see the Data Lake Storage [API Request Header Guide](api-guide-common).

| Name | Required | Description |
|--------------------------|-------|------------------------------------------------------------------------------------------------------------------------------------------------------------|
| x-amz-checksum-crc32 | N | Header for data integrity verification. Base64-encoded value of the object's 32-bit `CRC32` checksum |
| x-amz-checksum-crc32c | N | Header for data integrity verification. Base64-encoded value of the object's 32-bit `CRC32C` checksum |
| x-amz-checksum-crc64nvme | N | Header for data integrity verification. Base64-encoded value of the object's 64-bit `CRC64NVME` checksum |
| x-amz-checksum-sha1 | N | Header for data integrity verification. Base64-encoded value of the object's 160-bit `SHA1` digest |
| x-amz-checksum-sha256 | N | Header for data integrity verification. Base64-encoded value of the object's 256-bit `SHA256` digest |
| x-amz-checksum-sha512 | N | Header for data integrity verification. Base64-encoded value of the object's 512-bit `SHA512` digest |
| x-amz-checksum-md5 | N | Header for data integrity verification. Base64-encoded value of the object's 128-bit `MD5` digest |
| x-amz-checksum-xxhash64 | N | Header for data integrity verification. Base64-encoded value of the object's 64-bit `XXHASH64` checksum |
| x-amz-checksum-xxhash3 | N | Header for data integrity verification. Base64-encoded value of the object's 64-bit `XXHASH3` checksum |
| x-amz-checksum-xxhash128 | N | Header for data integrity verification. Base64-encoded value of the object's 128-bit `XXHASH128` checksum |
| x-amz-checksum-type | N | Method used to combine part-level checksums in a multipart upload to generate an object-level checksum. If the value does not match the checksum type specified in the `CreateMultipartUpload` request, a `BadDigest` error is returned. Valid values: `COMPOSITE \| FULL_OBJECT` |

<a id="completemultipartupload-request-request-parameter"></a>
#### Request Parameter

| Name                | Category  | Type   | Required | Description         |
|---------------------|-----------|--------|----------|---------------------|
| bucket              | Path      | String | Y        | Bucket name         |
| objectKey           | Path      | String | Y        | Object name         |
| x-amz-storage-class | Header    | String | N        | Storage class       |
| uploadId            | Parameter | String | Y        | Multipart upload ID |

<a id="completemultipartupload-request-request-body"></a>
#### Request Body

| Name | Type | Required | Description |
|------------------------------------------------|---------|----------|---------------------------------------------|
| CompleteMultipartUpload | Object | Y | Request to complete multipart upload |
| CompleteMultipartUpload.Part | Object | N | Part list |
| CompleteMultipartUpload.Part.PartNumber | Integer | N | Part number |
| CompleteMultipartUpload.Part.ETag | String | N | Unique identifier of the object |
| CompleteMultipartUpload.Part.ChecksumCRC32 | String | N | Base64-encoded value of the part's 32-bit `CRC32` checksum |
| CompleteMultipartUpload.Part.ChecksumCRC32C | String | N | Base64-encoded value of the part's 32-bit `CRC32C` checksum |
| CompleteMultipartUpload.Part.ChecksumCRC64NVME | String | N | Base64-encoded value of the part's 64-bit `CRC64NVME` checksum |
| CompleteMultipartUpload.Part.ChecksumSHA1 | String | N | Base64-encoded value of the part's 160-bit `SHA1` checksum |
| CompleteMultipartUpload.Part.ChecksumSHA256 | String | N | Base64-encoded value of the part's 256-bit `SHA256` checksum |
| CompleteMultipartUpload.Part.ChecksumSHA512 | String | N | Base64-encoded value of the part's 512-bit `SHA512` checksum |
| CompleteMultipartUpload.Part.ChecksumMD5 | String | N | Base64-encoded value of the part's 128-bit `MD5` checksum |
| CompleteMultipartUpload.Part.ChecksumXXHASH64 | String | N | Base64-encoded value of the part's 64-bit `XXHASH64` checksum |
| CompleteMultipartUpload.Part.ChecksumXXHASH3 | String | N | Base64-encoded value of the part's 64-bit `XXHASH3` checksum |
| CompleteMultipartUpload.Part.ChecksumXXHASH128 | String | N | Base64-encoded value of the part's 128-bit `XXHASH128` checksum |

<a id="completemultipartupload-response"></a>
### Response { #completemultipartupload-response }

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

<a id="completemultipartupload-response-response-body"></a>
#### Response Body

| Name | Type | Description |
|-------------------------------------------------|--------|-----------------------------------------------|
| CompleteMultipartUploadResult | Object | Multipart upload completion result |
| CompleteMultipartUploadResult.Location | String | Path of the created object |
| CompleteMultipartUploadResult.Bucket | String | Target bucket name |
| CompleteMultipartUploadResult.Key | String | Generated object key |
| CompleteMultipartUploadResult.ETag | String | Unique identifier of the final combined object |
| CompleteMultipartUploadResult.ChecksumCRC32 | String | Base64-encoded value of the part's 32-bit `CRC32` checksum |
| CompleteMultipartUploadResult.ChecksumCRC32C | String | Base64-encoded value of the part's 32-bit `CRC32C` checksum |
| CompleteMultipartUploadResult.ChecksumCRC64NVME | String | Base64-encoded value of the part's 64-bit `CRC64NVME` checksum |
| CompleteMultipartUploadResult.ChecksumSHA1 | String | Base64-encoded value of the part's 160-bit `SHA1` checksum |
| CompleteMultipartUploadResult.ChecksumSHA256 | String | Base64-encoded value of the part's 256-bit `SHA256` checksum |
| CompleteMultipartUploadResult.ChecksumSHA512 | String | Base64-encoded value of the part's 512-bit `SHA512` checksum |
| CompleteMultipartUploadResult.ChecksumMD5 | String | Base64-encoded value of the part's 128-bit `MD5` checksum |
| CompleteMultipartUploadResult.ChecksumXXHASH64 | String | Base64-encoded value of the part's 64-bit `XXHASH64` checksum |
| CompleteMultipartUploadResult.ChecksumXXHASH3 | String | Base64-encoded value of the part's 64-bit `XXHASH3` checksum |
| CompleteMultipartUploadResult.ChecksumXXHASH128 | String | Base64-encoded value of the part's 128-bit `XXHASH128` checksum |


<a id="createmultipartupload"></a>
## CreateMultipartUpload { #createmultipartupload }

Initiates a multipart upload and generates an upload ID to upload large objects. The upload ID is valid for up to one hour.

<a id="createmultipartupload-request"></a>
### Request { #createmultipartupload-request }

```http
POST /{bucket}/{objectKey}?uploads HTTP/1.1
x-amz-checksum-algorithm: ChecksumAlgorithm
x-amz-checksum-type: ChecksumType
```

<a id="createmultipartupload-request-request-header"></a>
#### Request Header

For the common header information for Data Lake Storage API, see the Data Lake Storage [API Request Header Guide](api-guide-common).

| Field                       | Required | Description                                                                                                                                       |
|--------------------------|-------|------------------------------------------------------------------------------------------------------------------------------------------|
| x-amz-checksum-algorithm | N     | Specifies the algorithm to use for generating the object's checksum. Valid values: `CRC32 \| CRC32C \| SHA1 \| SHA256 \| CRC64NVME \| SHA512 \| MD5 \| XXHASH64 \| XXHASH3 \| XXHASH128` |
| x-amz-checksum-type      | N     | Specifies the method used to calculate the object's checksum value. Valid values: `COMPOSITE \| FULL_OBJECT` |

<a id="createmultipartupload-request-request-parameter"></a>
#### Request Parameter

| Name                | Category | Type   | Required | Description         |
|---------------------|----------|--------|----------|---------------------|
| bucket              | Path     | String | Y        | Bucket name         |
| objectKey           | Path     | String | Y        | Object name         |
| Content-Type        | Header   | String | N        | Object content type |
| x-amz-storage-class | Header   | String | N        | Storage class       |
| x-amz-meta-\*       | Header   | String | N        | Custom metadata     |

<a id="createmultipartupload-response"></a>
### Response { #createmultipartupload-response }

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

<a id="createmultipartupload-response-response-header"></a>
#### Response Header

| Field | Description |
|--------------------------|--------------------------|
| x-amz-checksum-algorithm | Algorithm used to generate the object's checksum |
| x-amz-checksum-type | Method used to calculate the object's checksum value |

<a id="createmultipartupload-response-response-body"></a>
#### Response Body

| Name                                   | Type   | Description                     |
|----------------------------------------|--------|---------------------------------|
| InitiateMultipartUploadResult          | Object | Results of the multipart upload |
| InitiateMultipartUploadResult.Bucket   | String | Target bucket name              |
| InitiateMultipartUploadResult.Key      | String | Object key                      |
| InitiateMultipartUploadResult.UploadId | String | Multipart upload ID             |


<a id="listparts"></a>
## ListParts { #listparts }

Retrieves a list of parts in a multipart upload.

<a id="listparts-request"></a>
### Request { #listparts-request }

```http
GET /{bucket}/{objectKey}?uploadId={uploadId} HTTP/1.1
```

<a id="listparts-request-request-header"></a>
#### Request Header

For the common header information for Data Lake Storage API, see the Data Lake Storage [API Request Header Guide](api-guide-common).

<a id="listparts-request-request-parameter"></a>
#### Request Parameter

| Name | Category | Type | Required | Description |
| --- | --- | --- | --- | --- |
| bucket | Path | String | Y | Bucket name |
| objectKey | Path | String | Y | Object name |
| x-amz-storage-class | Header | String | N | Storage class |
| uploadId | Parameter | String | Y | Multipart upload ID |

<a id="listparts-response"></a>
### Response { #listparts-response }

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

<a id="listparts-response-response-body"></a>
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


<a id="uploadpart"></a>
## UploadPart { #uploadpart }

Uploads the parts of the multipart upload. Before uploading, you must call the CreateMultipartUpload API to generate a multipart upload ID.

<a id="uploadpart-request"></a>
### Request { #uploadpart-request }

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

<a id="uploadpart-request-request-header"></a>
#### Request Header

For the common header information for Data Lake Storage API, see the Data Lake Storage [API Request Header Guide](api-guide-common).

| Field | Required | Description |
|--------------------------|-------|--------------------------------------------|
| Content-Length | N | Size of the request body (in bytes). Used when the size of the body cannot be determined automatically. |
| Content-MD5 | N | Base64-encoded value of the 128-bit `MD5` digest of the part data |
| x-amz-checksum-crc32 | N | Base64-encoded value of the part's 32-bit `CRC32` checksum |
| x-amz-checksum-crc32c | N | Base64-encoded value of the part's 32-bit `CRC32C` checksum |
| x-amz-checksum-crc64nvme | N | Base64-encoded value of the part's 64-bit `CRC64NVME` checksum |
| x-amz-checksum-sha1 | N | Base64-encoded value of the part's 160-bit `SHA1` digest |
| x-amz-checksum-sha256 | N | Base64-encoded value of the part's 256-bit `SHA256` digest |
| x-amz-checksum-sha512 | N | Base64-encoded value of the part's 512-bit `SHA512` digest |
| x-amz-checksum-md5 | N | Base64-encoded value of the part's 128-bit `MD5` digest |
| x-amz-checksum-xxhash64 | N | Base64-encoded value of the part's 64-bit `XXHASH64` checksum |
| x-amz-checksum-xxhash3 | N | Base64-encoded value of the part's 64-bit `XXHASH3` checksum |
| x-amz-checksum-xxhash128 | N | Base64-encoded value of the part's 128-bit `XXHASH128` checksum |

<a id="uploadpart-request-request-parameter"></a>
#### Request Parameter

| Name | Category | Type | Required | Description |
| --- | --- | --- | --- | --- |
| bucket | Path | String | Y | Bucket name |
| objectKey | Path | String | Y | Object name |
| x-amz-storage-class | Header | String | N | Storage class |
| partNumber | Parameter | Integer | Y | Part number (1 to 10,000) |
| uploadId | Parameter | String | Y | Multipart upload ID |

<a id="uploadpart-request-request-body"></a>
#### Request Body

| Name | Type | Required | Description |
| --- | --- | --- | --- |
| Body | Binary | Y | Part binary data, up to 5 GiB can be uploaded |

<a id="uploadpart-response"></a>
### Response { #uploadpart-response }

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

<a id="uploadpart-response-response-header"></a>
#### Response Header

| Field | Description |
|--------------------------|---------------------------------------------|
| ETag | Entity tag of the uploaded part |
| x-amz-checksum-crc32 | Base64-encoded value of the part's 32-bit `CRC32` checksum |
| x-amz-checksum-crc32c | Base64-encoded value of the part's 32-bit `CRC32C` checksum |
| x-amz-checksum-crc64nvme | Base64-encoded value of the part's 64-bit `CRC64NVME` checksum |
| x-amz-checksum-sha1 | Base64-encoded value of the part's 160-bit `SHA1` checksum |
| x-amz-checksum-sha256 | Base64-encoded value of the part's 256-bit `SHA256` checksum |
| x-amz-checksum-sha512 | Base64-encoded value of the part's 512-bit `SHA512` checksum |
| x-amz-checksum-md5 | Base64-encoded value of the part's 128-bit `MD5` checksum |
| x-amz-checksum-xxhash64 | Base64-encoded value of the part's 64-bit `XXHASH64` checksum |
| x-amz-checksum-xxhash3 | Base64-encoded value of the part's 64-bit `XXHASH3` checksum |
| x-amz-checksum-xxhash128 | Base64-encoded value of the part's 128-bit `XXHASH128` checksum |
