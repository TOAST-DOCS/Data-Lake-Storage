# HeadObject

**Data & Analytics > Data Lake Storage > API 가이드 > Object > HeadObject**

버킷에 저장된 객체의 메타데이터를 조회합니다.

### 요청

```http
HEAD /{bucket}/{objectKey} HTTP/1.1
```

### 요청 파라미터

Data Lake Storage API에서 공통으로 사용하는 헤더에 대한 정보는 Data Lake Storage API 요청 헤더(API 가이드 > Data Lake Storage API 공통 정보 > 요청 > 요청 헤더 링크 필요)를 참조해 주세요.

| 이름 | 구분 | 타입 | 필수 | 설명 |
| --- | --- | --- | --- | --- |
| bucket | Path | String | Y | 버킷 이름 |
| objectKey | Path | String | Y | 객체 이름 |
| x-amz-storage-class | Header | String | N | 스토리지 클래스 |

### 응답

```http
HTTP/1.1 200 OK
Content-Type: String
Content-Length: Long
ETag: String
Last-Modified: Timestamp
x-amz-storage-class: String
x-amz-meta-{key}: String
```
