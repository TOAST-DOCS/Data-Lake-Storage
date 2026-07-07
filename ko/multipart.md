## Multipart
**Data & Analytics > Data Lake Storage > Amazon S3 호환 API 가이드 > Multipart**


## AbortMultipartUpload

진행 중인 멀티파트 업로드를 중단합니다.

### 요청

```http
DELETE /{bucket}/{objectKey}?uploadId={uploadId} HTTP/1.1
```

#### 요청 헤더

Data Lake Storage API에서 공통으로 사용하는 헤더 정보는 Data Lake Storage [API 요청 헤더 가이드](api-guide-common)를 참고하세요.

#### 요청 파라미터

| 이름 | 구분 | 타입 | 필수 | 설명 |
| --- | --- | --- | --- | --- |
| bucket | Path | String | Y | 버킷 이름 |
| objectKey | Path | String | Y | 객체 이름 |
| x-amz-storage-class | Header | String | N | 스토리지 클래스 |
| uploadId | Parameter | String | Y | 멀티파트 업로드 ID |

### 응답

```http
HTTP/1.1 204 No Content
```


## CompleteMultipartUpload

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

| 이름                       | 필수 여부 | 설명                                                                                                                                                         |
|--------------------------|-------|------------------------------------------------------------------------------------------------------------------------------------------------------------|
| x-amz-checksum-crc32     | N     | 데이터 무결성 검증을 위한 헤더. 객체의 32비트 `CRC32` 체크섬 값을 Base64로 인코딩한 값                                                                                                  |
| x-amz-checksum-crc32c    | N     | 데이터 무결성 검증을 위한 헤더. 객체의 32비트 `CRC32C` 체크섬 값을 Base64로 인코딩한 값                                                                                                 |
| x-amz-checksum-crc64nvme | N     | 데이터 무결성 검증을 위한 헤더. 객체의 64비트 `CRC64NVME` 체크섬 값을 Base64로 인코딩한 값                                                                                              |
| x-amz-checksum-sha1      | N     | 데이터 무결성 검증을 위한 헤더. 객체의 160비트 `SHA1` 다이제스트 값을 Base64로 인코딩한 값                                                                                                |
| x-amz-checksum-sha256    | N     | 데이터 무결성 검증을 위한 헤더. 객체의 256비트 `SHA256` 다이제스트 값을 Base64로 인코딩한 값                                                                                              |
| x-amz-checksum-sha512    | N     | 데이터 무결성 검증을 위한 헤더. 객체의 512비트 `SHA512` 다이제스트 값을 Base64로 인코딩한 값                                                                                              |
| x-amz-checksum-md5       | N     | 데이터 무결성 검증을 위한 헤더. 객체의 128비트 `MD5` 다이제스트 값을 Base64로 인코딩한 값                                                                                                 |
| x-amz-checksum-xxhash64  | N     | 데이터 무결성 검증을 위한 헤더. 객체의 64비트 `XXHASH64` 체크섬 값을 Base64로 인코딩한 값                                                                                               |
| x-amz-checksum-xxhash3   | N     | 데이터 무결성 검증을 위한 헤더. 객체의 64비트 `XXHASH3` 체크섬 값을 Base64로 인코딩한 값                                                                                                |
| x-amz-checksum-xxhash128 | N     | 데이터 무결성 검증을 위한 헤더. 객체의 128비트 `XXHASH128` 체크섬 값을 Base64로 인코딩한 값                                                                                             |
| x-amz-checksum-type      | N     | 멀티파트 업로드에서 파트별 체크섬을 결합하여 객체 수준의 체크섬을 생성하는 방식. `CreateMultipartUpload` 요청에서 지정한 체크섬 타입과 일치하지 않을 경우 `BadDigest` 오류가 반환됩니다. 유효한 값: `COMPOSITE \| FULL_OBJECT` |

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

| 이름                                              | 타입     | 설명                                          |
|-------------------------------------------------|--------|---------------------------------------------|
| CompleteMultipartUploadResult                   | Object | 멀티파트 업로드 완료 결과                              |
| CompleteMultipartUploadResult.Location          | String | 생성된 객체 경로                                   |
| CompleteMultipartUploadResult.Bucket            | String | 대상 버킷 이름                                    |
| CompleteMultipartUploadResult.Key               | String | 생성된 객체 키                                    |
| CompleteMultipartUploadResult.ETag              | String | 최종 결합된 객체 고유 식별자                            |
| CompleteMultipartUploadResult.ChecksumCRC32     | String | 파트의 32비트 `CRC32` 체크섬 값을 Base64로 인코딩한 값      |
| CompleteMultipartUploadResult.ChecksumCRC32C    | String | 파트의 32비트 `CRC32C` 체크섬 값을 Base64로 인코딩한 값     |
| CompleteMultipartUploadResult.ChecksumCRC64NVME | String | 파트의 64비트 `CRC64NVME` 체크섬 값을 Base64로 인코딩한 값  |
| CompleteMultipartUploadResult.ChecksumSHA1      | String | 파트의 160비트 `SHA1` 체크섬 값을 Base64로 인코딩한 값 |
| CompleteMultipartUploadResult.ChecksumSHA256    | String | 파트의 256비트 `SHA256` 체크섬 값을 Base64로 인코딩한 값    |
| CompleteMultipartUploadResult.ChecksumSHA512    | String | 파트의 512비트 `SHA512` 체크섬 값을 Base64로 인코딩한 값    |
| CompleteMultipartUploadResult.ChecksumMD5       | String | 파트의 128비트 `MD5` 체크섬 값을 Base64로 인코딩한 값       |
| CompleteMultipartUploadResult.ChecksumXXHASH64  | String | 파트의 64비트 `XXHASH64` 체크섬 값을 Base64로 인코딩한 값   |
| CompleteMultipartUploadResult.ChecksumXXHASH3   | String | 파트의 64비트 `XXHASH3` 체크섬 값을 Base64로 인코딩한 값    |
| CompleteMultipartUploadResult.ChecksumXXHASH128 | String | 파트의 128비트 `XXHASH128` 체크섬 값을 Base64로 인코딩한 값 |


## CreateMultipartUpload

대용량 객체를 업로드할 수 있도록 멀티파트 업로드를 시작하고 업로드 ID를 생성합니다. 업로드 ID는 최대 한 시간 동안 유효합니다.

### 요청

```http
POST /{bucket}/{objectKey}?uploads HTTP/1.1
x-amz-checksum-algorithm: ChecksumAlgorithm
x-amz-checksum-type: ChecksumType
```

#### 요청 헤더

Data Lake Storage API에서 공통으로 사용하는 헤더 정보는 Data Lake Storage [API 요청 헤더 가이드](api-guide-common)를 참고하세요.

| 필드                       | 필수 여부 | 설명                                                                                                                                       |
|--------------------------|-------|------------------------------------------------------------------------------------------------------------------------------------------|
| x-amz-checksum-algorithm | N     | 객체의 체크섬 생성에 사용할 알고리즘을 지정. 유효한 값: `CRC32 \| CRC32C \| SHA1 \| SHA256 \| CRC64NVME \| SHA512 \| MD5 \| XXHASH64 \| XXHASH3 \| XXHASH128` |
| x-amz-checksum-type      | N     | 객체의 체크섬 값을 계산하는 방식을 지정. 유효한 값: `COMPOSITE \| FULL_OBJECT`                                                                              |

#### 요청 파라미터

| 이름                  | 구분     | 타입     | 필수 | 설명           |
|---------------------|--------|--------|----|--------------|
| bucket              | Path   | String | Y  | 버킷 이름        |
| objectKey           | Path   | String | Y  | 객체 이름        |
| Content-Type        | Header | String | N  | 객체 콘텐츠 타입    |
| x-amz-storage-class | Header | String | N  | 스토리지 클래스     |
| x-amz-meta-\*       | Header | String | N  | 사용자 정의 메타데이터 |

### 응답

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

#### 응답 헤더

| 필드                       | 설명                       |
|--------------------------|--------------------------|
| x-amz-checksum-algorithm | 객체의 체크섬 생성에 사용한 알고리즘 값 |
| x-amz-checksum-type      | 객체의 체크섬 값을 계산한 방식      |

#### 응답 본문

| 이름                                     | 타입     | 설명             |
|----------------------------------------|--------|----------------|
| InitiateMultipartUploadResult          | Object | 멀티파트 업로드 시작 결과 |
| InitiateMultipartUploadResult.Bucket   | String | 대상 버킷 이름       |
| InitiateMultipartUploadResult.Key      | String | 객체 키           |
| InitiateMultipartUploadResult.UploadId | String | 멀티파트 업로드 ID    |


## ListParts

멀티파트 업로드의 파트 목록을 조회합니다.

### 요청

```http
GET /{bucket}/{objectKey}?uploadId={uploadId} HTTP/1.1
```

#### 요청 헤더

Data Lake Storage API에서 공통으로 사용하는 헤더 정보는 Data Lake Storage [API 요청 헤더 가이드](api-guide-common)를 참고하세요.

#### 요청 파라미터

| 이름                  | 구분        | 타입     | 필수 | 설명          |
|---------------------|-----------|--------|----|-------------|
| bucket              | Path      | String | Y  | 버킷 이름       |
| objectKey           | Path      | String | Y  | 객체 이름       |
| x-amz-storage-class | Header    | String | N  | 스토리지 클래스    |
| uploadId            | Parameter | String | Y  | 멀티파트 업로드 ID |

### 응답

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

#### 응답 본문

| 이름                                   | 타입        | 설명                     |
|--------------------------------------|-----------|------------------------|
| ListPartsResult                      | Object    | 파트 목록 조회 결과            |
| ListPartsResult.Bucket               | String    | 버킷 이름                  |
| ListPartsResult.Key                  | String    | 객체 키                   |
| ListPartsResult.UploadId             | String    | 멀티파트 업로드 ID            |
| ListPartsResult.StorageClass         | String    | 스토리지 클래스               |
| ListPartsResult.PartNumberMarker     | Integer   | 조회 시작 기준               |
| ListPartsResult.NextPartNumberMarker | Integer   | 다음 조회 시작 기준            |
| ListPartsResult.MaxParts             | Integer   | 최대 파트 수                |
| ListPartsResult.IsTruncated          | Boolean   | 추가 페이지 존재 여부           |
| ListPartsResult.Part                 | Object    | 파트 정보                  |
| ListPartsResult.Part.PartNumber      | Integer   | 파트 번호                  |
| ListPartsResult.Part.LastModified    | Timestamp | 마지막 수정 일시(ISO 8601 형식) |
| ListPartsResult.Part.ETag            | String    | 파트 객체 고유 식별자           |
| ListPartsResult.Part.Size            | Long      | 파트 크기(바이트)             |


## UploadPart

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

| 필드                       | 필수 여부 | 설명                                         |
|--------------------------|-------|--------------------------------------------|
| Content-Length           | N     | 요청 본문의 크기(바이트). 본문의 크기를 자동으로 결정할 수 없는 경우 사용 |
| Content-MD5              | N     | 파트 데이터의 128비트 `MD5` 다이제스트 값을 Base64로 인코딩한 값 |
| x-amz-checksum-crc32     | N     | 파트의 32비트 `CRC32` 체크섬 값을 Base64로 인코딩한 값     |
| x-amz-checksum-crc32c    | N     | 파트의 32비트 `CRC32C` 체크섬 값을 Base64로 인코딩한 값    |
| x-amz-checksum-crc64nvme | N     | 파트의 64비트 `CRC64NVME` 체크섬 값을 Base64로 인코딩한 값 |
| x-amz-checksum-sha1      | N     | 파트의 160비트 `SHA1` 다이제스트 값을 Base64로 인코딩한 값   |
| x-amz-checksum-sha256    | N     | 파트의 256비트 `SHA256` 다이제스트 값을 Base64로 인코딩한 값 |
| x-amz-checksum-sha512    | N     | 파트의 512비트 `SHA512` 다이제스트 값을 Base64로 인코딩한 값 |
| x-amz-checksum-md5       | N     | 파트의 128비트 `MD5` 다이제스트 값을 Base64로 인코딩한 값    |
| x-amz-checksum-xxhash64  | N     | 파트의 64비트 `XXHASH64` 체크섬 값을 Base64로 인코딩한 값  |
| x-amz-checksum-xxhash3   | N     | 파트의 64비트 `XXHASH3` 체크섬 값을 Base64로 인코딩한 값   |
| x-amz-checksum-xxhash128 | N     | 파트의 128비트 `XXHASH128` 체크섬 값을 Base64로 인코딩한 값 |

#### 요청 파라미터

| 이름                  | 구분        | 타입      | 필수 | 설명              |
|---------------------|-----------|---------|----|-----------------|
| bucket              | Path      | String  | Y  | 버킷 이름           |
| objectKey           | Path      | String  | Y  | 객체 이름           |
| x-amz-storage-class | Header    | String  | N  | 스토리지 클래스        |
| partNumber          | Parameter | Integer | Y  | 파트 번호(1~10,000) |
| uploadId            | Parameter | String  | Y  | 멀티파트 업로드 ID     |

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
