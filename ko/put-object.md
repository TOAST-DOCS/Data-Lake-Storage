## PutObject

**Data & Analytics > Data Lake Storage > API 가이드 > Object > PutObject**

버킷에 객체를 저장합니다.

### 요청

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

#### 요청 헤더

Data Lake Storage API에서 공통으로 사용하는 헤더 정보는 Data Lake Storage [API 요청 헤더 가이드](api-guide-common)를 참고하세요.

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

#### 요청 파라미터

| 이름                  | 구분     | 타입     | 필수 | 설명           |
|---------------------|--------|--------|----|--------------|
| bucket              | Path   | String | Y  | 버킷 이름        |
| objectKey           | Path   | String | Y  | 객체 이름        |
| Content-Type        | Header | String | N  | 객체 콘텐츠 타입    |
| Content-Length      | Header | Long   | Y  | 객체 크기(bytes) |
| x-amz-storage-class | Header | String | N  | 스토리지 클래스     |
| x-amz-meta-\*       | Header | String | N  | 사용자 정의 메타데이터 |

#### 요청 본문

| 이름   | 타입     | 필수 | 설명     |
|------|--------|----|--------|
| Body | Binary | Y  | 객체 데이터 |

### 응답

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

#### 응답 헤더

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
