## ListBuckets

**Data & Analytics > Data Lake Storage > API 가이드 > Bucket > ListBuckets**

버킷 목록을 조회합니다.

### 요청

```http
GET /?max-buckets=20 HTTP/1.1
```

### 요청 파라미터

Data Lake Storage API에서 공통으로 사용하는 헤더 정보는 Data Lake Storage [API 요청 헤더 가이드](api-guide-common)를 참고하세요.

| 이름 | 구분 | 타입 | 필수 | 설명 |
| --- | --- | --- | --- | --- |
| continuation-token | Parameter | String | N | 다음 페이지 조회를 위한 연속 토큰 |
| max-buckets | Parameter | Integer | N | 반환할 최대 버킷 수 |
| prefix | Parameter | String | N | 버킷 이름 필터링 접두어 |

### 응답

```http
HTTP/1.1 200 OK

<?xml version="1.0" encoding="UTF-8"?>
<ListAllMyBucketsResult>
  <Buckets>
    <Bucket>
      <BucketRegion>string</BucketRegion>
      <CreationDate>timestamp</CreationDate>
      <Name>string</Name>
    </Bucket>
  </Buckets>
  <Owner>
    <DisplayName>string</DisplayName>
    <ID>string</ID>
  </Owner>
  <ContinuationToken>string</ContinuationToken>
  <Prefix></Prefix>
</ListAllMyBucketsResult>
```

| 이름 | 타입 | 설명 |
| --- | --- | --- |
| ListAllMyBuckets | Object | 버킷 목록 조회 결과 |
| ListAllMyBuckets.Buckets | Object | 버킷 정보 |
| ListAllMyBuckets.Buckets.BucketRegion | String | 버킷이 위치한 리전 |
| ListAllMyBuckets.Buckets.CreationDate | Timestamp | 버킷 생성 일시 (ISO 8601) |
| ListAllMyBuckets.Buckets.Name | String | 버킷 이름 |
| ListAllMyBuckets.Owner | Object | 버킷 소유자 정보 |
| ListAllMyBuckets.Owner.DisplayName | String | 소유자 표시 이름 |
| ListAllMyBuckets.Owner.ID | String | 소유자 ID |
| ListAllMyBuckets.ContinuationToken | String | 다음 페이지 조회용 연속 토큰 (마지막 페이지면 미포함) |
| ListAllMyBuckets.Prefix | String | 요청에 사용된 접두어 필터 |
