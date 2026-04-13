## ListObjectsV2

**Data & Analytics > Data Lake Storage > API 가이드 > Object > ListObjectsV2**

버킷에 저장된 객체 목록을 조회합니다.

### 요청

```http
GET /{bucket}?list-type=2&continuation-token={continuationToken}&delimiter={delimiter}&encoding-type={encodingType}&fetch-owner={fetchOwner}&max-keys={maxKeys}&prefix={prefix}&start-after={startAfter} HTTP/1.1
```

### 요청 파라미터

Data Lake Storage API에서 공통으로 사용하는 헤더 정보는 Data Lake Storage [API 요청 헤더 가이드](https://docs.beta-nhncloud.com/ko/Data%20&%20Analytics/Data%20Lake%20Storage/ko/api-guide-common/)를 참고하세요.

| 이름 | 구분 | 타입 | 필수 | 설명 |
| --- | --- | --- | --- | --- |
| bucket | Path | String | Y | 버킷 이름 |
| x-amz-storage-class | Header | String | N | 스토리지 클래스 |
| list-type | Parameter | String | Y | 2 고정(V2 API 구분자) |
| continuation-token | Parameter | String | N | 다음 페이지 조회 식별자 |
| delimiter | Parameter | String | N | 키 그룹 구분자(기본: /) |
| encoding-type | Parameter | String | N | 키 인코딩 방식 |
| fetch-owner | Parameter | Boolean | N | 소유자 정보 포함 여부 |
| max-keys | Parameter | Integer | N | 반환할 최대 객체 수 |
| prefix | Parameter | String | N | 객체 이름 접두어 |
| start-after | Parameter | String | N | 조회 시작 기준 |

### 응답

```http
HTTP/1.1 200 OK

<?xml version="1.0" encoding="UTF-8"?>
<ListBucketResult>
  <Name>String</Name>
  <Prefix>String/</Prefix>
  <StartAfter>String</StartAfter>
  <ContinuationToken>String</ContinuationToken>
  <NextContinuationToken>String</NextContinuationToken>
  <KeyCount>Integer</KeyCount>
  <MaxKeys>Integer</MaxKeys>
  <Delimiter>String</Delimiter>
  <IsTruncated>Boolean</IsTruncated>
  <EncodingType>String</EncodingType>
  <Contents>
    <Key>String</Key>
    <LastModified>Timestamp</LastModified>
    <ETag>String</ETag>
    <Size>Long</Size>
    <StorageClass>String</StorageClass>
    <Owner>
      <ID>String</ID>
    </Owner>
  </Contents>
  <CommonPrefixes>
    <Prefix>String</Prefix>
  </CommonPrefixes>
</ListBucketResult>
```

| 이름 | 타입 | 설명 |
| --- | --- | --- |
| ListBucketResult | Object | 객체 목록 조회 결과 |
| ListBucketResult.Name | String | 버킷 이름 |
| ListBucketResult.Prefix | String | 요청 시 지정한 객체 이름 접두어 |
| ListBucketResult.StartAfter | String | 요청 시 지정한 조회 시작 기준 |
| ListBucketResult.ContinuationToken | String | 이번 요청에 사용된 페이지 조회 식별자 |
| ListBucketResult.NextContinuationToken | String | 다음 페이지 요청 시 사용할 페이지 조회 식별자 |
| ListBucketResult.KeyCount | Integer | 응답 객체 수 |
| ListBucketResult.MaxKeys | Integer | 요청 시 지정한 최대 객체 수 |
| ListBucketResult.Delimiter | String | 요청 시 지정한 키 그룹 구분자 |
| ListBucketResult.IsTruncated | Boolean | 추가 페이지 존재 여부 |
| ListBucketResult.Contents | Array | 객체 목록 |
| ListBucketResult.Contents.Key | String | 객체 키 |
| ListBucketResult.Contents.LastModified | Timestamp | 마지막 수정 일시 (ISO 8601 형식) |
| ListBucketResult.Contents.ETag | String | 객체 고유 식별자 |
| ListBucketResult.Contents.Size | Long | 객체 크기(bytes) |
| ListBucketResult.Contents.StorageClass | String | 스토리지 클래스 |
| ListBucketResult.Contents.Owner.ID | String | 소유자 ID (fetch-owner=true 시 포함) |
| ListBucketResult.CommonPrefixes | Array | delimiter 기준으로 그룹화된 공통 prefix 목록 |
| ListBucketResult.CommonPrefixes.Prefix | String | delimiter 기준으로 그룹화된 경로 |
