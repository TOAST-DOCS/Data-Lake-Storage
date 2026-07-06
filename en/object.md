## Object
**Data & Analytics > Data Lake Storage > Amazon S3 호환 API 가이드 > Object**


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

| 필드                       | 설명                                                                               |
|--------------------------|----------------------------------------------------------------------------------|
| ETag                     | 특정 버전의 리소스에 대해 서버가 할당하는 고유 식별자                                                   |
| x-amz-checksum-crc32     | 오브젝트의 32비트 `CRC32` 체크섬 값을 Base64로 인코딩한 값                                         |
| x-amz-checksum-crc32c    | 오브젝트의 32비트 `CRC32C` 체크섬 값을 Base64로 인코딩한 값                                        |
| x-amz-checksum-crc64nvme | 오브젝트의 64비트 `CRC64NVME` 체크섬 값을 Base64로 인코딩한 값                                     |
| x-amz-checksum-sha1      | 오브젝트의 160비트 `SHA1` 다이제스트 값을 Base64로 인코딩한 값.                                      |
| x-amz-checksum-sha256    | 오브젝트의 256비트 `SHA256` 다이제스트 값을 Base64로 인코딩한 값                                     |
| x-amz-checksum-sha512    | 오브젝트의 512비트 `SHA512` 다이제스트 값을 Base64로 인코딩한 값                                     |
| x-amz-checksum-md5       | 오브젝트의 128비트 `MD5` 다이제스트 값을 Base64로 인코딩한 값                                        |
| x-amz-checksum-xxhash64  | 오브젝트의 64비트 `XXHASH64` 체크섬 값을 Base64로 인코딩한 값                                      |
| x-amz-checksum-xxhash3   | 오브젝트의 64비트 `XXHASH3` 체크섬 값을 Base64로 인코딩한 값                                       |
| x-amz-checksum-xxhash128 | 오브젝트의 128비트 `XXHASH128` 체크섬 값을 Base64로 인코딩한 값                                    |
| x-amz-checksum-type      | 멀티파트 오브젝트의 파트별 체크섬을 결합하여 오브젝트 수준의 체크섬을 생성한 방식. 유효한 값: `COMPOSITE \| FULL_OBJECT` |


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

| 필드                       | 설명                                                                               |
|--------------------------|----------------------------------------------------------------------------------|
| Content-Length           | 응답 본문의 크기(바이트)                                                                   |
| Content-Type             | 오브젝트 데이터의 형식을 나타내는 표준 MIME 타입                                                    |
| ETag                     | 특정 버전의 리소스에 대해 서버가 할당하는 고유 식별자.                                                  |
| Last-Modified            | 오브젝트가 마지막으로 수정된 날짜 및 시간                                                          |
| x-amz-checksum-crc32     | 오브젝트의 32비트 `CRC32` 체크섬 값을 Base64로 인코딩한 값                                         |
| x-amz-checksum-crc32c    | 오브젝트의 32비트 `CRC32C` 체크섬 값을 Base64로 인코딩한 값                                        |
| x-amz-checksum-crc64nvme | 오브젝트의 64비트 `CRC64NVME` 체크섬 값을 Base64로 인코딩한 값                                     |
| x-amz-checksum-sha1      | 오브젝트의 160비트 `SHA1` 다이제스트 값을 Base64로 인코딩한 값                                       |
| x-amz-checksum-sha256    | 오브젝트의 256비트 `SHA256` 다이제스트 값을 Base64로 인코딩한 값                                     |
| x-amz-checksum-sha512    | 오브젝트의 512비트 `SHA512` 다이제스트 값을 Base64로 인코딩한 값                                     |
| x-amz-checksum-md5       | 오브젝트의 128비트 `MD5` 다이제스트 값을 Base64로 인코딩한 값                                        |
| x-amz-checksum-xxhash64  | 오브젝트의 64비트 `XXHASH64` 체크섬 값을 Base64로 인코딩한 값                                      |
| x-amz-checksum-xxhash3   | 오브젝트의 64비트 `XXHASH3` 체크섬 값을 Base64로 인코딩한 값                                       |
| x-amz-checksum-xxhash128 | 오브젝트의 128비트 `XXHASH128` 체크섬 값을 Base64로 인코딩한 값                                    |
| x-amz-checksum-type      | 멀티파트 오브젝트의 파트별 체크섬을 결합하여 오브젝트 수준의 체크섬을 생성한 방식. 유효한 값: `COMPOSITE \| FULL_OBJECT` |


## ListObjectsV2

Retrieves an object list stored in a bucket.

### Request

```http
GET /{bucket}?list-type=2&continuation-token={continuationToken}&delimiter={delimiter}&encoding-type={encodingType}&fetch-owner={fetchOwner}&max-keys={maxKeys}&prefix={prefix}&start-after={startAfter} HTTP/1.1
```

#### Request Header

For the common header information for Data Lake Storage API, see the Data Lake
Storage [API Request Header Guide](api-guide-common).

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
| ListBucketResult.Contents.ChecksumAlgorithm | Array     | 오브젝트의 체크섬 생성에 사용된 알고리즘                                         |
| ListBucketResult.Contents.ChecksumType      | String    | 오브젝트의 체크섬 값을 계산하는 방식                                           |
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

| 필드                           | 필수 여부 | 설명                                                                                                                                                 |
|------------------------------|-------|----------------------------------------------------------------------------------------------------------------------------------------------------|
| Content-Length               | N     | 요청 본문의 크기(바이트)입니다. 본문의 크기를 자동으로 결정할 수 없는 경우 사용.                                                                                                    |
| Content-MD5                  | N     | RFC 1864에 따라 메시지 본문을 128비트 `MD5` 다이제스트로 계산한 후 Base64로 인코딩한 값. 데이터 무결성 검증을 위해 사용할 수 있으며, 필수는 아니지만 종단 간 무결성 검증 수단으로 사용하는 것을 권장                       |
| x-amz-checksum-crc32         | N     | 오브젝트의 32비트 `CRC32` 체크섬 값을 Base64로 인코딩한 값                                                                                                           |
| x-amz-checksum-crc32c        | N     | 오브젝트의 32비트 `CRC32C` 체크섬 값을 Base64로 인코딩한 값 .                                                                                                        |
| x-amz-checksum-crc64nvme     | N     | 오브젝트의 64비트 `CRC64NVME` 체크섬 값을 Base64로 인코딩한 값                                                                                                       |
| x-amz-checksum-sha1          | N     | 오브젝트의 160비트 `SHA1` 다이제스트 값을 Base64로 인코딩한 값                                                                                                         |
| x-amz-checksum-sha256        | N     | 오브젝트의 256비트 `SHA256` 다이제스트 값을 Base64로 인코딩한 값                                                                                                       |
| x-amz-checksum-sha512        | N     | 오브젝트의 512비트 `SHA512` 다이제스트 값을 Base64로 인코딩한 값                                                                                                       |
| x-amz-checksum-md5           | N     | 오브젝트의 128비트 `MD5` 다이제스트 값을 Base64로 인코딩한 값                                                                                                          |
| x-amz-checksum-xxhash64      | N     | 오브젝트의 64비트 `XXHASH64` 체크섬 값을 Base64로 인코딩한 값                                                                                                        |
| x-amz-checksum-xxhash3       | N     | 오브젝트의 64비트 `XXHASH3` 체크섬 값을 Base64로 인코딩한 값                                                                                                         |
| x-amz-checksum-xxhash128     | N     | 오브젝트의 128비트 `XXHASH128` 체크섬 값을 Base64로 인코딩한 값                                                                                                      |
| x-amz-sdk-checksum-algorithm | N     | SDK를 사용하여 오브젝트의 체크섬을 생성할 때 사용한 알고리즘을 지정. 이 헤더를 전송할 경우 반드시 `x-amz-checksum-algorithm` 또는 `x-amz-trailer` 헤더를 함께 전송해야 하며, 그렇지 않을 경우 HTTP 400 오류가 반환됨 |

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

| 필드                       | 설명                                                                                                                                  |
|--------------------------|-------------------------------------------------------------------------------------------------------------------------------------|
| ETag                     | 업로드된 오브젝트의 엔티티 태그입. ETag가 오브젝트의 MD5 다이제스트인 경우, 오브젝트 업로드 시 계산한 MD5 값과 반환된 ETag를 비교하여 데이터 무결성을 확인 가능                                  |
| x-amz-checksum-crc32     | 오브젝트의 32비트 `CRC32` 체크섬 값을 Base64로 인코딩한 값                                                                                            |
| x-amz-checksum-crc32c    | 오브젝트의 32비트 `CRC32C` 체크섬 값을 Base64로 인코딩한 값                                                                                           |
| x-amz-checksum-crc64nvme | 오브젝트의 64비트 `CRC64NVME` 체크섬 값을 Base64로 인코딩한 값. `CRC64NVME` 알고리즘으로 업로드되었거나, 체크섬 없이 업로드되어 기본 체크섬(`CRC64NVME`)이 자동으로 추가된 경우에 응답에 포함됩니다. |
| x-amz-checksum-sha1      | 오브젝트의 160비트 `SHA1` 다이제스트 값을 Base64로 인코딩한 값                                                                                          |
| x-amz-checksum-sha256    | 오브젝트의 256비트 `SHA256` 다이제스트 값을 Base64로 인코딩한 값                                                                                        |
| x-amz-checksum-sha512    | 오브젝트의 512비트 `SHA512` 다이제스트 값을 Base64로 인코딩한 값                                                                                        |
| x-amz-checksum-md5       | 오브젝트의 128비트 `MD5` 다이제스트 값을 Base64로 인코딩한 값                                                                                           |
| x-amz-checksum-xxhash64  | 오브젝트의 64비트 `XXHASH64` 체크섬 값을 Base64로 인코딩한 값                                                                                         |
| x-amz-checksum-xxhash3   | 오브젝트의 64비트 `XXHASH3` 체크섬 값을 Base64로 인코딩한 값                                                                                          |
| x-amz-checksum-xxhash128 | 오브젝트의 128비트 `XXHASH128` 체크섬 값을 Base64로 인코딩한 값                                                                                       |
| x-amz-checksum-type      | 멀티파트 오브젝트의 파트별 체크섬을 결합하여 오브젝트 수준의 체크섬을 생성한 방식. PutObject 업로드의 경우 항상 `FULL_OBJECT`로 반환됨                                              |
