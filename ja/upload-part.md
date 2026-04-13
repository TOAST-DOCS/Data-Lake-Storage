## UploadPart

**Data & Analytics > Data Lake Storage > API 가이드 > Multipart > UploadPart**

멀티파트 업로드의 파트를 업로드합니다. 업로드하기 전 CreateMultipartUpload API를 호출하여 멀티파트 업로드 ID를 생성해야 합니다.

### 요청

```http
PUT /{bucket}/{objectKey}?partNumber={partNumber}&uploadId={uploadId} HTTP/1.1

body
```

### 요청 파라미터

Data Lake Storage API에서 공통으로 사용하는 헤더 정보는 Data Lake Storage [API 요청 헤더 가이드](https://docs.beta-nhncloud.com/ja/Data%20&%20Analytics/Data%20Lake%20Storage/ko/api-guide-common/)를 참고하세요.

| 이름 | 구분 | 타입 | 필수 | 설명 |
| --- | --- | --- | --- | --- |
| bucket | Path | String | Y | 버킷 이름 |
| objectKey | Path | String | Y | 객체 이름 |
| x-amz-storage-class | Header | String | N | 스토리지 클래스 |
| partNumber | Parameter | Integer | Y | 파트 번호(1 \~ 10,000) |
| uploadId | Parameter | String | Y | 멀티파트 업로드 ID |

### 요청 본문
| 이름 | 타입 | 필수 | 설명 |
| --- | --- | --- | --- |
| Body | Binary | Y | 파트 바이너리 데이터, 최대 5GiB까지 업로드 가능 |

### 응답

```http
HTTP/1.1 200 OK
```
