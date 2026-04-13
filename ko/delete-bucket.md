## DeleteBucket

**Data & Analytics > Data Lake Storage > API 가이드 > Bucket > DeleteBucket**

버킷을 삭제합니다.

### 요청

```http
DELETE /{bucket} HTTP/1.1
```

### 요청 파라미터

Data Lake Storage API에서 공통으로 사용하는 헤더 정보는 Data Lake Storage [API 요청 헤더 가이드](https://docs.beta-nhncloud.com/ko/Data%20&%20Analytics/Data%20Lake%20Storage/ko/api-guide-common/)를 참고하세요.

| 이름 | 구분 | 타입 | 필수 | 설명 |
| --- | --- | --- | --- | --- |
| bucket | Path | String | Y | 버킷 이름 |

### 응답

```http
HTTP/1.1 204 No Content
```
