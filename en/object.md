## Object
**Data & Analytics > Data Lake Storage > Amazon S3-Compatible API Guide > Object**


## DeleteObject

Deletes objects stored in the bucket.

### Request

```http
DELETE /{bucket}/{objectKey} HTTP/1.1
```

#### Request Header

For the common header information for Data Lake Storage API, see the Data Lake Storage [API Request Header Guide](api-guide-common).

#### Request Parameter

| Name | Category | Type | Required | Description |
| --- | --- | --- | --- | --- |
| bucket | Path | String | Y | Bucket name |
| objectKey | Path | String | Y | Object name |

### Response

```http
HTTP/1.1 204 No Content
```


## DeleteObjects

Deletes multiple objects in a single request. Up to 1,000 object keys can be specified per request.

### Request

```http
POST /{bucket}?delete HTTP/1.1

<?xml version="1.0" encoding="UTF-8"?>
<Delete>
  <Object>
    <ETag>string</ETag>
    <Key>string</Key>
    <LastModifiedTime>timestamp</LastModifiedTime>
    <Size>long</Size>
  </Object>
  ...
  <Quiet>Boolean</Quiet>
</Delete>
```

#### Request Header

For header information commonly used in Data Lake Storage APIs, see the Data Lake Storage [API Request Header Guide](api-guide-common).

#### Request Parameter

| Name | In | Type | Required | Description |
| --- | --- | --- | --- | --- |
| bucket | Path | String | Y | Bucket name |
| Content-MD5 | Header | String | Y | MD5 hash of the request body (used to verify integrity during transmission) |

#### Request Body

| Name | Type | Required | Description |
| --- | --- | --- | --- |
| Delete | Object | Y | Root element of the request |
| Delete.Object | Array | Y | List of objects to delete. Up to 1,000 |
| Delete.Object.ETag | String | N | ETag value of the object. If specified, only objects with a matching ETag are deleted. |
| Delete.Object.Key | String | Y | Key of the object to delete |
| Delete.Object.LastModifiedTime | String | N | Last modified time of the object. If specified, only objects with a matching time are deleted. |
| Delete.Object.Size | Long | N | Size of the object (in bytes). If specified, only objects with a matching size are deleted. |
| Delete.Quiet | Boolean | N | When set to `true`, operates in Quiet mode, and only failed items are included in the response. The default is Verbose mode (returns all results). |

### Response

```http
HTTP/1.1 200 OK

<?xml version="1.0" encoding="UTF-8"?>
<DeleteResult>
  <Deleted>
    <Key>String</Key>
  </Deleted>
  ...
  <Error>
    <Key>String</Key>
    <Code>String</Code>
    <Message>String</Message>
  </Error>
  ...
</DeleteResult>
```

#### Response Body

| Name | Type | Description |
| --- | --- | --- |
| DeleteResult | Object | Root element of the deletion result |
| DeleteResult.Deleted | Array | List of successfully deleted objects |
| DeleteResult.Deleted.Key | String | Key of the deleted object |
| DeleteResult.Error | Array | List of objects that failed to delete |
| DeleteResult.Error.Key | String | Key of the object that failed to delete |
| DeleteResult.Error.Code | String | Error code |
| DeleteResult.Error.Message | String | Error message |


## GetObject

Retrieves an object stored in a bucket.

### Request

```http
GET /{bucket}/{objectKey} HTTP/1.1
```

#### Request Header

For the common header information for Data Lake Storage API, see the Data Lake Storage [API Request Header Guide](api-guide-common).

#### Request Parameter

| Name | Category | Type | Required | Description |
| --- | --- | --- | --- | --- |
| bucket | Path | String | Y | Bucket name |
| objectKey | Path | String | Y | Object name |
| Range | Header | String | N | Partial download range |
| x-amz-storage-class | Header | String | N | Storage class |

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
x-amz-checksum-type: ChecksumType

data
```

#### Response Header

| Field | Description |
|--------------------------|---------------------------------------------------------------------------------|
| ETag | Unique identifier assigned by the server for a specific version of the resource |
| x-amz-checksum-crc32 | Base64-encoded value of the object's 32-bit `CRC32` checksum |
| x-amz-checksum-crc32c | Base64-encoded value of the object's 32-bit `CRC32C` checksum |
| x-amz-checksum-crc64nvme | Base64-encoded value of the object's 64-bit `CRC64NVME` checksum |
| x-amz-checksum-sha1 | Base64-encoded value of the object's 160-bit `SHA1` digest |
| x-amz-checksum-sha256 | Base64-encoded value of the object's 256-bit `SHA256` digest |
| x-amz-checksum-sha512 | Base64-encoded value of the object's 512-bit `SHA512` digest |
| x-amz-checksum-md5 | Base64-encoded value of the object's 128-bit `MD5` digest |
| x-amz-checksum-xxhash64 | Base64-encoded value of the object's 64-bit `XXHASH64` checksum |
| x-amz-checksum-xxhash3 | Base64-encoded value of the object's 64-bit `XXHASH3` checksum |
| x-amz-checksum-xxhash128 | Base64-encoded value of the object's 128-bit `XXHASH128` checksum |
| x-amz-checksum-type | Method used to combine part-level checksums of a multipart object to generate an object-level checksum. Valid values: `COMPOSITE \| FULL_OBJECT` |


## HeadObject

Retrieves the metadata of an object stored in a bucket.

### Request

```http
HEAD /{bucket}/{objectKey} HTTP/1.1
```

#### Request Header

For the common header information for Data Lake Storage API, see the Data Lake Storage [API Request Header Guide](api-guide-common).

#### Request Parameter

| Name | Category | Type | Required | Description |
| --- | --- | --- | --- | --- |
| bucket | Path | String | Y | Bucket name |
| objectKey | Path | String | Y | Object name |
| x-amz-storage-class | Header | String | N | Storage class |

### Response

```http
HTTP/1.1 200 OK
Content-Type: String
Content-Length: Long
ETag: String
Last-Modified: Timestamp
x-amz-storage-class: String
x-amz-meta-{key}: String
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
```

#### Response Header

| Field | Description |
|--------------------------|------------------------------------------------------------------------------|
| Content-Length | Size of the response body (in bytes) |
| Content-Type | Standard MIME type indicating the format of the object data |
| ETag | Unique identifier assigned by the server for a specific version of the resource |
| Last-Modified | Date and time when the object was last modified |
| x-amz-checksum-crc32 | Base64-encoded value of the object's 32-bit `CRC32` checksum |
| x-amz-checksum-crc32c | Base64-encoded value of the object's 32-bit `CRC32C` checksum |
| x-amz-checksum-crc64nvme | Base64-encoded value of the object's 64-bit `CRC64NVME` checksum |
| x-amz-checksum-sha1 | Base64-encoded value of the object's 160-bit `SHA1` digest |
| x-amz-checksum-sha256 | Base64-encoded value of the object's 256-bit `SHA256` digest |
| x-amz-checksum-sha512 | Base64-encoded value of the object's 512-bit `SHA512` digest |
| x-amz-checksum-md5 | Base64-encoded value of the object's 128-bit `MD5` digest |
| x-amz-checksum-xxhash64 | Base64-encoded value of the object's 64-bit `XXHASH64` checksum |
| x-amz-checksum-xxhash3 | Base64-encoded value of the object's 64-bit `XXHASH3` checksum |
| x-amz-checksum-xxhash128 | Base64-encoded value of the object's 128-bit `XXHASH128` checksum |
| x-amz-checksum-type | Method used to combine part-level checksums of a multipart object to generate an object-level checksum. Valid values: `COMPOSITE \| FULL_OBJECT` |


## ListObjectsV2

Retrieves an object list stored in a bucket.

### Request

```http
GET /{bucket}?list-type=2&continuation-token={continuationToken}&delimiter={delimiter}&encoding-type={encodingType}&fetch-owner={fetchOwner}&max-keys={maxKeys}&prefix={prefix}&start-after={startAfter} HTTP/1.1
```

#### Request Header

For the common header information for Data Lake Storage API, see the Data Lake Storage [API Request Header Guide](api-guide-common).

#### Request Parameter

| Name                | Category  | Type    | Required | Description                             |
|---------------------|-----------|---------|----------|-----------------------------------------|
| bucket              | Path      | String  | Y        | Bucket name                             |
| x-amz-storage-class | Header    | String  | N        | Storage class                           |
| list-type           | Parameter | String  | Y        | Fixed as 2 (V2 API identifier)          |
| continuation-token  | Parameter | String  | N        | Identifier for retrieving the next page |
| delimiter           | Parameter | String  | N        | Key group delimiter (default: /)        |
| encoding-type       | Parameter | String  | N        | Key encoding method                     |
| fetch-owner         | Parameter | Boolean | N        | Whether to include owner information    |
| max-keys            | Parameter | Integer | N        | Maximum number of objects to return     |
| prefix              | Parameter | String  | N        | Object name prefix                      |
| start-after         | Parameter | String  | N        | Starting point for retrieval            |

### Response

```http
HTTP/1.1 200 OK

<?xml version="1.0" encoding="UTF-8"?>
<ListBucketResult>
  <Name>String</Name>
  <Prefix>String/</Prefix>
  <StartAfter>String</StartAfter>
  <ContinuationToken>String</ContinuationToken>
  <NextContinuationToken>String</NextContinuationToken>
  <KeyCount>Integer</KeyCount>
  <MaxKeys>Integer</MaxKeys>
  <Delimiter>String</Delimiter>
  <IsTruncated>Boolean</IsTruncated>
  <EncodingType>String</EncodingType>
  <Contents>
    <ChecksumAlgorithm>string</ChecksumAlgorithm>
    ...
    <ChecksumType>string</ChecksumType>
    <Key>String</Key>
    <LastModified>Timestamp</LastModified>
    <ETag>String</ETag>
    <Size>Long</Size>
    <StorageClass>String</StorageClass>
    <Owner>
      <ID>String</ID>
    </Owner>
  </Contents>
  <CommonPrefixes>
    <Prefix>String</Prefix>
  </CommonPrefixes>
</ListBucketResult>
```

#### Response Body

| Name                                        | Type      | Description                                                    |
|---------------------------------------------|-----------|----------------------------------------------------------------|
| ListBucketResult                            | Object    | Result of object list retrieval                                |
| ListBucketResult.Name                       | String    | Bucket name                                                    |
| ListBucketResult.Prefix                     | String    | Object name prefix specified in the request                    |
| ListBucketResult.StartAfter                 | String    | Starting point for retrieval specified in the request          |
| ListBucketResult.ContinuationToken          | String    | Page retrieval identifier used the current request             |
| ListBucketResult.NextContinuationToken      | String    | Page retrieval identifier to use when requesting the next page |
| ListBucketResult.KeyCount                   | Integer   | Number of objects in the response                              |
| ListBucketResult.MaxKeys                    | Integer   | Maximum number of objects specified in the request             |
| ListBucketResult.Delimiter                  | String    | Key group delimiter specified in the request                   |
| ListBucketResult.IsTruncated                | Boolean   | Whether additional pages exist                                 |
| ListBucketResult.Contents                   | Array     | Object list                                                    |
| ListBucketResult.Contents.ChecksumAlgorithm | Array     | Algorithm used to generate the object's checksum |
| ListBucketResult.Contents.ChecksumType      | String    | Method used to calculate the object's checksum value |
| ListBucketResult.Contents.Key               | String    | Object key                                                     |
| ListBucketResult.Contents.LastModified      | Timestamp | Last modified time (in ISO 8601 format)                        |
| ListBucketResult.Contents.ETag              | String    | Unique identifier of the object                                |
| ListBucketResult.Contents.Size              | Long      | Object size (bytes)                                            |
| ListBucketResult.Contents.StorageClass      | String    | Storage class                                                  |
| ListBucketResult.Contents.Owner.ID          | String    | Owner ID (included when fetch-owner=true)                      |
| ListBucketResult.CommonPrefixes             | Array     | List of common prefixes grouped by delimiter                   |
| ListBucketResult.CommonPrefixes.Prefix      | String    | Path grouped by delimiter                                      |


## PutObject

Stores an object in a bucket.

### Request

```http
PUT /{bucket}/{objectKey} HTTP/1.1
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

Body
```

#### Request Header

For the common header information for Data Lake Storage API, see the Data Lake Storage [API Request Header Guide](api-guide-common).

| Field | Required | Description |
|------------------------------|-------|---------------------------------------------------------------------------------------------------------------------------------------------------|
| Content-Length | N | Size of the request body (in bytes). Used when the size of the body cannot be determined automatically. |
| Content-MD5 | N | Base64-encoded value of the message body calculated as a 128-bit `MD5` digest in accordance with RFC 1864. Can be used for data integrity verification. Although not required, it is recommended as a means of end-to-end integrity verification. |
| x-amz-checksum-crc32 | N | Base64-encoded value of the object's 32-bit `CRC32` checksum |
| x-amz-checksum-crc32c | N | Base64-encoded value of the object's 32-bit `CRC32C` checksum |
| x-amz-checksum-crc64nvme | N | Base64-encoded value of the object's 64-bit `CRC64NVME` checksum |
| x-amz-checksum-sha1 | N | Base64-encoded value of the object's 160-bit `SHA1` digest |
| x-amz-checksum-sha256 | N | Base64-encoded value of the object's 256-bit `SHA256` digest |
| x-amz-checksum-sha512 | N | Base64-encoded value of the object's 512-bit `SHA512` digest |
| x-amz-checksum-md5 | N | Base64-encoded value of the object's 128-bit `MD5` digest |
| x-amz-checksum-xxhash64 | N | Base64-encoded value of the object's 64-bit `XXHASH64` checksum |
| x-amz-checksum-xxhash3 | N | Base64-encoded value of the object's 64-bit `XXHASH3` checksum |
| x-amz-checksum-xxhash128 | N | Base64-encoded value of the object's 128-bit `XXHASH128` checksum |
| x-amz-sdk-checksum-algorithm | N | Specifies the algorithm used to generate the object's checksum via the SDK. When this header is sent, the `x-amz-checksum-algorithm` or `x-amz-trailer` header must also be sent. Otherwise, an HTTP 400 error is returned. |

#### Request Parameter

| Name | Category | Type | Required | Description |
| --- | --- | --- | --- | --- |
| bucket | Path | String | Y | Bucket name |
| objectKey | Path | String | Y | Object name |
| Content-Type | Header | String | N | Object content type |
| Content-Length | Header | Long | Y | Object size (bytes) |
| x-amz-storage-class | Header | String | N | Storage class |
| x-amz-meta-* | Header | String | N | Custom metadata |

#### Request Body

| Name | Type | Required | Description |
| --- | --- | --- | --- |
| Body | Binary | Y | Object data |

### Response

```http
HTTP/1.1 200 OK
ETag: String
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
```

#### Response Header

| Field | Description |
|--------------------------|--------------------------------------------------------------------------------------------------------------------------------|
| ETag | Entity tag of the uploaded object. If the ETag is the MD5 digest of the object, data integrity can be verified by comparing the MD5 value calculated during upload with the returned ETag. |
| x-amz-checksum-crc32 | Base64-encoded value of the object's 32-bit `CRC32` checksum |
| x-amz-checksum-crc32c | Base64-encoded value of the object's 32-bit `CRC32C` checksum |
| x-amz-checksum-crc64nvme | Base64-encoded value of the object's 64-bit `CRC64NVME` checksum. Included in the response when the object was uploaded using the `CRC64NVME` algorithm, or when a default checksum (`CRC64NVME`) was automatically added to an object uploaded without a checksum. |
| x-amz-checksum-sha1 | Base64-encoded value of the object's 160-bit `SHA1` digest |
| x-amz-checksum-sha256 | Base64-encoded value of the object's 256-bit `SHA256` digest |
| x-amz-checksum-sha512 | Base64-encoded value of the object's 512-bit `SHA512` digest |
| x-amz-checksum-md5 | Base64-encoded value of the object's 128-bit `MD5` digest |
| x-amz-checksum-xxhash64 | Base64-encoded value of the object's 64-bit `XXHASH64` checksum |
| x-amz-checksum-xxhash3 | Base64-encoded value of the object's 64-bit `XXHASH3` checksum |
| x-amz-checksum-xxhash128 | Base64-encoded value of the object's 128-bit `XXHASH128` checksum |
| x-amz-checksum-type | Method used to combine part-level checksums of a multipart object to generate an object-level checksum. Always returned as `FULL_OBJECT` for PutObject uploads. |
