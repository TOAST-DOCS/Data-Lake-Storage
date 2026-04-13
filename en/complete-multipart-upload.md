## CompleteMultipartUpload

**Data & Analytics > Data Lake Storage > API 가이드 > Multipart > CompleteMultipartUpload**

업로드된 파트들을 조합하여 객체를 저장하고 멀티파트 업로드를 완료합니다.

### 요청

```http
POST /{bucket}/{objectKey}?uploadId={uploadId} HTTP/1.1

<?xml version="1.0" encoding="UTF-8"?>
<CompleteMultipartUpload xmlns="http://s3.amazonaws.com/doc/2006-03-01/">
   <Part>
     <PartNumber>Integer</PartNumber>
     <ETag>String</ETag>
   </Part>
</CompleteMultipartUpload>
```

### 요청 파라미터

Data Lake Storage API에서 공통으로 사용하는 헤더 정보는 Data Lake Storage [API 요청 헤더 가이드](https://docs.beta-nhncloud.com/en/Data%20&%20Analytics/Data%20Lake%20Storage/ko/api-guide-common/)를 참고하세요.

| 이름 | 구분 | 타입 | 필수 | 설명 |
| --- | --- | --- | --- | --- |
| bucket | Path | String | Y | 버킷 이름 |
| objectKey | Path | String | Y | 객체 이름 |
| x-amz-storage-class | Header | String | N | 스토리지 클래스 |
| uploadId | Parameter | String | Y | 멀티파트 업로드 ID |

### 요청 본문
| 이름 | 타입 | 필수 | 설명 |
| --- | --- | --- | --- |
| CompleteMultipartUpload | Object | Y | 멀티파트 업로드 완료 요청 |
| CompleteMultipartUpload.Part | Object | N | 파트 목록 |
| CompleteMultipartUpload.Part.PartNumber | Integer | N | 파트 번호 |
| CompleteMultipartUpload.Part.ETag | String | N | 객체 고유 식별자 |

### 응답

```http
HTTP/1.1 200 OK

<?xml version="1.0" encoding="UTF-8"?>
<CompleteMultipartUploadResult>
  <Location>String</Location>
  <Bucket>String</Bucket>
  <Key>String</Key>
  <ETag>String</ETag>
</CompleteMultipartUploadResult>
```

| 이름 | 타입 | 설명 |
| --- | --- | --- |
| CompleteMultipartUploadResult | Object | 멀티파트 업로드 완료 결과 |
| CompleteMultipartUploadResult.Location | String | 생성된 객체 경로 |
| CompleteMultipartUploadResult.Bucket | String | 대상 버킷 이름 |
| CompleteMultipartUploadResult.Key | String | 생성된 객체 키 |
| CompleteMultipartUploadResult.ETag | String | 최종 결합된 객체 고유 식별자 |
