## UploadPart

**Data & Analytics > Data Lake Storage > API 가이드 > Multipart > UploadPart**

멀티파트 업로드의 파트를 업로드합니다. 업로드하기 전 CreateMultipartUpload API를 호출하여 멀티파트 업로드 ID를 생성해야 합니다.

### 요청

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

#### 요청 헤더

Data Lake Storage API에서 공통으로 사용하는 헤더 정보는 Data Lake Storage [API 요청 헤더 가이드](api-guide-common)를 참고하세요.

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

#### 요청 파라미터

| 이름                  | 구분        | 타입      | 필수 | 설명                 |
|---------------------|-----------|---------|----|--------------------|
| bucket              | Path      | String  | Y  | 버킷 이름              |
| objectKey           | Path      | String  | Y  | 객체 이름              |
| x-amz-storage-class | Header    | String  | N  | 스토리지 클래스           |
| partNumber          | Parameter | Integer | Y  | 파트 번호(1 \~ 10,000) |
| uploadId            | Parameter | String  | Y  | 멀티파트 업로드 ID        |

#### 요청 본문

| 이름   | 타입     | 필수 | 설명                            |
|------|--------|----|-------------------------------|
| Body | Binary | Y  | 파트 바이너리 데이터, 최대 5GiB까지 업로드 가능 |

### 응답

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

#### 응답 헤더

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
