## AbortMultipartUpload

**Data & Analytics > Data Lake Storage > API 가이드 > Multipart > AbortMultipartUpload**

진행 중인 멀티파트 업로드를 중단합니다.

### 요청

```http
DELETE /{bucket}/{objectKey}?uploadId={uploadId} HTTP/1.1
```

### 요청 파라미터

Data Lake Storage API에서 공통으로 사용하는 헤더 정보는 Data Lake Storage [API 요청 헤더 가이드](https://docs.beta-nhncloud.com/ko/Data%20&%20Analytics/Data%20Lake%20Storage/ko/api-guide-common/)를 참고하세요.

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
