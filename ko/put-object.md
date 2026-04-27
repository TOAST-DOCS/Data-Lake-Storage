## PutObject

**Data & Analytics > Data Lake Storage > API 가이드 > Object > PutObject**

버킷에 객체를 저장합니다.

### 요청

```http
PUT /{bucket}/{objectKey} HTTP/1.1

Body
```

### 요청 파라미터

Data Lake Storage API에서 공통으로 사용하는 헤더 정보는 Data Lake Storage [API 요청 헤더 가이드](api-guide-common)를 참고하세요.

| 이름 | 구분 | 타입 | 필수 | 설명 |
| --- | --- | --- | --- | --- |
| bucket | Path | String | Y | 버킷 이름 |
| objectKey | Path | String | Y | 객체 이름 |
| Content-Type | Header | String | N | 객체 콘텐츠 타입 |
| Content-Length | Header | Long | Y | 객체 크기(bytes) |
| x-amz-storage-class | Header | String | N | 스토리지 클래스 |
| x-amz-meta-\* | Header | String | N | 사용자 정의 메타데이터 |

### 요청 본문

| 이름 | 타입 | 필수 | 설명 |
| --- | --- | --- | --- |
| Body | Binary | Y | 객체 데이터 |


### 응답

```http
HTTP/1.1 200 OK
ETag: String
```
