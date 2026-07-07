## Object
**Data & Analytics > Data Lake Storage > Amazon S3 호환 API 가이드 > Object**


## DeleteObject

버킷에 저장된 객체를 삭제합니다.

### 요청

```http
DELETE /{bucket}/{objectKey} HTTP/1.1
```

#### 요청 헤더

Data Lake Storage API에서 공통으로 사용하는 헤더 정보는 Data Lake Storage [API 요청 헤더 가이드](api-guide-common)를 참고하세요.

#### 요청 파라미터

| 이름 | 구분 | 타입 | 필수 | 설명 |
| --- | --- | --- | --- | --- |
| bucket | Path | String | Y | 버킷 이름 |
| objectKey | Path | String | Y | 객체 이름 |

### 응답

```http
HTTP/1.1 204 No Content
```


## DeleteObjects

하나의 요청으로 여러 객체를 삭제합니다. 한 번의 요청에 최대 1,000개의 객체 키를 지정할 수 있습니다.

### 요청

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

#### 요청 헤더

Data Lake Storage API에서 공통으로 사용하는 헤더 정보는 Data Lake Storage [API 요청 헤더 가이드](api-guide-common)를 참고하세요.

#### 요청 파라미터

| 이름 | 구분 | 타입 | 필수 | 설명 |
| --- | --- | --- | --- | --- |
| bucket | Path | String | Y | 버킷 이름 |
| Content-MD5 | Header | String | Y | 요청 본문의 MD5 해시 값(전송 중 변조 검증용) |

#### 요청 본문

| 이름 | 타입 | 필수 | 설명 |
| --- | --- | --- | --- |
| Delete | Object | Y | 요청의 루트 요소 |
| Delete.Object | Array | Y | 삭제할 객체 목록. 최대 1,000개 |
| Delete.Object.ETag | String | N | 객체의 ETag 값. 지정한 경우 ETag가 일치하는 객체만 삭제 |
| Delete.Object.Key | String | Y | 삭제할 객체 키 |
| Delete.Object.LastModifiedTime | String | N | 객체의 최종 수정 시간. 지정한 경우 해당 시간과 일치하는 객체만 삭제 |
| Delete.Object.Size | Long | N | 객체의 크기(바이트). 지정한 경우 해당 크기와 일치하는 객체만 삭제 |
| Delete.Quiet | Boolean | N | `true`로 설정하면 Quiet 모드로 동작하며, 실패한 항목만 응답에 포함. 기본값은 Verbose 모드(전체 결과 반환) |

### 응답

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

#### 응답 본문

| 이름 | 타입 | 설명 |
| --- | --- | --- |
| DeleteResult | Object | 삭제 결과의 루트 요소 |
| DeleteResult.Deleted | Array | 삭제에 성공한 객체 목록 |
| DeleteResult.Deleted.Key | String | 삭제된 객체 키 |
| DeleteResult.Error | Array | 삭제에 실패한 객체 목록 |
| DeleteResult.Error.Key | String | 삭제에 실패한 객체 키 |
| DeleteResult.Error.Code | String | 오류 코드 |
| DeleteResult.Error.Message | String | 오류 메시지 |


## GetObject

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

| 필드                       | 설명                                                                          |
|--------------------------|-----------------------------------------------------------------------------|
| ETag                     | 특정 버전의 리소스에 서버가 할당하는 고유 식별자                                                 |
| x-amz-checksum-crc32     | 객체의 32비트 `CRC32` 체크섬 값을 Base64로 인코딩한 값                                      |
| x-amz-checksum-crc32c    | 객체의 32비트 `CRC32C` 체크섬 값을 Base64로 인코딩한 값                                     |
| x-amz-checksum-crc64nvme | 객체의 64비트 `CRC64NVME` 체크섬 값을 Base64로 인코딩한 값                                  |
| x-amz-checksum-sha1      | 객체의 160비트 `SHA1` 다이제스트 값을 Base64로 인코딩한 값                                    |
| x-amz-checksum-sha256    | 객체의 256비트 `SHA256` 다이제스트 값을 Base64로 인코딩한 값                                  |
| x-amz-checksum-sha512    | 객체의 512비트 `SHA512` 다이제스트 값을 Base64로 인코딩한 값                                  |
| x-amz-checksum-md5       | 객체의 128비트 `MD5` 다이제스트 값을 Base64로 인코딩한 값                                     |
| x-amz-checksum-xxhash64  | 객체의 64비트 `XXHASH64` 체크섬 값을 Base64로 인코딩한 값                                   |
| x-amz-checksum-xxhash3   | 객체의 64비트 `XXHASH3` 체크섬 값을 Base64로 인코딩한 값                                    |
| x-amz-checksum-xxhash128 | 객체의 128비트 `XXHASH128` 체크섬 값을 Base64로 인코딩한 값                                 |
| x-amz-checksum-type      | 멀티파트 객체의 파트별 체크섬을 결합하여 객체 수준의 체크섬을 생성한 방식. 유효한 값: `COMPOSITE \| FULL_OBJECT` |


## HeadObject

버킷에 저장된 객체의 메타데이터를 조회합니다.

### 요청

```http
HEAD /{bucket}/{objectKey} HTTP/1.1
```

#### 요청 헤더

Data Lake Storage API에서 공통으로 사용하는 헤더 정보는 Data Lake Storage [API 요청 헤더 가이드](api-guide-common)를 참고하세요.

#### 요청 파라미터

| 이름                  | 구분     | 타입     | 필수 | 설명       |
|---------------------|--------|--------|----|----------|
| bucket              | Path   | String | Y  | 버킷 이름    |
| objectKey           | Path   | String | Y  | 객체 이름    |
| x-amz-storage-class | Header | String | N  | 스토리지 클래스 |

### 응답

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

#### 응답 헤더

| 필드                       | 설명                                                                           |
|--------------------------|------------------------------------------------------------------------------|
| Content-Length           | 응답 본문의 크기(바이트)                                                               |
| Content-Type             | 객체 데이터의 형식을 나타내는 표준 MIME 타입                                                  |
| ETag                     | 특정 버전의 리소스에 서버가 할당하는 고유 식별자                                                  |
| Last-Modified            | 객체가 마지막으로 수정된 날짜 및 시간                                                        |
| x-amz-checksum-crc32     | 객체의 32비트 `CRC32` 체크섬 값을 Base64로 인코딩한 값                                       |
| x-amz-checksum-crc32c    | 객체의 32비트 `CRC32C` 체크섬 값을 Base64로 인코딩한 값                                      |
| x-amz-checksum-crc64nvme | 객체의 64비트 `CRC64NVME` 체크섬 값을 Base64로 인코딩한 값                                   |
| x-amz-checksum-sha1      | 객체의 160비트 `SHA1` 다이제스트 값을 Base64로 인코딩한 값                                     |
| x-amz-checksum-sha256    | 객체의 256비트 `SHA256` 다이제스트 값을 Base64로 인코딩한 값                                   |
| x-amz-checksum-sha512    | 객체의 512비트 `SHA512` 다이제스트 값을 Base64로 인코딩한 값                                   |
| x-amz-checksum-md5       | 객체의 128비트 `MD5` 다이제스트 값을 Base64로 인코딩한 값                                      |
| x-amz-checksum-xxhash64  | 객체의 64비트 `XXHASH64` 체크섬 값을 Base64로 인코딩한 값                                    |
| x-amz-checksum-xxhash3   | 객체의 64비트 `XXHASH3` 체크섬 값을 Base64로 인코딩한 값                                     |
| x-amz-checksum-xxhash128 | 객체의 128비트 `XXHASH128` 체크섬 값을 Base64로 인코딩한 값                                  |
| x-amz-checksum-type      | 멀티파트 객체의 파트별 체크섬을 결합하여 객체 수준의 체크섬을 생성한 방식. 유효한 값: `COMPOSITE \| FULL_OBJECT` |


## ListObjectsV2

버킷에 저장된 객체 목록을 조회합니다.

### 요청

```http
GET /{bucket}?list-type=2&continuation-token={continuationToken}&delimiter={delimiter}&encoding-type={encodingType}&fetch-owner={fetchOwner}&max-keys={maxKeys}&prefix={prefix}&start-after={startAfter} HTTP/1.1
```

#### 요청 헤더

Data Lake Storage API에서 공통으로 사용하는 헤더 정보는 Data Lake Storage [API 요청 헤더 가이드](api-guide-common)를 참고하세요.

#### 요청 파라미터

| 이름                  | 구분        | 타입      | 필수 | 설명               |
|---------------------|-----------|---------|----|------------------|
| bucket              | Path      | String  | Y  | 버킷 이름            |
| x-amz-storage-class | Header    | String  | N  | 스토리지 클래스         |
| list-type           | Parameter | String  | Y  | 2 고정(V2 API 구분자) |
| continuation-token  | Parameter | String  | N  | 다음 페이지 조회 식별자    |
| delimiter           | Parameter | String  | N  | 키 그룹 구분자(기본: /)  |
| encoding-type       | Parameter | String  | N  | 키 인코딩 방식         |
| fetch-owner         | Parameter | Boolean | N  | 소유자 정보 포함 여부     |
| max-keys            | Parameter | Integer | N  | 반환할 최대 객체 수      |
| prefix              | Parameter | String  | N  | 객체 이름 접두어        |
| start-after         | Parameter | String  | N  | 조회 시작 기준         |

### 응답

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

#### 응답 본문

| 이름                                          | 타입        | 설명                               |
|---------------------------------------------|-----------|----------------------------------|
| ListBucketResult                            | Object    | 객체 목록 조회 결과                      |
| ListBucketResult.Name                       | String    | 버킷 이름                            |
| ListBucketResult.Prefix                     | String    | 요청 시 지정한 객체 이름 접두어               |
| ListBucketResult.StartAfter                 | String    | 요청 시 지정한 조회 시작 기준                |
| ListBucketResult.ContinuationToken          | String    | 이번 요청에 사용된 페이지 조회 식별자            |
| ListBucketResult.NextContinuationToken      | String    | 다음 페이지 요청 시 사용할 페이지 조회 식별자       |
| ListBucketResult.KeyCount                   | Integer   | 응답 객체 수                          |
| ListBucketResult.MaxKeys                    | Integer   | 요청 시 지정한 최대 객체 수                 |
| ListBucketResult.Delimiter                  | String    | 요청 시 지정한 키 그룹 구분자                |
| ListBucketResult.IsTruncated                | Boolean   | 추가 페이지 존재 여부                     |
| ListBucketResult.Contents                   | Array     | 객체 목록                            |
| ListBucketResult.Contents.ChecksumAlgorithm | Array     | 객체의 체크섬 생성에 사용된 알고리즘             |
| ListBucketResult.Contents.ChecksumType      | String    | 객체의 체크섬 값을 계산하는 방식               |
| ListBucketResult.Contents.Key               | String    | 객체 키                             |
| ListBucketResult.Contents.LastModified      | Timestamp | 마지막 수정 일시 (ISO 8601 형식)          |
| ListBucketResult.Contents.ETag              | String    | 객체 고유 식별자                        |
| ListBucketResult.Contents.Size              | Long      | 객체 크기(바이트)                       |
| ListBucketResult.Contents.StorageClass      | String    | 스토리지 클래스                         |
| ListBucketResult.Contents.Owner.ID          | String    | 소유자 ID (fetch-owner=true 시 포함)   |
| ListBucketResult.CommonPrefixes             | Array     | delimiter 기준으로 그룹화된 공통 prefix 목록 |
| ListBucketResult.CommonPrefixes.Prefix      | String    | delimiter 기준으로 그룹화된 경로           |


## PutObject

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

| 필드                           | 필수 여부 | 설명                                                                                                                                               |
|------------------------------|-------|--------------------------------------------------------------------------------------------------------------------------------------------------|
| Content-Length               | N     | 요청 본문의 크기(바이트)입니다. 본문의 크기를 자동으로 결정할 수 없는 경우 사용.                                                                                                  |
| Content-MD5                  | N     | RFC 1864에 따라 메시지 본문을 128비트 `MD5` 다이제스트로 계산한 후 Base64로 인코딩한 값. 데이터 무결성 검증을 위해 사용할 수 있으며, 필수는 아니지만 종단 간 무결성 검증 수단으로 사용하는 것을 권장                     |
| x-amz-checksum-crc32         | N     | 객체의 32비트 `CRC32` 체크섬 값을 Base64로 인코딩한 값                                                                                                         |
| x-amz-checksum-crc32c        | N     | 객체의 32비트 `CRC32C` 체크섬 값을 Base64로 인코딩한 값                                                                                                        |
| x-amz-checksum-crc64nvme     | N     | 객체의 64비트 `CRC64NVME` 체크섬 값을 Base64로 인코딩한 값                                                                                                     |
| x-amz-checksum-sha1          | N     | 객체의 160비트 `SHA1` 다이제스트 값을 Base64로 인코딩한 값                                                                                                       |
| x-amz-checksum-sha256        | N     | 객체의 256비트 `SHA256` 다이제스트 값을 Base64로 인코딩한 값                                                                                                     |
| x-amz-checksum-sha512        | N     | 객체의 512비트 `SHA512` 다이제스트 값을 Base64로 인코딩한 값                                                                                                     |
| x-amz-checksum-md5           | N     | 객체의 128비트 `MD5` 다이제스트 값을 Base64로 인코딩한 값                                                                                                        |
| x-amz-checksum-xxhash64      | N     | 객체의 64비트 `XXHASH64` 체크섬 값을 Base64로 인코딩한 값                                                                                                      |
| x-amz-checksum-xxhash3       | N     | 객체의 64비트 `XXHASH3` 체크섬 값을 Base64로 인코딩한 값                                                                                                       |
| x-amz-checksum-xxhash128     | N     | 객체의 128비트 `XXHASH128` 체크섬 값을 Base64로 인코딩한 값                                                                                                    |
| x-amz-sdk-checksum-algorithm | N     | SDK를 사용하여 객체의 체크섬을 생성할 때 사용한 알고리즘을 지정. 이 헤더를 전송할 경우 반드시 `x-amz-checksum-algorithm` 또는 `x-amz-trailer` 헤더를 함께 전송해야 하며, 그렇지 않을 경우 HTTP 400 오류가 반환됨 |

#### 요청 파라미터

| 이름                  | 구분     | 타입     | 필수 | 설명           |
|---------------------|--------|--------|----|--------------|
| bucket              | Path   | String | Y  | 버킷 이름        |
| objectKey           | Path   | String | Y  | 객체 이름        |
| Content-Type        | Header | String | N  | 객체 콘텐츠 타입    |
| Content-Length      | Header | Long   | Y  | 객체 크기(바이트)   |
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

| 필드                       | 설명                                                                                                                                 |
|--------------------------|------------------------------------------------------------------------------------------------------------------------------------|
| ETag                     | 업로드된 객체의 엔티티 태그. ETag가 객체의 MD5 다이제스트인 경우, 객체 업로드 시 계산한 MD5 값과 반환된 ETag를 비교하여 데이터 무결성을 확인 가능                                  |
| x-amz-checksum-crc32     | 객체의 32비트 `CRC32` 체크섬 값을 Base64로 인코딩한 값                                                                                           |
| x-amz-checksum-crc32c    | 객체의 32비트 `CRC32C` 체크섬 값을 Base64로 인코딩한 값                                                                                          |
| x-amz-checksum-crc64nvme | 객체의 64비트 `CRC64NVME` 체크섬 값을 Base64로 인코딩한 값. `CRC64NVME` 알고리즘으로 업로드되었거나, 체크섬 없이 업로드되어 기본 체크섬(`CRC64NVME`)이 자동으로 추가된 경우에 응답에 포함됩니다. |
| x-amz-checksum-sha1      | 객체의 160비트 `SHA1` 다이제스트 값을 Base64로 인코딩한 값                                                                                         |
| x-amz-checksum-sha256    | 객체의 256비트 `SHA256` 다이제스트 값을 Base64로 인코딩한 값                                                                                       |
| x-amz-checksum-sha512    | 객체의 512비트 `SHA512` 다이제스트 값을 Base64로 인코딩한 값                                                                                       |
| x-amz-checksum-md5       | 객체의 128비트 `MD5` 다이제스트 값을 Base64로 인코딩한 값                                                                                          |
| x-amz-checksum-xxhash64  | 객체의 64비트 `XXHASH64` 체크섬 값을 Base64로 인코딩한 값                                                                                        |
| x-amz-checksum-xxhash3   | 객체의 64비트 `XXHASH3` 체크섬 값을 Base64로 인코딩한 값                                                                                         |
| x-amz-checksum-xxhash128 | 객체의 128비트 `XXHASH128` 체크섬 값을 Base64로 인코딩한 값                                                                                      |
| x-amz-checksum-type      | 멀티파트 객체의 파트별 체크섬을 결합하여 객체 수준의 체크섬을 생성한 방식. PutObject 업로드의 경우 항상 `FULL_OBJECT`로 반환됨                                             |
