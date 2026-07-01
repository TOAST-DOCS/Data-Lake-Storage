## GetObject

**Data & Analytics > Data Lake Storage > API 가이드 > Object > GetObject**

버킷에 저장된 객체를 조회합니다.

### 요청

```http
GET /{bucket}/{objectKey} HTTP/1.1
```

#### 요청 헤더

Data Lake Storage API에서 공통으로 사용하는 헤더 정보는 Data Lake Storage [API 요청 헤더 가이드](api-guide-common)를 참고하세요.

#### 요청 파라미터

| 이름                  | 구분     | 타입     | 필수 | 설명         |
|---------------------|--------|--------|----|------------|
| bucket              | Path   | String | Y  | 버킷 이름      |
| objectKey           | Path   | String | Y  | 객체 이름      |
| Range               | Header | String | N  | 부분 다운로드 범위 |
| x-amz-storage-class | Header | String | N  | 스토리지 클래스   |

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
x-amz-checksum-type: ChecksumType

data
```

#### 응답 헤더

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
