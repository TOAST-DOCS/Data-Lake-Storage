## CompleteMultipartUpload

**Data & Analytics > Data Lake Storage > API 가이드 > Multipart > CompleteMultipartUpload**

업로드된 파트들을 조합하여 객체를 저장하고 멀티파트 업로드를 완료합니다.

### 요청

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

#### 요청 헤더

Data Lake Storage API에서 공통으로 사용하는 헤더 정보는 Data Lake Storage [API 요청 헤더 가이드](api-guide-common)를 참고하세요.

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

#### 요청 파라미터

| 이름                  | 구분        | 타입     | 필수 | 설명          |
|---------------------|-----------|--------|----|-------------|
| bucket              | Path      | String | Y  | 버킷 이름       |
| objectKey           | Path      | String | Y  | 객체 이름       |
| x-amz-storage-class | Header    | String | N  | 스토리지 클래스    |
| uploadId            | Parameter | String | Y  | 멀티파트 업로드 ID |

#### 요청 본문

| 이름                                             | 타입      | 필수 | 설명                                          |
|------------------------------------------------|---------|----|---------------------------------------------|
| CompleteMultipartUpload                        | Object  | Y  | 멀티파트 업로드 완료 요청                              |
| CompleteMultipartUpload.Part                   | Object  | N  | 파트 목록                                       |
| CompleteMultipartUpload.Part.PartNumber        | Integer | N  | 파트 번호                                       |
| CompleteMultipartUpload.Part.ETag              | String  | N  | 객체 고유 식별자                                   |
| CompleteMultipartUpload.Part.ChecksumCRC32     | String  | N  | 파트의 32비트 `CRC32` 체크섬 값을 Base64로 인코딩한 값      |
| CompleteMultipartUpload.Part.ChecksumCRC32C    | String  | N  | 파트의 32비트 `CRC32C` 체크섬 값을 Base64로 인코딩한 값     |
| CompleteMultipartUpload.Part.ChecksumCRC64NVME | String  | N  | 파트의 64비트 `CRC64NVME` 체크섬 값을 Base64로 인코딩한 값  |
| CompleteMultipartUpload.Part.ChecksumSHA1      | String  | N  | 파트의 160비트 `SHA1` 체크섬 값을 Base64로 인코딩한 값      |
| CompleteMultipartUpload.Part.ChecksumSHA256    | String  | N  | 파트의 256비트 `SHA256` 체크섬 값을 Base64로 인코딩한 값    |
| CompleteMultipartUpload.Part.ChecksumSHA512    | String  | N  | 파트의 512비트 `SHA512` 체크섬 값을 Base64로 인코딩한 값    |
| CompleteMultipartUpload.Part.ChecksumMD5       | String  | N  | 파트의 128비트 `MD5` 체크섬 값을 Base64로 인코딩한 값       |
| CompleteMultipartUpload.Part.ChecksumXXHASH64  | String  | N  | 파트의 64비트 `XXHASH64` 체크섬 값을 Base64로 인코딩한 값   |
| CompleteMultipartUpload.Part.ChecksumXXHASH3   | String  | N  | 파트의 64비트 `XXHASH3` 체크섬 값을 Base64로 인코딩한 값    |
| CompleteMultipartUpload.Part.ChecksumXXHASH128 | String  | N  | 파트의 128비트 `XXHASH128` 체크섬 값을 Base64로 인코딩한 값 |

### 응답

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

#### 응답 본문

| 이름                                              | 타입     | 설명                                           |
|-------------------------------------------------|--------|----------------------------------------------|
| CompleteMultipartUploadResult                   | Object | 멀티파트 업로드 완료 결과                               |
| CompleteMultipartUploadResult.Location          | String | 생성된 객체 경로                                    |
| CompleteMultipartUploadResult.Bucket            | String | 대상 버킷 이름                                     |
| CompleteMultipartUploadResult.Key               | String | 생성된 객체 키                                     |
| CompleteMultipartUploadResult.ETag              | String | 최종 결합된 객체 고유 식별자                             |
| CompleteMultipartUploadResult.ChecksumCRC32     | String | 파트의 32비트 `CRC32` 체크섬 값을 Base64로 인코딩한 값       |
| CompleteMultipartUploadResult.ChecksumCRC32C    | String | 파트의 32비트 `CRC32C` 체크섬 값을 Base64로 인코딩한 값      |
| CompleteMultipartUploadResult.ChecksumCRC64NVME | String | 파트의 64비트 `CRC64NVME` 체크섬 값을 Base64로 인코딩한 값   |
| CompleteMultipartUploadResult.ChecksumSHA1      | String | 파트의 160비트 `SHA1` 체크섬 값을 Base64로 인코딩하여 인코딩한 값 |
| CompleteMultipartUploadResult.ChecksumSHA256    | String | 파트의 256비트 `SHA256` 체크섬 값을 Base64로 인코딩한 값     |
| CompleteMultipartUploadResult.ChecksumSHA512    | String | 파트의 512비트 `SHA512` 체크섬 값을 Base64로 인코딩한 값     |
| CompleteMultipartUploadResult.ChecksumMD5       | String | 파트의 128비트 `MD5` 체크섬 값을 Base64로 인코딩한 값        |
| CompleteMultipartUploadResult.ChecksumXXHASH64  | String | 파트의 64비트 `XXHASH64` 체크섬 값을 Base64로 인코딩한 값    |
| CompleteMultipartUploadResult.ChecksumXXHASH3   | String | 파트의 64비트 `XXHASH3` 체크섬 값을 Base64로 인코딩한 값     |
| CompleteMultipartUploadResult.ChecksumXXHASH128 | String | 파트의 128비트 `XXHASH128` 체크섬 값을 Base64로 인코딩한 값  |
