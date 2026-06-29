## GetObject

**Data & Analytics > Data Lake Storage > API Guide > Object > GetObject**

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
