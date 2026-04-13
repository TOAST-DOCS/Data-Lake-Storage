## CreateMultipartUpload

**Data & Analytics > Data Lake Storage > API 가이드 > Multipart > CreateMultipartUpload**

대용량 객체를 업로드할 수 있도록 멀티파트 업로드를 시작하고 업로드 ID를 생성합니다. 업로드 ID는 최대 한 시간 동안 유효합니다.

### 요청

```http
POST /{bucket}/{objectKey}?uploads HTTP/1.1
```

### 요청 파라미터

Data Lake Storage API에서 공통으로 사용하는 헤더 정보는 Data Lake Storage [API 요청 헤더 가이드](https://docs.beta-nhncloud.com/ko/Data%20&%20Analytics/Data%20Lake%20Storage/ko/api-guide-common/)를 참고하세요.

| 이름 | 구분 | 타입 | 필수 | 설명 |
| --- | --- | --- | --- | --- |
| bucket | Path | String | Y | 버킷 이름 |
| objectKey | Path | String | Y | 객체 이름 |
| Content-Type | Header | String | N | 객체 콘텐츠 타입 |
| x-amz-storage-class | Header | String | N | 스토리지 클래스 |
| x-amz-meta-\* | Header | String | N | 사용자 정의 메타데이터 |

### 응답

```http
HTTP/1.1 200 OK

<?xml version="1.0" encoding="UTF-8"?>
<InitiateMultipartUploadResult>
  <Bucket>String</Bucket>
  <Key>String</Key>
  <UploadId>String</UploadId>
</InitiateMultipartUploadResult>
```

| 이름 | 타입 | 설명 |
| --- | --- | --- |
| InitiateMultipartUploadResult | Object | 멀티파트 업로드 시작 결과 |
| InitiateMultipartUploadResult.Bucket | String | 대상 버킷 이름 |
| InitiateMultipartUploadResult.Key | String | 객체 키 |
| InitiateMultipartUploadResult.UploadId | String | 멀티파트 업로드 ID |
