## CreateMultipartUpload

**Data & Analytics > Data Lake Storage > API 가이드 > Multipart > CreateMultipartUpload**

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
| x-amz-checksum-algorithm | N     | 오브젝트의 체크섬 생성에 사용할 알고리즘을 지정. 유효한 값: `CRC32 \| CRC32C \| SHA1 \| SHA256 \| CRC64NVME \| SHA512 \| MD5 \| XXHASH64 \| XXHASH3 \| XXHASH128` |
| x-amz-checksum-type      | N     | 오브젝트의 체크섬 값을 계산하는 방식을 지정. 유효한 값: `COMPOSITE \| FULL_OBJECT`                                                                              |

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
| x-amz-checksum-algorithm | 오브젝트의 체크섬 생성에 사용한 알고리즘 값 |
| x-amz-checksum-type      | 오브젝트의 체크섬 값을 계산한 방식      |

#### 응답 본문

| 이름                                     | 타입     | 설명             |
|----------------------------------------|--------|----------------|
| InitiateMultipartUploadResult          | Object | 멀티파트 업로드 시작 결과 |
| InitiateMultipartUploadResult.Bucket   | String | 대상 버킷 이름       |
| InitiateMultipartUploadResult.Key      | String | 객체 키           |
| InitiateMultipartUploadResult.UploadId | String | 멀티파트 업로드 ID    |
