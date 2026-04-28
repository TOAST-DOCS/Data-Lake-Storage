## GetObject

**Data & Analytics > Data Lake Storage > API 가이드 > Object > GetObject**

버킷에 저장된 객체를 조회합니다.

### 요청

```http
GET /{bucket}/{objectKey} HTTP/1.1
```

### 요청 파라미터

Data Lake Storage API에서 공통으로 사용하는 헤더 정보는 Data Lake Storage [API 요청 헤더 가이드](api-guide-common)를 참고하세요.

| 이름 | 구분 | 타입 | 필수 | 설명 |
| --- | --- | --- | --- | --- |
| bucket | Path | String | Y | 버킷 이름 |
| objectKey | Path | String | Y | 객체 이름 |
| Range | Header | String | N | 부분 다운로드 범위 |
| x-amz-storage-class | Header | String | N | 스토리지 클래스 |

### 응답

```http
HTTP/1.1 200 OK

data
```
