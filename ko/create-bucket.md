# CreateBucket

**Data & Analytics > Data Lake Storage > API 가이드 > Bucket > CreateBucket**


버킷을 생성합니다. Data Lake Storage 서비스의 버킷 이름은 리전 유니크합니다.

### 요청

```http
PUT /{bucket} HTTP/1.1

<?xml version="1.0" encoding="UTF-8"?>
<CreateBucketConfiguration xmlns="http://s3.amazonaws.com/doc/2006-03-01/">
   <LocationConstraint>KR3</LocationConstraint>
</CreateBucketConfiguration>
```

### 요청 파라미터

Data Lake Storage API에서 공통으로 사용하는 헤더에 대한 정보는 Data Lake Storage API 요청 헤더(API 가이드 > Data Lake Storage API 공통 정보 > 요청 > 요청 헤더 링크 필요)를 참조해 주세요.

| 이름 | 구분 | 타입 | 필수 | 설명 |
| --- | --- | --- | --- | --- |
| bucket | Path | String | Y | 버킷 이름 |

### 요청 본문

| 이름 | 타입 | 필수 | 설명 |
| --- | --- | --- | --- |
| CreateBucketConfiguration | Object | Y | 생성할 버킷 정보 |
| CreateBucketConfiguration.LocationConstraint | String | Conditional | 리전 코드, 미 입력 시 엔드 포인트에 맞는 리전 정보 적용 |

### 응답

```http
HTTP/1.1 200 OK
Location: "/{bucket}"
```