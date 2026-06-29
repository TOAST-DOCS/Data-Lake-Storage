## CompleteMultipartUpload

**Data & Analytics > Data Lake Storage > API Guide > Multipart > CompleteMultipartUpload**

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
